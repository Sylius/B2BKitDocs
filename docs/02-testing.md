# Sylius B2B Kit
## Testing

- [⬅️ Back](../README.md)

```bash
$ composer install
$ cd tests/Application
$ yarn install
$ yarn prod
$ APP_ENV=test bin/console assets:install
$ APP_ENV=test bin/console doctrine:database:create
$ APP_ENV=test bin/console doctrine:schema:create
$ APP_ENV=test symfony server:start --port=8080 -d
$ open http://localhost:8080
$ cd ../..
$ vendor/bin/phpunit
$ vendor/bin/phpspec run
$ vendor/bin/behat
```
