pipeline{
    agent any
    tools{
        maven 'MVN'
        jdk 'JDK11'
        git 'git'
    }
    environment{
        DOCKER_LOGIN_NAME = 'akashz'
        DOCKER_PASSWORD = credentials('docker_token')
        APP_NAME = 'myjava'
        K8S_NAMESPACE = 'prod-app'
        DEPLOYMENT_NAME = 'mydeployment'
        CONTAINER_NAME = 'mycontainer'
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    stages{
        stage('Fetch Code'){
            steps{
                git branch: 'main', url: 'https://github.com/akashzakde/Java_CICD_Project_EKS.git'
            }
        }
        stage('Code Analysis'){
            steps{
                withSonarQubeEnv(credentialsId: 'sonarqube-creds',installationName: 'sonarqube') {
                sh '''mvn clean verify sonar:sonar \
                    -Dsonar.projectKey=Java-Project \
                    -Dsonar.projectName='Java-Project' \
                    -Dsonar.host.url=http://172.31.4.11:9000 \
                    -Dsonar.token=sqp_156397e53f4007d40e1a790c9ffebbe0442cace1'''
                    }
                }
            }
        stage('QualityGate Result'){
            steps {
                    timeout(time: 1, unit: 'HOURS'){
                    waitForQualityGate abortPipeline: true
                    }
                }
            }
        stage('Build Code'){
            steps{
                sh 'mvn clean -DskipTests package'
            }
        }
        stage('Test Code'){
            steps{
                sh 'mvn test'
            }
        }
        stage('Docker Image Build'){
            steps{
                sh '''
                    docker build -t $DOCKER_LOGIN_NAME/$APP_NAME .
                    docker tag $DOCKER_LOGIN_NAME/$APP_NAME $DOCKER_LOGIN_NAME/$APP_NAME:$IMAGE_TAG
                    '''
            }
        }
        stage('Push Image'){
            steps{
                sh '''
                    docker login -u $DOCKER_LOGIN_NAME -p $DOCKER_PASSWORD
                    docker push $DOCKER_LOGIN_NAME/$APP_NAME:latest
                    docker push $DOCKER_LOGIN_NAME/$APP_NAME:$IMAGE_TAG
                    '''
            }
        }
        stage('Clean Images'){
            steps{
                sh '''
                docker rmi $DOCKER_LOGIN_NAME/$APP_NAME:latest
                docker rmi $DOCKER_LOGIN_NAME/$APP_NAME:$IMAGE_TAG
                '''
            }
        }
        stage('Deploy to Kubernetes'){
            steps{
                sh "kubectl set image deployment/${DEPLOYMENT_NAME} ${CONTAINER_NAME}=${DOCKER_LOGIN_NAME}/${APP_NAME}:${IMAGE_TAG} --namespace=${K8S_NAMESPACE}"
            }
        }
    }
}
