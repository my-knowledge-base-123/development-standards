# Tech Solutions

## # Database

### ## Get error `SQLSTATE[HY000] [2002] Access denied for user 'root'@'172.xx.x.x'` or `SQLSTATE[HY000] [2002] Connection Refused` or `php_network_getaddresses: getaddrinfo for mysql failed: Name or service not known`

This is because mysql image failed to be built correctly.

Solution:

- Check you database settings in `.env`, should be like following:

    ```dotenv
    DB_CONNECTION=mysql
    DB_HOST=mysql
    DB_PORT=3306
    DB_DATABASE=laravel-sample
    DB_USERNAME=root
    DB_PASSWORD=
    ```

- Rebuild Docker container: remove images and volumes and rebuild

    ```shell
    sail down --rmi all -v
    sail up -d
    ```
