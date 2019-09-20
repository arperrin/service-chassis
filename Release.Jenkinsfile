pipeline {
    agent any
    tools {
        maven 'Maven 3.6.1'
    }
    stages { 
        stage('Clean') {
            steps {
                cleanWs()
            }
        }
        stage('Checkout') {
            steps {
                git credentialsId: 'github', url: 'https://github.com/arperrin/service-chassis.git'
            }
        }
        stage('Release') {
            steps {                    
                withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'pass', usernameVariable: 'user')]) {
                    configFileProvider([configFile(fileId: 'maven-settings', targetLocation: 'settings.xml', variable: 'MAVEN_SETTINGS')]) {
                        sh 'mvn clean release:prepare release:perform -B -s ${MAVEN_SETTINGS} -Dusername=${user} -Dpassword=${pass}'
                    }                
                }
            }
        }
    }
}