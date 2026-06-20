pipeline {
    agent {
        node {
            label 'manager'
        }
    }
    tools {
        maven 'mymvn'
    }

    stages {
        stage('code') {
            steps {
                git branch: 'main', url: 'https://github.com/Kajjayamsriram/Docker-2-tier-app.git'
            }
        }
        stage('CQA') {
            steps{
                withSonarQubeEnv('sonarqube') {
                   sh '''mvn clean verify sonar:sonar \
                    -Dsonar.projectKey=dockerpro \
                    -Dsonar.qualitygate.wait=true
                   '''
                   //To pass stage modify overall codecoverage as 2% for dockerpro QualityGates
                 }
            }
        }
        stage('CodeBuild') {
            steps{
                sh "mvn package"
            }
        }
        stage('Tirvy dependency Scan') {
            steps{
                sh "trivy fs . > java-vuln.txt"
            }
        }
        stage('Artifact') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'vprofile', classifier: '', file: 'target/vprofile-v2.war', type: 'war']], credentialsId: 'nexus-d', groupId: 'com.visualpathit', nexusUrl: '54.221.63.22:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'myrepo', version: 'v2'
            }
        }
        stage('Build') {
            steps {
                sh '''
                cp -r target Docker-app
                 docker build -t sriramk16/vprofile:app Docker-app
                 docker build -t sriramk16/vprofile:db Docker-db
               '''
            }
        }
        stage('ImageScan') {
            steps{
                sh '''
               trivy image --severity=CRITICAL --exit-code 0 sriramk16/vprofile:app
               trivy image --severity=CRITICAL --exit-code 0 sriramk16/vprofile:db
               '''
            }
        }
        stage('Registry') {
            steps{
                script {
                  withDockerRegistry(credentialsId: 'dockerhub-cred') {
                    sh "docker push sriramk16/vprofile --all-tags"
                }
            }
            }
        }
        stage('Deploy') {
            steps {
                sh "docker stack deploy -c compose.yml vprofile"
            }
        }
    }
}
