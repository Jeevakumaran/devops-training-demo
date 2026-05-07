#!/usr/bin/env groovy

// Scripted Pipeline — runs inside a node (Jenkins agent)`  
node {
    // Stage 1: Fetch the code from GitHub
    stage('Checkout') {
        // This tells Jenkins to check out the source code
        // from the branch that triggered this build
        checkout scm
    }

    // Stage 2: Display basic info about the build
    stage('Build Info') {
        echo "============================================"
        echo "  Branch: ${env.BRANCH_NAME}"
        echo "  Build:  #${env.BUILD_NUMBER}"
        echo "  Job:    ${env.JOB_NAME}"
        echo "============================================"
    }

    // Stage 3: Validate the project files exist
    stage('Validate') {
        sh '''
            echo "--- Validating project structure ---"

            if [ -f "project.json" ]; then
                echo "PASS: project.json found"
            else
                echo "FAIL: project.json MISSING"
                exit 1
            fi

            if [ -f "app/index.html" ]; then
                echo "PASS: app/index.html found"
            else
                echo "FAIL: app/index.html MISSING"
                exit 1
            fi

            if [ -f "README.md" ]; then
                echo "PASS: README.md found"
            else
                echo "FAIL: README.md MISSING"
                exit 1
            fi

            echo "All validations passed!"
        '''
    }

    // Stage 4: Run a basic HTML check
    stage('Test') {
        sh '''
            echo "--- HTML Validation ---"
            if grep -q "<html" app/index.html; then
                echo "PASS: index.html contains valid HTML structure"
            else
                echo "FAIL: index.html does not look like valid HTML"
                exit 1
            fi
        '''
    }

    // Stage 5: Report success
    stage('Report') {
        echo "BUILD SUCCESSFUL — All checks passed on branch ${env.BRANCH_NAME}!"
    }
}

  
