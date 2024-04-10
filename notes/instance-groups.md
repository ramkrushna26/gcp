# Managed Instance Groups (MIG)

### Creating instance-groups
```
$ gcloud compute instance-groups managed create <name>
OPTIONS:
  --zone <zone>
  --template <name>
  --size <num>
  --health-check=HEALTH_CHECK # conditions to decide whether instance is healthy
  --initial-delay # initial instance start up time delay
```
### Other similar commands
```
$ gcloud compute instance-groups managed delete
$ gcloud compute instance-groups managed describe
$ gcloud compute instance-groups managed list
```

### Setting auto-scaling
```
$ gcloud compute instance-groups managed auto-scaling <name> --max-num-replicas=<num>
OPTIONS:
  --cool-down-period # (default-60s) - Time auto scalar should wait after initiating an autoscaling action
  --scale-based-on-cpu --target-cpu-utilization --scale-based-on-load-balancing --target-load-balancing-utilization
  --min-num-replicas --mode (off/on(default)/only scale out)
```

### Stop auto-scaling
```
$ gcloud compute instance-groups managed stop-autoscaling <name>
```

### Updating existing MIG policies
```
$ gcloud compute instance-groups managed update <name>
OPTIONS:
  --initial-delay
  --health-check
```

### Resize the group
```
$ gcloud compute instance-groups managed resize <mig> --size=<num>
```

### Recreate one or more instances
```
$ gcloud compute instance-groups managed recreate-instances <mig> --instances=<name1>,<name2>
```

### Update specific instances from group
```
$ gcloud compute instance-groups managed update-instances <mig> --instances=<inst1>,<inst2>
OPTIONS:
  --minimal-action=none(default)/refresh/replace/restart
  --most-disruptive-allowed-action=none(default)/refresh/replace/restart
```

### Update instance templates
```
$ gcloud compute instance-groups managed set-instance-template <mig> --template=<template>
```

### Manage new release of v1 to v2 without downtime
* Restart (stop & start)
```
$ gcloud compute instance-groups managed rolling-action restart <mig>
  --max-surge=10%  # Max no. of instances updated at a time
```

* Replace (delete & create)
```
$ gcloud compute instance-groups managed rolling-action replace <mig>
  --max-surge=10%  # Max no. of instances updated at a time
  --max-unavailable=10%  # Max no. of instances that can be down for update
  --replacement-method=recreate/substitute(default)
    # substitute: creates instances with new names
    # recreate: craetes with reuse names
```

* Update instances to new template
> Basic Version: update all instances slowly step by step  
```
$ gcloud compute instance-groups managed rolling-action start-update <mig> --version=template=v1-template
```
> Canary Version: update subset of instances to v2
```
$ gcloud compute instance-groups managed rolling-action start-update <mig> --version=template=v1-template --canary-version=template=v2-template,target-size=10%
```
