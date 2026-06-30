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
                script {
                    def pom = readMavenPom file: 'webapp/pom.xml'

                    nexusArtifactUploader(
                        artifacts: [[
                            artifactId: pom.artifactId,
                            classifier: '',
                            file: file: "webapp/target/${pom.artifactId}.war",
                            type: 'war'
                        ]],
                        credentialsId: 'nexus_cre',
                        groupId: pom.groupId,
                        nexusUrl: '192.168.124.129:8081',
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        repository: pom.version.endsWith('SNAPSHOT') ? 'upendra-snapshot' : 'upendra-release',
                        version: pom.version
                    )
                }
            }
        }

        stage('Deploy Application') {
            steps {
                script {
                    def pom = readMavenPom file: 'webapp/pom.xml'

                    sshagent(credentials: ['tomcat-server-agent']) {
                        bat """
                        scp -o StrictHostKeyChecking=no webapp\\target\\${pom.artifactId}.war root@192.168.124.129:/opt/tomcat/webapps/
                        """
                    }
                }
            }
        }

    }
}