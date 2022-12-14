pipeline {
    agent any
    
    environment {
        IMAGE_TAG = "${env.BRANCH_NAME}-${env.BUILD_NUMBER}"
    }

  stages {

     stage("Initial cleanup") {
          steps {
            dir("${WORKSPACE}") {
              deleteDir()
            }
          }
        }
  
    stage('Checkout Git') {
      steps {
        checkout([$class: 'GitSCM', branches: [[name: 'main']], extensions: [], userRemoteConfigs: [[credentialsId: 'a0cfb614-7112-4f19-a952-d0fe685f7899', url: 'https://github.com/scholarship-task/docker']]])
      }
    }
      
    stage('build and login image') {
            steps {
                script {
                    echo "building the docker image..."
                    withCredentials([usernameColonPassword(credentialsId: '0749aa12-736c-4030-84cc-9251547326b4', variable: 'docker-password')]) {
                        sh "cd tooling/"               
                    }
                }
            }
    }

    stage('Docker Push') {
            steps {
                echo "Login to DockerHub and push the docker image..."
                withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'password', usernameVariable: 'username')]) {
                    sh "docker login -u ${env.username} -p ${env.password}"
                    echo "Login succeeded......."
                    
                    // sh "docker push dapetoo/tooling:${IMAGE_TAG}"
                }
            }
        }
      
    stage('Cleaning Docker Images') {
            steps {
                echo "Clean the docker image on the Jenkins Build Server..."
                withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'password', usernameVariable: 'username')]) {
                    echo "Clean all docker images......."
                    // sh "docker rmi dapetoo/php-todo:${IMAGE_TAG}"
                }
            }
        }
  }
}