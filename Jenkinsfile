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
            stage('Compile  Code'){
                steps{
                    sh 'mvn clean package'
                }
            }
        }
    }
