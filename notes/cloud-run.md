## Cloud Run
* **container to production** in seconds
* built on top of open standard - **knative**
* **fully managed** serverless platform for containerized applications
  * zero instrastruction management
  * pay-per-use (CPU, memory, requests, networking)
* fully integrated **end-to-end developer experience**
  * no limitations in language, binaries, dependencies
  * easily portable because of container based architecture
  * cloud code, cloud build, cloud monitoring, cloud logging integrations



* **Anthos** - run kubernetes cluster everywhere (cloud, multi-cloud, on-premise)
* **Cloud run for anthos** - deploy your workloads to anthos clusters running on premise or on google cloud

## Cloud run from command line
* deploy a new container
```
$ gcloud run deploy <service> --image <image> --revision-suffix v1
# first deployment create a service and first revision
# next deployments for same service creates new revisions
```
* list available revisions
```
$ gcloud run revisions list
```
* adjust traffic assisgnments
```
$ gcloud run services update-traffic <service> --to-revision=v2-10,v1=90
```
