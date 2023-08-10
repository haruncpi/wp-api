<h1 align="center">WP API</h1>
<p align="center">
    <a href="https://creativecommons.org/licenses/by/4.0/"><img src="https://badgen.net/badge/licence/CC BY 4.0/23BCCB" />
    <a href="https://packagist.org/packages/haruncpi/wp-api"><img src="https://badgen.net/packagist/v/haruncpi/wp-api" /></a>
    </a>
    <a href=""><img src="https://badgen.net/packagist/dt/haruncpi/wp-api"/></a>
</p>
<p align="center">An elegant WordPress REST API routing system.</p>

![WP API](wp-api.png)

## Documentation

### Installation
```
composer require haruncpi/wp-api
```

### Configuration
In your plugin init file, write this simple config code.
```php
ApiConfig::set_route_file( __DIR__ . '/api-routes.php' )
		->set_namespace( 'MyPlugin\Api' )
		->init();
```

### Route Define

Open `api-routes.php` file and write route

```php
ApiRoute::get( 'myplugin/v1', '/me', 'ApiController@me' );
```
You can pass another callback to check auth
```php
ApiRoute::get( 'myplugin/v1', '/me', 'ApiController@me', 'AuthController@check' );
```

### Various way to write callback.
```php
ApiRoute::get( 'myplugin/v1', '/me', 'ApiController@me' );
```
```php
ApiRoute::get( 'myplugin/v1', '/me', array( ApiController:class, 'me' ) );
```
```php
ApiRoute::get( 'myplugin/v1', '/me', array( 'MyPlugin\Api\ApiController', 'me' ) );
```
```php
ApiRoute::get( 'myplugin/v1', '/me', function() {
    // Do something.
});
```

### Multiple route register
```php
ApiRoute::prefix( 'myplugin/v1', function( ApiRoute $route ) {
    $route->get( '/products', 'ApiController@products' );
    $route->get( '/categories', 'ApiController@categories' );
});
```

### With auth check
```php
// With auth check
ApiRoute::prefix( 'myplugin/v1', function( ApiRoute $route ) {
    $route->get( '/me', 'ApiController@me' );
    $route->get( '/settings', 'ApiController@settings' );
    $route->post( '/logout', 'ApiController@logout' );
})->auth( 'AuthController@check' );
```