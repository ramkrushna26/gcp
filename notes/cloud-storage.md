## Cloud Storage
* Popular, **flexible**, **inexpensive** storage device
  * serverless: auto-scaling & infinite scale
* Store large objects using **key-value** approach
  * treats entire object as a UNIT (partial updates not allowed)
  * access control at object level
* Also called **object storage**

</br>

* provide REST API to access & modify objects
* Also provides CLI(**gsutil**) & client libraries (java, python, etc)
* Stores all types of files -- text, binary and archives

</br>

## Buckets and Objects
* Objects are stored in buckets
  * bucket names are globally unique
  * bucket names are used as part of object URL ==> can contains only lower letters, number, hyphen, underscore & period
  * 3-63 char max. can't start with goog prefix & should not contain google
  * unlimited objects in bucket
  * each bucket is associated with project
* each object is identified by unique keys (key is unique in bucket)
* max object size is 5TB (but can store unlimited objects)

</br>

## Storage classes
![image](https://github.com/ramkrushna26/gcp/assets/45620457/87826fc8-e4b4-4ebd-9a7d-7a995e66eabb)

> **In the same bucket you can different objects with different storgare clases**

## Features of Storage classes
* High durability (99.999999999% annual durability)
* low latency
* unlimited storage
* autoscaling (no configuration needed)
* no-minimum object size
* same APIs accross storage classes
* commited SLA is 99.95% for multi-region & 99.9% for single region for standard, nearline, coldline
* no commited SLA for archive storage

![image](https://github.com/ramkrushna26/gcp/assets/45620457/1d2bf9cf-b99a-4dcc-8d16-eee66e92ea91)

</br>

## Storage classes - Uploading & Downloading
![image](https://github.com/ramkrushna26/gcp/assets/45620457/22625a67-0c0c-4874-a51f-f4e8de7e7bff)

</br>

## Object Versioning
Prevents accidental deletes & provides history
* enabled at bucket level (can be turned off/on anytime)
* live version is the latest version
  * if you delete live version, then becomes non-current object version
  * if you delete non-current object version, it is deleted
* older versions are uniquely identified by (object key+a generation number)
* reduce cost by deleting older (non-current) version

## Object Lifecycle Management
files are frequently accessed when they created
  * generally usage reduce with time
  * with object lifecycle management you can reduce cost by automcatically moving files between storage classes

Identify objects using conditions based on:
  * age, createbefore, islive, matchesstorageclass, number of newer versions, etc
  * set multiple conditions: all conditions must be satisfied for action to happen

Two kinds of action:
  * SetStorageClass actions --> change from one storage class to another
  * deletion action --> delete objects

Allowed transision
  * (standard/multi-regional/regional) to (nearline/coldline/archive)
  * nearline to (coldline/archive)
  * coldline to archive

</br>

## Object lifecycle management -- example rules
![image](https://github.com/ramkrushna26/gcp/assets/45620457/03f12fc4-1482-4b8a-ae2d-645adf88aeac)

## Cloud Storage - Encryption
* Cloud storage always encrypt data on server side
* configure **server-side** encryption: encryption perform by cloud storage
  * google-managed encryption key - default (no configuration required)
  * customer-managed encryption key - created using cloud key management server (KMS). Managed by customer in KMS.
    * Cloud storage service account should have access to keys in KMS for encrypting & decrypting using the customer-managed encryption key
* (oprional) client-side encryption: encryption performed by customer before upload
  * GCP does not know about the keys used

### Cloud Storage - Scenarioes
![image](https://github.com/ramkrushna26/gcp/assets/45620457/48ae4994-7a0d-45b7-a988-99201a33fd4d)

### Cloud Storage - Commands
> Set project first
```
$ glcoud config set project <project id>
```
> Creating bucket
```
$ gsutil mb gs://<bucket name>
```
> Display current and non-current objects versions
```
@ gsutil ls -a gs://<bucket name>
```
> Copy objects
```
$ gsutil cp gs://<source bucket>/<object id> gs://<target bucket>/<object id>
  -o 'GSUtil:encryption_key=<encryption key>' - option to specify encryption
  gsutil mv - to move objects to same or another bucket
```
> Change storage class for objects
```
$ gsutil rewrite -s <storage class> gs://<bucket name>/<object name>
```
> Upload & Download
```
$ gsutil cp <local location> gs://<bucket name> # upload
$ gsutil cp gs://<bucket name>/<obj path> <local location>  # download
```
> Versioning enable/disable
```
$ gsutil versioning set on/off gs:/<bucket name>
```
> Uniform level access : gives uniform access to all objects in bucket
```
$ gsutil uniformbucketlevelaccess set on/off gs://<bucket name>
```
> Give access permission to specific objects
```
$ gsutil acl ch -u allusers:R gs://<bkt name>/<obj name> # read permission to <obj name> to all users
$ gsutil acl ch -u jonh.joe:W gs://<bkt name>/<obj name> # write permission to <obj name> to john.joe
    permissions - READ(D), WRITE(W), OWNER(O)
    Scope - User, allAuthenticatedUsers, allUsers(-u), Group(-g), Project(-p)
$ gsutil acl set <json file> gs://<bkt name>
```
> Setup AIM Roles
```
$ gsutil aim ch <member type>:<member name>:<iam role> gs://<bkt name>
```
> Signed URL for temporary access
```
$ gsutil signurl -d 10m <key> gs://<bkt name>/<obj path> 
```
