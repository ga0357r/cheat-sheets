# To specify a script path irrespective of the operating system

```bash
#!/bin/bash

# Resolve the absolute path of the script
SCRIPT_DIR="$(dirname "$(realpath "$0")")"

# Adjust the migrate script path based on the operating system
if [ -d "$SCRIPT_DIR/../byjg/migration-cli/scripts" ]; then
    MIGRATE_SCRIPT_PATH="$SCRIPT_DIR/../byjg/migration-cli/scripts/migrate"
elif [ -d "$SCRIPT_DIR\\..\\byjg\\migration-cli\\scripts" ]; then
    MIGRATE_SCRIPT_PATH="$SCRIPT_DIR\\..\\byjg\\migration-cli\\scripts\\migrate"
else
    echo "Unsupported operating system or script location"
    exit 1
fi

# Print the resulting migrate script path
echo "Migrate script path: $MIGRATE_SCRIPT_PATH"

# Execute the migrate script with the specified relative path
"$MIGRATE_SCRIPT_PATH" "$@"
```

# cases in bash and passing arguments

```bash
#!/bin/sh

ENV=$1 #the passed argument when running the bash script with "sh script.sh local"
case "$ENV" in
"local")
    cp env/.env.local .env;;
"dev")
    cp env/.env.dev .env;;
esac
```

# Platform dependent compilation
```bash
if uname -s | grep -iq "darwin"; then
    # macOS
    echo "Running on macOS"
else
    # Other OS
    echo "Running on Other OS"
fi
echo "Setup Complete - OS Name: $(uname -s)"
```

# Source the .env file and return variables
```bash
source .env   #source the env file and return variables

# Remove newline characters from DB_USER and DB_PASSWORD
DB_USER=$(echo -n "$DB_USER" | tr -d '\n\r')
DB_PASSWORD=$(echo -n "$DB_PASSWORD" | tr -d '\n\r')

# Replace the placeholders in the SQL file
sed -i "s|{{DB_USER}}|$DB_USER|g" src/.env.example
sed -i "s|{{DB_PASSWORD}}|$DB_PASSWORD|g" src/.env.example
```
