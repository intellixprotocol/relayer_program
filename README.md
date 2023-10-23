# Relayer

## Deploy with docker-compose

docker-compose.yml contains a stack that will automatically provision SSL certificates for your domain name and will add a https redirect to port 7002.

1. Clone Project

2. `cp .env.example .env `

3. `cp abi.example.json  abi.json `

4. Setup environment variables

   - set `PRIVATE_KEY` your private key
   - set `PUBLIC_KEY` your public key
   - set `RPC_URL` your public key
   - set `NETWORKS` network list . example BSC,ETH,Matic
   - set `PORT_LOCAL` local port
   - set `URL` server url
   - set `CONTRACTS` your contracts

5. Make sure you staked before . https://mixtoearn.app/

6. Make sure your docker is running

7. Run `docker-compose up -d`

## ## Run locally

1. In order to execute withdraw request, you can run following command

```bash
curl --location 'http://localhost:${your_local_port}/api/v1/withdraw' \
--header 'Content-Type: application/json' \
--data '{
   "network" : "ETH"
   "input" : "array of proof"
}'

```

## Architecture

1. TreeWatcher module keeps track of Account Tree changes and automatically caches the actual state in Redis and emits `treeUpdate` event to redis pub/sub channel
2. Server module is Express.js instance that accepts http requests
3. Controller contains handlers for the Server endpoints. It validates input data and adds a Job to Queue.
4. Queue module is used by Controller to put and get Job from queue (bull wrapper)
5. Status module contains handler to get a Job status. It's used by UI for pull updates
6. Validate contains validation logic for all endpoints
7. Worker is the main module that gets a Job from queue and processes it
