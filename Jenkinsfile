pipeline {
    agent any // This tells Jenkins to run on any available agent/node

    stages {
        // Stage 1: Get the Code
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/yourusername/yournodeapp.git'
            }
        }

        // Stage 2: Install Dependencies
        stage('Install') {
            steps {
                sh 'npm install' // Runs the shell command 'npm install'
            }
        }

        // Stage 3: Check Code Quality
        stage('Lint') {
            steps {
                sh 'npm run lint' // Assumes you have a lint script in package.json
            }
        }

        // Stage 4: Run Tests
        stage('Test') {
            steps {
                sh 'npm test' // Runs the test script
            }
            post {
                always {
                    junit 'testresults.xml' // Archive test results (if your test framework generates JUnit XML)
                }
            }
        }

        // Stage 5: Deploy (only if on the main branch and tests passed)
        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                sh './deploy.sh' // A hypothetical script that deploys your app
            }
        }
    }

    // Postbuild actions
    post {
        failure {
            emailext body: 'Project ${JOB_NAME} build ${BUILD_NUMBER} failed.\\nCheck it at ${BUILD_URL}',
                    subject: 'Jenkins Build Failed',
                    to: 'devteam@yourcompany.com'
        }
        success {
            echo 'Build and deployment were successful!'
        }
    }
}
