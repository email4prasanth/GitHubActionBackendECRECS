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
- `npm install moment nodemon` will create node_modules,package-lock.json file under backend.
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
- Create an IAM user `githubaction-ecr` that allows `AmazonEC2ContainerRegistryFullAccess` generate Accesskeyid and secret
- Open **settings > security > Secrets and variable > Acitons > Secrets > New Repo secret** 
    1. AWS_ACCESS_KEY_ID
    2. AWS_SECRET_ACCESS_KEY
    3. AWS_ACCOUNT_ID - 180294218712
    4. AWS_REGION - us-east-1
