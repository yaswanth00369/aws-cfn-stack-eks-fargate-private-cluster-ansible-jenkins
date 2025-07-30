pipeline {
    agent { label 'ansible-node' }

    environment {
        AWS_SHARED_CREDENTIALS_FILE = '/home/yaswanth/.aws/credentials'
    }

    stages {
        stage('Run Ansible Playbook') {
            steps {
                dir("${WORKSPACE}") {
                    // Run this if you want to deploy the stack
                    // sh 'ansible-playbook Ansible-PlayBooks/deploy-stack.yml'

                    // Run this if you want to delete the stack
                    sh 'ansible-playbook Ansible-PlayBooks/deploy-stack.yml --extra-vars "state=absent"'
                }
            }
        }
    }
}
