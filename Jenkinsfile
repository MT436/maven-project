pipeline {
    agent any
    stages{
        stage('SCM : GIT'){
            steps{
               checkout scmGit(
                   branches: [[name: '*/proto1']], 
                   extensions: [], 
                   userRemoteConfigs: [[url: 'https://github.com/MT436/maven-project.git']]
                   )
            }
        }
        stage('Integration testing : mvn'){
            steps{
                sh 'mvn test'
            }
        }
        stage('build: unittest'){
            steps{
                sh 'mvn verify -DskipUnitTests'
            }
        }
        stage('build: maven'){
            steps{
                sh 'mvn clean package'
            }
        }
        stage('codequality: sonar'){
            steps{
                sh ''' mvn clean verify sonar:sonar \
                       -Dsonar.projectKey=proto1 \
                       -Dsonar.projectName='proto1' \
                       -Dsonar.host.url=http://172.31.18.18:9000 \
                       -Dsonar.token=sqp_daaa28efa996b5307d4fa9c1f8d1d051c7b71d27'''
            }
        }
        stage('Artifactory: jfrog'){
            steps{
                 rtUpload (
                    serverId: 'jfrog', // ID of the Artifactory server configured in Jenkins
                    spec: '''{
                        "files": [{
                            "pattern": "target/*.war",
                            "target": "http://172.31.18.18:8082/artifactory/thejeshmm-thejeshmm/"
                        }]
                    }'''
                )
            }
        }
    }
}

