pipeline {
    agent any

    tools {
        maven 'maven-3.8.6'
    }

    stages {
        stage('Preparation') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/willbrid/content-cje-prebuild.git']]])
            }
        }
        stage('Build') {
            steps {
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }
        stage('Post Job') {
             steps {
                sh 'bin/makeindex'
            }
        }
        stage('Results') {
            steps {
                junit '**/target/surefire-reports/TEST-*.xml'
                archiveArtifacts 'index.jsp'
                fingerprint 'index.jsp'
            }
        }
    
    }
}
