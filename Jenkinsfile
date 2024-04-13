pipeline {
    agent any

    // triggers {
    //     cron('H/5 * * * *') // Poll SCM every 5 minutes
    // }

    tools {
        nodejs 'NodeJS'
    }

    environment {     
        IMAGE_NAME = "amonte13/eshop:backend-dev"
        IMAGE_NAME_VERSION = "amonte13/eshop:backend-dev-${BUILD_ID}"
    }

    stages {
        stage('Checkout code') {
            steps {
                
                git branch: 'master', url: 'https://github.com/ay-ike/comp367-Group9-backend'
            }
        }

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

    // stage('sonar'){     
    //     steps {
    //         script {
                
    //             def scannerHome = tool 'SonarQube';
    //             withSonarQubeEnv('SonarQube') {
    //                 bat "${scannerHome}/bin/sonar-scanner"
    //             }
    //         }
    //     }
    // }


    stage('Docker Build') {
            steps {
               script {                  
                    bat "docker build -t ${IMAGE_NAME_VERSION} ."                   
                }
            }
        }
        
    // stage('Test and Coverage') {
    //     steps {
    //         script {               
    //             bat 'npm install'                    
    //             bat 'npm test'
    //         }
    //     }
    // }

        stage('Docker Login') {
            steps {
               script {    
                      withCredentials([usernamePassword(credentialsId: 'dockerhubtoken', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                      bat "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                      }
                }
            }
        }
        
        stage('Docker Push') {
            steps {
                script {
                    bat "docker tag ${IMAGE_NAME_VERSION} ${IMAGE_NAME}"
                    bat "docker push ${IMAGE_NAME}"
                    bat "docker push ${IMAGE_NAME_VERSION}"                 
                }       
            }
        }

        stage('Docker Pull') {
            steps {
                script {
                   
                    bat "docker pull ${IMAGE_NAME}"
                               
                }       
            }
        }

        // stage('Deployment DEV') {
        //     steps {
        //         script {
        //             bat "docker compose -f docker-compose.yaml down"
        //             bat "docker compose -f docker-compose.yaml up -d --build"
                               
        //         }       
        //     }
        // }

        // stage('Deployment QAT') {
        //     steps {
        //         script {
        //             bat "docker tag amonte13/eshop:backend-dev amonte13/eshop:backend-qat"
        //             bat "docker push amonte13/eshop:backend-qat"      
        //             bat "docker pull amonte13/eshop:backend-qat"    
        //             bat "docker compose -f docker-compose-qat.yaml down"
        //             bat "docker compose -f docker-compose-qat.yaml up -d --build"      
        //         }       
        //     }
        // }

        // stage('Deployment Staging') {
        //     steps {
        //         script {
        //             bat "docker tag amonte13/eshop:backend-dev amonte13/eshop:backend-staging"
        //             bat "docker push amonte13/eshop:backend-staging"      
        //             bat "docker pull amonte13/eshop:backend-staging"    
        //             bat "docker compose -f docker-compose-staging.yaml down"
        //             bat "docker compose -f docker-compose-staging.yaml up -d --build"      
        //         }       
        //     }
        // }


        // stage('Deployment Production') {
        //     steps {
        //         script {
        //             bat "docker tag amonte13/eshop:backend-dev amonte13/eshop:backend-prod"
        //             bat "docker push amonte13/eshop:backend-prod"      
        //             bat "docker pull amonte13/eshop:backend-prod"    
        //             bat "docker compose -f docker-compose-prod.yaml down"
        //             bat "docker compose -f docker-compose-prod.yaml up -d --build"      
        //         }       
        //     }
        // }

        

    }

    // post {
    //     always {
    //         cobertura coberturaReportFile: '**/coverage/cobertura-coverage.xml'
    //         echo 'The pipeline is finished.'
    //     }
    // }
}
