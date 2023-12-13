## Env
Use https://github.com/sweeneyb/devEnvs to get an ubuntu GCP box running w/ old-ish docker.
Follow https://docs.docker.com/compose/install/linux/#install-using-the-repository to install the ubuntu official repo and update everything so that `docker compose` works and uses the v2 compose CLI/API.

## Zeebe, Operate and Tasklist – to automate

Download an example configuration (YAML file) to create and start multiple components of Camunda 8 via Docker Compose. This includes Operate, Zeebe, Tasklist, Connectors, as well as Elasticsearch, which is used for data storage.

Step 1: [Download or clone this repo.](https://github.com/camunda/camunda-platform)

Step 2: Go to the directory where you saved the file.

Step 3: Run

```javascript
docker compose -f docker-compose-core.yaml up
```

Copy

Step 4: You’re done! Call Zeebe, launch Operate or launch Tasklist
Call Zeebe engine via localhost:26500
Operate http://localhost:8081/
Tasklist: http://localhost:8082/

User/Password: demo/demo.


## Gain access
ssh -i id_rsa -L 8081:localhost:8081 -L 26500:localhost:26500 -L 8082:localhost:8082 user@<terraform output ip>