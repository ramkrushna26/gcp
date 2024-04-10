## Load Balancer

* **Backend**: group of endpoints that receives traffic from google cloud load balancer (ex: instance groups)
* **Frontend**: specify IP address, port & protocol. This IP address is frontend IP for client requests.
  * SSL certificates are must assinged to frontend
* **Host & Path Rules** (for https load balancing): define rules for redirecting traffic to different backends
  * based on path: site/a v site/b
  * based on host: a.site.com v b.site.com
  * based on HTTP headers (authorization headers): POST, GET etc.  

  
* Client to load balancer: over internet --> **HTTPS** recommended
* Load balancer to VM instances --> google internal network (HTTP/HTTPS)
* SSL/TLS Termination/offloading
  * client to load balancer --> HTTPS/TLS
  * load balancer to VM instances --> HTTP/TCP  
 
### Load balancer traffic flow
![image](https://github.com/ramkrushna26/gcp/assets/45620457/295361e0-39f6-46a1-bfc2-1b43380d018b)

### Types of load balancer
![image](https://github.com/ramkrushna26/gcp/assets/45620457/9a29b959-6ff6-460f-b23b-ab921a33290f)

### Scenarioes
* You want **only healthy instances to recieve traffic** ----> configure health check  
* You want **high availibility for your VM instances** ----> create multiple MIGs for your instances in multiple regions & balance the load using load balancer  
* You want **to route requests to multiple micro-services using the same load balancer** ---->
  * create individual MIGs and backend for each micro-services  
  * create host and path rules to redirect to specific micro-service backend based on path  
  * you can route to backend cloud storage as well  
* You want **to load balance global external HTTPS traffic across backend instances, across multiple regions** ----> choose external HTTPS load balancer
* You want **SSL termination for global non-HTTPS traffic with load balancing** ----> choose SSL proxy load balancer








