pipeline {
    agent any

    stages {
        stage('Basic configurations') {
            steps {
                script {
                    git branch: 'master', url: 'https://github.com/EmmanuelaTurkson/netauto'
                }
                echo 'Initial Router Configurations...'
                sh 'ansible-playbook -i hosts config_routers.yml'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing IP Brief'
                sh 'ansible-playbook -i hosts testipbrief.yml'
            }
        }

        stage('Routing configurations') {
            steps {
                echo 'Running OSPF and EIGRP configuration...'
                sh 'ansible-playbook -i hosts ospf_config.yml'
                sh 'ansible-playbook -i hosts eigrp_config.yml'
            }
        }

        stage('Routing Test') {
            steps {
                echo 'Testing OSPF configuration between Router 2 and 3...'
                sh 'ansible-playbook -i hosts testospf.yml'
                echo 'Testing EIGRP configuration between Router 1 and 2..'
                sh 'ansible-playbook -i hosts testeigrp.yml'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}
