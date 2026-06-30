pipeline {
    agent any

    tools {
        maven 'Maven3.9.9'
    }

    stages {

        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Code Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/puspaperam/Anon-website.git'
            }
        }

        stage('Unit Testing') {
            steps {
                bat 'mvn test'
            }
        }

        stage('Compile Code') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('Code Analysis') {
            steps {
                withSonarQubeEnv('sonnar-anon-website') {
                    bat 'mvn sonar:sonar'
                }
            }
        }

        stage('Upload Artifacts') {
            steps {
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: '192.168.124.129:8081',
                    repository: 'upendra-snapshot',
                    credentialsId: 'nexus_cre',

                    groupId: 'com.example.maven-project',
                    version: '1.0-SNAPSHOT',

                    artifacts: [[
                        artifactId: 'webapp',
                        classifier: '',
                        file: 'webapp/target/webapp.war',
                        type: 'war'
                    ]]
                )
            }
        }

        stage('Deploy Application') {
            steps {
                bat '''
                scp -o StrictHostKeyChecking=no webapp\\target\\webapp.war root@192.168.124.129:/opt/tomcat/webapps/
                '''
            }
        }
    }
}