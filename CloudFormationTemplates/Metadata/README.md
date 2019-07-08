Metadata:

It Provides details about the cfn template.

Syntax:

	Metadata:
	  Instance:
	    Description: "details"
	  Databases:
	    Description: "details"

Example:

	AWSTemplateFormatVersion: 2010-09-09

	Metadata:
	  Instances:
	    Description: MyInstance

Three types of Metadata keys:

	* AWS::CloudFormation::Designer
	
		Auto Generated 	during resources drag and drop to canvas.

	* AWS::CloudFormation::Interface

		Used for Parameter grouping

	* AWS::CloudFormation::Init

		Used for application installation and configurationson our aws
		compute.
		This is core and important feature of CloudFormation.
	

