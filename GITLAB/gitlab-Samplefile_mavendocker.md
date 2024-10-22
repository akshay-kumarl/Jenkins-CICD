*Prerequisites:*

1. GitLab account
2. Java microservice project using Maven
3. Docker Hub account

**Step 1:** Create a new GitLab CI/CD pipeline

1. Go to your GitLab project repository.
2. Click on "CI/CD" > "Pipelines" > "New pipeline".
3. Choose "New pipeline from template" and select "Maven".
4. Name your pipeline (e.g., "Java Microservice Pipeline").

**Step 2:** Configure Maven build

1. In the .gitlab-ci.yml file, add the following stage:

```   
stages:
  - build

build:
  stage: build
  image: maven:3-jdk-11
  script:
    - mvn clean package
  artifacts:
    paths:
      - target/*.jarThis stage builds your Java microservice using Maven.
```

**Step 3:** Configure Docker build*

1. Add a new stage for Docker build:
```
docker-build:
  stage: docker-build
  image: docker:latest
  script:
    - docker build -t $DOCKER_IMAGE .
  only:
    - mainReplace $DOCKER_IMAGE with your Docker Hub image name (e.g., your-username/your-image-name).
```

**Step 4:** Configure Docker push

1. Add a new stage for Docker push:
```
docker-push:
  stage: docker-push
  image: docker:latest
  script:
    - docker tag $DOCKER_IMAGE:$CI_COMMIT_SHORT_SHA $DOCKER_IMAGE:latest
    - docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
    - docker push $DOCKER_IMAGE:latest
  only:
    - mainReplace $DOCKER_USERNAME and $DOCKER_PASSWORD with your Docker Hub credentials.
```

**Step 5:** Configure deployment

1. Add a new stage for deployment (e.g., to Kubernetes):
```
deploy:
  stage: deploy
  image: kubernetes:latest
  script:
    - kubectl apply -f deployment.yaml
  only:
    - mainReplace deployment.yaml with your Kubernetes deployment configuration file.
```

---

### Complete .gitlab-ci.yml file
```
stages:
  - build
  - docker-build
  - docker-push
  - deploy

build:
  stage: build
  image: maven:3-jdk-11
  script:
    - mvn clean package
  artifacts:
    paths:
      - target/*.jar

docker-build:
  stage: docker-build
  image: docker:latest
  script:
    - docker build -t $DOCKER_IMAGE .

docker-push:
  stage: docker-push
  image: docker:latest
  script:
    - docker tag $DOCKER_IMAGE:$CI_COMMIT_SHORT_SHA $DOCKER_IMAGE:latest
    - docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
    - docker push $DOCKER_IMAGE:latest

deploy:
  stage: deploy
  image: kubernetes:latest
  script:
    - kubectl apply -f deployment.yaml
```

VARIABLES
1. DOCKER_IMAGE: your Docker Hub image name
2. DOCKER_USERNAME: your Docker Hub username
3. DOCKER_PASSWORD: your Docker Hub password

*Save and commit changes*

Commit the updated .gitlab-ci.yml file to your GitLab repository.

*Trigger the pipeline*

1. Go to your GitLab project repository.
2. Click on "CI/CD" > "Pipelines".
3. Click on "Run pipeline" to trigger the pipeline manually.

.
