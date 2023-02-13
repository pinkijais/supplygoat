pipeline {
    agent any
    
    environment {
        PRISMA_API_URL='https://api2.prismacloud.io'
    }
    
    stages {
        stage('Checkout') {
          steps {
              git branch: 'master', url: 'https://github.com/pinkijais/supplygoat.git'
              stash includes: '**/*', name: 'source'
          }
        }
        stage('Checkov') {
            steps {
                withCredentials([string(credentialsId: 'f4a90b7a-f29e-48bb-b88c-8e888735410a', variable: 'pc_user'),string(credentialsId: 'nJhEtnqOsZSHRlFkA1tbRM0U3Y4=', variable: 'pc_password')]) {
                    script {
                        docker.image('bridgecrew/checkov:latest').inside("--entrypoint=''") {
                          unstash 'source'
                          try {
                              sh 'checkov -d . --use-enforcement-rules -o cli -o junitxml --output-file-path console,results.xml --bc-api-key f4a90b7a-f29e-48bb-b88c-8e888735410a::nJhEtnqOsZSHRlFkA1tbRM0U3Y4= --repo-id  pinkijais/supplygoat --branch main'
                              junit skipPublishingChecks: true, testResults: 'results.xml'
                          } catch (err) {
                              junit skipPublishingChecks: true, testResults: 'results.xml'
                              throw err
                          }
                        }
                    }
                }
            }
        }
    }
    options {
        preserveStashes()
        timestamps()
    }
}
