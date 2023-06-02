node {
    stage('Code Checkout') { 
        
        git 'https://github.com/geetha0914/insurance-project-demo.git'
      
    }
    
    stage('Build the code') { 
        
        sh 'mvn clean package'
      
    }
    
    stage('Build Docker Image') { 
        
         sh 'docker build -t geetha0914/insure-me:1.0 .'

    stage('Docker image into Docker hub')
        withCredentials([string(credentialsId: 'docker-pwd', variable: 'insurence')]) {
         sh "docker login -u geetha0914 -p ${insurence}"
         sh 'docker push geetha0914/insure-me:1.0'
      }
        
        
    } 
    stage('config and deploy insure app into test server'){
        ansiblePlaybook become: true, credentialsId: 'ssh-Ansible-key', disableHostKeyChecking: true, installation: 'Ansible', inventory: '/etc/ansible/hosts/', playbook: 'configure-testserver.yml'  
    }
    stage('Run a Selenium jar file') {
        sh 'java -jar seleniumtest.jar'
        
    }
    stage('config and deploy insure app into prod server'){
        
        
        ansiblePlaybook become: true, credentialsId: 'ssh-Ansible-key', disableHostKeyChecking: true, installation: 'Ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-prodserver.yml'
    
    }
}
