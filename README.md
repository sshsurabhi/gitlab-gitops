# GITLAB PROJECT including GITOPS

# Refer my [Gitlab Project](https://gitlab.com/SaiSriHarsha/gitlab-devops) repository for other reference

# Microservices, API Gateway, Authentification with FastAPI (K8S HELM CHARTS)

- This repository is composed of a set of small microservices taking into account the API gateway approach.
- The planned number of microservices was two, but given that the services must not create dependencies on each other to prevent SPOF, also to avoid duplicate code, I decided to put an api gateway in front that does JWT authentication for the two services I'm inspired by Netflix/Zuul
- We have 3 services including a gateway.
- Only the gateway can access internal microservices via the internal network (users, commands).

## Services

- gateway: built on top of FastAPI, a simple api gateway whose sole task is to provide clean
   routing while managing authentication and authorization
- users (a.k.a admin): stores user information in its own fake database (file system).
   Can perform simple CRUD operations via the service. There is also another
   endpoint for the connection, but the client is extracted from the actual response. Thus, the gateway service
   will manage the connection response and generate the jwt token accordingly.
- commands: Users (subscribers - authentication) can create and view (their - authorizations) commands.

## Execution
- check ./gateway/.env => 2 service URLs are defined based on the conf of twelve factors
- docker-composer --build
- visit address => http://localhost:8001/docs

# Example queries
- 2 users already exist in the user database
- get an api token with the administrator user
  ```
  curl --header “Content-Type: application/json” \
       --request POST
       --data '{“username”: “admin”, “password”: “a”}' \
       http://localhost:8001/api/login
  ```
- You'll see something similar to the following
  ```
  {“access_token”:“***”,“token_type”:“bearer”}
  ```
- use this token to make administrative requests
  ```
  curl --header “Content-Type: application/json” \
       --header “Authorization: Bearer ***” \
       --request GET
       http://localhost:8001/api/users
  ```
- Similar tests can also be performed with the default user to create and display the commands


##Diagram
![ScreenShot](https://github.com/sshsurabhi/gitlab-gitops/blob/master/diagram.png)

![ScreenShot](https://github.com/sshsurabhi/gitlab-gitops/blob/master/images-implementation/gitlab-1.png)

![ScreenShot](https://github.com/sshsurabhi/gitlab-gitops/blob/master/images-implementation/gitlab-2.png)

![ScreenShot](https://github.com/sshsurabhi/gitlab-gitops/blob/master/images-implementation/gitlab-5.png)

![ScreenShot](https://github.com/sshsurabhi/gitlab-gitops/blob/master/images-implementation/gitlab-8.png)

![ScreenShot](https://github.com/sshsurabhi/gitlab-gitops/blob/master/images-implementation/gitlab-7.png)

##Documentation Page
![ScreenShot](https://github.com/sshsurabhi/gitlab-gitops/blob/master/images-implementation/gitlab-9.png)
