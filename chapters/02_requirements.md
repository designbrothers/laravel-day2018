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
### in dev remember
<p class="fragment text-left text-07">use xdebug</p>
<p class="fragment text-left text-07">use the App::environment() helper to enable or disable specific features</p>

+++
### Suggestion
<table>
  <tr>
    <th>ENV</th>
    <th>DATABASE</th>
    <th>STORAGE</th>
  </tr>
  <tr>
    <td>DEV</td>
    <td>local MySQL</td>
    <td>local / minio</td>
  </tr>
  <tr>
    <td>STAGE</td>
    <td>PaaS MySQL</td>
    <td>PaaS s3</td>
  </tr>
  <tr>
    <td>PROD</td>
    <td>PaaS MySQL</td>
    <td>PaaS s3</td>
  </tr>
</table>