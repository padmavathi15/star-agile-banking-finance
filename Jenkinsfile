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
            git credentialsId: 'github', url: 'https://github.com/padmavathi15/star-agile-banking-finance.git'
        }
    }
    stage('Maven package'){
        steps{
            sh 'mvn clean package'
        }
    }
    stage('docker build image'){
        steps{
            sh 'docker build . -t padmavathi/banking:${DOCKER_TAG}'
     }
}
    stage('docker hub push'){
        steps{
            withCredentials([string(credentialsId: 'dockerhub', variable: 'Dockerhubpwd')]) {
            sh 'docker login -u padmavathihg -p ${Dockerhubpwd}'
}
            sh 'docker push padmavathi/banking:${DOCKER_TAG}'
     }
  }
   stage('Deploy') {
            steps {
               script {
                    def dockerCmd = 'sudo docker run -itd --name My-first-containe21 -p 8081:80 padmavathi/banking:v1'
                    sshagent(['sshkeypair']) {
                        //chnage the private ip in below code
                        // sh "docker run -itd --name My-first-containe211 -p 8082:80 akshu20791/phpprojectimg:v1"
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.40.154 ${dockerCmd}"
                    }
                }
            }
}
}
}
