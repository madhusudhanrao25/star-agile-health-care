pipeline {
     agent { label 'slave' }

	environment {	
		DOCKERHUB_CREDENTIALS=credentials('docker-cred')
	}
		
    stages {
        stage('SCM_Checkout') {
            steps {
                echo 'Perform SCM Checkout'
                git 'https://github.com/madhusudhanrao25/star-agile-health-care.git'
            }
        }
        stage('Application Build') {
            steps {
                echo 'Perform Application Build'
                sh 'mvn clean package'
            }
        }
        stage('Docker Build') {
            steps {
                echo 'Perform Docker Build'
				sh "docker build -t maddy2964/healthcare:${BUILD_NUMBER} ."
				sh "docker tag maddy2964/healthcare:${BUILD_NUMBER} maddy2964/healthcare:latest"
            }
        }
        stage('Login to Dockerhub') {
            steps {
                echo 'Login to DockerHub'				
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                
            }
        }
        stage('Publish the Image to Dockerhub') {
            steps {
                echo 'Publish to DockerHub'
				sh "docker push maddy2964/healthcare:latest"                
            }
        }
        // stage('Copy Playbook to Master') {
        //     steps {
        //         echo 'Copying Ansible Playbook to Master Node'
        //         sh 'scp /home/devopsadmin/workspace/Banking-App/ansible-playbook.yml ansibleadmin@43.204.13.114:/tmp/'
        //     }
        // }
        // stage('Deploy to test-Server') {
        //     steps {
        //         echo 'Running Ansible Playbook on Master Node'
        //         sh '''
        //             ssh ansibleadmin@43.204.13.114 "/usr/bin/ansible-playbook -i /etc/ansible/hosts /tmp/ansible-playbook.yml"
        //         '''
        //     }
        // } 
        // stage('Deploy to Kubernetes Cluster') {
        //     steps {
		// script {
		// 	sshPublisher(publishers: [sshPublisherDesc(configName: 'Kubernetes_Master', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'kubectl apply -f kubernetesdeploy.yaml', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '.', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '*.yaml')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
		// 	}				          
        //     }  

        // }
    }
}