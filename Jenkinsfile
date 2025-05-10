Step 5: Create a Jenkinsfile
Create a file named Jenkinsfile in the root of your repository with the following content:
groovypipeline {
    agent any
    
    triggers {
        pollSCM('H/5 * * * *') // Poll repository every 5 minutes for changes
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                // For simple static sites, the build step might just involve validation
                sh 'echo "Building static website"'
                
                // Optional: Add HTML validation
                // sh 'npm install -g html-validator-cli'
                // sh 'html-validator --file ./index.html --verbose'
                
                // Optional: Add CSS/JS minification
                // sh 'npm install -g minify'
                // sh 'minify ./css/* --output ./dist/css/'
                // sh 'minify ./js/* --output ./dist/js/'
            }
        }
        
        stage('Test') {
            steps {
                // Optional: Add tests for your static site
                sh 'echo "Running tests"'
                
                // Example: Check for broken links
                // sh 'npm install -g broken-link-checker'
                // sh 'blc http://localhost:8000 -ro'
            }
        }
        
        stage('Deploy') {
            steps {
                sshagent(['deployment-server-credentials']) {
                    // Create the target directory if it doesn't exist
                    sh 'ssh user@your-server.com "mkdir -p /var/www/html/your-site"'
                    
                    // Copy all files to the deployment server
                    sh 'scp -r ./* user@your-server.com:/var/www/html/your-site/'
                    
                    // Optional: Set correct permissions
                    sh 'ssh user@your-server.com "chmod -R 755 /var/www/html/your-site"'
                }
            }
        }
    }
    
    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
