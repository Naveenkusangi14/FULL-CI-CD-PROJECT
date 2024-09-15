pipeline {
    agent { label 'Jenkins-Agent' } // Make sure 'Jenkins-Agent' is correctly configured
    tools {
        maven 'M3' // Ensure Maven is configured correctly in Jenkins
        // jdk 'Java17' // Uncomment if needed, and ensure the JDK is configured
    }
   environment {
	    APP_NAME = "register-app-pipeline"
            RELEASE = "1.0.0"
            DOCKER_USER = "staya1407"
            DOCKER_PASS = 'dockerhub'
            IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
            IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
	    
    }
    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs() // Cleans up the workspace before each run
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'GITHUB', url: 'https://github.com/Naveenkusangi14/FULL-CI-CD-PROJECT.git'
            }
        }

        stage("Build Application") {
            steps {
                sh "mvn clean package" // Compiles and packages the application
            }
        }

        stage("Test Application") {
            steps {
                sh "mvn test" // Runs unit tests
            }
        }

        // stage("SonarQube Analysis") {
        //     steps {
        //         script {
        //     // Use the exact name of your SonarQube installation from Jenkins configuration
        //     withSonarQubeEnv('SonarQube') { 
        //         sh "mvn sonar:sonar -X"
        //     }
        // }
        //     }
        // }

        // Optional: Uncomment the following stage if you have a quality gate configured
        /*
        stage("Quality Gate") {
            steps {
                script {
                    // This waits for the quality gate result from SonarQube
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        */

        // Optional: Uncomment this stage for Docker image build and push to a registry
        
        stage("Build & Push Docker Image") {
            steps {
                script {
                    docker.withRegistry('', DOCKER_PASS) {
                        docker_image = docker.build "${IMAGE_NAME}"
                    }

                    docker.withRegistry('', DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }
        }
        
    }


	  stage("Trivy Scan") {
           steps {
               script {
	            sh ('docker run -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image staya1407/register-app-pipeline:latest --no-progress --scanners vuln  --exit-code 0 --severity HIGH,CRITICAL --format table')
               }
           }
       }

    post {
        always {
            // Archive test results, logs, or artifacts after every build
            junit '**/target/surefire-reports/*.xml' // JUnit reports for test results
            cleanWs() // Clean workspace after build to save space
        }
    }
}

