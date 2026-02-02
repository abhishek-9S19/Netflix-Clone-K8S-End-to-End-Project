pipeline {
    agent any

    tools {
        nodejs 'nodejs'
    }

    stages {

        stage('Workspace Cleaning') {
            steps {
                deleteDir()
            }
        }

        stage('Checkout from Git') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/abhishek-9S19/Netflix-Clone-K8S-End-to-End-Project.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                dir('Application-Code') {
                    sh 'npm install'
                }
            }
        }

        /* ===========================
           SonarQube (DISABLED)
        ============================

        stage("SonarQube Analysis") {
            steps {
                ...
            }
        }

        stage("Quality Gate") {
            steps {
                ...
            }
        }
        */

        /* ===========================
           OWASP Dependency Check (DISABLED)
        ============================

        stage('OWASP Dependency Check') {
            steps {
                ...
            }
        }
        */

        /* ===========================
           Trivy FS Scan (DISABLED)
        ============================

        stage('TRIVY FS SCAN') {
            steps {
                ...
            }
        }
        */

        stage('Docker Image Build') {
            steps {
                dir('Application-Code') {
                    withDockerRegistry(credentialsId: 'dockerhub-creds', toolName: 'docker') {
                        withCredentials([string(credentialsId: 'tmdb-api', variable: 'TMDB_API_KEY')]) {
                            sh """
                            docker build \
                              --build-arg TMDB_V3_API_KEY=${TMDB_API_KEY} \
                              -t abhishekdocker0118/netflix-clone:latest .
                            """
                        }
                    }
                }
            }
        }

        stage('Docker Image Push') {
            steps {
                withDockerRegistry(credentialsId: 'dockerhub-creds', toolName: 'docker') {
                    sh 'docker push abhishekdocker0118/netflix-clone:latest'
                }
            }
        }

        stage('TRIVY Image Scan') {
            steps {
                sh '''
                docker run --rm \
                  -v /var/run/docker.sock:/var/run/docker.sock \
                  aquasec/trivy:latest image \
                  --exit-code 0 \
                  --no-progress \
                  abhishekdocker0118/netflix-clone:latest
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                dir('Kubernetes') {
                    sh 'kubectl apply -f deployment.yml'
                    sh 'kubectl apply -f service.yml'
                    sh 'kubectl get svc'
                }
            }
        }
    }

    /* ===========================
       Slack Notifications (DISABLED)
    ============================

    post {
        always {
            slackSend(...)
        }
    }
    */
}
