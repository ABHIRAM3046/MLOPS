pipeline{
    agent any
    stages{
        stage("Train Model"){
            steps{
                script{
                    sh '''python3 -m venv venv
                    . venv/bin/activate
                    pip install --upgrade pip
                    pip3 install -r requirements.txt
                    python3 train.py'''
                }
            }
        }
        stage("Docker Build"){
            steps{
                sh'''docker build -t abhiram3046/mlops-app:${BUILD_NUMBER} .'''
            }
        }
        stage("Docker Push"){
            steps{
                withDockerRegistry(credentialsId:'dockerhub-credentials',url:''){
                    sh'''docker push abhiram3046/mlops-app:${BUILD_NUMBER}
                    docker tag abhiram3046/mlops-app:${BUILD_NUMBER} abhiram3046/mlops-app:latest
                    docker push abhiram3046/mlops-app:latest'''
                }
            }
        }
        stage("Deploy to Kubernetes"){
            steps{
                sh '''kubectl apply -f mlops-deployment.yml --validate=false'''
            }
        }
    } 
}