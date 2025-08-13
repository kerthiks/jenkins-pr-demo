pipeline {
    agent any

    environment {
        // Reference to a secret text credential in Jenkins (GitHub personal access token)
        // Make sure 'github-creds' is the correct credentials ID
        GH_TOKEN = credentials('github-creds')
    }

    stages {
        stage('Build') {
            steps {
                echo "Building branch ${env.BRANCH_NAME}"
                sh 'echo Building application...'
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                sh 'echo All tests passed!'
            }
        }
    }

    post {
        success {
            script {
                if (env.CHANGE_ID) {
                    echo "This is a PR. Attempting to merge PR #${env.CHANGE_ID}..."

                    def repo = "kerthiks/jenkins-pr-demo" // ✅ Set your repo here

                    // Use single-quoted sh block to prevent Groovy interpolation
                    // This keeps $GH_TOKEN secure
                    sh '''
                        curl -X PUT \
                             -H "Authorization: token $GH_TOKEN" \
                             -H "Accept: application/vnd.github+json" \
                             https://api.github.com/repos/kerthiks/jenkins-pr-demo/pulls/$CHANGE_ID/merge
                    '''
                }
            }
        }
    }
}
