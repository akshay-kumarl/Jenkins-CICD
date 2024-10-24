# Gitlab sample file to run static web host application 

#### gitlab adding custom URL for pages 

![gitlabcustomname](https://github.com/user-attachments/assets/d20b9072-660f-4fea-9695-78a173e00200)

---

### samplefile 1

```
image: alpine:latest
pages:
  script:
    - echo "Starting deployment to GitLab Pages"
    - mkdir .public           or mkdir public
    - cp -r * .public/        or cp -r css images js about-us.html article.html articles.html contact-us.html index.html sitemap.html
    - mv .public public       
  artifacts:
    paths:
      - public
  only:
    - main
```
---

### Gitlab sample file


```
image: alpine:latest
pages:
  script:
    - echo "Starting deployment to GitLab Pages"
    - mkdir public
    - cp -r css images js about-us.html article.html articles.html contact-us.html index.html sitemap.html
  artifacts:
    paths:
      - public
  only:
    - main

```
