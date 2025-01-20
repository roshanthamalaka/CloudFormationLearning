This is repo is created for Practising the CloudFormation. Templates in this repo will used to created below infrastructure.

![Project Screenshot](assets/screenshot.png "Architecture")

It has Two VPC each has a public and private subnet. Public Subnet has a IGW and Nat Gateway. EC2 instances will be deployed to Each Subnet in order to test connectivity. SG will be deployed to allow traffic 

In the CloudFormation Template Resource section describes what resource should be deployed. 
Refer AWS documentation https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html for template and it is property (Resource has multiple Properties for example VPC as CIDR,Subnet ..etc). IF we search here VPC resource will not be available. It is under EC2.

Parameters used pass user input to the template 
https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/parameters-section-structure.html  

Mappings are fixed variables in the Cloudformation template 
https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/mappings-section-structure.html 

Outputs used to declares output values that we can import into other cloudfromation stacks
https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/outputs-section-structure.html 