#!/usr/bin/env groovy

// Import the shared library by name
@Library('devops-shared-lib') _

// Scripted Pipeline
node {

    stage('Checkout') {
        checkout scm
    }

    stage('Build Info') {
        echo "============================================"
        echo "  Branch: ${env.BRANCH_NAME}"
        echo "  Build:  #${env.BUILD_NUMBER}"
        echo "  Job:    ${env.JOB_NAME}"
        echo "============================================"
    }

    stage('Validate') {

        // Shared library function
        validateProject(requiredFiles: [
            'project.json',
            'README.md',
            'app/index.html'
        ])
    }

    stage('Test') {

        sh '''
            echo "--- HTML Validation ---"

            if grep -q "<html" app/index.html; then
                echo "PASS: index.html contains valid HTML"
            else
                echo "FAIL: invalid HTML"
                exit 1
            fi
        '''
    }

    stage('Stats') {

        // Shared library function
        codeStats()
    }

    stage('Report') {

        echo "--- Build completed successfully ---"
    }
}
