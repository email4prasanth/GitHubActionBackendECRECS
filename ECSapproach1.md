- [Create Task Definition, Cluster and Service in Amazon ECS | Using Load Balancer in ECS & Validation](https://www.youtube.com/watch?v=MpumO3TP8VY)
#### ALB Target group
- ec2 > Load Balancing > TG > Create TG > basic config on IP Instead of instances and name as `github-alb-tg` > protocol HTTP PORT 3000 >Ipv4 >VPC >`HTTP1` > Health checks click on advance `Healthy threshold 2`s, next Check port of selected instance create target group with 10.20.0.0/20,10.20.16.0/20,10.20.32.0/20 PORT 3000.
#### ALB
- ec2 > Load Balancing > Load Balancers > ALB name `ecs-alb`  with  internet facting, ipv4 > Network mapping > VPC `k8s-vpc` atleast 3 **public** subnets, choose `ecs-sg` inbound `22, 80,3000`, listener and routing 80 select `github-alb-tg` create.
#### Task Definition
- Create Task execution role Go to IAM > Roles > create role > AWS service > Elastic Container Service > Elastic Container Service Task > Attach Permissions AmazonECSTaskExecutionRolePolicy >create role.
- Task Definition > create > Task definition configuration `ecr-task-def` > Infrastructure requirements AWS Fargate, 1CPU, 3 GB, Task execution role ECSTaskExecutionRole > Container - 1 > Name `ecs-container1`, Image URI, continer port 80, create.
#### ECS Cluster
- ECS cluster > Cluster name `ecs-cluster` > infra `AWS Fargate serverless` and create.
#### ECS service
- Now select Service > create, compute configuration select capacity provider strategy, capacity provider Use custom (Advanced)`FARGATE` version latest.  Deployment config > Application type > service > family **ecr-task-def** and service name is `ecs-service`,  desired task 2`, `deployment type Rollingupdate, min-100, max-200, Networking selected vpc k8s and public subnet -1a,1b,1c, mew sg ecs-sg http and source anywhere, expand load balancer choose ALB select `github-alb`, my-ecs-container3000:3000, listner use existing listner  is 80, create a new target group `ecs-service-tg`, delay `300` path pattern `/ecs-service*` and evaluation order is 1, Health check protocol `HTTP`, Health check path `/` create.