Outputs:

1. Outputs section declares output values that we can

	* Import into other stacks ( to create cross-stack references )
	* When using Nested Stacks, we can see how outputs of a nested stack are used
	  in Root Stack.

2. We Can declare maximun of 60 outputs in a cfn template

Syntax:

	Outputs:
	  Logical ID:
		Description: Information about the value
		Value: Value to return
		Export:
		  Name: Value to export


Example:

	Outputs:
		InstanceId:
			Description: Instance ID
			Value: !Ref MyVMInstance
			Export:
				Name: !Sub "${AWS::StackName}-InstanceId"
		MyAZ:
			Description: AZ
			Value: !GetAtt MyVMInstance
			Export:
				Name: !Sub "${AWS::StackName}-AZ"


Export:
	
	* Export contains resource output for cross-stack reference.
	* For each AWS account, Export name must be unique with in region. As it should
	  be unique we can use the export name as "AWS::StackName"-ExportName.
	* We can't create cross-stack references across regions.
	* We can use the intrinsic function Fn::ImportValue to import values that have been
	  exported within the same region. We will see this Practically.
	  	-> In simple terms, export availability zone in stack1 and use it stack2.
	* For outputs, the value of the Name property of an Export can't use Ref and GetAtt
	  functions that depend on a resource.
	* We can't delete a stack if another stack references one of its outputs.
	* We can modify or remove an output value that is referenced by another stack.
	* We can use Outputs in combination with Conditions. We will see that in our
	  practise sessions for Outputs. 