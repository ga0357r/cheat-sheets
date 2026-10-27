# Laravel

For React developers new to Laravel (TriFold build)

---

## Getting Started

```
# 1. Create the project
composer create-project laravel/laravel trifold
cd trifold

# 2. Set up the environment
cp .env.example .env
php artisan key:generate

# 3. Set your database in .env
# DB_CONNECTION, DB_DATABASE, DB_USERNAME, DB_PASSWORD

# 4. Create the tables
php artisan migrate

# 5. Install frontend packages
npm install

# 6. Run everything in one command
composer run dev
```

`composer run dev` starts the Laravel server, the queue worker, the log viewer and Vite together (Laravel 11+). It replaces running `php artisan serve`, `php artisan queue:listen` and `npm run dev` in separate terminals.

If you only want the server and the frontend separately:

```
php artisan serve        # http://localhost:8000
npm run dev              # Vite
```

### React starter (recommended)

```
composer require laravel/breeze --dev
php artisan breeze:install react
```

This gives you React + Inertia + login, register and password reset. Data comes from the controller as props, so you do not need `useEffect` fetches for most screens.

---

## React to Laravel Mental Model

| React world | Laravel world |
| --- | --- |
| `package.json` / npm | `composer.json` / Composer |
| `.env` / `import.meta.env` | `.env` / `env()` and `config()` |
| React Router | `routes/web.php`, `routes/api.php` |
| Page component | Controller method + Inertia page |
| `fetch` / axios | Route -> Controller -> JSON response |
| Redux / state store | Database via Eloquent models |
| Auth context | Middleware + auth guard |
| Zod / Yup | Form Requests / `$request->validate()` |
| Vite | Vite (built in) |

Request flow: **Route -> Middleware -> Controller -> Model/Service -> Response**

---

## Cheat Sheet

```
# To create a model and controller..use
php artisan make:model ModelName -mcr
```

`-m` migration, `-c` controller, `-r` resource controller (index, create, store, show, edit, update, destroy). Add `-f` for a factory and `-s` for a seeder.

### Common artisan commands

```
php artisan make:model Fixture -mfc          # model + migration + factory + controller
php artisan make:controller PlanController --resource
php artisan make:migration add_odds_to_fixtures_table
php artisan make:request StorePlanRequest
php artisan make:resource FixtureResource
php artisan make:job SendTeamNewsAlert
php artisan make:middleware EnsureSubscribed
php artisan make:seeder LeagueSeeder

php artisan migrate                          # run migrations
php artisan migrate:rollback                 # undo last batch
php artisan migrate:fresh --seed             # wipe and reseed (dev only)
php artisan route:list                       # list every route
php artisan tinker                           # REPL, like the browser console
php artisan config:clear && php artisan cache:clear
php artisan test                             # run tests
```

### Routing

```php
// routes/web.php
Route::middleware('auth')->group(function () {
    Route::get('/plan', [PlanController::class, 'show']);
    Route::get('/matches', [MatchController::class, 'index']);
    Route::get('/matches/{fixture}', [MatchController::class, 'show']);
    Route::post('/plan/lock', [PlanController::class, 'lock']);
});

// Resource routes (all CRUD routes in one line)
Route::resource('fixtures', FixtureController::class);
```

`{fixture}` with a type-hinted `Fixture $fixture` parameter loads the model automatically (route model binding).

### Controller with Inertia (React page)

```php
public function index()
{
    return Inertia::render('Matches/Index', [
        'fixtures' => Fixture::with(['homeTeam', 'awayTeam'])
            ->whereDate('kickoff_at', today())
            ->orderBy('kickoff_at')
            ->get(),
    ]);
}
```

```jsx
import { Head, Link } from '@inertiajs/react';

export default function Index({ fixtures }) {
  return (
    <>
      <Head title="Matches" />
      {fixtures.map(f => (
        <Link key={f.id} href={`/matches/${f.id}`}>
          {f.home_team.name} vs {f.away_team.name}
        </Link>
      ))}
    </>
  );
}
```

### Migration

```php
Schema::create('fixtures', function (Blueprint $table) {
    $table->id();
    $table->foreignId('league_id')->constrained();
    $table->foreignId('home_team_id')->constrained('teams');
    $table->foreignId('away_team_id')->constrained('teams');
    $table->timestamp('kickoff_at');
    $table->decimal('home_odds', 5, 2)->nullable();
    $table->json('ai_reasoning')->nullable();
    $table->timestamps();
});
```

