pipeline { 
    agent any
    
    environment {
        EC2_IP = '13.127.214.101'  
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
                    
                    withCredentials([sshUserPrivateKey(credentialsId: 'GhToken', keyFileVariable: 'SSH_KEY')]) {
                       
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
                            sudo apt install php php-mysql php-gd php-xml php-mbstring -y
                            cd /var/www/html
                            sudo wget -q https://www.drupal.org/download-latest/tar.gz -O drupal.tar.gz                            
                            sudo tar -xzvf drupal.tar.gz
                            sudo mv drupal-* drupal
                            sudo chown -R www-data:www-data /var/www/html/drupal
                            sudo chmod -R 755 /var/www/html/drupal
                            sudo systemctl restart apache2
                            echo "LAMP stack and Drupal 10 setup complete."
EOF
                        '''
                    }
                }
            }
        }
    }
}

