# Google Cloud VPC (Virtual Private network)

![image](https://github.com/ramkrushna26/gcp-notes/assets/45620457/32edce8f-49da-48e8-abe2-bfa330668c56)  
In a corporate network provides **secure internal network** protecting your resources, data & communication from external users  
You can use Gcloud VPC to create your own virtual private environment in google cloud  
  - network traffic within VPC is isolated from other gcloud VPC

You control all the traffic coming in & going out of VPC  
**Best practices**: create all your GCP resource within VPC  
  - secure resource from unauthorized access
  - enable secure communication between cloud resources  

VPC is **global** resource & contains subnets in one or more region  
  - not tied to region or zone. VPC resource can be in any region or zone

## VPC - Subnets
Different types of resource are created on cloud  
  - each type of resource has its own access needs
  - load balancers are accessible from internet (public resource)
  - databases & VM instances should not be accessible from internet
    - only application withing your VPC are able to access them (private resources)

**create subnets**: 
  - to seperate public resources from private resources
  - to distribute resources accross multiple regions for high availability

Each subnet is created within a region  

## Things to remember while creating VPC & Subnets
![image](https://github.com/ramkrushna26/gcp-notes/assets/45620457/51fc2965-435c-4524-a70b-98b9b2e4ec4f)   

by default, each project has a default VPC  
You can create your own vpc  
  - Option 1: auto mode VPC network
    - subnets are automatically created in each region
    - default VPC created automatically in the project uses auto mode
  - Option 2: custom mode VPC network
    - no subnets are automatically created
    - you can complete control over subnets & their IP range
    - recommended for production

Options when you create a subnet -  
  - Enable private google access - allow VM's to connect to google API's using private IPs
  - Enable FlowLogs - to troubleshoot any VPC related network issue

## CIDR (Classless Inter Domain Routing) Blocks

Resources in a network use continuous IP addresses to make routing easy (10.208.0.0 to 10.208.0.15)   
CIDR block -- expresses range of address that resource has in a network  
  - ex: 10.208.0.0/28 -- represents 10.208.0.0 to 10.208.0.15
    - 28 indicates - first 28 bits out of 32 is fixed
    - (32-28=4 and 2 to the power of 4 is 16 so 0 to 15 in this case)

[CIDR](https://cidr.xyz/)

</br>

![image](https://github.com/ramkrushna26/gcp-notes/assets/45620457/606f12a9-c909-47e5-99ef-b752ff742537)

## Shared VPC
use case: When organization has multiple project and you want resource in different project to talk each other with internal IPs securely & effeciently  
Shared VPC created as organization or shared folder level (access needed-Shared VPC admin)  
Allows VPC network to be shared between projects in same organization   
Shared VPC contains one host project & multiple service projects  
  -  Host projects : contains shared VPC network
  -  service projects : attached to host projects  

Helps you achieve seperation of concern  
  - Network admistrator responsible for host projects & resource users uses service project 

## VPC Peering

use case: To connect VPC networks accross different organizations  
Networks in same projects, different projects & accross projects in different organizations can be peered   
All communications happens using internal IP addresses  
  - highly efficient because all communication happens inside google network
  - highly secure because not accessible from internet
  - no data transfer charges for data transfer between services  

Network administration is NOT changed  
  - Admin of one VPC do not get the role automatically in a peered network   

