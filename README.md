## Laravel + MongoDB

If you want to use MongoDB Atlas (the cloud version):
Sign in with Google, make a new cluster, copy the connection string and the rest will be same as if you were running a MongoDB Server locally, which i am focussing on in this little project..

The connection can be a bit tricky, especially if you are working on a Windows machine like i am at the moment.

1. Download the MongoDB Community, Compass (if you like or you use the VSCode extension for MongoDB) and/or the MongoShell
2. pecl seems to have problems on my system, so for me only the following worked:
- download latest version of MongoDB Driver (1.13.0) on: https://pecl.php.net/
- but: make sure that you download the correct Thread Safe dll (x64 or x86!), AND it has to match with the PHP version on your system!
- in your php.ini add: 
```sh
extension=mongodb
```
- copy the "php_mongodb.dll" into the extension folder
- and with 
```sh
php -m
```
- ..you can check if the mongodb driver is loaded correctly

3. If not already running, you can turn on the MongoDB Server under: Start -> services -> search for "MongoDB Server", and start the service
4. In Compass or the VSCode-MongoDB-Extension you can add the "mongodb://localhost:27017" and you are good to go..

---

### Getting started..

- mkdir for project
- cd into and type: code .
- open integrated terminal and type
```sh
composer create-project laravel/laravel . 
```

- if the latest mongodb driver is loaded correctly you can now install the MongoDB library

```sh
composer require jenssegers/mongodb
```

- in order for Laravel to communicate with the DB configure it under "connections" in config/database.php:
```sh
'connections' => [
  'mongodb' => [
        'driver' => 'mongodb',
        'dsn' => env('DB_URI', 'mongodb://localhost:27017'),
        'database' => 'laravel_mongo',
],
```
- In the same file define mongodb as default DB:
```sh
'default' => env('DB_CONNECTION', 'mongodb'),
```

- in "app.php" add:

```sh
'providers' => [
    Jenssegers\Mongodb\MongodbServiceProvider::class,
```

- finally change your .env like this for local development:
```sh
DB_CONNECTION=mongodb
DB_HOST=127.0.0.1
DB_PORT=27017
DB_DATABASE=laravel_mongo
DB_USERNAME=
DB_PASSWORD=
```



---

- delete the default migrations and create with artisan a new 
```sh
php artisan make:migration create_products_table
```

- if your connection works you can run migrate
```sh
php artisan migrate
```

- delete the UserFactory and create a new 
```sh
php artisan make:factory ProductFactory
```

- in "DatabaseSeeder.php" you can define what has to be seeded:
```sh
Product::factory(50)->create();
```

- seed the products table:
```sh
php artisan db:seed
```
---






