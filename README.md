pipeline {
    agent any
    stages {
        stage('Git Clone') {
            steps {
                git url: 'https://github.com/durga049/Packer.git', branch: 'main'
            }
        }
        stage('Initialize Packer') {
            steps {
                sh 'packer init packer.pkr.hcl'
            }
        }
        stage('Validation') {
            steps {
                sh 'packer validate -var-file=variables.pkrvars.hcl packer.pkr.hcl'
            }
        }
        stage('Build') {
            steps {
                sh 'packer build -var-file=variables.pkrvars.hcl packer.pkr.hcl'
            }
        }
    }
    post {
        always {
            cleanWs()
        }
        failure {
            echo 'Pipeline failed! Check logs for details.'
        }
        success {
            echo 'Pipeline completed successfully!'
        }
    }
}
