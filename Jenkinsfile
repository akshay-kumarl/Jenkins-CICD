pipeline {
    agent any


    stages {
        stage('Checkout') {
            steps {                
                // Checkout the repository using provided credentials
                git credentialsId: 'a577a309-0446-48fa-a031-372abed13568', url: 'https://github.com/akshay9700/carwebsite.git', branch: 'main'
            }
        }
        stage ('build'){
            steps {
                echo 'build....'
            }
        }
        stage('Deploy'){
            steps {
                echo 'Deploying....'
            }
       }
    }
}
