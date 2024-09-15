pipeline {
    agent { label 'Jenkins-Agent' } // Make sure 'Jenkins-Agent' is correctly configured
    tools {
        maven 'M3' // Ensure Maven is configured correctly in Jenkins
        // jdk 'Java17' // Uncomment if needed, and ensure the JDK is configured
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

        stage("SonarQube Analysis") {
            steps {
                script {
                    // Ensure 'SonarQube' matches the SonarQube instance name configured in Jenkins
                    withSonarQubeEnv('SonarQube') { 
                        sh "mvn sonar:sonar -X" // Run SonarQube analysis with debug logs enabled
                    }
                }
            }
        }

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
        /*
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
        */
    }

    post {
        always {
            // Archive test results, logs, or artifacts after every build
            junit '**/target/surefire-reports/*.xml' // JUnit reports for test results
            cleanWs() // Clean workspace after build to save space
        }
    }
}

