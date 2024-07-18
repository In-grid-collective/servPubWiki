
## File uploads

###  update php.ini

you can check the info from the [[https://www.mediawiki.org/wiki/Manual:Configuring_file_uploads#Set_maximum_size_for_file_uploads]] but we will run through the steps here.

#### Finding php.ini

php.ini can be in many places depending on setup, but you can easily find it with:

``` shell
sudo find / -name php.ini
```

This will give out directories of where it is, and ther can be a couple. For file upload we want the one with `/fpm/` in the directory as this is the **file package manager** bit.

open it up the directory in the last step with nano by:
``` shell
sudo nano <directory>
```


#### Editing php.ini

now we can set both of these values to what you desire. The max without extra configuration is 100M, and us `M` after the number.  The main thing 

- [post_max_size](https://php.net/post_max_size), 8 megabytes large by default, if you set it to 0 it means infinite.
- [upload_max_filesize](https://php.net/upload_max_filesize), 2 megabytes large by default. and the limit is 100M without extra configuration. 



### Update nginx

open your nginx conf in nano and then add this line with the same value  you set for upload_max_file.

```
server {
    client_max_body_size 8M;

    //other lines...
}
```

If you are hosting multiple sites add it to the http context like so;

```
http {
    client_max_body_size 8M;

    //other lines...
}
```

**NOTE:** If you have a vpn setup like we have in other places of this doc, this needs to be done on both the sever and client in the subnet.

## Restarting Nginx and PHP

If you don't know your php version check it with:

``` shell 
php -v
```

Now you need to restart nginx and php to reload the configs. This can be done using the following commands, and changing `8.1` to the right version:

```
sudo service nginx restart
sudo service php8.1-fpm restart
```

