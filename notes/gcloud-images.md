### Gcloud imanges commands
```
$ gcloud compute images create/list/delete/deprecate/describe/export/import/update
```
### Creating images
```
$ gcloud compute images create <image name>
  --source-disk=<disk name> # to create from disk
  --source-snapshot=<snapshot name> # to create from snapshot
  --source-image=<image name> --source-image-project=<project name> # to create from another image
  --source-image-family=<image family name> --source-image-project=<project name> # to create from latest non-deprecated image from family
```
### Deprecating old images
```
$ gcloud compute images deprecate <image name> --state=DEPRECATE
```
### Exporting an image
```
$ gcloud compute images export --image=<image name> --destination-uri=<cloud storage> --export-format=vmdk --project=<project name>
  # cloud storage like gs://my-bucket/<image name>.vmdk
```
### Deleting images
```
$ gcloud compute images delete <image 1> <image 2>
```







