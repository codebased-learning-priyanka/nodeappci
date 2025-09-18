pipeline {
    agent any // Run on any available Jenkins agent/node

    options {
        timeout(time: 1, unit: 'HOURS') // Set a timeout for the pipeline
        buildDiscarder(log: 5) // Keep only the last 5 builds to save space
    }

    stages {
        // Stage 1: Checkout code from version control
        stage('Checkout') {
            steps {
                echo 'Checking out source code from GitHub...'
                git branch: 'main', url: 'https://github.com/yourusername/yoursimplenodeapp.git'
            }
        }

        // Stage 2: Install dependencies using npm
        stage('Install Dependencies') {
            steps {
                echo 'Installing npm dependencies...'
                sh 'npm install'
            }
        }

        // Stage 3: Run a linter to check code quality
        stage('Lint Code') {
            steps {
                echo 'Running ESLint for code style checks...'
                sh 'npm run lint' // This script must be defined in package.json
            }
        }

        // Stage 4: Run the unit tests
        stage('Run Tests') {
            steps {
                echo 'Running unit tests with Mocha/Jest...'
                sh 'npm test'
            }
            post {
                // This action runs AFTER the "Test" stage completes
                always {
                    junit 'testresults.xml' // Archive JUnitstyle test results (if generated)
                    echo 'Test stage completed. Results archived.'
                }
            }
        }

        // Stage 5: Build the application (e.g., create a package)
        stage('Build') {
            steps {
                echo 'Building the application...'
                sh 'npm run build' // Could create a /dist folder
            }
        }

        // Stage 6: Archive the built application as an artifact
        stage('Archive Artifact') {
            steps {
                echo 'Archiving the build artifact...'
                archiveArtifacts artifacts: 'dist//', fingerprint: true
                // Archives everything in the 'dist' directory
            }
        }
    }

    // Postbuild actions run after ALL stages are finished
    post {
        always {
            echo "Pipeline execution for build ${env.BUILD_ID} is complete."
            cleanWs() // Clean up the workspace
        }
        success {
            emailext (
                subject: "SUCCESS: Pipeline '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: "The pipeline succeeded. Check it out at ${env.BUILD_URL}",
                to: "devteam@yourcompany.com"
            )
            echo 'This will run only if the entire pipeline was successful.'
        }
        failure {
            emailext (
                subject: "FAILED: Pipeline '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: "The pipeline failed. Check the logs at ${env.BUILD_URL}",
                to: "devteam@yourcompany.com"
            )
            echo 'This will run only if the pipeline had a failure.'
        }
    }
}
