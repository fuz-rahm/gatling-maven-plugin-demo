pipeline {
    agent {
        label 'master'
    }
    tools {
        maven 'maven'
        jdk 'java8'
        nodejs "Node10"
        
    }  

    stages {
        stage('clean workspace') {
            steps{
                cleanWs()
            }
        }

        stage('Checkout from Github') {
        
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/mr/addpa11y-ci']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'git@github.com:fuz-rahm/gatling-maven-plugin-demo.git']]])
            }
        }

        stage ('JUnit Unit Test') {
            steps{
                sh 'mvn clean verif y'
            }
        }

        stage('508'){
            steps{
                sh label: '', script: 'lighthouse --quiet --no-update-notifier --no-enable-error-reporting --output=json --output-path=./lighthouse-report.json --chrome-flags="--headless" https://cynerge.com'

       lighthouseReport 'lighthouse-report.json'

            }
        }
        

        stage('build') {
            steps {
                sh 'echo "path: ${PATH}"'
                sh 'echo "M2_HOME: ${M2_HOME}"'
                sh 'mvn clean install -Dmaven.test.failure.ignore=true'
            }
        }

        stage('Notification'){

            steps {

                sh label: '', returnStdout: true, script: 'echo "Email Notification"'

             }

        }    

        
    }

    post {
        always {
           junit '**/reports/junit/*.xml'
        }     
    
    }
}