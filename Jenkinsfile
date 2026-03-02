//git checkout main
//cat > Jenkinsfile << 'EOF'
pipeline {
  agent any
  options {
    //keep builds for 30 days, optional
    buildDiscarder(logRotator(daysToKeepStr: '30'))
    timestamps()
  }
  parameters {
    string(name:'NAME', defaultValue: 'Captain America', description: 'Who should we greet?')
  }
  stages {
    stage('Checkout (scm)') {
      steps {
        //'checkout scm' will use the Jenkins job SCM configuration (branch/credentials)
        checkout scm
      }
    }
    stage('Show workspace and agent info') {
      steps {
        echo "Workspace: ${env.WORKSPACE}"
        echo "Build number: ${env.BUILD_NUMBER}"
        //detect OS/ node
        sh '''
        echo "uname -a:"
        uname -a || true
        echo "lsb_release (if available):"
        lsb_release -s || true
        echo "whoami:"
        whoami || true
        '''
      }
    }
    stage('Build / Demo') {
      steps {
        echo "Hello ${params.NAME}"
        //run your demo script (make sure its executable)
        sh 'if [ -f app.sh ]; then bash app.sh; else echo "no app.sh found"; fi'
      }
    }
    stage('Create artifact') {
      steps {
        sh '''
        echo "Create output file" > output.txt
        echo "Build number: ${BUILD_NUMBER}" >> output.txt
        date >> output.txt
        ls -ls >> output.txt
        '''
        }
        }
    stage('Archive artifact') {
      steps {
        archiveArtifacts artifacts: 'output.txt', fingerprint: true
      }
    }
  }
    post {
      always {
        echo "Build finished: ${currentBuild.currentResult}"
      }
      failure {
        echo "There was a failure. Check console output."
      }
      success {
        echo "Success - artifact archived."
      }
    }
  }
  EOF

  git add Jenkinsfile
  git commit -m "chore: add Jenkinsfile for CI demo"
  git push origin main
        
