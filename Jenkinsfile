pipeline {
agent any

```
environment {
    ACR = "esraadevopsacr.azurecr.io"
    IMAGE = "flask-app"
    DEPLOYMENT = "flask-app"
    CONTAINER = "flask-app"
}

stages {

    stage('Checkout Code') {
        steps {
            checkout scm
        }
    }

    stage('Build Docker Image') {
        steps {
            sh """
            docker build -t $ACR/$IMAGE:${BUILD_NUMBER} .
            """
        }
    }

    stage('Login to ACR') {
        steps {
            sh """
            echo $ACR_PASSWORD | docker login $ACR -u $ACR_USERNAME --password-stdin
            """
        }
    }

    stage('Push Image to ACR') {
        steps {
            sh """
            docker push $ACR/$IMAGE:${BUILD_NUMBER}
            """
        }
    }

    stage('Deploy to AKS') {
        steps {
            sh """
            kubectl set image deployment/$DEPLOYMENT \
            $CONTAINER=$ACR/$IMAGE:${BUILD_NUMBER}
            """
        }
    }

    stage('Verify Deployment') {
        steps {
            sh """
            kubectl rollout status deployment/$DEPLOYMENT
            kubectl get pods
            """
        }
    }
}
```

}

