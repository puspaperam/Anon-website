pipeline{
    agent any
    tools{
        maven 'Maven3.9.9'
    }
        stages{
            stage('Code Checkout'){
                steps{
                    git branch: 'main', url: 'https://github.com/puspaperam/Anon-website.git'
                }
            }
            stage('Unit Testig'){
                steps{
                    sh 'mvn test'
                }
            }
            stage('Compile  Code'){
                steps{
                    sh 'mvn clean package'
                }
            }
            stage('Code Analysis'){
                steps{
                    withSonarQubeEnv(credentialsId: 'sonnar-anon-website', installationName: 'sonnar-anon-website') {
                    sh 'mvn sonar:sonar' 
                }
              }
            }
            stage('Upload  Artifacts'){
                steps{
                    nexusArtifactUploader artifacts: [[artifactId: 'maven-project', 
                    classifier: '', 
                    file: 'webapp/target/webapp-1.0-SNAPSHOT.war', 
                    type: 'war']], 
                    credentialsId: 'linux-agent-creds', 
                    groupId: 'com.example.maven-project', 
                    nexusUrl: '192.168.124.129:8081', 
                    nexusVersion: 'nexus3', protocol: 'http', 
                    repository: 'upendra-snapshot', 
                    version: '1.0-SNAPSHOT'
                }
              }
            }
            
        }
    
