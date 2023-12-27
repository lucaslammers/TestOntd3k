pipeline {
    agent any

    environment {
        // Set your Kubernetes cluster credentials
        KUBE_CONFIG = credentials('c7bc4fb6-be60-4879-86aa-b1362598deeb')
        KUBE_NAMESPACE = 'jenkins'
    }

    stages {
      stage('Checkout') {
            steps {
                // Assuming you have your code in a Git repository
                checkout scm
            }
        }
        stage('Build and Deploy') {
            steps {
                script {
                    // Define the pod YAML configuration
                    def podConfig = """
                    apiVersion: v1
                    kind: Pod
                    metadata:
                      name: nginx-pod123
                      labels:
                        app: nginx
                    spec:
                      containers:
                      - name: nginx-container
                        image: nginx:latest
                        ports:
                        - containerPort: 80"""
                    
                    // Save the pod configuration to a file
                    writeFile file: 'nginx-pod.yaml', text: podConfig

                    // Use kubectl to apply the pod configuration
                    sh('kubectl --kubeconfig=${KUBE_CONFIG} apply -f nginx-pod.yaml -n ${KUBE_NAMESPACE}')
                }
            }
        }
    }
}
