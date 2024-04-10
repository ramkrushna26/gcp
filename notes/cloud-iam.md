## IAM - Cloud Identity and Access Management
![image](https://github.com/ramkrushna26/gcp/assets/45620457/5d882320-ef37-49f9-a4ea-1d7d780a7f36)
![image](https://github.com/ramkrushna26/gcp/assets/45620457/78eadd95-ae1d-4bde-823d-8db79e2a9161)

## Roles
![image](https://github.com/ramkrushna26/gcp/assets/45620457/91375a22-f7cd-4459-8f37-309ad2cf67c6)
![image](https://github.com/ramkrushna26/gcp/assets/45620457/cd3f5b4d-ad5d-4008-8a45-a7d148146692)

## Policy
![image](https://github.com/ramkrushna26/gcp/assets/45620457/26e94333-47ce-4045-990b-a85ac4b6a6cb)
![image](https://github.com/ramkrushna26/gcp/assets/45620457/e8c597e9-0415-4eea-a321-b9f8d40c4e59)
![image](https://github.com/ramkrushna26/gcp/assets/45620457/ebe82e52-102e-423e-ab64-44dfd88d1820)

> To describe the current project
```
$ gcloud compute project-info describe
```
> To login & logout
```
$ glcoud auth login
$ gcloud auth revoke
```
> To list active accounts
```
$ gcloud auth list
```
> To get all bindings of specific project
```
$ gcloud projects get-iam-policy <project name>
```
> To add IAM policy binding
```
$ gcloud projects add-iam-policy-binding <project name> --member=user:<user mail> --role=roles/storage.objectAdmin
```
> To remove IAM policy binding
```
$ gcloud projects remove-iam-policy-binding <project name> --member=user:<user mail> --role=roles/storage.objectAdmin
```
> To overwrite existing IAM policies for a project
```
$ gcloud projects set-iam-policy 
```
> To delete the project
```
$ gcloud projects delete 
```
> To describe the IAM Role
```
gcloud iam roles describe roles/storage.objectAdmin
```
> To create and copy a role
```
$ glcoud iam roles create (--project, --permissions, --stage)
$ gcloud iam roles copy --source=roles/storage.objectAdmin --destination=my.custom.role --dest-project=<project name>
```



