pipeline {
    agent {label 'REDHAT'}
    triggers {
        pollSCM('H * * * *')
    }
    parameters{
        string(name: 'BRANCH', defaultValue: 'master', description: 'branch to build')
        choice(name: 'GOAL', choices: ['package', 'compile', 'clean package'], description: 'maven goals')
    }
    environment {
        DEVOPS = 'JENKINS'
    }   
    stages {
        stage('SCM'){
            steps {
                git branch: "${params.BRANCH}", url: 'https://github.com/manjulaaporeddy/boxfuse-sample-java-war-hello.git'
            }    
        }
        stage('build'){
            steps {
                sh "mvn ${params.GOAL}"
            }
        }
        stage('ansible') {
            agent{label 'ANSIBLE'}
            steps {
               sh 'cd Deployment && ansible-playbook -i hosts deploy.yaml'
            }
        } 
    }               
                  
    post {
      success {
         archive '**/*.war'
         junit '**/TEST-*.xml'
        }
        
    }
}
