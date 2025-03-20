pipeline {
  agent { label 'principal' } // Ensure this agent is a Windows machine

  environment {
    // Define the SonarScanner tool name (configured in Jenkins Global Tool Configuration)
    SONAR_SCANNER_HOME = tool name: 'sq_test', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
  }

  stages {
    // Stage 1: Checkout the code from GitHub
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/RanfordSAS/SONAR-TEST-JS.git'
      }
    }

    // Stage 2: Print files to be reviewed
    stage('List Files') {
      steps {
        bat 'dir /s /b' // Lists all files in the workspace (Windows)
        // For Linux/Mac, use: sh 'find .'
      }
    }

    /*
    // Stage 3: Create and activate a Python virtual environment
    stage('Setup Virtual Environment') {
      steps {
        bat 'python -m venv venv'
      }
    }

    // Stage 4: Install Python dependencies inside the virtual environment
    stage('Install Dependencies') {
      steps {
        bat 'call venv\\Scripts\\activate && pip install -r requirements.txt'
      }
    }

    // Stage 5: Run tests and generate coverage report (optional)
    stage('Run Tests') {
      steps {
        bat 'call venv\\Scripts\\activate && pytest --cov=./ --cov-report=xml:coverage.xml'
      }
    }
    */

    // Stage 6: Run SonarQube analysis
    stage('SonarQube Analysis') {
      steps {
        withSonarQubeEnv(installationName: 'sq_test') { // 'sq_test' is the name of the SonarQube server configured in Jenkins
          bat """
          ${SONAR_SCANNER_HOME}\\bin\\sonar-scanner.bat"""
        }
      }
    }
  }

  // Post-build actions
  post {
    success {
      echo 'SonarQube analysis completed successfully!'
    }
    failure {
      echo 'SonarQube analysis failed.'
    }
  }
}
