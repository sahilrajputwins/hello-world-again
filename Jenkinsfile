pipeline{
    agent any
    environment{
        container_name = hello_container
    }
    stages{
        stage("Build Image"){
            steps{
                bat 'docker build -t sahilrajputwins/hla .:%BUILD_ID%'
            }
        }
        stage("Run Container"){
            steps{
                script{
                    def ifExists=bat(script: 'docker ps -a --format "{{.Names}}" | findstr %container_name%', returnStatus: true)
                    if(ifExists==0){
                        bat 'docker stop %container_name%'
                        bat 'docker stop %container_name%'
                    } else {
                        echo 'no container found, running new one...'
                    }       
                    bat 'docker run -p 80:80 -d sahilrajputwins/hla:%BUILD_ID%'
                }
            }
        }
        stage("Login"){
            steps{
                withCredentials([usernamePassword(credentialsID: 'dockerhub-cred', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]){
                    bat '''echo %DOCKER_PASSWORD% | docker login -u %DOCKER_USERNAME% --password-stdin'''
                }
            }
        }
        stage("Push Image"){
            steps{
                bat 'docker push sahilrajputwins/hla:%BUILD_ID%'
            }
        }
        stage("Deployed"){
            steps{
                echo 'successfully deployed. Build id: %BUILD_ID%'
            }
        }
    }
}