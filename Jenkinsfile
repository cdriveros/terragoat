pipeline {
    agent none
    stages {
        stage('Scan checkov') {
            agent {
                docker { 
                    image 'bridgecrew/checkov:latest' 
                    args ' --entrypoint ""' 
                }
            }
            steps {
                sh 'checkov --soft-fail -o cli -o json --output-file-path console,results.json -d .'
                archiveArtifacts artifacts: 'results.json', fingerprint: true, allowEmptyArchive: true
            }
        }
        stage('TruffleHog Scan') {
            agent {
                docker { 
                    image 'trufflesecurity/trufflehog:latest'
                    args '-u root --entrypoint ""'
                } 
            }
            steps {
                script {
                    sh '/usr/bin/trufflehog filesystem "$PWD" --json > trufflehog-report.json'
                    
                    archiveArtifacts artifacts: 'trufflehog-report.json', fingerprint: true, allowEmptyArchive: true
                }
            }
        }

    }
}
