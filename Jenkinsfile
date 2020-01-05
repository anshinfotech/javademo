pipeline {
    agent {
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
                    dir('target/my-app-1.0-SNAPSHOT.jar'){

            pwd(); //Log current directory

            withAWS(region:'us-east-1',credentials:'94e6f76d-5e0d-49ae-aa50-b514623ff7f1') {

                 //def identity=awsIdentity();//Log AWS credentials

                // Upload files from working directory 'dist' in your project workspace
                s3Upload(bucket:"aitdemobucket", workingDir:'dist', includePathPattern:'**/*');
            }

			};
                }
            }
            
        }
    }
}