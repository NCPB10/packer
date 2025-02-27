pipeline {
    agent any

    environment {
        AWS_REGION = "ap-south-1"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', credentialsId: 'naveen', url: 'https://github.com/NCPB10/packer.git'  // Update with your repo
            }
        }

        stage('Validate Packer Template') {
            steps {
                sh 'packer validate ami.json'
            }
        }

        stage('Build AMI with Packer') {
            steps {
                script {
                    def buildOutput = sh(script: 'packer build ami.json', returnStdout: true).trim()
                   
                    def amiIdMatch = buildOutput =~ /AMI: (ami-[a-zA-Z0-9]+)/
                    if (amiIdMatch) {
                        def newAmiId = amiIdMatch[0][1]
                        echo "New AMI ID: ${newAmiId}"
                        env.NEW_AMI_ID = newAmiId
                    } else {
                        error "Failed to extract AMI ID from Packer output."
                    }
                }
            }
        }
    }
}
