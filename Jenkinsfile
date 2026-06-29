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
            stage('CodeAnalysis'){
                steps{
                    withSonarQubeEnv(credentialsId: 'sonnar-anon-website', installationName: 'sonnar-anon-website') {
                    sh 'mvn sonar:sonar' 
                }
              }
            }
            
        }
    }
