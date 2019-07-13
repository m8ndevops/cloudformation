Configsets:

	* We can create more than one config key and have cfn-init process
	  them in a specific order.
	  - Single ConfigSet
	  - Multiple ConfigSet

Example:

Single ConfigSets:

	Metadata:
	  Comment: Deploy App
	  AWS::CLoudFormation::Init:
	    configSets:
	      App1AndApp2:
	        - app1
	        - app2

Multiple ConfigSets:

	Metadata:
	  Comment: Deploy App
	  AWS::CLoudFormation::Init:
	    configSets:
	      SingelCS:
	        - app1
	      DoubleCS:
	        - ConfigSet: "SingleCS"
	        - app2
	      default:
	        - ConfigSet: "DoubleCS"

