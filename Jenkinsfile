pipeline {
    agent any

    stages {
        stage('Basic configurations') {
            steps {
                script {
                    // Checkout code from GitHub repository
                    git branch: 'master', url: 'https://github.com/EmmanuelaTurkson/netauto'
                    
                    // Initial Router Configurations
                    echo 'Initial Router Configurations...'
                    if (isUnix()) {
                        sh 'ansible-playbook -i hosts config_routers.yml'
                    } else {
                        bat 'ansible-playbook -i hosts config_routers.yml'
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    echo 'Testing IP Brief'
                    if (isUnix()) {
                        sh 'ansible-playbook -i hosts testipbrief.yml'
                    } else {
                        bat 'ansible-playbook -i hosts testipbrief.yml'
                    }
                }
            }
        }

        stage('Routing configurations') {
            steps {
                script {
                    echo 'Running OSPF and EIGRP configuration...'
                    if (isUnix()) {
                        sh 'ansible-playbook -i hosts ospf_config.yml'
                        sh 'ansible-playbook -i hosts eigrp_config.yml'
                    } else {
                        bat 'ansible-playbook -i hosts ospf_config.yml'
                        bat 'ansible-playbook -i hosts eigrp_config.yml'
                    }
                }
            }
        }

        stage('Routing Test') {
            steps {
                script {
                    echo 'Testing OSPF configuration between Router 2 and 3...'
                    if (isUnix()) {
                        sh 'ansible-playbook -i hosts testospf.yml'
                        sh 'ansible-playbook -i hosts testeigrp.yml'
                    } else {
                        bat 'ansible-playbook -i hosts testospf.yml'
                        bat 'ansible-playbook -i hosts testeigrp.yml'
                    }
                }
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
