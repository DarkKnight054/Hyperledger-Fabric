# Deploying a smart contract to a channel

<h2>N.B: Contents are taken from hyperledger fabric official documentation</h2>

### Tracking the logs (OPTIONAL)

- uses Logspout tool.
- Command - ./monitordocker.sh fabric_test
- Recommend to use it in another terminal to track the whole process while we'll deploy chaincode and smart contracts

### Package the smart contract

- We need to package the chaincode before it can be installed on our peers. The steps are different if you want to install a smart contract written in Go, JavaScript, or Typescript. Here, I'll use javascript as chaincode language.

#### Some keynotes about some libraries

- The fabric-contract-api provides the contract interface. a high level API for application developers to implement Smart Contracts. Within Hyperledger Fabric, Smart Contracts are also known as Chaincode. Working with this API provides a high level entry point to writing business logic.

- The fabric-shim provides the chaincode interface, a lower level API for implementing "Smart Contracts". It also provides the implementation to support communication with Hyperledger Fabric peers for Smart Contracts written using the fabric-contract-api together with the fabric-chaincode-node cli to launch Chaincode or Smart Contracts.

- To confirm that the fabric-shim maintains API and functional compatibility with previous versions of Hyperledger Fabric.

## Work flow of deploying a smart contract to a channel

<ol>

<li>At first, chaincode need to be packaged before it can be installed on peers</li>

<li>Install the chaincode package on different peers</li>

<li>Approve chaincode definition from majority of the org in the network.

<h4>Remember</h4>

- <h5>majority of the org need to approve chaincode definition</h5>

- <h5>Orgs those didn't approve the chaincode can't endorse a transaction through this chaincode</h5>

- <h5>approval must be committed by admin of that org not client user</h5>
  </li>

<li>Committing the chaincode definition to the channel</li>

<li>Invoking the chaincode <b>(Note that the invoke command needs to target a sufficient number of peers to meet the chaincode endorsement policy.)</b></li>

<li><b>(Optional)</b> Chaincode can be upgraded too.</li>

</ol>
