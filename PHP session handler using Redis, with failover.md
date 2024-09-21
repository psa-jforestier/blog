# PHP session handler using Redis, with failover

Here is a a PHP file to include in your project, to add session support with a Redis storage backend, with failover support. It is a custom PHP session handler.

The code use the [PHPRedis extension](https://github.com/phpredis/phpredis). With this class, you can have a Redis session handler for PHP with failover support : if ou have multiple Redis server, if on is down, an other up server is used.
Warning, session are not replicated so in case the main Redis server is down, the session are lost, but your application is still up.

## Architecture

If you want to have high-disponibility appliance, you should have something like :
- 2 Apache servers running the same application
- you dont want to rely on a NFS server for session sharing between server (because of bad perf and failure of NFS).
- 2 Redis servers are available and you want to use them for session sharing and failover

A possible solution is to use your Redis servers for failover : the 1st server is the “master” and in case of failure, the Apache servers automaticaly switch to the 2nd server.

## How to use this class
- Copy and install the file “RedisSessionHandler.php” in your project.
- Copy and install the config file (or merge it with your existing file) “.htaccess” (or somewhere in your httpd.conf). See values inside this file and adapt them to your usage (server name, timeout etc…)
- Register the RedisSessionHandler by including the following inside all your PHP file :
`include_once('RedisSessionHandler.php');`

## Documentation

In the archive, you can download the RedisSessionHandler.php, the .htaccess config file, and a sample index.php file.
Download here the Redis session handler for PHP with failover support.
