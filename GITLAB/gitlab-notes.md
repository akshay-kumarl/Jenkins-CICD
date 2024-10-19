# Notes and samplefiles

#### gitlab adding custom URL for pages 

![gitlabcustomname](https://github.com/user-attachments/assets/d20b9072-660f-4fea-9695-78a173e00200)


### samplefile 1

```
image: alpine:latest
pages:
  script:
    - echo "Starting deployment to GitLab Pages"
    - mkdir .public
    - cp -r * .public/
    - mv .public public
  artifacts:
    paths:
      - public
  only:
    - main
```
