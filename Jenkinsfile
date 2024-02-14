pipeline{
    agent any
    tools {
  maven 'MAVEN'
  }
 environment {
  DOCKER_TAG = getVersion()
  }
    stages{
        stage('git checkout'){
        steps{
            git credentialsId: 'github', url: 'https://github.com/aniketmore620/star-agile-banking-finance.git'
        }
    }
    stage('Maven package'){
        steps{
            sh 'mvn clean package'
        }
    }
    stage('docker build image'){
        steps{
            sh 'docker build . -t moreaniket/banking:${DOCKER_TAG}'
     }
}
    stage('docker hub push'){
        steps{
            withCredentials([string(credentialsId: 'dockerhub', variable: 'Dockerhubpwd')]) {
            sh 'docker login -u moreaniket -p ${Dockerhubpwd}'
}
            sh 'docker push moreaniket/banking:${DOCKER_TAG}'
     }
  }
    stage('deploy'){
        steps{
            ansiblePlaybook credentialsId: 'dev-server', disableHostKeyChecking: true, extras: '-e DOCKER_TAG=${DOCKER_TAG}', installation: 'ansible', inventory: 'ans-inventory.inv', playbook: 'ansible-playbook.yml', vaultTmpPath: ''
   }
}
}
}
def getVersion(){
def commitHash = sh returnStdout: true, script: 'git rev-parse --short HEAD'
   return commitHash    
}
