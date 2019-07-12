Parameters:

	Enable us to input custom values to our template each time
	when we create or update stack.

	We can have maximun of 60 parameters on a cfn template.

	Each parameter must be given a logical name (logical id) which
	must be alaphanumeric and unique among all logical names within
	the template.

	Each parameter must be assigned a parameter type that is supported
	by AWS CloudFormation.

	Each parameter must be assigned a value at runtime for AWS CloudFormation
	to successfully provision the stack. We can optionally specify a default
	value for AWS CloudFormation to use unless another value is provided.

	Parameters must declared and referenced within the same template.

	We can reference parameters from the resources and outputs sections of 
	the template.


Syntax:

	Parameters:
	  ParameterLogicalId:
	    Type:
	    ParameterProperties: value


Example:

	Parameters:
	  InstanceTypeParameter:
	    Type: String
	    Deafult: t2.micro
	    AllowedValues:
	      - t2.micro
	      - m1.small
	      - m1.large
	     Description: Enter instance type. Default is t2.micro


ParameterProperties:

	* AllowedPattern
	* AllowedValues
	* ConstraintDescription
	* Default
	* Description
	* MaxLength
	* MaxValue
	* MinLength
	* MinValue
	* NoEcho

ParameterTypes:

	Type: (Mandatory)
		* String
		* Number
		* List<Number>
		* CommaDelimitedList
		* AWS Specific
			* AWS::EC2::Instance::Id
			* AWS::EC2::VPC::Id
			* List<AWS::EC2::Subnet::Id>

	Type: (Mandatory)
		* SSM Parameter Type:
			* AWS::SSM::Parameter::Name
			* AWS::SSM::Parameter::Value<String>
			* AWS::SSM::Parameter::Value<List<String>>

AWS Specific Parameters:

	AWS::AccountId
	AWS::NotificationARNs
	AWS::NoValue
	AWS::Partition
	AWS::StackId
	AWS::StackName
	AWS::URLSuffix