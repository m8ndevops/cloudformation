Nested Stacks:

	* The AWS::CloudFormation::Stack type nests a stack as a resource
	  in a top level template
	* Can add output values from a nested stack to the root stack
	* Use Fn::GetAtt function the nested stacks logical name and the
	  name of the output value in nested stack

Syntax:

	VpcId: !GetAtt NestedStackName.Outputs.NestedStackOutputName

Example:

	NetworKinterfaces:
	  - AssociatePublicIpAddress: "true"
	    DeviceIndex: "0"
	    SubnetId: !GetAtt VPCStack.<Outputs class="Subnet01Id"></Outputs>