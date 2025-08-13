pipeline {
    agent any
    environment {
        GH_TOKEN = credentials('github-creds') // references Jenkins stored token
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

                    def repo = "kerthiks/jenkins-pr-demo" // change this

                    sh """
                    curl -X PUT -H "Authorization: token ${GH_TOKEN}" \
                    -H "Accept: application/vnd.github+json" \
                    https://api.github.com/repos/${repo}/pulls/${env.CHANGE_ID}/merge
                    """
                }
            }
        }
    }
}
