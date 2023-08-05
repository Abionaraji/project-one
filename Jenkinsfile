pipeline{
    agent any
    tools{
        jdk 'JDK'
        maven 'Maven'
    }
    stages{
        stage('Git Checkout'){
            steps{
                git branch: 'ci-jenkins', url: 'https://github.com/Abionaraji/project-one.git'
            }
        }
        stage('Build Maven'){
            steps{
                sh 'mvn clean install'
            }
        }
        stage('Test Unit'){
            steps{
                sh 'mvn test'
            }
        }
        stage('Checkstyle Analysis'){
            steps{
                sh 'mvn checkstyle:checkstyle'
            }
        }
        stage('Intergrated Testing'){
            steps{
                sh 'mvn verify -DiskipUnitTest'
            }
        }
        stage('Sonarqube Analysis'){
            steps{
                withSonarQubeEnv(installationName: 'SonarQube', credentialsId: 'jenkins-sonar') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('Quality Gate Status'){
            steps{
                waitForQualityGate abortPipeline: true, credentialsId: 'jenkins-sonar'
            }
        }
        stage('Upload War into Nexus'){
            steps{
                nexusArtifactUploader artifacts: 
                [
                    [
                        artifactId: 'spring-web', 
                        classifier: '', 
                        file: 'target/vprofile-v2.war', 
                        type: 'war'
                        ]
                    ], 
                    credentialsId: 'nexus-jenkis', 
                    groupId: 'production', 
                    nexusUrl: '54.172.243.16:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'vpro-maven', 
                    version: 'v2'
            }
        }
    }
}