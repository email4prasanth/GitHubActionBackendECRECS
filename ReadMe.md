> [!NOTE]
> - Set up a complete CI/CD pipeline using GitHub Actions, which includes:
>   - Building and testing your backend
>   - Creating a Docker image from the Dockerfile
>   - Pushing the Docker image to Amazon ECR (Elastic Container Registry)
>   - Deploying the application in AWS using ECS (Elastic Container Service) with task definitions
> - Finally, expose the backend to the public via a URL endpoint.
### Stage-1 
#### Automate the Build and Testing of Your Backend Code Using GitHub Actions
- [Related viedo](https://www.youtube.com/watch?v=89ymSXkGwhE)
- `https://github.com/actions` to know the uses syntax
- Create `.github/workflows/deploy.yml` file in your Git repository for GitHub Actions. This file will define your CI/CD pipeline.
- push and pull when the branch is prasanth.
- jobs with build-and-test that runs-on: ubuntu-latest
- Create `backend` folder
```
cd backend
npm init (click enter for all)
```
- This will create `package.json` file and 
- `npm install moment nodemon express` will create node_modules,package-lock.json file under backend.
- we need to create index.js since while builing package.json` we accepted "main":"index.js"
- create a `.gitignore` file add `node_modules/, package-lock.json`.
- If you run the script npm run start it will through an error, to resolve this open package.json under scripts add "start":"nodemon index.js" instead of test. 
##### local check
- use any one `node index.js` or `npm run start` if it is working fine
##### GithubActions check
- Edit `deploy.yml` file with basic things
    1. correct path under push `backend/**` since index.
    2. check node version.
    3. Install dependencies that are used in the above 
        - `npm init`, since `package.json` is available in git repo. 
        - we need `node_modules/, package-lock.json` so `npm install moment nodemon`
    4. add correct path to run javascript `node backend/index.js` instead of `node index.js`.
> [!IMPORTANT]
> - Whether i need to create a Github Action workflow
> - what brach you are looking for DEV or else any name
> - what type of runs on is required `self-hosted` or `github-hosted`, if `self-hosted` we need to `settings> Rightside code and automation > Actions > Runners` select `runner image, archi, download, config`.
> - wheter i need to create folder Backend folder to keep all the files in one place. 
### Stage-2
#### Build Docker Image and Push to ECR
- To push Image to ECR we need `AccountId, Region`, 
- Create an IAM user `githubaction-ecr` that allows `AmazonEC2ContainerRegistryFullAccess, AmazonECS_FullAccess` generate Accesskeyid and secret
- Open **settings > security > Secrets and variable > Acitons > Secrets > New Repo secret** 
    1. AWS_ACCESS_KEY_ID
    2. AWS_SECRET_ACCESS_KEY
    3. AWS_ACCOUNT_ID - 180294218712
    4. AWS_REGION - us-east-1
- Login to ECR if it exits skip else create `my-backend-app` 
- Create `Dockerfile, .dockerignore` files and generate Time stamp to give the tag name.
- Build Docker image and tag it and push it.
> [!IMPORTANT]
> - What is account id we have use and whether we need to use `env or secret`
> - Naming convention of ECR repo
> - Docker file image is okay or need to use docker multi stage build.
> - Naming convention of tag.
> - Check the dockerfile and paths
### Stage-3
#### Deploy the Docker Container in AWS ECS
- Create ECS service, cluster, task definition.
-  Create ECS role as `github-ecs-role` with  `AWS service > Elastic Container Service > Elastic Container Service Task`, Attach Permissions `AmazonECSTaskExecutionRolePolicy, ECRFullAccess` open trust relationship make sure that "Service": "ecs-tasks.amazonaws.com" in json format.
- Create Task definition `my-task-definition > aws fargate > OS > github-ecs-role > container my-ecs-container > copy my-backend-app uri, port map -80 > use log collectionn (if required)`.
- Generate a **task-definiton.json** use vscode under path `H:\2_GitHub_Actions\GitHubActions\Gitbud_BE_ECR_ECS`.
```
aws ecs describe-task-definition --task-definition my-task-definition --query taskDefinition > task-definition.json
```
- Create cluster name `my-github-ecs-cluster` infra `AWS Fargate serverless` and create. 



- Go to ALB add tg and create 



- Now select `Service > create, compute configuration select lauchtype, version latest. Deployment config > service > family **my-task-definition** and service name is my-ecs-service`,  desired task 1, deployment type Rollingupdate, min-100, max-200, Deployment failure detection use amazon ECS, **Service discovery**  uses Amazon Route 53 to create a namespace for your service, which allows it to be discoverable via DNS, selected vpc k8s and public subnet -1a 10.20.0.0/20,1b 10.20.16.0/20 allow all traffic, create
- will see later select ALB, my-ecs-container80:80, and autoscaling.
- Set env variable with ECS_SERVICE, ECS_CLUSTER, ECS_TASKDEFINITION

