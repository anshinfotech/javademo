pipeline {
    
	agent{
		docker {
            image 'maven:3-alpine' 
            args '-v /root/.m2:/root/.m2' 
        }
	}
	options {
        skipStagesAfterUnstable()
    }
    stages {
		
        stage('Build') { 
		
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
		stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
		 stage('Deliver') {
			steps {
                sh 'mvn package'
            }
            post {
                always {
                    //dir('target/my-app-1.0-SNAPSHOT.jar'){

            pwd(); //Log current directory

            withAWS(region:'us-east-1',credentials:'925264290682') {

                 //def identity=awsIdentity();//Log AWS credentials

                // Upload files from working directory 'dist' in your project workspace
                s3Upload(bucket:"aitdemobucket", workingDir:'target/', includePathPattern:'*.jar');
            }

			//};
                }
            }
            
        }
    }
}