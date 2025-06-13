pipeline { 
    agent any
    
    environment {
        EC2_IP = '65.2.161.67'
        EC2_USER = 'ubuntu'    
    }
 
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/prakashjram/cxoProj.git'
            }
        }
        
        stage('Deploy to EC2') {
            steps {
                script {
                    
                    withCredentials([sshUserPrivateKey(credentialsId: 'CxoSshPrivKey', keyFileVariable: 'SSH_KEY')]) {
 
                    {
                       
                        sh '''
                            ssh -o StrictHostKeyChecking=no -i ${SSH_KEY} ${EC2_USER}@${EC2_IP} <<EOF
                            sudo apt update
                            sudo apt install apache2 -y
                            sudo apt install mysql-server -y
                            sudo mysql -u root <<MYSQL_EOF
                            CREATE DATABASE IF NOT EXISTS jpdrupal;
                            CREATE USER IF NOT EXISTS 'jpdrupal'@'localhost' IDENTIFIED BY '12345';
                            GRANT ALL PRIVILEGES ON jpdrupal.* TO 'jpdrupal'@'localhost';
                            FLUSH PRIVILEGES;
MYSQL_EOF
                           
EOF
                        '''
                    }
                }
            }
        }
    }
}
