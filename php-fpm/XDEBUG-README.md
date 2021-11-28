# xDebug Setup Notes 
You can verify that "phpstorm" is listening on the configured port via:
 - Linux: netstat -tulpn | grep "9000" (swap 9000 for configured IDE listening port)
 - Mac: sudo lsof -i -n -P | grep "TCP.*:9000" (swap 9000 for configured IDE listening port)
 - Windows: ?
 Steps to Debug xDebug not working:
 - php-fpm
   - Check if xDebug is installed:
     - Run docker-compose exec php-fpm bash
     - Run php-config --extension-dir to get the php extensions dir
     - Run ls THE_DIR_FROM_PREV_STEP to view installed extensions
     - If xdebug is not present, double-check your PHP_FPM_INSTALL_XDEBUG env var is set to true, and look for
       errors during the build process that indicate failure to install xdebug. The installation process depends on
       the PHP version--see [laradock dir]/php-fpm/Dockerfile > xDebug: section
   - Check if xDebug is loaded/running:
     - Add phpinfo(); exit; to the beginning of your public/index.php script and search for xdebug. If you don't see
       sections detailing xdebug settings (ie. remote_port), it's not loaded. xdebug is supposedly supposed to start
       automatically if PHP_FPM_INSTALL_XDEBUG = true, but if it doesn't, you can run ./php-fpm xdebug
       start|stop|status from the same place you run docker-compose to manually start/stop it
   - When attempting a debug session, check xdebug logs:
     - You should see that it connected: http://zsl.io/BS8H3d
     - If you have set breakpoints, you should see xdebug communicating with your IDE for each breakpoint:
       http://zsl.io/eRjb09
   - Make sure your PHPStorm Config is correct:
     - Set the Debug port to your configured xdebug.client_port: http://zsl.io/37fLTD
     - Set your DBGp Proxy settings to IDE Key: PHPSTORM & Port: [client_port]: http://zsl.io/cNpz8P
     - Set your server settings correctly: http://zsl.io/va14WN
       - NOTE: It evidently doesn't like the leading slash on the /var/www for path mappings
       - NOTE: I was not able to get zero-config working w/ no path mappings
   - If xDebug is connecting to PHPStorm (as shown in the xdebug logs), PHPStorm should show some sign of receiving
     the connection, and lead you form there (might have path mappings or other settings wrong)
 - workspace
   - Setup and xdebug.ini is pretty much the same, but I chose to send to a different log file


# Refs:
- Settings Docs: https://2.xdebug.org/docs/all_settings
- xDebug v2 > v3 Upgrade Guide: https://xdebug.org/docs/upgrade_guide
- How xDebug Works w/ PHPStorm: https://crosp.net/blog/software-development/web/php/understanding-and-using-xdebug-with-phpstorm-and-magento-remotely/
