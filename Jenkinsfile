pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')
        IMAGE_NAME = 'varsha200/gitloop-app'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$BUILD_NUMBER ./app'
            }
        }

        stage('Push') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh 'docker push $IMAGE_NAME:$BUILD_NUMBER'
            }
        }

        stage('Update Manifest') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github-creds', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASS')]) {
                    sh '''
                        sed -i "s|image: .*gitloop-app:.*|image: $IMAGE_NAME:$BUILD_NUMBER|" k8s-manifests/deployment.yaml
                        git config user.email "jenkins@gitloop.local"
                        git config user.name "Jenkins CI"
                        git add k8s-manifests/deployment.yaml
                        git commit -m "Update image to build $BUILD_NUMBER"
                        git push https://$GIT_USER:$GIT_PASS@github.com/$GIT_USER/gitloop.git HEAD:main
                    '''
                }
            }
        }
    }
}