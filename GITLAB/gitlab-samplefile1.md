# GitLab sample file

### file for compile analysis build and deploy

```
stages:          
  - compile
  - sonarqube-analysis
  - build
  - deploy

compile-job:      
  stage: compile
  tags:
    - "self-hosted"
  script:
    - echo "Compiling the code..."
    - mvn clean compile

sonarqube-job:  
  stage: sonarqube-analysis    
  script:
    - echo "Running Sonarqube Analysis"
    - mvn sonar:sonar -Dsonar.host.url=http://3.7.66.224:9000/ -Dsonar.login=squ_a7a0e1c0eb741e8feb9d77c2ee6d6c84fd826764 -Dsonar.projectKey=Petclinic -Dsonar.projectName=Petclinic -Dsonar.java.binaries=.

build-job:   
  stage: build
  tags:
    - "self-hosted"    
  script:
    - echo "Maven Build"
    - mvn clean package

deploy-job:      
  stage: deploy
  tags:
    - "self-hosted"  
  script:
    - echo "Deploying application..."
    - docker run -p 8080:8080 springcommunity/spring-framework-petclinic

```
