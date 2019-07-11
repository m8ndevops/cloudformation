Resources:

	Resources are key components od a stack.
	Resources section is a required section that need to be defined in cloud formation template.

Syntax:

	Resources:
	  Logical ID:
	    Type: Resource Type
	    Properties:
	      Set of Properties

Example:

    Resources:
      MyEC2Instance:
        Type: AWS::EC2::Instance
        Properties:
          ImageId: mmmnnnxxxzzz

Intrinsic Function: Ref

	The intrinsic function Ref returns the value of the specific parameter
	or resource.
	
	Resource Case: 
		When we specify a resource logical name, it returns a value
	that we can typically use to refer to that resource.
	
	Parameter Case: 
	    When we specify a parameter logical name, it returns the value of that
	parameter.