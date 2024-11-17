# What is the AWS CDK?<a name="home"></a>

* AWS Cloud Development Kit \(AWS CDK\)
  * := open-source software development framework / 
    * allows
      * defining cloud infrastructure -- via -- code
      * provisioning cloud infrastructure -- via -- AWS CloudFormation 
  * supports
    * TypeScript,
    * JavaScript,
    * Python,
    * Java,
    * C#/.Net,
    * Go
  * == **[AWS CDK Construct Library](constructs.md)** + **[AWS CDK Command Line Interface (AWS CDK CLI)](cli.md)**
    * [AWS CDK Construct Library](constructs.md)
      * == collection of constructs
        * constructs == pre-written modular and reusable pieces of code 
          * / can be
            * modified
            * integrated
          * | before supported languages
      * uses
        * develop your infrastructure quickly
      * goal
        * reduce the complexity to define & integrate AWS services together | building applications | AWS
    * **[AWS CDK Command Line Interface (AWS CDK CLI)](cli.md)**
      * == CL tool / -- for interacting with -- CDK apps
      * named ALSO CDK Toolkit
      * uses, about your AWS CDK projects, 
        * create,
        * manage,
        * deploy
  * see
    * [stacks](stacks.md)
    * [apps](apps.md)

![\[\]](http://docs.aws.amazon.com/cdk/v2/guide/images/AppStacks.png)

**Topics**
+ [Benefits of the AWS CDK](#home-benefits)
+ [Example of the AWS CDK](#home-example)
+ [AWS CDK features](#home-features)
+ [Next steps](#home-next)
+ [Learn more](#home-learn)

## Benefits of the AWS CDK<a name="home-benefits"></a>

* **Develop and manage your IaC**  <a name="home-benefits-iac"></a>
  * *infrastructure as code*
    * allows, via programmatic, descriptive and declarative way, to
      * create,
      * deploy,
      * maintain infrastructure
    * see [Infrastructure as code](https://docs.aws.amazon.com/whitepapers/latest/introduction-devops-aws/infrastructure-as-code.html)
  * infrastructure + application code + configuration | 1! place
  * employ software engineering best practices
    * code reviews,
    * unit tests,
    * source control
  * / 
    * reliable,
    * scalable,
    * cost-effective

* **Define your cloud infrastructure -- via -- general-purpose programming languages**  <a name="home-benefits-languages"></a> 
  * -- via -- your preferred supported language -> you can use elements:
    * parameters,
    * conditionals,
    * loops,
    * composition,
    * inheritance
  * recommendations
    * application logic's programming language == infrastructure's programming language 

    ![\[\]](http://docs.aws.amazon.com/cdk/v2/guide/images/CodeCompletion.png)

* **Deploy infrastructure -- through -- AWS CloudFormation**  <a name="home-benefits-cfn"></a>
  * With AWS CloudFormation, you can perform infrastructure deployments
    * predictably
    * repeatedly,
    * rollback on error 

* **Get started developing your application quickly with constructs**  <a name="home-benefits-constructs"></a>
  * allows
    * developing faster 
  * types of constructs
    * low-level constructs
      * define individual AWS CloudFormation resources + their properties
    * high-level constructs
      * define larger components of your application /
        * defaults for your AWS resources
          * sensible,
          * secure
  * your own constructs /
    * customizable
    * share them | ACROSS your organization or with the public

## Example of the AWS CDK<a name="home-example"></a>

* _Example:_ create an Amazon ECS / AWS Fargate launch type
  * see [Example: Create an AWS Fargate service using the AWS CDK](ecs_example.md)

------
#### [ TypeScript ]

```
export class MyEcsConstructStack extends Stack {
  constructor(scope: App, id: string, props?: StackProps) {
    super(scope, id, props);

    const vpc = new ec2.Vpc(this, "MyVpc", {
      maxAzs: 3 // Default is all AZs in region
    });

    const cluster = new ecs.Cluster(this, "MyCluster", {
      vpc: vpc
    });

    // Create a load-balanced Fargate service and make it public
    new ecs_patterns.ApplicationLoadBalancedFargateService(this, "MyFargateService", {
      cluster: cluster, // Required
      cpu: 512, // Default is 256
      desiredCount: 6, // Default is 1
      taskImageOptions: { image: ecs.ContainerImage.fromRegistry("amazon/amazon-ecs-sample") },
      memoryLimitMiB: 2048, // Default is 512
      publicLoadBalancer: true // Default is false
    });
  }
}
```

------
#### [ JavaScript ]

```
class MyEcsConstructStack extends Stack {
  constructor(scope, id, props) {
    super(scope, id, props);

    const vpc = new ec2.Vpc(this, "MyVpc", {
      maxAzs: 3 // Default is all AZs in region
    });

    const cluster = new ecs.Cluster(this, "MyCluster", {
      vpc: vpc
    });

    // Create a load-balanced Fargate service and make it public
    new ecs_patterns.ApplicationLoadBalancedFargateService(this, "MyFargateService", {
      cluster: cluster, // Required
      cpu: 512, // Default is 256
      desiredCount: 6, // Default is 1
      taskImageOptions: { image: ecs.ContainerImage.fromRegistry("amazon/amazon-ecs-sample") },
      memoryLimitMiB: 2048, // Default is 512
      publicLoadBalancer: true // Default is false
    });
  }
}

module.exports = { MyEcsConstructStack }
```

------
#### [ Python ]

```
class MyEcsConstructStack(Stack):

    def __init__(self, scope: Construct, id: str, **kwargs) -> None:
        super().__init__(scope, id, **kwargs)

        vpc = ec2.Vpc(self, "MyVpc", max_azs=3)     # default is all AZs in region

        cluster = ecs.Cluster(self, "MyCluster", vpc=vpc)

        ecs_patterns.ApplicationLoadBalancedFargateService(self, "MyFargateService",
            cluster=cluster,            # Required
            cpu=512,                    # Default is 256
            desired_count=6,            # Default is 1
            task_image_options=ecs_patterns.ApplicationLoadBalancedTaskImageOptions(
                image=ecs.ContainerImage.from_registry("amazon/amazon-ecs-sample")),
            memory_limit_mib=2048,      # Default is 512
            public_load_balancer=True)  # Default is False
```

------
#### [ Java ]

```
public class MyEcsConstructStack extends Stack {

    public MyEcsConstructStack(final Construct scope, final String id) {
        this(scope, id, null);
    }

    public MyEcsConstructStack(final Construct scope, final String id,
            StackProps props) {
        super(scope, id, props);

        Vpc vpc = Vpc.Builder.create(this, "MyVpc").maxAzs(3).build();

        Cluster cluster = Cluster.Builder.create(this, "MyCluster")
                .vpc(vpc).build();

        ApplicationLoadBalancedFargateService.Builder.create(this, "MyFargateService")
                .cluster(cluster)
                .cpu(512)
                .desiredCount(6)
                .taskImageOptions(
                       ApplicationLoadBalancedTaskImageOptions.builder()
                               .image(ContainerImage
                                       .fromRegistry("amazon/amazon-ecs-sample"))
                               .build()).memoryLimitMiB(2048)
                .publicLoadBalancer(true).build();
    }
}
```

------
#### [ C\# ]

```
public class MyEcsConstructStack : Stack
{
    public MyEcsConstructStack(Construct scope, string id, IStackProps props=null) : base(scope, id, props)
    {
        var vpc = new Vpc(this, "MyVpc", new VpcProps
        {
            MaxAzs = 3
        });

        var cluster = new Cluster(this, "MyCluster", new ClusterProps
        {
            Vpc = vpc
        });

        new ApplicationLoadBalancedFargateService(this, "MyFargateService", 
            new ApplicationLoadBalancedFargateServiceProps
        {
            Cluster = cluster,
            Cpu = 512,
            DesiredCount = 6,
            TaskImageOptions = new ApplicationLoadBalancedTaskImageOptions
            {
                Image = ContainerImage.FromRegistry("amazon/amazon-ecs-sample")
            },
            MemoryLimitMiB = 2048,
            PublicLoadBalancer = true,
        });
    }
}
```

------
#### [ Go ]

```
func NewMyEcsConstructStack(scope constructs.Construct, id string, props *MyEcsConstructStackProps) awscdk.Stack {

	var sprops awscdk.StackProps

	if props != nil {
		sprops = props.StackProps
	}

	stack := awscdk.NewStack(scope, &id, &sprops)

	vpc := awsec2.NewVpc(stack, jsii.String("MyVpc"), &awsec2.VpcProps{
		MaxAzs: jsii.Number(3), // Default is all AZs in region
	})

	cluster := awsecs.NewCluster(stack, jsii.String("MyCluster"), &awsecs.ClusterProps{
		Vpc: vpc,
	})

	awsecspatterns.NewApplicationLoadBalancedFargateService(stack, jsii.String("MyFargateService"),
		&awsecspatterns.ApplicationLoadBalancedFargateServiceProps{
			Cluster:        cluster,           // required
			Cpu:            jsii.Number(512),  // default is 256
			DesiredCount:   jsii.Number(5),    // default is 1
			MemoryLimitMiB: jsii.Number(2048), // Default is 512
			TaskImageOptions: &awsecspatterns.ApplicationLoadBalancedTaskImageOptions{
				Image: awsecs.ContainerImage_FromRegistry(jsii.String("amazon/amazon-ecs-sample"), nil),
			},
			PublicLoadBalancer: jsii.Bool(true), // Default is false
		})

	return stack

}
```

------

This class produces an AWS CloudFormation [template of more than 500 lines](https://github.com/awsdocs/aws-cdk-guide/blob/main/doc_source/my_ecs_construct-stack.yaml)\. Deploying the AWS CDK app produces more than 50 resources of the following types\.
+  [AWS::EC2::EIP](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-eip.html) 
+  [AWS::EC2::InternetGateway](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-internetgateway.html) 
+  [AWS::EC2::NatGateway](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-natgateway.html) 
+  [AWS::EC2::Route](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-route.html) 
+  [AWS::EC2::RouteTable](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-routetable.html) 
+  [AWS::EC2::SecurityGroup](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-security-group.html) 
+  [AWS::EC2::Subnet](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-subnet.html) 
+  [AWS::EC2::SubnetRouteTableAssociation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-subnet-route-table-assoc.html) 
+  [AWS::EC2::VPCGatewayAttachment](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-vpc-gateway-attachment.html) 
+  [AWS::EC2::VPC](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-vpc.html) 
+  [AWS::ECS::Cluster](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ecs-cluster.html) 
+  [AWS::ECS::Service](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ecs-service.html) 
+  [AWS::ECS::TaskDefinition](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ecs-taskdefinition.html) 
+  [AWS::ElasticLoadBalancingV2::Listener](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-elasticloadbalancingv2-listener.html) 
+  [AWS::ElasticLoadBalancingV2::LoadBalancer](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-elasticloadbalancingv2-loadbalancer.html) 
+  [AWS::ElasticLoadBalancingV2::TargetGroup](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-elasticloadbalancingv2-targetgroup.html) 
+  [AWS::IAM::Policy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-iam-policy.html) 
+  [AWS::IAM::Role](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-iam-role.html) 
+  [AWS::Logs::LogGroup](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-logs-loggroup.html) 

## AWS CDK features<a name="home-features"></a>

### The AWS CDK GitHub repository<a name="home-features-repo"></a>

* see [aws-cdk](https://github.com/aws/aws-cdk)

### The AWS CDK API reference<a name="home-features-api"></a>

* AWS CDK Construct Library
  * provides
    * APIs 
      * see [AWS CDK API Reference](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-construct-library.html)

### The Construct Programming Model<a name="home-features-cpm"></a>

* Construct Programming Model \(CPM\)
  * concepts behind the AWS CDK -- are extended into -- additional domains
  * used by
    + [CDK for Terraform](https://www.terraform.io/docs/cdktf/index.html) \(CDKtf\)
    + [CDK for Kubernetes](https://cdk8s.io/) \(CDK8s\)
    + [Projen](https://github.com/projen/projen), for building project configurations

### The Construct Hub<a name="home-features-hub"></a>

* [Construct Hub](https://constructs.dev/)
  * == ONLINE registry abut open-source AWS CDK libraries, -- to --
    * find,
    * publish,
    * share 

## Next steps<a name="home-next"></a>

* see [Getting started with the AWS CDK](getting_started.md)

## Learn more<a name="home-learn"></a>

* TODO:
To continue learning about the AWS CDK, see the following:
+ **[Learn AWS CDK core concepts](core_concepts.md)** – Important concepts and terms for the AWS CDK\.
+ **[AWS CDK Workshop](https://cdkworkshop.com/)** – Hands\-on workshop to learn and use the AWS CDK\.
+ **[AWS CDK Patterns](https://cdkpatterns.com/)** – Open\-source collection of AWS serverless architecture patterns, built for the AWS CDK by AWS experts\.
+ **[AWS CDK code examples](https://github.com/aws-samples/aws-cdk-examples) ** – GitHub repository of example AWS CDK projects\.
+ **[cdk\.dev](https://cdk.dev/)** – Community\-driven hub for the AWS CDK, including a community Slack workspace\.
+ **[Awesome CDK](https://github.com/kalaiser/awesome-cdk)** – GitHub repository containing a curated list of AWS CDK open\-source projects, guides, blogs, and other resources\.
+ **[AWS Solutions Constructs](https://aws.amazon.com/solutions/constructs/) ** – Vetted, configuration infrastructure as code \(IaC\) patterns that can easily be assembled into production\-ready applications\.
+ **[AWS Developer Tools Blog](https://aws.amazon.com/blogs/developer/category/developer-tools/aws-cloud-development-kit/)** – Blog posts filtered for the AWS CDK\.
+ **[AWS CDK on Stack Overflow](https://stackoverflow.com/questions/tagged/aws-cdk)** – Questions tagged with ** aws\-cdk** on Stack Overflow\.
+ **[AWS CDK tutorial for AWS Cloud9](https://docs.aws.amazon.com/cloud9/latest/user-guide/sample-cdk.html)** – Tutorial on using the AWS CDK with the AWS Cloud9 development environment\.

To learn more about related topics to the AWS CDK, see the following:
+ **[AWS CloudFormation concepts](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-whatis-concepts.html)** – Since the AWS CDK is built to work with AWS CloudFormation, we recommend that you learn and understand key AWS CloudFormation concepts\.
+ **[AWS Glossary](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html) ** – Definitions of key terms used across AWS\.

To learn more about tools related to the AWS CDK that can be used to simplify serverless application development and deployment, see the following:
+ **[AWS Serverless Application Model](https://aws.amazon.com/serverless/sam/)** – An open\-source developer tool that simplifies and improves the experience of building and running serverless applications on AWS\.
+ **[AWS Chalice](https://github.com/aws/chalice)** – A framework for writing serverless apps in Python\.