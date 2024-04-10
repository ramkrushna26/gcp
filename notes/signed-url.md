## Signed-URL
* Use when you want to allow user limited time access to your object
  * Do not need google account
* To create signed-url
  * create key for service account with required permissions
  * then create signed url using below command
```
$ gsutil singurl -d 10m <your_key> gs://<bucket_name>/<object path>/  
```

## Cloud storage - explose static website 
* Create a bucket with same name as website name (dns name of website)
  * Verify domain is owned by you
* Copy the files to bucket (add all html files like index and error)
* Add member AllUsers and grant Storage Object Viewer option and Allow Public Access 
