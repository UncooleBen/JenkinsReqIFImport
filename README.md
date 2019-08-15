# Jenkins ReqIF Import

* Jenkins ReqIF Import is a collection of configurations for importing ReqIF files (*.reqifz) to PREEVision model.
* The depends on Apache Ant 1.10.6 and Jenkins 2.176.2.
* The required PREEVision version 9.0.*. (We built it on v9.0.3 with a database model.)

## Installation

1. Download Package
	* You may download the installation archives by cloning this repository.
	
2. Install Apache Ant
	* Unzip 'apache-ant-1.10.6-bin.zip' to your ideal location
	* You will get a directory named 'apache-ant-1.10.6'. This is the ant directory.

3. Install Jenkins
	* Unzip 'jenkins.zip.001-004' and you shall get a jenkins.msi.
	* Run the installer.
	* Follow the installer's instruction to finish the installation.
	* You will get a directory named 'Jenkins'. This is the Jenkins directory.
	
4. Install JDK 1.8
	* If you've installed JDK (NOT JRE!!) 1.8, skip this step.
	* Unzip 'jdk1.8.0_201.zip.001-007' to your ideal location.
	
## Configuration

1. Configure Apache Ant
	* Set the system environment variable ANT_HOME and PATH
		* Computer -> Right-click -> Properties -> Advanced system settings -> Environment Variables
		* In System Variables section, new a variable named ANT_HOME and use the installaion directory as value. e.g. 'D:\apache-ant-1.10.6\'
		* In System Variables section, edit a variable named PATH and add ';ANT_INSTALLATION_DIRECTORY\bin' to the end of the value. e.g. ';D:\apache-ant-1.10.6\bin\'
	* Install Ant Plugin: ant-contrib
		* Unzip 'ant-contrib-1.0b3-bin.zip'
		* Find a jar file named 'ant-contrib-version.jar' and copy it to 'ANT_INSTALLATION_DIRECTORY\lib\' e.g. 'D:\apache-ant-1.10.6\lib\'

2. Configure JDK 1.8
	* Set the system environment variable JAVA_HOME and PATH
		* Computer -> Right-click -> Properties -> Advanced system settings -> Environment Variables
		* In System Variables section, new a variable named JAVA_HOME and use the installaion directory as value. e.g. 'D:\jdk1.8.0_201'
		* In System Variables section, edit a variable named PATH and add ';JDK_INSTALLATION_DIRECTORY\bin' to the end of the value. e.g. ';D:\jdk1.8.0_201\'

3. Configure Jenkins
	* Start Jenkins
		* The Jenkins will automatically start after the installaion is completed. If not, please follow the instructions below.
		* Change your directory to 'Jenkins' in cmd
		* Use command 'java -jar jenkins.war --httpPort=XXXX', where XXXX is an unoccupied port (e.g. 3927 4728).
		* After the jenkins started, open your web browser and go to site 'http://localhost:XXXX/'
		* Configure the jenkins account and password by following the web instructions.
	* Start Configuration
		* Find your initial password at 'YOUR_JENKINS_DIRECTORY\secrets\initialAdminPassword' e.g. 'D:\Jenkins\secrets\initialAdminPassword'.
		* Open it with notepad++ copy the contents and paste it on the jenkins localhost website. Click 'continue'.
		* Click 'Skip Plugin Installation'.
		* Create your adminstrator account.
		* In 'Instance Configuration' change the port in 'http://localhost:XXXX/'. (The port 8080 is probably occupied by other services. ) Click 'Save and Finish'.
	* Install Jenkins Plugin: ant & struts
		* Visit 'http://localhost:XXXX/'
		* Manage Jenkins -> Manage Plugins -> Advanced -> Update Plugin 
		* Choose file 'jenkins-plugins\struts.hpi' and wait until the plugin is installed.
		* Manage Jenkins -> Manage Plugins -> Advanced -> Update Plugin 
		* Choose file 'jenkins-plugins\ant.hpi' and wait until the plugin is installed.
		* Save 
	* Add ANT & JDK path
		* Visit 'http://localhost:XXXX/'
		* Manage Jenkins -> Global Tools Configuration -> JDK Installations -> Add JDK -> NAME='Java 1.8.0', JAVA_HOME='YOUR_JDK_LOCATION' e.g. 'D:\jdk1.8.0_201\' 
		* Ant Installations -> Uncheck 'Install Automatically' -> Add Ant -> NAME='apache ant 1.10.6', ANT_HOME='YOUR_ANT_LOCATION' e.g 'D:\apache-ant-1.10.6\'
		* Save
	* Create Jenkins Project
		* Visit 'http://localhost:XXXX/'
		* New Item -> Enter Name 'ReqIF Auto Import' ->  freestyle -> OK
		* General -> Build Triggers -> check 'Build periodically' and enter '00 0 * * 1-5' (This is for automatic execution. You can set a different time period.)
		* Build Environment -> check 'Use Ant' -> Set Ant Version and JDK to 'apache ant 1.10.6' and 'Java 1.8.0'
		* Build -> Add build step -> Invoke Ant -> Set Ant Version to 'apache ant 1.10.6' -> Advanced -> Buildfile -> enter 'CONFIGURATION_LOCATION\runmetric.xml' e.g. 'D:\reqif-jenkins-project\executionConfigs\runmetric.xml'
		* Save

