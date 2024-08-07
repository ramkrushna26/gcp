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

### Instance Types
**General-purpose** — best price-performance ratio for a variety of workloads.  
**Storage-optimized** — best for workloads that are low in core usage and high in storage density.  
**Compute-optimized** — highest performance per core on Compute Engine and optimized for compute-intensive workloads.  
**Memory-optimized** — ideal for memory-intensive workloads, offering more memory per core than other machine families, with up to 12 TB of memory.  
**Accelerator-optimized** — ideal for massively parallelized Compute Unified Device Architecture (CUDA) compute workloads, such as machine learning (ML) and high performance computing (HPC). This family is the best option for workloads that require GPUs.  
