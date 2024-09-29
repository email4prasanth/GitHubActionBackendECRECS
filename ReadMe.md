> [!NOTE]
> - Set up a complete CI/CD pipeline using GitHub Actions, which includes:
>   - Building and testing your backend
>   - Creating a Docker image from the Dockerfile
>   - Pushing the Docker image to Amazon ECR (Elastic Container Registry)
>   - Deploying the application in AWS using ECS (Elastic Container Service) with task definitions
> - Finally, expose the backend to the public via a URL endpoint.
###
##### Automate the Build and Testing of Your Backend Code Using GitHub Actions
- Create .github/workflows/deploy.yml file in your Git repository for GitHub Actions. This file will define your CI/CD pipeline.
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
##### To check run
- use any one `node index.js` or `npm run start`