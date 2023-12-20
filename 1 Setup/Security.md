# Security

- DONT DUMP `$_SERVER` or `$_ENV` or `phpinfo()` it will show database credentials
- The [Symfony Profiler](https://symfony.com/doc/5.4/profiler.html) (the debug toolbar) must never be enabled in production
  - It shows env vars and other sensitive things

## Forms

### [CSRF Protection](https://symfony.com/doc/5.4/security/csrf.html)
