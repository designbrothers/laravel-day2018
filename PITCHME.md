
@title[Splash]
## Deploying Laravel from dev to production

---
@title[Who and What]
### about me
```php
$speaker = new Nerd();
$speaker->fullName = 'Riccardo Scasseddu';
$speaker->twitterHandle = '@ennetech';
$speaker->education = 'Graduated in Computer Science';
$speaker->occupation = 'Technical lead @ designbrothers';
$speaker->roles = ['Full Stack Developer', 'DevOps'];
$speaker->wannaBe = 'System architect';
$speaker->talkSpeed = 1.2;
$speaker->save();
```

### focus
<p class="fragment text-left text-07">Give an overview of different 'infrastructures' and 'platforms'</p>

---
@title[Requirements]
## Laravel Requirements
<p class="fragment text-left text-07">WEBSERVER: apache2 / nginx / caddy / (swoole / php-pm / roadrunner)</p>
<p class="fragment text-left text-07">DATABASE: MySQL / PostgreSQL / SQL Server / <span class="text-red">SQLite</span></p>
<p class="fragment text-left text-07">CACHE: memcached / redis / database / <span class="text-red">file</span></p>
<p class="fragment text-left text-07">SESSION: cookie / database / memcached / redis / <span class="text-red">file</span> / <span class="text-red">array</span></p>
<p class="fragment text-left text-07">STORAGE: s3 / sftp / <span class="text-red">local</span></p>
<p class="fragment text-left text-07">QUEUE: database / redis</p>
<p class="fragment text-left text-07">LOGGING: monolog-remote / slack / <span class="text-red">single (file)</span></p>

+++
### Suggestion
<table>
  <tr>
    <th>ENV</th>
    <th>DATABASE</th>
    <th>STORAGE</th>
    <th>LOGGING</th>
    <th>MAIL</th>
  </tr>
  <tr>
    <td>DEV</td>
    <td>local MySQL</td>
    <td>local / minio</td>
    <td>single</td>
    <th>log</th>
  </tr>
  <tr>
    <td>STAGE</td>
    <td>SaaS MySQL</td>
    <td>SaaS s3</td>
    <td>slack</td>
    <th>MailHog</th>
  </tr>
  <tr>
    <td>PROD</td>
    <td>SaaS MySQL</td>
    <td>SaaS s3</td>
    <td>any remote</td>
    <th>SaaS SMTP</th>
  </tr>
</table>

---
@title[Deployment routine]
## Deployment routine
<p class="fragment text-left text-07">composer install --optimize-autoloader --no-dev</p>
<p class="fragment text-left text-07">php artisan config:cache</p>
<p class="fragment text-left text-07">php artisan route:cache</p>
<p class="fragment text-left text-07">php artisan migrate</p>


---
@title[Panoramic]
![QR](assets/img/comparison.jpg)


---
@title[Local]
## local / self-hosted / OnPremises
<p class="fragment text-left text-07">total control of the environment</p>
<p class="fragment text-left text-07">usually difficult to scale</p>
<p class="fragment text-left text-07">no failover</p>
<p class="fragment text-left text-07">slow setup time</p>
<p class="fragment text-left text-07">requires hardening</p>
<p class="fragment text-left text-07">very flexible</p>
<p class="fragment text-left text-07">total control of the underlying OS</p>

+++
### dev Tools
<p class="fragment text-left text-07">Homestead</p>
<p class="fragment text-left text-07">Valet</p>
<p class="fragment text-left text-07">php artisan serve</p>

+++
### Example
<p class="fragment text-left text-07">install and configure the os</p>
<p class="fragment text-left text-07">configure webserver + php</p>
<p class="fragment text-left text-07">configure cron</p>
<p class="fragment text-left text-07">configure queue</p>
<p class="fragment text-left text-07">copy files</p>

---
@title[IaaS]
## iaas
<p class="fragment text-left text-07">pay as you go</p>
<p class="fragment text-left text-07">deploy new resources in very short time</p>
<p class="fragment text-left text-07">if used with configuration management allows consistent and fast deployment</p>

+++
### Example
<p class="fragment text-left text-07">build configuration files to embrace infrastructure as code</p>
<p class="fragment text-left text-07">package the new artifact</p>
<p class="fragment text-left text-07">'deploy'</p>
---
@title[PaaS]
## paas
<p class="fragment text-left text-07">no control on the underlying OS</p>
<p class="fragment text-left text-07">every provider uses his own configurations</p>
<p class="fragment text-left text-07">very simple to start using git or a cli</p>
<p class="fragment text-left text-07">trusted proxies have to be configured</p>

+++
### Example
<p class="fragment text-left text-07">add configuration file to the repo</p>
<p class="fragment text-left text-07">use the provider cli / git to deploy</p>
---
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

---
@title[PaaS]
![QR](assets/img/faas_furious.jpg)
---
@title[Questions]
# Questions?
![QR](assets/img/qr.png)
<p style="text-align: center !important;">https://joind.in/talk/53009</p>
---
