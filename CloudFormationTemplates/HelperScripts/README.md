Helper Scripts:

	* AWS CloudFormation provides the following Python helper Scripts
	  that we can use to install software and start services on
	  Amazon EC2 that we create as part of Stack.

	  * cfn-init
	  * cfn-signal
	  * cfn-get-metadata
	  * cfn-hub	


Step-01: 

Metadata:AWS::CloudFormation::Init

	* Type AWS::CloudFormation::Init will be used to include metadata
	  section on an EC2 instance for cfn-init helper script.
	* Configuration is separated into sections.
	* Metadata is organized into config keys, which we can even group
	  in configsets.
	* We can even specify configsets as input to cfn-init script so 
	  that it can process the entire configset with all its configkeys.
	  We will see it in detail in configsets section.
	* The cfn-init helper script processes the configuration sections
	  in the order specified in syntax section.

Example:

	Resources:
	  MyVMInstance:
	    Type: AWS::EC2::Instance
	    Metadata:
	      AWS::CloudFormation::Init:
	        config:
              packages:
                :
              groups:
                :
              users:
                :
              sources:
                :
              files:
                :
              commands:
                :
              services:
                :
        Properties:
          ImageId:
          InstanceType:
          KeyName:

Metadata:Structure
	
	* If we want to process it in different order, we need to separate
	  them into different config keys and then use the order of execution	
	  for config keys in a configset.
	* In this step we will just add the metadata section with structure.
	* We will incrementally build the metadata sections in upcoming steps.

Example:

	Resources:
	  MyVMInstances:
	    Type: AWS::EC2::Instance
	    Metadata:
	      AWS::CloudFormation::Init:
	        configSets:
	          InstallAndconfigure:
	            - Install
	            - Configure
	        Install:
	          packages:
	            :
	          groups:
	            :
	          users:
	            :
	          sources:
	            :
	          files:
	            :
	          services:
	            :
	         Configure:
	           commands:
	             :
	    Properties:
	      ImageId:
	      InstanceType:
	      KeyName:


Step-02:

Metadata: packages

	* We can use packages key to download and install pre-packaged applications.
	* On windows systems packages key supports only the MSI Installer.
	* Supported Packages:
		- apt
		- msi
		- python
		- rpm 
		- rubygems
		- yum
	* Packages with Versions:
		
		packages:
		  rpm:
		    epel: "https://zzxxcc.rpm"
		  yum:
		    httpd: []
		    php: []
		  rubygems:
		  chef:
		    - "1.0.2"

Example:

	Metadata:
	  Comment: Deploy a Tomcat App
	  AWS::CLoudFormation::Init:
	    config:
	      packages:
	        yum:
	          jdk.x86_64: []
	          jdk-devel: []
	          tomcat8: []


Step-03:

Metadata:groups

	* We can use groups to create Linux/Unix groups and assign to group id's.
	* Groups key is not supported for windows systems.
	* We can create multiple groups as required.
	* We can create without group id or create with a desired group id.

Syntax:

	group:
	  groupone: {}
	  grouptwo:
	    gid: "501"


Step-04:

Metadata:users

	* We can use the users key to create Linux/Unix users in EC2 Instance.
	* Users Key is not supported for windows Systems.
	* The following are the supported keys:
		- uid
		- groups
		- homeDir

Syntax:

	users:
	  userone:
	    groups:
	      - groupone
	      - grouptwo
	    uid: "501"
	    homeDir: "/tmp"


Step-05:

Metadata:Sources

	* We can use the sources key to download an archieve file and unpack
	  it in a target directory on EC2 Instance.
	* This key is fully supported on both Linux and Windows Systems.
	* Supported Archieve Formats:
		- tar
		- tar + gzip
		- tar + bz2
		- zip

Syntax/Example:

	sources:
	 /tmp: "https://s3/demo1.zip"

Steps:

	* Create S3 bucket.
	* Disable block public access to bucket.
	* Create cfn folder.
	* Upload the zip files demo1.zip demo2.zip which contains demo.war
	  (version 1 and 2)
	  - Unzip AWS-CloudFormation.zip to local directory
	  - Navigate to 11-cfn-init/WAR-Files folder
	  - Upload the demo1.zip, demo2.zip to S3 bucket cfn folder
	  - Path: /AWS-CloudFormation/11-cfn-init/WAR-files
	  - Make the demo1.zip, demo2.zip as public file
	  - Copy the S3 http url for both files and perform public access test
	  - Update demo1.zip url in sources section of template


Step-06:

Metadata:files

	* We can use the files key to create files on EC2 Instace.
	* The content can be either inline in the template or the content
	  can be pulled from a URL.
	* The files are written to disk in alphabetical order.
	* Supported Keys:
		- content
		- source
		- Encoding
		- group
		- owner
		- mode
		- authentication
		- context

Syntax/Sample:

	files:
	  "/etc/cfn/cfn-hub.conf":
	    content: !Sub |
	      [main]
	      stack=${AWS::StackId}
	      region=${AWS::Region}
	    mode: "000400"
	    owner: "root"
	    group: "root"
	  "/etc/cfn/hooks.d/cfn-auto-reloader.conf":
	    content: !Sub |
	      [cfn-auto-reloader-hook]
	      triggers=post.update
	      path=Resources.MyVMInstance.Metadata.AWS::CloudFormation::Init
	      action=/opt/aws/bin/cfn-init -v --stack ${AWS::StackName} --resource MyVMInstance --region ${AWS::Region}
	    mode: "000400"
	    owner: "root"
	    group: "root"

