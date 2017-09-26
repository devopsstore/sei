# ansible-role-nginx

A versatile role to setup nginx, created using this philosophy: 
https://github.com/jriguera/ansible-role-pattern/blob/master/README.md

Ansible 2.0, works with Ubuntu Trusty, Xenial and Centos 7

This role supports the definition of different vhosts and parameters, 
for example, it is capable of copying ssl certificates to the server 
if they are defined. 


## Configuration

Have a look at the default configuration parameters defined in `default/main.yml`

You can see some examples there. For instance, to define a virtual
host with all available options (name is the only required parameter):
```
nginx_server_list:
  - name: "example.com"
    root: "/var/www/example.com"
    delete: False
    listen: 
      - ["80", "default_server"]
      - ["443", "ssl"]
    alias: ["www.example.com"]
    charset: "utf-8"
    disable_symlinks: "off"
    limit_rate: 0
    limit_rate_after: 0
    error_page: ["404", "/404.html"]
    access_log: "/var/log/nginx/name_access.log"
    index: ["index.html", "index.htm"]
    rewrite: 
      - "^/users/(.*)$ /show?user=$1? last"
    return: "403"
    location_list:
      - name: "^~ /images/"
        confiration: "parameters"
        try_files: "$uri $uri/ 404"
```

Example with a proxy backend
```
nginx_server_list:
  - name: "proxybackend"
    delete: False
    listen: 
      - ["80", "default_server"]
    access_log: "/var/log/nginx/name_access.log"
    upstream:
      strategy: "ip_hash"
      server_list:
        - "srv1.example.com"
        - "srv2.example.com weight=3"
        - "srv3.example.com"
    location_list:
      - name: "/"
        proxy_pass: "http://proxybackend"
``` 
You can overwrite these default parameters as role variables. Have a look at 
the example in `site.yml` with vagrant and test it by using `vagrant up`.


## Author

José Riguera López <jriguera@gmail.com>
