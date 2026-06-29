pipeline {
    agent any

    tools {
        maven 'Maven3.9.9'
    }

    stages {

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
                    groupId: 'com.example.maven-project',
                    version: '1.0-SNAPSHOT',
                    repository: 'upendra-snapshot',
                    credentialsId: 'linux-agent-creds',
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
                sshagent(['tomcat-server-agent']) {
                    bat '''
                    scp -o StrictHostKeyChecking=no webapp\\target\\webapp.war root@192.168.124.129:/opt/tomcat/webapps/
                    '''
                }
            }
        }
    }
}