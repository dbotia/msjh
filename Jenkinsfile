#!/usr/bin/env groovy

node {
    stage('checkout') {
        checkout scm
    }

    stage('check java') {
        bat "java -version"
    }

    stage('clean') {
    
        bat "mvnw clean"
    }

    stage('backend tests') {
        try {
            bat "mvnw test"
        } catch(err) {
            throw err
        } finally {
            junit '**/target/surefire-reports/TEST-*.xml'
        }
    }

    stage('packaging') {
        bat "mvnw  -Pprod"
        archiveArtifacts artifacts: '**/target/*.war', fingerprint: true
    }
    stage('quality analysis') {
        withSonarQubeEnv('sonar') {
            bat "mvnw sonar:sonar"
        }
    }
}
