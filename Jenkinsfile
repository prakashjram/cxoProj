pipeline { 
    agent any
        
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/prakashjram/cxoProj.git'
            }
        }
         stage('EC2') {
            steps {
                sh 'welcome to ec2'
            }
        }
    }
}
