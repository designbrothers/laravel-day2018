@title[Deployment routine]
## Deployment routine
<p class="fragment text-left text-07">composer install --optimize-autoloader --no-dev</p>
<p class="fragment text-left text-07">php artisan config:cache</p>
<p class="fragment text-left text-07">php artisan route:cache</p>
<p class="fragment text-left text-07">php artisan migrate</p>

