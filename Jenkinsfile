pipeline {
    agent any

    stages {
        stage('get chekout git...'){
            steps{
               git branch : 'main',
                url : 'https://github.com/medaziz161/Achat'
            }
        }
        stage('Running the unit test...'){
            steps{
                sh 'mvn clean test'
            }
        }
        stage("Build") {
            steps {
                sh 'mvn install -DskipTests=true'
            }
        }
        stage("SonarQube Analysis") {
            steps {
                sh 'mvn sonar:sonar'
            }
        }
        stage('Nexus Deploy ') {
            steps {
                nexusArtifactUploader artifacts: [
                    [
                        artifactId: 'achat',
                        classifier: '',
                        file: 'target/achat.jar',
                        type: 'jar'
                    ]
                ],
                 credentialsId: 'nexus3',
                 groupId: 'tn.esprit.rh',
                 nexusUrl: 'localhost:8081',
                 nexusVersion: 'nexus3',
                 protocol: 'http',
                 repository: 'Achat-release',
                 version: '1.0.0'
            }
        }

        stage('Build backend docker image') {
            steps {
                sh 'docker build -t bmakes/spring .'
            }
        }
        
      
        stage('Push images to Dockerhub') {
            steps{
                script{                    
                    sh 'docker login -u bmakes -p ehNRW3QFpeL835h'
                    sh 'docker push bmakes/spring'
                }
            }
        }
        
        stage("Email notification sender ..."){
               steps{
                      emailext attachLog: true, body: "${env.BUILD_URL} has result ${currentBuild.result}", compressLog: true, subject: "Status of pipeline: ${currentBuild.fullDisplayName}", to: 'bassem.jadoui@esprit.tn,karim.mannai@esprit.tn,naceuramine.saddem@esprit.tn,ahmed.jelassi@esprit.tn,khaled.battiche@esprit.tn'
               }
        }
   }
}
