# dev
#6th
pipeline { 
 agent any 
 stages { 
 stage('Checkout') { 
 steps { 
 // Check out code from Git repository 
 git url: 'https://github.com/yourusername/your-maven project.git', branch: 'main' 
 }
 } 
 stage('Build') { 
 steps { 
 // Run Maven build 
 sh 'mvn clean package' 
 } 
 } 
 stage('Test') { 
 steps { 
 // Optionally, separate test execution if needed  sh 'mvn test' 
 } 
 } 
 } 
  
 post { 
 always { 
 // Archive test reports 
 junit '**/target/surefire-reports/*.xml'  } 
 success { 
 echo 'Build and tests succeeded!' 
 } 
 failure { 
 echo 'Build or tests failed.' 
 } 

 #8th
 name: Deploy Artifact to Localhost
  hosts: localhost
  tasks:
	- name: Copy the artifact to the target location
  	become: true
  	become_user: student
  	become_method: su
  	copy:
    	src: "/var/lib/jenkins/workspace/exp8/target/my-app-1.0-SNAPSHOT.jar"
    	dest: "/home/student/t.jar"


pipeline:
pipeline 
{
     agent any

     stages 
     {
       stage('Checkout') 
        {
    	steps 
            {
       git branch: 'main', url: 'https://github.com/cmritsarada/m2.git' // Replace with your repository URL
    	}
        }


        stage('Build') 
        {
    	steps 
            {
        	       // Use 'mvn clean package' for Maven or 'gradle build' for Gradle
        	       sh 'mvn clean package'
    	}
        }

       stage('Test') 
        {
    	steps 
            {
        	     // Run unit tests using Maven or Gradle
        	     sh 'mvn test'
    	}
        }
  	

       stage('Archive Artifacts') 
       {
        	steps {
            	archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
        	}
       }

   
       stage('Deploy')
       {
        	steps{
            	sh """
                	export ANSIBLE_HOST_KEY_CHECKING=False
                	ansible-playbook -i hosts.ini deploy.yml  --extra-vars='ansible_become_pass=exam@cse'
          	                	"""
//enter student password above 
        	}
    	}
	}

 } 
} 
