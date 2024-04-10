## Cloud Functions
* Run code in response to events
  * write your business logic in Node.js, Python, etc
  * Don't worry about servers or scaling or availability (only need to handle code)
* Pay only for what you use
  * number of invocations
  * compute time of the invocations
  * memory & cpu provisioned
* Time bound - default 1 min and max 60 min
* 2 product versions
  * cloud functions (1st gen): first version
  * cloud functions **(2nd gen) (Recommended)**: new version built on top of cloud run & Eventarc

* Use case: when you want to trigger an event whenever file is uploaded to cloud storage or error log is written to cloud logging or message arrives in cloud pub/sub or a http/https invocation is received  

  
* Key enhancements in 2ng gen cloud functions
  * **longer request time**: up to 60min for http triggered functions
  * **larger instance sizes**: up to 16Gb RAM with 4 CPU (v1: 8gb RAM with 2 CPU)
  * **concurrency**: upto 1000 concurrent requests per function instance (v1: 1 concurrent request per function)
  * **multiple function revisions** & traffic splitting supported (v1: not supported)
  * support for 90+ evet types - enabled by Eventarc (v1: only 7)


```
$ gcloud functions deploy <function name>
OPTIONS:
  --docket-registry # registry to store function's docket image
    - default -container-registry
    - alternative -artifact-registry

  --docker-repository # repository to store function's docker images
    - Ex: /${project}/${location}/${repository}

  --gen2 # to use gen2, otherwise by default gen1 will be used
  --runtime
  --timeout
  --max-instances
  --min-instance $ to avoid cold start

  --service-account
    - 1gen: app engine default service account - PROJECT_ID@appspot@.gserviceaccount.com
    - 2gen: default compute service account - PROJECT_NO-compute@developer.gserviceaccount.com

  --source
    - zip file from cloud storage or
    - source repo or
    - local file system
```

* Example
```
$ gcloud functions deploy <function> --gen2 --region=eurpoe-west1 --runtime=nodejs18 --source=gs://bucket/source.zip --trigger-topic=pubsub-topic
```
