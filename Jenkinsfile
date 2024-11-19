pipeline {
    agent any
    environment {
        AWS_CREDENTIALS = 'ecr:us-east-1:aws-credentials' // AWS credentials for ECR
        BACKEND_IMAGE = '839073959602.dkr.ecr.us-east-1.amazonaws.com/docker-learn-mern/mern/backend'
        FRONTEND_IMAGE = '839073959602.dkr.ecr.us-east-1.amazonaws.com/docker-learn-mern/mern/frontend'
    }
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Clone repository') {
            steps {
                script {
                    checkout scm
                }
            }
        }

        stage('Build Backend') {
            steps {
                script {
                    backendApp = docker.build("${BACKEND_IMAGE}:${env.BUILD_NUMBER}", "./mern/backend")
                }
            }
        }

        stage('Build Frontend') {
            steps {
                script {
                    frontendApp = docker.build("${FRONTEND_IMAGE}:${env.BUILD_NUMBER}", "./mern/frontend")
                }
            }
        }

        stage('Push to ECR') {
            steps {
                script {
                    docker.withRegistry('https://839073959602.dkr.ecr.us-east-1.amazonaws.com', AWS_CREDENTIALS) {
                        backendApp.push("${env.BUILD_NUMBER}")
                        backendApp.push("latest")
                        frontendApp.push("${env.BUILD_NUMBER}")
                        frontendApp.push("latest")
                    }
                }
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests... (Add actual test commands here)'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh 'docker-compose down || true'
                    sh 'docker-compose up -d'
                }
            }
        }
    }
}



// pipeline {
//     agent any
//     environment {
//         AWS_CREDENTIALS = 'ecr:us-east-1:aws-credentials' // Update with your Jenkins credential ID for AWS ECR
//         BACKEND_IMAGE = '839073959602.dkr.ecr.us-east-1.amazonaws.com/docker-learn-mern/mern/backend'
//         FRONTEND_IMAGE = '839073959602.dkr.ecr.us-east-1.amazonaws.com/docker-learn-mern/mern/frontend'
//     }
//     options {
//         skipStagesAfterUnstable()
//     }
//     stages {
//         stage('Clone repository') {
//             steps {
//                 script {
//                     checkout scm
//                 }
//             }
//         }

//         stage('Build Services') {
//             steps {
//                 script {
//                     // Build backend
//                     backendApp = docker.build("${BACKEND_IMAGE}:${env.BUILD_NUMBER}", './mern/backend')
//                     // Build frontend
//                     frontendApp = docker.build("${FRONTEND_IMAGE}:${env.BUILD_NUMBER}", './mern/frontend')
//                 }
//             }
//         }

//         stage('Test') {
//             steps {
//                 echo 'Running tests... (Optional - Add test commands here)'
//             }
//         }

//         stage('Push Backend to ECR') {
//             steps {
//                 script {
//                     docker.withRegistry('https://720766170633.dkr.ecr.us-east-2.amazonaws.com', AWS_CREDENTIALS) {
//                         backendApp.push("${env.BUILD_NUMBER}")
//                         backendApp.push("latest")
//                     }
//                 }
//             }
//         }

//         stage('Push Frontend to ECR') {
//             steps {
//                 script {
//                     docker.withRegistry('https://720766170633.dkr.ecr.us-east-2.amazonaws.com', AWS_CREDENTIALS) {
//                         frontendApp.push("${env.BUILD_NUMBER}")
//                         frontendApp.push("latest")
//                     }
//                 }
//             }
//         }

//         stage('Deploy') {
//             steps {
//                 script {
//                     echo 'Deploying services with Docker Compose'
//                     sh 'docker-compose down' // Stops existing containers
//                     sh 'docker-compose up -d' // Starts the services in detached mode
//                 }
//             }
//         }
//     }
// }
