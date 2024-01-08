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