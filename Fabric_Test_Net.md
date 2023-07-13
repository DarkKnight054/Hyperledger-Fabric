# Hyperledger Fabric Test Network

<h2>N.B: Contents in this repository have been gained or taken from hyperledger fabric official documentation </h2>

### Configuaration of Test-Net

- It includes two peer organizations and an ordering organization.
- For simplicity, a single node Raft ordering service is configured.
- To reduce complexity, a TLS Certificate Authority (CA) is not deployed. All certificates are issued by the root CAs.
- The sample network deploys a Fabric network with <b>Docker Compose. Because the nodes are isolated within a Docker Compose network, the test network is not configured to connect to other running Fabric nodes.</b>

### Bring up the test network

Script - network.sh , PWD - fabric-samples/test-network

- Up the test net using the command <span style="color: green">./network.sh up</span>
- Down the test net using the command <span style="color: red">./network.sh down</span>

<b style="color: orange">Key Notes</b>

- The <span style="color: green">network.sh</span> script creates all of the cryptographic material that is required to deploy and operate the network before it creates the peer and ordering nodes.

- By default, the script uses the cryptogen tool to create the certificates and keys. The tool is provided for development and testing.

- For Production, its better to bring up the network using CA(Certificate authority)

### Create a Channel

command - <span style="color: green">./network.sh createChannel -c <Channel_Name> </span>

- Replace <Channel_Name> with the channel name.
- Channel Name contains only lower case ASCII alphanumerics, dots ‘.’, and dashes ‘-‘
- Channel Name is shorter than 250 characters
- Channel Name starts with a letter

### Start a ChainCode on the channel

command - <span style="color: green">./network.sh deployCC -c <Channel_Name> -ccn <Chaincode_Name> -ccp <Chaincode_Path> -ccl <Chaincode_Language> </span>

### Interacting with the network

- <h3>Make sure you're in fabric-samples/test-network directory</h3>
  <h3>Run these command for running the CLI as peer of org1</h3>

- export PATH=${PWD}/../bin:$PATH **(Redirecting to binaries folder of test-network)**
- export FABRIC_CFG_PATH=$PWD/../config/ **(Use for configuring peer of org)**

#### Make Peer CLI for org1

<h4>Environment Variables for org1</h4>

- export CORE_PEER_TLS_ENABLED=true
- export CORE_PEER_LOCALMSPID="Org1MSP"
- export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
- export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
- export CORE_PEER_ADDRESS=localhost:7051

#### Make Peer CLI for org2

<h4>Environment Variables for org2</h4>

- export CORE_PEER_TLS_ENABLED=true
- export CORE_PEER_LOCALMSPID="Org2MSP"
- export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
- export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
- export CORE_PEER_ADDRESS=localhost:9051

### Initialize ledger

<h4>Here, peer0 of org1 initialize the ledger with initledger function decribed in <span style="color: yellow">asset-transfer-basic/chaincode/javasrcipt/lib/assetTransfer.js</span> file</h4>

**Initialize Chaincode**

- peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile "${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem" -C <Channel_Name> -n <Chaincode_Name> --peerAddresses localhost:7051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt" --peerAddresses localhost:9051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt" -c '{"function":"InitLedger","Args":[]}'

**Get all the asset**

- peer chaincode query -C <Channel_Name> -n <Chaincode_Name> -c '{"Args":["GetAllAssets"]}'

**Transfer Ownership of an asset**

- peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile "${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem" -C <Channel_Name> -n <Chaincode_Name> --peerAddresses localhost:7051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt" --peerAddresses localhost:9051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt" -c '{"function":"TransferAsset","Args":["asset6","Christopher"]}'

**Read Data of an specific asset**

- peer chaincode query -C <Channel_Name> -n <Chaincode_Name> -c '{"Args":["ReadAsset","asset6"]}'
