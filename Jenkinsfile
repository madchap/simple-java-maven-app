pipeline {
    agent any
    // agent { docker 'maven:3-alpine' }

    tools {
        maven 'maven-3.6.0'
        jdk 'openjdk-8'
    }

    stages {
        stage('Init') {
            steps {
                sh '''
                    echo "PATH: ${PATH}"
                    echo "M2_HOME: ${M2_HOME}"
                '''
            }
        }

        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }

        stage('Test') {
            steps {
                withMaven(maven: 'maven-3.6.0') {
                    sh 'mvn dependency:tree -DoutputType=dot --file="pom.xml"'
                }
            }
        }

        stage('snykSecurity') {
            steps {
            //    withMaven(maven: 'maven-3.6.0') {
                        snykSecurity \
                            organisation: 'cloudbees', \
                            projectName: 'simple-app', \
                            severity: 'medium', \
                            snykInstallation: 'snyk-latest', \
                            snykTokenId: 'snyk-jenkins-home', \
                            targetFile: 'pom.xml'
            //    }
            }
        }
    }
}
