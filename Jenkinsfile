pipeline {
     agent any
        
     stages {
         stage('Checkout') {
             steps {
                 git branch: 'master', url: 'https://github.com/pinkijais/supplygoat.git'
                 stash includes: '**/*', name: 'supplygoat'
             }
         }
         stage('Checkov') {
             steps {
                 script {
                     docker.image('bridgecrew/checkov:latest').inside("--entrypoint=''") {
                         unstash 'terragoat'
                         try {
                             sh 'checkov -d . --use-enforcement-rules -o cli -o junitxml --output-file-path console,results.xml --repo-id example/supplygoat --branch master'
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
     options {
         preserveStashes()
         timestamps()
     }
 }
