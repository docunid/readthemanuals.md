# Proxy

HAproxy = proxy + loadbalancer
Nginx = proxy + web server + load balancer
squid is a caching and forwarding HTTP web proxy

HaProxy is the best opensource loadbalancer on the market.
Varnish is the best opensource static file cacher on the market.
Nginx is the best opensource webserver on the market.

(of course this is my and many other peoples opinion)

But generally.. not all queries go through the entire stack..

Everything goes through haproxy and nginx/multiple nginx's.
The only difference is you "bolt" on varnish for static requests..


__HAproxy__ is simpler (being just a tcp proxy without an http implementation), which makes it faster and less bug prone. I've had nginx crash on me in a reverse-proxy-load-balancer configuration, but not haproxy. The downside is that you can't route based on information in the http layer, like session cookies or url paths. nginx can also cache requests, which haproxy can't do.

__HAProxy__ is a superior load balancer to nginx. HAProxy can do out-of-band health checks, whereas nginx only knows a backend to be "down" when it serves a 500. Also, HAProxy is a general TCP load balancer, whereas nginx will work only on HTTP traffic.

 Others in this thread say that nginx does "complicated tasks" better, but chances are, if you're doing a "complicated task" in your load balancer, you've made a mistake somewhere in your application design.

A **forward proxy** is a proxy configured to handle requests for a group of clients under the local Administrators control to an unknown or arbitrary group of resources that are outside of their control. Usually the word “forward” is dropped and it is referred to simply as a proxy, this is the case in Microsoft’s topology. A good example is a web proxy appliance which accepts web traffic requests from client machines in the local network and proxies them to servers on the internet. The purpose of a forward proxy is to manage traffic to the client systems

A **reverse proxy** is a proxy configured to handle requests from a group of remote or arbitrary clients to a group of known resources under the control of the local Administrator. An example of this is a load balancer (a.k.a. application delivery controller) that provides application high availability and optimization to workloads like as Microsoft Skype, Exchange and SharePoint. The purpose of a reverse proxy is to manage the server systems.
