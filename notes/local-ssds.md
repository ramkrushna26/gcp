## Local SSDs
* **physically attached** to host of VM instances
  * provides very high IOPS & very low latency
  * **ephemeral storage** -- temporary data (data persist only until instance is running)
    * enable live migration for data to service maintenance events
  * data automatically encrypted (however, you can not configure encryption keys)
  * life cycle tied to VM instances
  * only some machine types support local SSDs
  * support SCSI & NVMe intefaces

</br>

* Points to remember
  * choose NVMe-enabled & multi queue SCSI images ==> for better performance 
  * larger local SSDs (more storage), more vCPUs (attch to VM) ==> for better performance
 

</br>

### Disadvantages
* Ephemeral storage
  * Lower durability, lower availability, lower flexibility compared to PDs
* You can NOT detach and attach to another VM instance
