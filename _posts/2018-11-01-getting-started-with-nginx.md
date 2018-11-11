---
title: Getting Started with NGINX
author: tshanky
permalink: /2018/11/01/getting-started-with-nginx/
modified:
excerpt: "Getting started with NGINX"
tags: [nginx, webserver, reverse-proxy, load balancer]
---
Nginx, often written as NGINX or nginx, is a very versatile web server that can be used as a reverse proxy, load balancer, mail proxy, and HTTP cache. Its very easy to setup and requires minimal knowledge to accomplish basic things like serving web pages.

## Installing NGINX on macOS Mojave

The easiest way to install nginx on macOS Mojave is by using [Homebrew](https://brew.sh/ "Homebrew"){:target="_blank"}. If you have brew installed, update brew, and install NGINX as follows:

```
brew update
brew install nginx
```

Once installed, nginx can be started by simply invoke the `nginx` command.

```
sudo nginx
```

That's it! Open [http://localhost:8080](http://localhost:8080){:target="_blank"} in your favorite browser and you should see the Nginx welcome page.

## Configuring to Serve Static Content

As a pre-requisite get some static content ready to serve. Create a couple of directories, one for *html* pages and one for *images*.

```
sudo mkdir -p /data/www
sudo mkdir -p /data/images
```
> You don't have to create the directories at `/data`. They could be anywhere, in a place of your choice, on the file system. Just make sure to update the `location` parameter in the configuration file accordingly.

Then, put a of piece of content in each of these directories. For example, create an *html* file, named `index.html`, and add some text content to that file and put it in the `/data/www` directory. Similarly, get an image, say `an_image.png`, and put it in the `/data/images` directory.

Once you have your static content ready, its time to update the configuration to instruct nginx to serve content from these directories.

If you installed nginx using [Homebrew](https://brew.sh/ "Homebrew"){:target="_blank"}, your config file, `nginx.conf`, is at `/usr/local/etc/nginx/`. Open the config file and modify the `http` directive so that it looks like so:

```
http {
    include       mime.types;
    default_type  application/octet-stream;

    sendfile        on;

    keepalive_timeout  65;

    server {
        server_name  localhost;

        location / {
            root /data/www;
        }

        location /images/ {
            root /data;
        }
    }
}
```

Start the server using `sudo nginx` or reload the configuration for a running server using `sudo nginx -s reload`. Your static content is now ready to be served by nginx.

Open your browser and simply point it to [http://localhost](http://localhost "index at localhost"){:target="_blank"} to view the contents of `index.html`. Pointing to [http://localhost/images/an_image.png](http://localhost/images/an_image.png "images at localhost"){:target="_blank"} will display your image.

## Understanding nginx Configuration

Nginx consists of a number of powerful and feature rich modules. These modules are controlled by the directives in the configuration file. Understanding the structure, syntax, and semantics of the configuration file is key to leveraging nginx to your advantage.

Nginx configuration has a notion of hierarchical directives and instructions. Each hierarchical level forms a context. The root context in nginx terminology is the *main* context. This context includes directives for essential elements like *events* and protocols, like *http*. It also includes configuration for *error_log*,  *worker* processes, *ssl_engine*, and for debugging. Some of these could be in inner nested contexts as well.

The *main* context can, and most likely will, have inner contexts. *events* and *http* are typical inner contexts in web server configurations.

Configuration directives are either **simple** directives or **block** directives. **simple** directives are key-value pairs written in a single line, separated by spaces, and terminated by a semi-colon (;). For such configuration, the *name* is the key and the configuration *parameter* is the value. An example of such a configuration is:

```
worker_connections  1024;
```

A **block** directive is also a key-value pair but in this case, the value is enclosed within a pair of braces ({}). Also, a semi-colon isn't used to terminate such a configuration directive. The enclosed set of braces can contain additional nested directives, which could be **simple** or **block** directives. The configuration we used for serving static content in the last section shows the `http` **block** configuration directive that has both **simple** and **block** directives within it. `include  mime.types;` is a **simple** nested directive and the `server` directive is a **block** directive.

The `location` directive plays a special role for routing and directing requests. There can be, and often are, multiple `location` directives for a given `server` directive. The reason for having multiple `location` directives is to route requests basis URI values and patterns. This indirectly facilitates handling requests by resource type, content type, and request parameters.

Request URIs are matched to the longest and most specific pattern first and then matched to the next closest pattern and so on, until the only match is the shortest and catch all prefix, the forward slash(/). Regular expressions could be used in a `location` prefix allowing for definition of sophisticated patterns.

You will benefit from familiarizing yourself with the various configuration directives for the [core module](https://nginx.org/en/docs/ngx_core_module.html "Nginx Core Module Configuration"){:target="_blank"} and the [http module](http://nginx.org/en/docs/http/ngx_http_core_module.html "Nginx HTTP Core Module Configuration"){:target="_blank"}. 

## Proxying Requests

You are probably familiar with a forward proxy, sometimes simply known as a proxy or a proxy server, which is an intermediary for client requests seeking resources from other servers. Forward proxies could be in the same internal network as the clients.

A reverse proxy is a server side counterpart to a forward proxy. A reverse proxy acts as an intermediary for one or more servers. It accepts client requests and retrieves responses from appropriate upstream servers. A reverse proxy could provide or assist in additional features like buffering, caching, and load balancing. Additionally, it could act as an application firewall.

Setting up nginx as a reverse proxy is easy. For a proxy where communication is exclusively over HTTP, the `proxy_pass` directive can do most of the heavy lifting. Here is an example configuration where a proxy server setup sends all requests to an upstream server supporting HTTP. 

```
worker_processes  1;
events {
    worker_connections  1024;
}
http {
    include       mime.types;
    default_type  application/octet-stream;
    send_timeout 1800;
    sendfile        on;
    keepalive_timeout  6500;
    server {
        listen       80;
        server_name  localhost;
        location / {
          proxy_pass          http://localhost:3000;
          proxy_set_header    Host             $host;
          proxy_set_header    X-Real-IP        $remote_addr;
          proxy_set_header    X-Forwarded-For  $proxy_add_x_forwarded_for;
          proxy_set_header    X-Client-Verify  SUCCESS;
          proxy_set_header    X-Client-DN      $ssl_client_s_dn;
          proxy_set_header    X-SSL-Subject    $ssl_client_s_dn;
          proxy_set_header    X-SSL-Issuer     $ssl_client_i_dn;
          proxy_read_timeout 1800;
          proxy_connect_timeout 1800;
        }
    }
}
```

This could satisfy a case where nginx communicates via HTTP on port 80 and channels all communication to an internal web server, possibly running a Node.js or a Rails application on port 3000. In this case both the nginx server and the upstream server are running in the same environment. This isn't a requirement. Nginx can proxy requests over to remote servers as well.

We will explore the powerful features around buffering, caching, and load balancing in a subsequent post. For now, you should be convinced that simple reverse proxy setup is quick and easy.

## Looking through the Logs

By default, your nginx log files on your [Homebrew](https://brew.sh/ "Homebrew"){:target="_blank"} installed nginx are at `/usr/local/var/log/nginx`. There are 2 log files in this directory: `access.log` and `error.log`. The names suggest what they contain. Information about the remote user, remote address, and request time along with the request URI, referrer, and user agent are logged in `access.log`. All errors are logged in `error.log`. Fatal emergencies leading to server crash are logged as `emerg` in the error log.

> You can change the path, logging level, and format of the log files by specifying it in `nginx.conf`. For example, you could log everything that is a warning and more severe in error.log at `logs/error.log` using a configuration as follows: `error_log logs/error.log warn;`

## Conclusion

You have learnt the basics of nginx in this post. We have barely scratched the surface but hopefully you are beginning to believe that nginx is both very easy to use and very powerful in terms of features.