Never edit a migration that has run in production. Create a new one instead.

### Model and relationships

```php
class Fixture extends Model
{
    protected $fillable = ['league_id', 'kickoff_at', 'home_odds', 'ai_reasoning'];

    protected $casts = [
        'kickoff_at' => 'datetime',
        'ai_reasoning' => 'array',
    ];

    public function league()   { return $this->belongsTo(League::class); }
    public function homeTeam() { return $this->belongsTo(Team::class, 'home_team_id'); }
    public function news()     { return $this->hasMany(TeamNews::class); }
}
```

### Eloquent queries

```php
Fixture::find(1);
Fixture::where('league_id', 3)->latest()->get();
Fixture::with('news')->paginate(20);   // with() avoids N+1 queries
Fixture::create([...]);                // needs $fillable
$fixture->update(['home_odds' => 1.72]);
$fixture->delete();
```

### Validation

```php
// php artisan make:request StorePlanRequest
public function rules(): array
{
    return [
        'bankroll'   => ['required', 'numeric', 'min:1000'],
        'split.safe' => ['integer', 'between:0,100'],
    ];
}
```

Type-hint the request in the controller method and Laravel validates automatically.

### Auth and TriFold+ gating

```php
// Protect routes
Route::middleware(['auth', 'subscribed'])->get('/alerts/instant', ...);

// Current user
$request->user();
auth()->user();
```

Use Sanctum for API tokens: `composer require laravel/sanctum`.

### API resources (shape the JSON)

```php
class FixtureResource extends JsonResource
{
    public function toArray($request): array
    {
        return [
            'id'        => $this->id,
            'title'     => $this->homeTeam->name . ' vs ' . $this->awayTeam->name,
            'kickoff'   => $this->kickoff_at->toIso8601String(),
            'reasoning' => $this->when($request->user()?->isPlus(), $this->ai_reasoning),
        ];
    }
}
```

### Queues and scheduler (alerts and daily plan)

```php
// Dispatch a job
SendTeamNewsAlert::dispatch($fixture, $news);

// routes/console.php
Schedule::command('plan:generate')->dailyAt('08:00');
Schedule::command('news:refresh')->everyFiveMinutes();
```

```
php artisan queue:work                 # run a worker
* * * * * php artisan schedule:run     # one cron entry on the server
```

### Calling the AI service

```php
// config/services.php
'ai' => ['key' => env('AI_API_KEY')],

// app/Services/ReasoningService.php
$response = Http::withToken(config('services.ai.key'))
    ->timeout(60)
    ->post('https://api.example.com/v1/messages', ['prompt' => $prompt]);

$data = $response->throw()->json();
```

Run long AI calls in a queued job, save the result in the `ai_reasoning` column, and show the saved copy in the UI.

### Environment

```
# Only call env() inside config/*.php, then use config('services.ai.key') elsewhere
# Never commit .env, commit .env.example
php artisan config:clear      # after changing .env
```

### Deployment checklist

```
composer install --no-dev --optimize-autoloader
npm ci && npm run build
# .env: APP_ENV=production, APP_DEBUG=false, real DB and AI keys
php artisan migrate --force
php artisan config:cache && php artisan route:cache && php artisan view:cache
# Web root points to /public
# Run the queue worker (Supervisor) and the scheduler cron
```

---

## TriFold Quick Mapping

| Screen / area | Laravel pieces |
| --- | --- |
| Today's Plan / Plan detail | `PlanController`, `Plan` + `PlanLeg` models, daily scheduler job |
| Matches / Match detail | `FixtureController`, `Fixture`, `TeamNews`, `ai_reasoning` JSON |
| Alerts | `Alert` model, queued `SendTeamNewsAlert` job |
| Discover / Article | `Article` model, filtered by user consent flags |
| Track record | `PlanResult` model, aggregate queries |
| Me | users table + consent and settings columns |
| Paywall | subscription flag or Cashier, `EnsureSubscribed` middleware |
| Admin panel | Filament (or custom admin routes) for analysis, teams, fixtures, leagues, plans, users |

---

## Gotchas for React Developers

- Laravel is server-first: data arrives from the controller, not from `useEffect`.
- Variables start with `$`, methods use `->`, static calls use `::`, strings join with `.` not `+`.
- Forgetting `$fillable` causes a mass-assignment error.
- Use `with()` to avoid N+1 queries, the most common performance bug.
- After editing `.env` or config, run `php artisan config:clear`.
- Docs: laravel.com/docs, inertiajs.com, laracasts.com

---
