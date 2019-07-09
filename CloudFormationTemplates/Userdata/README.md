UserData

Use UserData in CloudFormation template for EC2

Use a intrinsic Function Fn::Base64 with UserData in CFN templates.
This function returns the Base64 represention of input String.
It Passes encoded data to EC2 instance

YAML Pipe (|): Any indented text that follows should be interpreted
as a multi-line scalar value which means value should be interpreted
literally in such a way that preserves newlines.

UserData Cons:

	By default, userdata scripts and cloud-init directives run only
	during the boot cycle when we first launch an instance

	Can update our configuration to ensure that our user data scripts
	and cloud-init directives run every time we restart our instance.
	(Reboot of server required)

Example:

	UserData:  
	  Fn::Base64: |
	    #!/bin/bash
	    sudo yum update
	    sudo yum -y erase java-1.7.0-openjdk.x86_64
	    sudo yum -y install java-1.8.0-openjdk.x86_64
	    sudo yum -y install java-1.7.0-openjdk-devel
	    sudo yum -y install tomcat8
	    service tomcat8 start
	    mkdir /usr/share/tomcat8/webapps/ROOT
	    touch /usr/share/tomcat8/webapps/ROOT/index.html
	    echo "Cloud Formation Tomcat8" > /usr/share/tomcat8/webapps/ROOT/index.html