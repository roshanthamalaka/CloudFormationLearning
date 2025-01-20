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

Instrinsic Func !Ref Can be used to reference 
    1. Parameters 
    2. Previously created resource value

Instrinsic Function Documentation: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference.html 
Ref Instrinsic Function Documentation: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-ref.

Examine vpc.yml for identify use of intrinsic function Ref, Creation of basic template.

**Deploy CloudFormation Stack** 

Cloudformation is regional service. Let say we deploy via the portal. In this case you have to go to respective region and deploy from there. If you want to deploy same stack to multiple regions make sure to use parameters because it may have region values such as avaliability zones.

**Validate the Template Before Deployment** 
Use below command to validate 

aws cloudformation validate-template --template-body file://vpc.yml --region ap-southeast-2 --profile sa-development

**Update an exsisting Stack**

In order to try this out. In the vpc.yml which I used to create VPC stack I added another VPC and Add tags to exisisting resources because tags does not require replace. Once template is finalized go to the cloudfromation then select the relevant stack.

In stack click on update. Then Choose **Replace exisisting template** option. This is the only option that is available 

**Infrastructure Composer** 

In the Stack if we go to the template section We can see the Template which is used to provision the stack. In here if We click on "View in Infrastructure Composer" we can see graphical representation of resoruce 

![Project Screenshot](assets/template1.png "Template Section")

![Project Screenshot](assets/template2.png "Infrastructure Composer")