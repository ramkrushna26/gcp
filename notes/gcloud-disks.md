### Gcloud disks commands
```
$ gcloud compute disks list/create/delete/resize/snapshot
```

### Creating disks
```
$ gcloud compute disks create <disk name>
    --zone=<zone> # ex: us-east2-b
    --size=<size> # ex: 1GB
    --type=<type> # default (pd-standard)
        $ glcoud compute disk-types list # to check the disk types

   # What data should be on disk, you can specify through below options
   --image
   --image-family
   --source-disk
   --source-snapshot

   # How data should be encrypted can be specify through below option
   --kms-key --kms-project
```

### Resizing Disks
```
$ gcloud compute disks resize <disk name> --size=20GB
   # Resing only increases size and not decrease it
```

### Taking snapshot of disks
```
$ gcloud compute disks snapshot <disk name> --zone=<zone> --snapshot-names=<snapshot name>

# you can play with snapshot with below command
$ gcloud compute snaphots list/delete/describe
```
