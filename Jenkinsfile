pipeline {
   agent {
        node {
            label 'AGENT-1'
        }
    }
    environment {
        appVersion = ''
        REGION = "us-east-1"
        ACC_ID = "784585544641"
        PROJECT = "roboshop"
        COMPONENT = "catalogue"

    }
    options {
        timeout(time: 30, unit: 'MINUTES') 
        disableConcurrentBuilds()
    }
     /*  parameters {
        string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
        text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')
        booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')
        choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')
        password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password') 
    } */
// build
    stages {
        stage('Read package.json') {
            steps {
                script {
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
                    echo "Project Version: ${appVersion}"
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                script {
                    sh """
                        npm install
                    """
                }
            }
        }
        stage('Unit Testing') {
            steps {
                script {
                    sh """
                        echo "unit testing"
                    """
                }
            }
        }
        stage('Docker Build') {
            steps {
                script { 
                    withAWS(credentials: 'aws-creds', region: 'us-east-1') {
                         sh """  
                             aws ecr get-login-password --region ${REGION} | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com
                             docker build -t ${ACC_ID}.dkr.ecr.${REGION}.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion} .
                             docker push ${ACC_ID}.dkr.${REGION}.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion} 
                         """
                    }    
                }
            }
        }
    
        stage('Test') {
            steps {
                script {
                    echo 'Testing..'
                }
            }
        }
    }
    post { 
        always { 
            echo 'I will always say Hello again!'
        }
        success {
            echo 'hi this is success'
            deleteDir()
        }
        failure {
            echo 'hi, this is failure'
        }
    }
    
}