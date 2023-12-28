def installTerraform() {
    // Check if terraform is already installed
    def terraformExists = sh(script: 'which terraform', returnStatus: true)
    if (terraformExists != 0) {
        // Install terraform
        sh '''
            echo "Installing Terraform..."
            wget https://releases.hashicorp.com/terraform/1.6.6/terraform_1.6.6_linux_amd64.zip
            unzip terraform_1.6.6_linux_amd64.zip
            sudo mv terraform /usr/local/bin/
            rm -f terraform_1.6.6_linux_amd64.zip
        '''
    } else {
        echo "Terraform already installed!"
    }
}

pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                // Pull the git repo
                cleanWs()
                checkout scm
            }
        }

        stage('Install Terraform') {
            steps {
                script {
                  installTerraform()
            }

          }
	}
        stage('Terraform Deployment') {
            steps {
                script {
                    // CD into deployment folder and run terraform commands
                    dir('deployment') {
                        sh '''
                            terraform init
                            terraform fmt
                            TF_LOG=debug terraform validate
                            TF_LOG=debug terraform apply -auto-approve
                        '''
                    }
                }
            }
        }
    }
}
