# Instances

[Creating GCP Instances](https://cloud.google.com/sdk/gcloud/reference/compute/instances/create)

### Commands
```
$ gcloud compute instances create <instance name>
OPTIONS:
  --machine-type <name> (default: n1-standard-1)
    $ gcloud compute machine-types list # to get machine types
  --custom-cpu <10> --custom-memory <4096MB> --custom-vm-type <n1/n2> # custom machine
  --image /--image-family /--source-snapshot /--source-instance-template /--source-machine-image
  --service-account / --no-service-account
  --zone=<name>
  --tags (tag list: allow network firewall rules & routes applied to VM instances)
  --preemptible
  --restart-on-failure (default) --no-restart-on-failure --maintenance-policy (MIGRATE(default)/TERMINATE)
  --boot-disk-size --boot-disk-type --boot-disk-auto-delete(default) /--no-boot-disk-auto-delete
  --deletion-protection /--no-deletion-protection (default)
  --metadata startup-script=<script>/--metadata-from-file startup-script<script> # shutdown-script
  --network --subnet --network-tier (PREMIUM(default), STANDARD)
  --accelerator="type=<name>,count=<num>" --metadata="<ex:install-nvidia-driver=True>" (GPU)
```

