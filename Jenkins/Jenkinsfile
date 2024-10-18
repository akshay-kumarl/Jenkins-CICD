pipeline {
    agent any
    tools {
        terraform 'terraform_1.9.6'  // Name from the Global Tool Configuration
    }
    stages {
        stage('Checkout') {
            steps {                
                // Checkout the repository using provided credentials
                git credentialsId: '02aa204b-1d28-4f7b-9aa1-14a71853f2b3', url: 'https://github.com/akshay-kumarl/sample_tf_project.git', branch: 'mainbranch'
            }
        }
        
        stage('terraform initialize'){
            steps{
            sh 'terraform init'
          }
        }
        
        stage('tf plan'){
          steps{
            sh 'terraform plan -out=tfplan'
          }
        }
        
        stage('run instance'){
          steps {
               echo "Terraform action is --> ${action}"
                sh ('terraform ${action} --auto-approve') 
          }
        }
        
    }
}
