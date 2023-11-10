


### http to https redirect

This is pretty simple just add a new server section to the top of the nginx .conf file

``` conf

server{
listen 80;
server_name <Your URL (e.g. serpub.net)>;
}

```

Then delete the last references to port 80 on the original  server