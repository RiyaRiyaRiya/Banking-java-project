pipeline{
    agent any
    stages{
        stage('checkout the code from github'){
            steps{
                 git url: 'https://github.com/RiyaRiyaRiya/Banking-java-project.git'
                 echo 'github url checkout'
            }
        }
        stage('codecompile with riya'){
            steps{
                echo 'starting compiling'
                sh 'mvn compile'
            }
        }
        stage('codetesting with riya'){
            steps{
                sh 'mvn test'
            }
        }
        stage('qa with riya'){
            steps{
                sh 'mvn checkstyle:checkstyle'
            }
        }
        stage('package with riya'){
            steps{
                sh 'mvn package'
            }
        }
         stage('Publish the HTML Reports') {
      steps {
          //publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '/var/lib/jenkins/workspace/BankingProject/target/surefire-reports', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])
          publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, icon: '', keepAll: false, reportDir: '/var/lib/jenkins/workspace/BankingProject/target/surefire-reports', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])
                        }
            }
        stage('run dockerfile'){
          steps{
               sh 'docker build -t riya0201/bankingproject:latest .'
           }
         }
        
        stage('Login to Dockerhub') {
          steps {
            withCredentials([usernamePassword(credentialsId: 'docker-id-user', passwordVariable: 'dockerpass', usernameVariable: 'dockeruser')]) {
            sh 'docker login -u ${dockeruser} -p ${dockerpass}'
                                          }
                          }
            }
       stage('Push the Docker image') {
         steps {
           sh 'docker push riya0201/bankingproject:latest'
                                } 
    }
        stage('Ansbile config and Deployment') {
          steps {
            ansiblePlaybook credentialsId: 'sshkey', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'deploy.yml', vaultTmpPath: ''     
        }   
     }
  }
}
