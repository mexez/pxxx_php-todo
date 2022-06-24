pipeline {
    agent any

    stages {

        stage('Initial Cleanup') {
            steps {
            dir("${WORKSPACE}") {
              deleteDir()
            }
          }
        }

        stage ('Checkout Repo'){
            steps {
            git branch: 'main', url: 'https://github.com/Ellawangari/php-todo.git'
      }
        }

        stage ('Build Docker Image') {
            steps {
                script {

                       sh "docker build -t nerd2021/php-todo:${env.BRANCH_NAME}-${env.BUILD_NUMBER} ."
                }
            }
        }


         stage ('Push Docker Image') {
             when { expression { response.status == 200 } }
            steps{
                script {
            sh "docker login -u ${env.username} -p ${env.password}"

            sh "docker push nerd2021/php-todo:${env.BRANCH_NAME}-${env.BUILD_NUMBER}"
                    
            
            }
          }
         }

         stage ('Docker LogOut') {
            steps {
                script {

                       sh "docker logout "
                }
            }
        }

  
    }
}
