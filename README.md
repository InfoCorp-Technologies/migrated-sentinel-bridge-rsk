![Sentinel Chain](https://cryptoindex.co/coinlogo/sentinel-chain.png "Sentinel Chain")

# Sentinel Bridge

### Watcher
A watcher listens for a certain event and creates proper jobs in the queue. These jobs contain the transaction data (without the nonce) and the transaction hash for the related event. The watcher runs on a given frequency, keeping track of the last processed block.

There are three Watchers:
- Signature Request Watcher: Listens to `UserRequestForSignature` events on Home network.
- Collected Signatures Watcher: Listens to `CollectedSignatures` events on Home network.
- Affirmation Request Watcher: Listens to `UserRequestForAffirmation` events on Foreign network.

### Sender
A sender subscribes to the queue and keeps track of the nonce. It takes jobs from the queue, extract transaction data, adds proper nonce and sends it to the network.

There are two Senders:
- Home Sender: Sends transaction to Home network.
- Foreign Sender: Sends transaction to Foreign network.

# How to use

1. Deploy bridge contracts
    1. Clone repo: `$ git clone git@github.com:InfoCorp-Technologies/sentinel-bridge-rsk.git`
    2. `$ cd sentinel-bridge-rsk`
    3. Go to contract folder: `$ cd contracts`
    4. Compile contracts: `$ truffle compile`
    5. Go to deploy folder: `$ cd deploy`
    6. create a `.env` file: `$ cp .env.example .env` (look at `.env.example` to see the variables that need to be present)
    7. Execute `$ node deploy.js`

2. Install [RabbitMQ](https://www.rabbitmq.com/) and [Redis](https://redis.io/)
  - RabbitMQ version: `3.7`
  - Redis version: `4.0`

3. Edit the `.env` file and fill in the information using the output data from previous deploy step. Check the `.env.example` file to see the required variables.

## Run the processes

### Native to Erc mode
  - `$ npm run watcher:signature-request`
  - `$ npm run watcher:collected-signatures`
  - `$ npm run watcher:affirmation-request`
  - `$ npm run sender:home`
  - `$ npm run sender:foreign`

To send deposits to home contract run `$ node tests/sendUserTxToHome.js`

To send withdrawals to foreign contract run `$ node tests/sendUserTxToForeign.js`

Make sure your `HOME_MIN_AMOUNT_PER_TX` and `FOREIGN_MIN_AMOUNT_PER_TX` is same as in your .env deployment contract

### Erc to Erc mode

On `.env` file set `BRIDGE_MODE=ERC_TO_ERC`

  - `$ npm run watcher:signature-request`
  - `$ npm run watcher:collected-signatures`
  - `$ npm run watcher:affirmation-request`
  - `$ npm run sender:home`
  - `$ npm run sender:foreign`

To send deposits to home contract run `$ node tests/sendUserTxToErcHome.js`

To send withdrawals to foreign contract run `$ node tests/sendUserTxToErcForeign.js`

### Useful commands for development

#### RabbitMQ
Command | Description
--- | ---
`rabbitmqctl list_queues` | List all queues
`rabbitmqctl purge_queue home` | Remove all messages from `home` queue

#### Redis
Use `redis-cli`

Command | Description
--- | ---
`KEYS *` | Returns all keys
`SET signature-request:lastProcessedBlock 1234` | Set key to hold the string value.
`GET signature-request:lastProcessedBlock` | Get the value of key.
`DEL signature-request:lastProcessedBlock` | Removes the specified key.
`FLUSHALL` | Delete all the keys of all the existing databases


### Env Variables
Variable | Description | Values
--- | --- | ---
`ALLOW_HTTP` | Explicitly allows the usage of `http` connections instead of `https`. | `yes` / `no`
