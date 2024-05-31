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
```
https://media.licdn.com/dms/document/media/D561FAQEh36iLanzBew/feedshare-document-pdf-analyzed/0/1717069741799?e=1718236800&v=beta&t=RW035I_cuG5AgFrmrdrOQ1WiRjL0orf0Z3LKrnLzHyE
```
