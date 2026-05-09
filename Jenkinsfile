#!/usr/bin/env groovy

// Import the shared library by name (configured in Jenkins global settings)
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
        // Calling the shared library function!
        // This runs the code from vars/validateProject.groovy
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
        // Another shared library function
        codeStats()
    }

    stage('Report') {
        // Shared library notification function
        notifyBuild(status: 'SUCCESS')
    }
}
  
