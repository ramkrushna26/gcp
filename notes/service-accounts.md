## Service Accounts
![image](https://github.com/ramkrushna26/gcp/assets/45620457/451b3532-5ede-47b8-9558-c58e3c3db3d5)

### Use case 1: VM --> cloud storage
> Create service account role with right permission  
> Assign service account role to VM instance  
> Do not delete service accounts used by running instances  

### Use case 2: On Prem --> cloud storage

> Can NOT assign service account to on-prem app  
> Create service account with right permission  
> Create service account User Managed Key  
>> $ gcloud aim service-accounts keys create   
>> Download service account file n keep it secure as you can not create keys again  

> Make service account file accessible to on-prem app  
>> export GOOGLE_APPLICATION_CREDENTIALS="<PATH TO FILE>"  

> Use google cloud client libraries - Application Default Credentials (ADC)  
>> ADC uses service account key file if GOOGLE_APPLICATION_CREDENTIALS exists  

### Use case 3: On Prem --> Google cloud APIs

> Making call from outside GCP to google cloud APIs with short lived permissions  
>> Less risk compared to sharing service account keys  

> Credential types  
>> OAuth 2.0 access tokens  
>> OpenID connect ID tokens  
>> Self-signed JSON web tokens(JWT)  

> Examples:  
>> when a member need elevated permissions, he can assume the service account role  
>>> create OAuth 2.0 access token for service account  
>> OpenID Connect ID tokens are recommended for service to service authentication  

## Service Account Use Case Scenarioes
![image](https://github.com/ramkrushna26/gcp/assets/45620457/2f83683a-efba-4ff2-9d5c-14d2cc97249c)

