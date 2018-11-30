@title[Common problems]
## Common problems
<p class="fragment text-left text-07">file permissions</p>
<p class="fragment text-left text-07">file ownership</p>
<p class="fragment text-left text-07">different configurations</p>
<p class="fragment text-left text-07">firewall rules</p>
<p class="fragment text-left text-07">url rewriting</p>
<p class="fragment text-left text-07">discrete versions in composer.json</p>
<p class="fragment text-left text-07">configure the .env (specially the APP_ENV variable)</p>

+++
## Example docker entrypoint
```bash
#!/usr/bin/env bash
set -e
role=${CONTAINER_ROLE:-app}
env=${APP_ENV:-production}
if [ "$env" != "local" ]; then
    echo "Caching configuration..."
    (cd /var/www/html && php artisan config:cache && php artisan route:cache && php artisan view:cache)
fi
if [ "$role" = "app" ]; then
    exec apache2-foreground
elif [ "$role" = "queue" ]; then
    echo "Running the queue..."
    php /var/www/html/artisan queue:work --verbose --tries=3 --timeout=90
elif [ "$role" = "scheduler" ]; then
    while [ true ]
    do
      php /var/www/html/artisan schedule:run --verbose --no-interaction &
      sleep 60
    done
else
    echo "Could not match the container role \"$role\""
    exit 1
fi
```
