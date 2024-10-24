## Gitlab sample file to run static web host application 


```
image: alpine:latest
pages:
  script:
    - echo "Starting deployment to GitLab Pages"
    - mkdir public
    - cp -r * public/
    # - mv .public public
  artifacts:
    paths:
      - public
  only:
    - main

```