Step-07:

Metadata:commands

	* We can use commands key to execute commands on EC2 Instance.
	* The commands are processed in alphabetical order by name.
	* Supported Keys:
		- command
		- env
		- cwd
		- test
		- ignoreErrors
		- waitAfterCompletion

Syntax/Example:

	commands:
	  test1:
	    command: "chmod 755 demo.war"
	    cwd: "/tmp"
	  test2:
	    command: "sudo yum erase -y java-1.7.0-openjdk.x86_64"
	    cwd: "~"
	  test3:
	    command: "rm -rf demo*"
	    cwd: "/var/lib/tomcat8/webapps"
	  test4:
	    command: "cp demo.war /var/lib/tomcat8/webapps"
	    cwd: "/tmp"

Step-08:

Metadata:services

	* We can user services key to define which services should be
	  enabled or disabled when the instance is launched.
	* On Linux systems this key is supported by usinf sysvinit.
	* On Windows systems, it is supported using Windows Service Namager.
	* Services key also allows us to specify dependencies on sources,
	  packages and files so that if a restart is needed due to files
	  being installed, cfn-init will take care of the service restart.
	* Supported Keys:
		- ensureRunning
		- enabled
		- files
		- sources
		- packages
		- commands

Syntax/Example:

	services:
	  sysvinit:
	    tomcat8:
	      enabled: "true"
	      ensureRunning: "true"

Step-09:

UserData:aws-cfn-bootstrap

	* Helper Scripts are updated periodically.
	* We need to ensure that the below listed command is included
	  in UserData of our template before we call the helper scripts
	  to ensure that our launched instances get the latest helper scripts.

Syntax/Example:

	UserData:
	  "Fn::Base64":
	    !Sub |
	      #!/bin/bash -xe
	      # Get periodical update frequently from cfn package
	      yum update -y aws-cfn-bootstrap

Step-10:

UserData:cfn-init

	* The cfn-init helper script reads template metadata from
	  the AWS::CloudFormation::Init key and acts accordingly to:
	  - Fetch and parse metadata from AWS CFN
	  - Install Packages
	  - Write files to disk
	  - Enable/disable and start/stop services
	* If we use cfn-init to update an existing file, it creates a backup
	  copy of the original file in the same directory with a .bak extension.
	* cfn-init does not require credentials. However, if no credentials are
	  specified, AWS cloudFormation checks for stack membership and limts
	  the scope of the call to the stack that the instance belongs to.

Command:

	# Start cfn-init and install from (packages,sources,files,commands and services)
	/opt/aws/bin/cfn-init -s ${AWS::StackId} -r MyVMInstance --region ${AWS::Region} || error_exit 'Failed to run cfn-init'

Step-11:

UserData:cfn-signal

	* The cfn-signal helper script AWS CFN to indicate whether Amazon EC2
	  instances have been successfully created or updated.
	* If we install and configure software applications on instances, we
	  can signal AWS CFN when those software applications are ready.
	* We can use the cfn-signal script in conjunction with a CreationPolicy.

UserData:cfn-hub

	* Important Note: Create here because changes should be included here
	  at the time of creation.

Step-12:

Outputs:

	* Add outputs in the template.
	* We will add AppURL output for easily accessing the application
	  after stack creation.

Step-13:

Creation Policy:

	* Associate the CreationPolicy attribute with a resource to prevent its
	  status from reaching create complete until AWS configuration receives
	  a specified number of success signals or the timeout period is exceeded.
	* To signal a resource we can use cfn-signal helper script.
	* The creation policy is invoked only when AWS CloudFormation creates
	  the associated resource.
	* Currently, the only AWS CloudFormation resources that support creation
	  policies are
	  - AWS::AutoScaling::AutoScalingGroup
	  - AWS::EC2::Instance
	  - AWS::CloudFormation::WaitCondition
	* Use the CreationPolicy attribute when you want to wait on resource
	  configuration actions before stack creation proceeds.

Syntax:

	CreationPolicy:
	  AutoScalingCreationPolicy:
	    MinSuccessfulInstancePercentage: int
	  ResourceSignal:
	    Count: int
	    Timeout: str

Example:

	MyVMInstance:
	  Type: AWS::EC2::Instance
	  CreationPolicy:
	    ResourceSignal:
	      Timeout: PT5M

Step-14:

UserData:cfn-hub

	* cfn-hub helper is daemon that detects changes in resource metadata
	  and runs the user-specified actions when a change is detected.
	* This allows us to make configuration updates on our running EC2 
	  Instance through the Update Stack feature.
	* cfn-hub.conf
	  - stores name of stack ad region
	  - Creating using Metadata key named files in template.
	* cfn-hub.conf
		- stack
		- credential-role
		- role
		- region
		- umask
		- interval
		- Verbose
	* hooks.d Directory
		- Auto check conf files