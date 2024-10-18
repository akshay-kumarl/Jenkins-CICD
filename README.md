# CI CD Repository Notes

<img width="749" alt="Screenshot 2024-10-18 at 12 28 11 PM" src="https://github.com/user-attachments/assets/cd393962-a5df-4d7f-a8e2-ec238f917563">


## Jenkins

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


---

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
          echo 'Deploying....'
    }
    post {
        always {
            echo 'Pipeline completed'
        }
    }
}
```

