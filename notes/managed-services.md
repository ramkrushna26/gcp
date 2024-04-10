## Managed Services

### Service (Category) ----> Details
* **Compute Engine (IaaS)** ----> High performance general purpose VMs that scale globally
* **Google Kubernetes Engine (CaaS)** ----> orchestrated containalized micro-service on kubernetes. Needs advanced cluster configurations & monitoring
* **App Engine (PaaS(CaaS,Serverless))** ----> Build highly scalable applications on a fully managed platform using open & familier languages & tools
* **Cloud Functions (FaaS, Serverless)** ----> build event driven applications using simple, single-purpose functions
* **Cloud Run (CaaS (Serverless))** ----> Develop & deploy highly scalable containerized applications. Do not need a cluster.


### Serverless
* Important features of serverless
  * 1: No need to worry about infrastructure, scaling & availibility
  * 2: zero invocations ----> Zero cost (can scale down to zero instances)
  * 3: pay for invocations & not for instances (or nodes or servers)
  * Serverless **level 1**: Features (1+2)
  * Serverless **level 2**: Features (1+2+3)
  
* When mostly talked about serverless, it will be referred to **serverless level 2**
  
* **Level 1**: Google app engine, AWS Fargate
  * scale down to zero instance when there is no load but you pay for number (& type) of instance running.
* **Level 2**: Google functions, AWS lambda, Azure functions etc


