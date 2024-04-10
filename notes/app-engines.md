# App Engine

## App Engines Environments
* **STANDARD**: Application runs in language specific sandboxes
  * complete isolation from OS/Disks/Other apps
  * **V1**: Java, Python, PHP, Go (old version)
    * only for python, php runtimes
      * restricted network access
      * only white-listed extensions & libraries are allowed
    * No restrictions for java, go runtimes
  * **V2**: Java, python, node.js, php, go (new version)
    * Full network access and no restrictions on language extensions  

  
* **FLEXIBLE**: Application instances run within docker containers
  * Make use of compute engine VMs
  * support any runtimes (python, java, go, php, node.js etc)
  * provides access to background processes and local disks  

  
## App Engines Environments - Comparision
![image](https://github.com/ramkrushna26/gcp/assets/45620457/7aca6c42-e5f2-4cac-b6d0-629727d9056b)

  
## App Engine Points
1. Can NOT change application region
2. Good for simple microservices (multiple services)
3. Use flexible for containarize apps
4. Use standard v2 for supported languages
5. **Standard**: scale down to zero when there is NO load
6. **Flexible**: atlead ONE container will be running
7. Use resident (continously running) and dynamic (added based on load) instances
8. Can have only **ONE app engine per project**

## Deploying App
#### deploy & shift all traffic to new version
```
$ glcoud app deploy
```
#### To get information about app engine
```
$ glcoud app services list
$ glcoud app versions list
$ glcoud app instances list
$ glcoud app browse
$ gcloud app browse --version=v2
$ gcloud app regions list
$ gcloud app logs tail
```
#### Deploy but don't send traffic new version
```
$ gcloud app deploy --version=v3 --no-promote
```
#### Test the new version
```
$ glcoud app browse --version v3
$ gcloud app browse --service=service-name --version=v2
```
#### Send half traffic to old and half traffic to new version
```
$ glcoud app services set--traffic --splits=v3=.5,v2=.5
# Default way to split traffic is by IP

# To split traffic randomly
$ glcoud app services set-traffic --splits=v3=.5,v2=.5 --split-by=random

# To split by cookie mentione --split-by=cookie
```
#### Send all traffic to v3
```
$ glcoud app services set-traffic --splits=v3=1
```
#### Gradual migration 
```
with --migrate option
# gradual migration not supported by app engine flexible environment
```
#### Creating app engine
```
$ gcloud app create --region=us-central
# gcloud app <create/deploy/describe/open-console/browse>
# can specify url with --image-url (only for flexible env)
# --stop-previous-version if old version need to be stopped after releasing new version
```
#### App Engine Instance
```
$ gcloud app instances delete/describe/list/scp/ssh
# --version & --service for specific version and service
```
#### App Engine Services
```
$ gcloud app instances delete/describe/list/browse/set-traffic
# --version & --service for specific version and service
```
#### App Engine Versions
```
$ gcloud app instances delete/describe/list/browse/migrate/start/stop
# --version & --service for specific version and service
```

## Reference for app.yaml 
```
runtime: python39 
api_version: 1.1  #Recommanded: $ gcloud app deploy --version=v1.1
instance_class: F1  #Type of instance class
service: service-name
#env: flex  # specify env here standard or flexible

#to pre warm up your app engine instances
inbound_services:
- warmup

env_variables:
  ENV_VARIABLE: "value"

handlers:
- url: /
  script: home.app

automatic_scaling:
  target_cpu_utilization: 0.7
  min_instances: 5
  max_instances: 50
  max_concurrent_requests: 50

#basic_scaling:
  #max_instances: 10
  #idle_timeout: 10m
#manual_scaling:
  #instances: 5
```
## App Engine - Cron
#### Configure cron.yaml
```
cron:
- description: "Daily Summary"
  url: /task/summary
  schedule: every 10 min
```
#### Run cron.yaml
```
$ gcloud app deploy cron.yaml
```

## App Engine Routing 
#### Routing with URL
```
1. https://project_id.region_id.r.appspot.com (default service)
2. https://service-dot-project_id.region_id.r.appspot.com (specific service)
3. https://version-dot-service-dot-project_id.region_id.r.appspot.com (specific version of service)
4. replace -dot- with . for custom domain
```
#### Routing with dispatch file
```
$ gcloud app deploy dispatch.yaml
# configure routes in dispatch.yaml
```
#### dispatch.yaml
```
dispatch:
  - url: "*/mobile/*"
    service: mobile-frontend
  - url: "*/work/*"
    service: mobile-backend
```
#### Configure queue to pick up tasks : queue.yaml
```
queue:
- name: fooqueue
  rate: 1/s
  retry_parameters:
    task_retry_limit: 7
    task_age_limit: 2d
```
#### Routing with cloud load balancing
--- Configure routes on load balancing instance


