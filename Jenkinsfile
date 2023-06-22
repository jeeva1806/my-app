pipeline {
    agent any
    tools {
        maven 'mymaven'
    }
    environment {
        DOCKER_PASSWORD = credentials('Docker')
    }
    stages {
        stage ('SVM Checkout'){
            steps {
                git 'https://github.com/jeeva1806/my-app.git'
            }
        }
        stage ('Maven Build'){
            steps {
                sh '/opt/apache-maven-3.8.5/bin/mvn package'
                sh 'mv target/myweb*.war target/newapp.war'
            }
        }
        stage ('Dcoker Image Build'){
            steps {
                sh 'docker build -t jeeva1806/myappimage:0.0.1 .'
            }
        }
        stage ('Docker Image Push'){
            steps {
                sh "docker login -u jeeva1806 -p ${DOCKER_PASSWORD}"
                sh 'docker push jeeva1806/myappimage:0.0.1'
            }
        }
        stage ('Remove Previous Container'){
            steps {
                script {
                    try {
                        sh 'docker rm -f myapp'
                    }
                    catch (error){
                    }
                }
            }
        }
        stage ('Docker Deploy'){
            steps {
                sh 'docker run -itd --name myapp -p "8070:8080" jeeva1806/myappimage:0.0.1'
            }
        }
    }
}
