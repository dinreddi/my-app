pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'docker build -t my-app-image .'
            }
        }
        stage('Test') {
            steps {
                sh 'pytest tests/'
            }
        }
        stage('Push to Registry') {
            steps {
                sh '''
                docker login -u $DOCKER_USER -p $DOCKER_PASS
                docker tag my-app-image my-docker-repo/my-app-image:latest
                docker push my-docker-repo/my-app-image:latest
                '''
            }
        }
        stage('Deploy to OpenShift') {
            steps {
                sh '''
                oc login --token=$OPENSHIFT_TOKEN --server=$OPENSHIFT_SERVER
                oc new-app my-docker-repo/my-app-image:latest
                oc expose svc/my-app
                '''
            }
        }
    }
}
