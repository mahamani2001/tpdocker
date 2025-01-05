pipeline {
    agent any

    stages {
        stage('Build Server') {
            when {
                expression {
                    // Vérifie si des fichiers dans le dossier "server" ont été modifiés
                    sh(script: 'git diff --name-only HEAD | grep "^server/"', returnStatus: true) == 0
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
            // Nettoyage Docker
            sh '''
            docker container prune -f
            docker image prune -f
            docker volume prune 
            '''
        }
    }
}
