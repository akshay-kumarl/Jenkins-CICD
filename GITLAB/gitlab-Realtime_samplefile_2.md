# GITLAB
## Realtime sample file 

```
stages: # List of stages for jobs, and their order of execution
  - install_tools
  - test
  - security
  - build
  - docker
  - deploy

install_mvn_trivy_docker_kubectl: # This job runs in the build stage, which runs first.
  stage: install_tools
  script:
    - sudo apt install openjdk-17-jre-headless -y
    - sudo apt install maven -y
    - wget -q0 - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
    - echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a / etc/apt/sources.list.d/trivy.list
    - sudo apt-get update && sudo apt-get install -y trivy
    - sudo apt install docker.io -y && sudo chmod 666 / var/run/docker.sock
    - sudo snap install kubectl --classic
  tags:
    - self-hosted

unit_testing: # This job runs in the build stage, which runs first.
  stage: test
  script:
    - mvn test
  tags:
    - self-hosted

trivy_fs_scan: # This job runs in the build stage, which runs first.
  stage: security
  script:
    - trivy fs --format table -o fs.html .
  tags:
    - self-hosted

sonarqube-check:
  stage: security
  image:
    name: sonarsource/sonar-scanner-cli:latest
  variables:
    SONAR_USER_HOME: "${CI_PROJECT_DIR}/.sonar" # Defines the location of the analysis task cache
    GIT_DEPTH: "0" # Tells git to fetch all the branches of the project, required by the analysis task
  cache:
    key: "$(CI_JOB_NAME}"
    paths:
      - . sonar/cache
  script:
    - sonar-scanner
  allow_failure: true
  only:
    - main

build_app:
  stage: build
  script:
    - mvn package
  tags:
    - self-hosted
  only:
    - main

build_and_tag_push_image:
  stage: docker # This job runs in the build stage, which runs first.
  script:
    - docker login -u $DOCKER_USERNAME -P $DOCKER_PASSWORD
    - mvn package
    - docker build -t adijaiswal/boardgitlab:latest .
    - docker push adijaiswal/boardgitlab:latest
  tags:
    - self-hosted
  only:
    - main

k8s-deploy:
  stage: deploy
  variables:
    KUBECONFIG_PATH: /home/ubuntu/. kube/config
  before_script:
    - mkdir -p $ (dirname "$KUBECONFIG PATH")
    - echo "$KUBECONFIG CONTENT" | base64 -d > "$KUBECONFIG PATH"
    - export KUBECONFIG="$KUBECONFIG_PATH"
  script:
    - kubectl apply -f deployment-service. yaml
  tags:
    - self-hosted
  only:
    - main

```
