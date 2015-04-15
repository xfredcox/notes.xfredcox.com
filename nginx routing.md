# NGINX Routing

*Notes predominantly from the documentation. See sources.*

### Virtual Server-level Routing
 
**Name-based Virtual Servers:**

- The first step is  to decide which *server* should process the request
  - Test request header *Host*
  - If the header does not match any *server* or is empty, the request gets sent to the *default\_server* (first if not specified)
- *default\_server* is a property of the listen port, not the *server\_name*;
- To prevent requests from undefined hosts, create a *server\_name*  virtual server which returns 444 (a non-standard http code that closes the connection;

**Mixed Name-based & IP-based virtual Servers:**

- The first step is to test the IP address and port and **then** the host header

 ### Location Routing
 
- Selection Algorithm
  - First, the most specific **literal** location is selected, regardless of the order.
  - Next, REGEX locations are evaluated **in order** and any matches are immediately returned.
  - If no REGEX matches are found, the previously selected literal is used.
- Notes: 
  - The location directive only inspects the URI, excluding the query string.

### Directives
 
-  Root: file serving directory, inside server or location blocks
-  Index: specifies default file(s)
-  Server\_name
  - Defaults to empty server name
  - Selection algorithm: exact name > longest wildcard starting * > longest wildcard ending with * > first matching regex.
  - A wildcard name may contain an asterisk only on the names start or end, and only on a dot border.
  - REGEX names start with tilde. Names containing curly braces should be quoted, literal dots escaped with backslash, etc.
  - Names REGEX expressions can be used later:

``nginx
server { # KWARGS
    server_name   ~^(www\.)?(?<domain>.+)$;
    location / {
        root   /sites/$domain;
    }
} 
server { # ARGS
    server_name   ~^(www\.)?(.+)$;
    location / {
        root   /sites/$2;
    }
}
``

  - IP Address requests are sent in the Host header just like named hosts.
  -   _\_ is the catch-all server name_
  - For optimization (hash lookups) it worth being explicit about the most common literal server names instead of relying on the wildcard (ie. server\_name  example.org  www.example.org  *.example.org;)
  - The implementation requires defining a max hash size if you start using too many server names.
 
### Keywords

- $hostname: machine name

----

### Source

-          [How Does NGINX Process a Request]( http://nginx.org/en/docs/http/request_processing.html)
-          [NGINX Server Names](http://nginx.org/en/docs/http/server_names.html)
 
### Upcoming Material

- [Using NGINX as HTTP Load Balancer]( http://nginx.org/en/docs/http/load_balancing.html)
- [Configuring HTTPS Servers]( http://nginx.org/en/docs/http/configuring_https_servers.html)
- [NGINX Docs](http://nginx.org/en/docs/)
