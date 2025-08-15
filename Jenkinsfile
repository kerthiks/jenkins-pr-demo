pipeline {
    agent any

    environment {
        GH_TOKEN = credentials('github-token') // optional — only if you use gh CLI
    }

    stages {
        stage('Build') {
            steps {
                echo "🔧 Building branch: ${env.BRANCH_NAME}"
                sh './run-tests.sh'
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                echo '🚀 Deploying to production...'
                sh './deploy.sh'
            }
        }
    }

    post {
        success {
            script {
                if (env.BRANCH_NAME == 'main') {
                    echo "✅ Build on main succeeded — deployed."
                } else if (env.CHANGE_ID) {
                    echo "✅ PR build succeeded — no merge needed here."
                }
            }
        }
        failure {
            echo "❌ Build failed."
        }
    }
}
