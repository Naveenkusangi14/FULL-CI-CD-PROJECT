pipeline{
  agent {label 'Jenkins-Agent'}
  tools {
    maven 'M3'
    jdk 'Java17'
  }


  stages{
        stage("Cleanup Workspace"){
                steps {
                cleanWs()
                }
        }

        stage("Checkout from SCM"){
                steps {
                    git branch: 'main', credentialsId: 'GITHUB', url: 'https://github.com/Naveenkusangi14/FULL-CI-CD-PROJECT.git'
                }
        }

        stage("Build Application"){
            steps {
                sh "mvn clean package"
            }

       }

       stage("Test Application"){
           steps {
                 sh "mvn test"
           }
       }

       // stage("SonarQube Analysis"){
       //     steps {
	      //      script {
		     //    withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token') { 
       //                  sh "mvn sonar:sonar"
		     //    }
	      //      }	
       //     }
       // }

       // stage("Quality Gate"){
       //     steps {
       //         script {
       //              waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'
       //          }	
       //      }

       //  }

       //  stage("Build & Push Docker Image") {
       //      steps {
       //          script {
       //              docker.withRegistry('',DOCKER_PASS) {
       //                  docker_image = docker.build "${IMAGE_NAME}"
       //              }

       //              docker.withRegistry('',DOCKER_PASS) {
       //                  docker_image.push("${IMAGE_TAG}")
       //                  docker_image.push('latest')
       //              }
       //          }
       //      }

       // }

    
  }
}
