pipeline {
    agent any

    stages {
        stage('Build Server') {
            when {
                expression {
                    // Check for changes in the 'server/' directory
                    def changedFiles = sh(script: 'git diff --name-only HEAD~1..HEAD | grep "^server/"', returnStdout: true).trim()
                    echo "Changed files in server directory: ${changedFiles}"
                    return changedFiles != ""  // Proceed if there are changes in the server directory
                }
            }
            steps {
                echo 'Building server...'
                sh 'docker build -t mern-server ./server'
            }
        }
        stage('Build Client') {
            when {
                expression {
                    // Check for changes in the 'client/' directory
                    def changedFiles = sh(script: 'git diff --name-only HEAD~1..HEAD | grep "^client/"', returnStdout: true).trim()
                    echo "Changed files in client directory: ${changedFiles}"
                    return changedFiles != ""  // Proceed if there are changes in the client directory
                }
            }
            steps {
                echo 'Building client...'
                sh 'docker build -t mern-client ./client'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up Docker artifacts...'
            // Docker cleanup to remove unused containers, images, and volumes
            sh '''
            docker container prune -f
            docker image prune -f
            docker volume prune -f
            '''
        }
    }
}
