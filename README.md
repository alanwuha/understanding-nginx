# Understanding nginx

Notes on the design and working principles of [nginx](https://nginx.org).

## Configuration File's Structure

nginx consists of modules which are controlled by __directives__ specified in the configuration file (/etc/nginx/nginx.conf):

### Directives
1. Simple directive
1. Block directive

#### Simple Directive
Name and parameters separated by spaces and ends with a semicolon (;).

```
worker_processes 1;

user nobody nogroup;
error_log /var/log/nginx/error.log warn;
pid /var/run/nginx.pid;
```

#### Block Directive
Name and parameters separated by spaces and ends with a set of additional instructions surrounded by braces (`{` and `}`).

```
location / {
    try_files $uri @proxy_to_app;
}
```

If a block directive can have other directives (both simple and block) inside braces, it is called a __context__. Examples of contexts are `events`, `http`, `server`, and `location`.

```
events {
    worker_connections 768;
    multi_accept on;
}
```

### Main Context

Directives placed in the configuration file outside of any contexts are considered to be in the __main context__. The `events` and `http` directives reside in the __main context__, `server` directive in `http`, and `location` directive in `server`.

```
worker_processes 1;                             # main context

http {                                          # main context
    server {                                    # context
        listen 80 default_server;
        return 444;
    }
}
```

### Comments

The rest of a line after the `#` sign is considered a comment.

```
# this is a comment
```