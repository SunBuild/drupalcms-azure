Drupal for Azure App Service    [![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/)

This template support [MySQL in-app feature](https://blogs.msdn.microsoft.com/appserviceteam/2016/08/18/announcing-mysql-in-app-preview-for-web-apps/) on App service. Click on Deploy to Azure button above to start deployment. 

Once the app is successfully deployed and the drupal installer has completed the installation , update settings.php to use environement variable whether you are using ClearDB MySQL database or MySQL in-app on Azure app service.  

```
        $connectstr_dbfullhost = '';
        $connectstr_dbhost = '';
        $connectstr_dbname = '';
        $connectstr_dbusername = '';
        $connectstr_dbpassword = '';
        foreach ($_SERVER as $key => $value) {
            if (strpos($key, "MYSQLCONNSTR_") !== 0) {
                continue;
            }

            $connectstr_dbfullhost = preg_replace("/^.*Data Source=(.+?);.*$/", "\\1", $value);
            $connectstr_dbhost = substr($connectstr_dbfullhost,0,strpos($connectstr_dbhost,":"));
            $connectstr_dbname = preg_replace("/^.*Database=(.+?);.*$/", "\\1", $value);
            $connectstr_dbusername = preg_replace("/^.*User Id=(.+?);.*$/", "\\1", $value);
            $connectstr_dbpassword = preg_replace("/^.*Password=(.+?)$/", "\\1", $value);
        }

         $connectstr_port =   getenv('WEBSITE_MYSQL_PORT');
         if (empty($connectstr_port)){
               $connectstr_port= 3306;
         }
           
        $databases['default']['default'] = array (
          'database' => $connectstr_dbname,
          'username' => $connectstr_dbusername,
          'password' => $connectstr_dbpassword,
          'prefix' => '',
          'host' => $connectstr_dbhost,
          'port' => $connectstr_port ,
          'namespace' => 'Drupal\\Core\\Database\\Driver\\mysql',
          'driver' => 'mysql',
        );

```
