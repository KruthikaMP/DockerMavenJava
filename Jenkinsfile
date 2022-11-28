pipeline {
    agent{
        label 'UbuntuAgent'
    }

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "MyMaven"
    }

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                 checkout poll: false, scm: [$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], gitTool: '/usr/bin/git', userRemoteConfigs: [[url: 'https://github.com/djgit17/DockerMavenJava.git']]]

                // Run Maven on a Unix agent.
                sh "mvn clean package"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }

        }
        
        stage('Build docker image') {
            steps {
                script {
                    
                    sh 'sudo docker build -t djddocker17/mymavenapp .'
                }
                
            }
        }
         stage('Run docker container') {
            steps {
                script {
                    
                    sh 'sudo docker run -d --name dockermaven -p 8080:8080 djddocker17/mymavenapp'
                }
                
            }
        }
        
        stage('Push to DockerHub') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'DockerhubPwd', variable: 'Dockerpwd')]) {
                        sh 'sudo docker login -u djddocker17 -p ${Dockerpwd}'
                        sh 'sudo docker push djddocker17/mymavenapp'
                    }
                }
            }
        }
    }
}
