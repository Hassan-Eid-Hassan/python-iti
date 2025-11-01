@Library('python')_

pipeline {
    agent {
        label 'agent-0'
    }

    tools {
        maven 'mvn-3-5-2'
        jdk 'java-11'
    }

    environment{
        dockerUserName = credentials("docker-user")
        dockerPassWord = credentials("docker-pass")
    }

    stages {
        stage('Build') { 
            steps {
                script{
                    def mavenIT = new edu.iti.maven()
                    mavenIT.mvn("package install")
                }
            }
        }
        stage('Docker Build') { 
            steps {
                script{
                    def dockerIT = new edu.iti.docker()
                    dockerIT.build("hassaneid/it", "${BUILD_NUMBER}")
                }
            }
        }
        stage('Docker push') { 
            steps {
                script{
                    def dockerIT = new edu.iti.docker()
                    dockerIT.login("${dockerUserName}", "${dockerPassWord}")
                    dockerIT.push("hassaneid/it", "${BUILD_NUMBER}")
                }
            }
        }
    }
}