4. Configure PREEVision
	* Open PREEVISION-9.X
	* Create Product Line (Structure as below)
		* Product Goals
			* Customer Features
			* Requirements (<- Copy This Artifact's UUID)
		* Test
			* Test Data
		* Library
			* Data Type Package
		* Adminstration
			* Definition Package
	* Copy UUID
		* Adminstration -> Metrics -> ImportExportMetrics -> ReqIF -> ReqIF Import Classic (Metric Package)
		* Open ReqIF Import (Metric Diagram) -> Edit the value of parameter block before File Chooser to 'JENKINS-PROJECT-LOCATION\working\working.reqifz' e.g. 'D:\reqif-jenkins-project\working\working.reqifz'
		* Choose the Commit Executor ReqIF Import Classic and copy its UUID.
		* Choose the Requirements you've just created and copy its UUID.
		
		
4. Configure runmetric.xml
	* Open runmetric.xml with notepad++
	* Edit the following tags' value property
	* `<property name="baseDir" value="JENKINS-PROJECT-LOCATION" />` e.g. 'D:\reqif-jenkins-project'
	* `<property name="preevision-home" value="PREEVISION-9.X-LOCATION" />` e.g. 'D:\Program Files\Vector\PREEvision_v903_x86_64'
	* `<property name="preevision-wokspace" value="PREEVISION-WORKSPACE" />` e.g. 'D:\Program Files\Vector\PREEvision_v903_x86_64_workspace\ws'
	* `<property name="vipropertyfile" value="PAREMETER.PROPERTIES-LOCATION" />` e.g. 'D:\reqif-jenkins-project\executionConfigs\parameters.properties'
	* `<property name="library-file" value="LIBRARY.XML-LOCATION" />` e.g. 'D:\reqif-jenkins-project\executionConfigs\library.xml'
	
5. Configure parameters.properties
	* Configure Metrics UUID
		* Open parameter.properties with notepad++
		* Paste the Commit Executor UUID as the parameter.properties metricexecutor value. e.g. 'metricexecutor = Na30ec1915b9fc6032628128xNa31610f16248b3c10221d4710Na30ec1915b9fc6032628127'
		* Paste the Requirements UUID as the parameter.properties selectedElements value. e.g. 'selectedElements = Na56096a16c4b3bd5b31bd57XNa56096a16c4b3bd5b31bd5610Na56096a16c4b3bd5b31bd56'
	* Configure Database information
		* protocol=HTTP
		* host=YOUR_DATABASE_IP_ADDRESS
		* port=YOUR_DATABASE_PORT
		* username=YOUR_USERNAME
		* password=YOUR_PASSWORD
		* modelname=YOUR_MODELNAME
		* Other information can be left blank.

6. Configure Jenkins Project Directory
	* Under your JENKINS-PROJECT-LOCATION, create a directory named 'reqifzs'.
	* Put your reqifz files into the 'reqifzs' directory.
	* Under your JENKINS-PROJECT-LOCATION, create a directory named 'tmp'.
	* Under your JENKINS-PROJECT-LOCATION, create a directory named 'working'.
	
7. Run Jenkins Automatic ReqIF Import
	* Make sure you close the PREEVISION-9 before start the Jenkins project.
	* Visit 'http://localhost:XXXX/'
	* Click 'ReqIF Auto Import'
	* Click 'Build Now'
	* Wait until the build process is completed.