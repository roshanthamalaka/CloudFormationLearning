This is repo is created for Practising the CloudFormation. Templates in this repo will used to created below infrastructure.

![Project Screenshot](assets/screenshot.png "Architecture")


It has Two VPC each has a public and private subnet. Public Subnet has a IGW and Nat Gateway. EC2 instances will be deployed to Each Subnet in order to test connectivity. SG will be deployed to allow traffic 

**Writing Basic Cloudformation Template**

In the CloudFormation Template Resource section describes what resource should be deployed.It is the one of mandatory parameters


Refer AWS documentation https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html for all the supported resources types and it is property types (Resource has multiple Properties for example VPC as CIDR,Subnet ..etc). 

IF we search here VPC resource will not be available. It is under EC2.

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

![Project Screenshot](assets/Template1.png "Template Section")

![Project Screenshot](assets/Template2.png "Infrastructure Composer")

**Managing Dependecny Between Resources** 

Situation where  Resource B depends on Resource A. When Cloudfromation try to deploy resources it is unable to identify which resource should deploy first. This where encounter an error saying **Circular dependency between resources:**. 
https://aws.amazon.com/blogs/infrastructure-and-automation/handling-circular-dependency-errors-in-aws-cloudformation/ 

In order to avoid this we have use DependsOn. This is similar with Terraform depends_on parameter. Refer Depends on Documentation below for clarity.
 https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-attribute-dependson.html

 See vpc.yml on how we use Depends to avoid this error.

**Output and Imports in Cloudformation** 

Let say you have created VPC in one stack. Now you want to create route tables in another stack. In this scenario how to Pass VPCID in VPCStack to Route Table Stack. This is the place where output and import come into play.

Output Documentation: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/outputs-section-structure.html 
Import Documentation: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-importvalue.html 

If you refer vpc.yml you can see that There is Seperate section called outputs after resource declaration.

Each Output has 
    1. Output Logical ID / Logical Name
    2. Description
    3. Value : Value of  the property returned. In here we want return VPC ID. So Passed VPC logical name as the value with Ref function. When Pass Ref function with LogicalName of the resource it Returns the Resource ID. So we pass the  Logical Name  of the VPC Resource so it should return ID.
    4. Export: Here specify name of the output. This name should be globally unique with in the region were cloudformation stack deployed. Using this name we can use this output in other stack

Once the template run in the cloudformation Console we can see the output values in the Outputs section in the stack.Refer below Screenshot.
![Project Screenshot](assets/OutputsInCLoudFormation.png "Outputs in Cloudformation")

<u> Import usage </u> 

Once the outputs are defined it can be imported to other stack. If you look at **routetables.yml** which used to create RouteTable Stack using **ImportValue** Function and use the name of the outputs defined in the VPC stack (in the vpc.yml file) we have referenced the VPC ID for each VPC