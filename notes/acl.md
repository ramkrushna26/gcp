## ACL (Access Control Lists)

* ACL : To control access to bucket at object level
* AIM v ACL
  * AIM : permissions apply to all objects in bucket
    * Use when common permission to all objects in bucket
  * ACL : different permissions can be applied to different objects in same bucket
    * Use when customize access to individual objects 
* User gets access if he is allowed by either AIM or ACL 

* Two Type of access control
  * Uniform (Recommended) - uniform bucket level access using AIM
    * use when uniform access required for all users accross all objects in bucket
  * Fine-grained -  Use AIM and ACL to control access
    * bucket level and individual object level permissions
    * use when customize access required at object level
      
