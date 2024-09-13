# Jenkins sample repo

### Declarative Pipeline 
sample declarative file

```
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Deploy') {
            steps {
                sh 'mvn deploy'
            }
        }
    }
    post {
        always {
            echo 'Pipeline completed'
        }
    }
}

```

### Scripted Pipeline
sample scripted pipeline

```
node {
    stage('Build') {
        sh 'mvn clean install'
    }
    stage('Test') {
        parallel(
            'Unit Tests': {
                sh 'mvn test'
            },
            'Integration Tests': {
                sh 'mvn integration-test'
            }
        )
    }
    stage('Deploy') {
        sh 'mvn deploy'
    }
    post {
        always {
            echo 'Pipeline completed'
        }
    }
}

```
---

