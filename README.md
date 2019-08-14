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
	
## Configuration

1. Configure Apache Ant
	* Set the system environment variable ANT_HOME and PATH
		* Computer -> Right-click -> Properties -> Advanced system settings -> Environment Variables
		* In System Variables section, new a variable named ANT_HOME and use the installaion directory as value. e.g. 'D:\apache-ant-1.10.6\'
		* In System Variables section, edit a variable named PATH and add ';ANT_INSTALLATION_DIRECTORY\bin' to the end of the value. e.g. ';D:\apache-ant-1.10.6\bin\'
	* Install Ant Plugin: ant-contrib
		* Unzip 'ant-contrib-1.0b3-bin.zip'
		* Find a jar file named 'ant-contrib-version.jar' and copy it to 'ANT_INSTALLATION_DIRECTORY\lib\' e.g. 'D:\apache-ant-1.10.6\lib\'

2. Configure Jenkins
	* Start Jenkins
		* Change your directory to 'Jenkins' in cmd
		* Use command 'java -jar jenkins.war --httpPort=XXXX', where XXXX is an unoccupied port (e.g. 3927 4728).
		* After the jenkins started, open your web browser and go to site 'http://localhost:XXXX/'
		* Configure the jenkins account and password by following the web instructions.
	* Install Jenkins Plugin: ant & struts
		* Visit 'http://localhost:XXXX/'
		* Manage Jenkins -> Manage Plugins -> Advanced -> Update Plugin 
		* Choose file 'jenkins-plugins\struts.hpi' and wait until the plugin is installed.
		* Choose file 'jenkins-plugins\ant.hpi' and wait until the plugin is installed.
		* Save 
	* Add ANT & JDK path
		* Visit 'http://localhost:XXXX/'
		* Manage Jenkins -> Global Tools Configuration -> JDK Installations -> Add JDK -> NAME='Java 1.8.0', JAVA_HOME='YOUR_PREEVISION_LOCATION\jre' e.g. 'D:\Program Files\Vector\PREEvision_v903_x86_64\jre\bin' 
		* Ant Installations -> Add Ant -> NAME='apache ant 1.10.6', ANT_HOME='YOUR_ANT_LOCATION' e.g 'D:\apache-ant-1.10.6\'
		* Save
	* Create Jenkins Project
		* Visit 'http://localhost:XXXX/'
		* New Item -> Enter Name 'ReqIF Auto Import' ->  freestyle -> OK
		* General -> Build Triggers -> Cross 'Build periodically' and enter '00 0 * * 1-5' (This is for automatic execution. You can set a different time period.)
		* Build Environment -> Cross 'Use Ant' -> Set Ant Version and JDK to 'apache ant 1.10.6' and 'Java 1.8.0'
		* Build -> Add build step -> Invoke Ant -> Set Ant Version to 'apache ant 1.10.6' -> Advanced -> Buildfile -> enter 'executionConfigs\runmetric.xml'
		* Save
		
 