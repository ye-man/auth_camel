#!groovy

pipeline{
	agent any
    tools {
        maven 'Maven 3.5.4'
        jdk 'jdk8'
    }

    options {
        timestamps()
        timeout(time: 30, unit: 'MINUTES')
    }

    environment{
	registry = "docker_hub_account/repository_name"
	registryCredential = 'dockerhub'
    }
	
   stages {
	stage('Build') {
            steps{
                echo "Building..."
                withMaven(maven: 'Maven 3.5.4') {
                    bat "mvn clean install"
                }
            }

        }

        stage('Deploy') {
            steps{
                echo "Deploying to FTP..."
                ftpPublisher paramPublish: null, masterNodeName: '', alwaysPublishFromMaster: true, continueOnError: false, failOnError: true, publishers: [
                        [configName: 'Test', transfers: [
                                [asciiMode: false, cleanRemote: false, excludes: '', flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: "", remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'target/**']
                        ], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: true]
                ]
            }
        }
	   
        stage('build_docker_image') {	    
				/*bat 'dir'
				bat 'docker build -t auth_camel .'
		    		bat 'docker image ls'*/
		steps {
			echo "=========== Build Docker Image! ==========="

			script {
				def app 
				app = docker.build("auth_camel")
			}
                echo "=========== FINISHED - Build Docker Image! ==========="
		}
        }
    
	}
}
