#### About
a simple helloworld written in Golang

#### How to run
```docker-compose up```

#### Routes
- Health check: http://localhost:8010/health/check
- Version: http://localhost:8010/v
- Applicative route: http://localhost:8010/

#### Goal
1. Setup CI Using Jenkins pipeline that build a new Docker image and push it to a Docker repository of your choice.
The CI need to be triggered upon any change to the master branch.
2. Setup CD Using Jenkins that will deploy the newly built image to a kubernetes.

#### Instructions
1. Fork this repo and work on your own copy.
2. We believe in infrastructure as code, therefore your Jenkins and kubernetes config files should be embedded into your fork.
3. Provide instructions on how to run your task.
4. Follow best practices.

Good Luck!