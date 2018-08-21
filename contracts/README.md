# Sentinel bridge smart contracts

The goal of these contracts is to allow users to transfer ERC20 tokens from Sentinel Chain to ERC20 token on RSK Bamboo.

Home Bridge is a smart contract that should be deployed in Sentinel Chain Network.
Foreign Bridge is a smart contract that should be deployed in RSK Bamboo.

Responsibilities and roles of the bridge:
- Administrator Role(representation of a multisig contract):
  - add/remove validators
  - set daily limits on both bridges
  - set maximum per transaction limit on both bridges
  - set minimum per transaction limit on both bridges
  - upgrade contracts in case of vulnerability
  - set minimum required signatures from validators in order to relay a user's transaction
- Validator Role :
  - provide 100% uptime to relay transactions
  - listen for Deposit events on Home bridge to unlock ERC20 tokens on Foreign Bridge
  - listen for Withdraw events on Foreign bridge to unlock ERC20 tokens on Home Bridge
- User role:
  - sends ERC20 tokens to Home bridge in order to receive ERC20 tokens on Foreign Bridge
  - sends SENC token on Foreign Bridge in order to receive ERC20 tokens on Home Bridge

# To Deploy
Read this -> [deployment](https://github.com/InfoCorp-Technologies/sentinel-bridge-rsk/blob/master/contracts/deploy/README.md)
