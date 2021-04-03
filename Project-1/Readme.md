# Steps to deploy project : 
The steps below based on commercial paper tutorial from <a href=https://hyperledger-fabric.readthedocs.io/en/release-2.3/tutorial/commercial_paper.html>HyperLeger Fabric documentation</a> : 

* copy project p1 directory to /../fabric-samples/commercial-paper/organization/magnetocorp
* copy project p1 directory to /../fabric-samples/commercial-paper/organization/digibank

# Bring up the network : 
1] run following command to start the network
```
cd /../fabric-samples/commercial-paper/
./network-starter.sh
```

You will see the below result: 
```
thinny81@hyperledgertest1002:/app/fabric-samples/commercial-paper$ docker ps

CONTAINER ID   IMAGE                               COMMAND                  CREATED              STATUS              PORTS                                            NAMES
0302beae2963   hyperledger/fabric-tools:latest     "/bin/bash"              About a minute ago   Up 57 seconds                                                        cli
e980f62aab76   hyperledger/fabric-peer:latest      "peer node start"        About a minute ago   Up About a minute   0.0.0.0:7051->7051/tcp                           peer0.org1.example.com
81cc1bf87329   hyperledger/fabric-peer:latest      "peer node start"        About a minute ago   Up About a minute   7051/tcp, 0.0.0.0:9051->9051/tcp                 peer0.org2.example.com
4b73d0160e42   hyperledger/fabric-orderer:latest   "orderer"                About a minute ago   Up About a minute   0.0.0.0:7050->7050/tcp, 0.0.0.0:7053->7053/tcp   orderer.example.com
195e06e35fa5   couchdb:3.1.1                       "tini -- /docker-ent…"   About a minute ago   Up About a minute   4369/tcp, 9100/tcp, 0.0.0.0:5984->5984/tcp       couchdb0
4f7a6632569a   couchdb:3.1.1                       "tini -- /docker-ent…"   About a minute ago   Up About a minute   4369/tcp, 9100/tcp, 0.0.0.0:7984->5984/tcp       couchdb1
c8231178d441   hyperledger/fabric-ca:latest        "sh -c 'fabric-ca-se…"   About a minute ago   Up About a minute   0.0.0.0:7054->7054/tcp                           ca_org1
544e50be259f   hyperledger/fabric-ca:latest        "sh -c 'fabric-ca-se…"   About a minute ago   Up About a minute   7054/tcp, 0.0.0.0:9054->9054/tcp                 ca_orderer
117af4bb5b50   hyperledger/fabric-ca:latest        "sh -c 'fabric-ca-se…"   About a minute ago   Up About a minute   7054/tcp, 0.0.0.0:8054->8054/tcp                 ca_org2
```

## Bring up logspout tool to monitor network : 
This will monitor on default network "fabric_test" which was created in previous step
```
cd /../fabric-samples/commercial-paper/organization/magnetocorp
./configuration/cli/monitordocker.sh 
```

## Install and approve ProductContract chaincode as MagnetoCorp
* Set env variables
```
cd /../fabric-samples/commercial-paper/organization/magnetocorp
source magnetocorp.sh
```

* Tthis will create a p1.tar.gz
```
peer lifecycle chaincode package p1.tar.gz --lang node --path ./p1 --label cp_0  
```

* Please take note cp_0:xxxxxx 
```
Command: 
peer lifecycle chaincode install p1.tar.gz

Result:
2021-04-03 00:01:31.317 UTC [cli.lifecycle.chaincode] submitInstallProposal -> INFO 001 Installed remotely: response:<status:200 payload:"\nEcp_0:1f8b85af35cbfdef94bd978f7a1256344b1ba11b119d8527ffeb25ad45b33d00\022\004cp_0" > 
2021-04-03 00:01:31.317 UTC [cli.lifecycle.chaincode] submitInstallProposal -> INFO 002 Chaincode code package identifier: cp_0:1f8b85af35cbfdef94bd978f7a1256344b1ba11b119d8527ffeb25ad45b33d00
```

* We can query the installed chaincode with below command
```
Command: 
peer lifecycle chaincode queryinstalled

Result:
Installed chaincodes on peer:
Package ID: cp_0:1f8b85af35cbfdef94bd978f7a1256344b1ba11b119d8527ffeb25ad45b33d00, Label: cp_0
```
* Approve Chaincode for MagnetoCorp using - 
```
Command: 
export PACKAGE_ID=cp_0:1f8b85af35cbfdef94bd978f7a1256344b1ba11b119d8527ffeb25ad45b33d00

peer lifecycle chaincode approveformyorg --orderer localhost:7050 --ordererTLSHostnameOverride orderer.example.com --channelID mychannel --name productcontract -v 0 --package-id $PACKAGE_ID --sequence 1 --tls --cafile $ORDERER_CA

Result: 
2021-04-03 00:15:12.778 UTC [chaincodeCmd] ClientWait -> INFO 001 txid [9d32ad35e34336359807166b6ec6ce80a24f4627e2570076de0b95bab37e0663] committed with status (VALID) at localhost:9051

```

# Install and approve the smart contract as DigiBank
* Set env variables
```
cd /.../fabric-samples/commercial-paper/organization/digibank
source digibank.sh
```

* Tthis will create a p1.tar.gz
```
peer lifecycle chaincode package p1.tar.gz --lang node --path ./p1 --label cp_0  
```

* Please take note cp_0:xxxxxx 
```
Command: 
peer lifecycle chaincode install p1.tar.gz

Result:
2021-04-03 00:01:31.317 UTC [cli.lifecycle.chaincode] submitInstallProposal -> INFO 001 Installed remotely: response:<status:200 payload:"\nEcp_0:1f8b85af35cbfdef94bd978f7a1256344b1ba11b119d8527ffeb25ad45b33d00\022\004cp_0" > 
2021-04-03 00:01:31.317 UTC [cli.lifecycle.chaincode] submitInstallProposal -> INFO 002 Chaincode code package identifier: cp_0:1f8b85af35cbfdef94bd978f7a1256344b1ba11b119d8527ffeb25ad45b33d00
```

* We can query the installed chaincode with below command
```
Command: 
peer lifecycle chaincode queryinstalled

Result:
Installed chaincodes on peer:
Package ID: cp_0:1f8b85af35cbfdef94bd978f7a1256344b1ba11b119d8527ffeb25ad45b33d00, Label: cp_0
```
* Approve Chaincode for DigiBank using - 
```
Command: 
export PACKAGE_ID=cp_0:1f8b85af35cbfdef94bd978f7a1256344b1ba11b119d8527ffeb25ad45b33d00


peer lifecycle chaincode approveformyorg --orderer localhost:7050 --ordererTLSHostnameOverride orderer.example.com --channelID mychannel --name productcontract -v 0 --package-id $PACKAGE_ID --sequence 1 --tls --cafile $ORDERER_CA

Result: 
2021-04-03 00:27:52.605 UTC [chaincodeCmd] ClientWait -> INFO 001 txid [b28f334ba0fda1939b3f35623dec1bbb9176e3035bc80a51dd1cf4a6bc1a4d01] committed with status (VALID) at localhost:7051

```
# Commit the chaincode definition to the channel
Use <code> peer lifecycle chaincode commit </code> command to commit chaincode definition of <code>productcontract</code> to <code>my channel</code>
```
Command:
peer lifecycle chaincode commit -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --peerAddresses localhost:7051 --tlsRootCertFiles ${PEER0_ORG1_CA} --peerAddresses localhost:9051 --tlsRootCertFiles ${PEER0_ORG2_CA} --channelID mychannel --name productcontract -v 0 --sequence 1 --tls --cafile $ORDERER_CA --waitForEvent

Result:
2021-04-03 00:27:52.605 UTC [chaincodeCmd] ClientWait -> INFO 001 txid [b28f334ba0fda1939b3f35623dec1bbb9176e3035bc80a51dd1cf4a6bc1a4d01] committed with status (VALID) at localhost:7051

```
This should create 2 more docker container : 
```
CONTAINER ID   IMAGE                                                                                                                                                               COMMAND                  CREATED          STATUS          PORTS                                            NAMES
a50f05219962   dev-peer0.org1.example.com-cp_0-1f8b85af35cbfdef94bd978f7a1256344b1ba11b119d8527ffeb25ad45b33d00-0b6b785465b2e5be79be35cb18e600faacec24eaefae5da5c0b88854698bb7da   "docker-entrypoint.s…"   15 seconds ago   Up 9 seconds                                                     dev-peer0.org1.example.com-cp_0-1f8b85af35cbfdef94bd978f7a1256344b1ba11b119d8527ffeb25ad45b33d00
aca5e5c30d05   dev-peer0.org2.example.com-cp_0-1f8b85af35cbfdef94bd978f7a1256344b1ba11b119d8527ffeb25ad45b33d00-975c4abcbeadd0e1d07259da458226daa39682d293ba661112ceeb5ed8f708d4   "docker-entrypoint.s…"   15 seconds ago   Up 10 seconds                                                    dev-peer0.org2.example.com-cp_0-1f8b85af35cbfdef94bd978f7a1256344b1ba11b119d8527ffeb25ad45b33d00

```

# Test the functionaliy of ProductContract
## Create product
```
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile $ORDERER_CA -C mychannel -n p1 --peerAddresses $CORE_PEER_ADDRESS --tlsRootCertFiles $CORE_PEER_TLS_ROOTCERT_FILE --peerAddresses localhost:9051 --tlsRootCertFiles $PEER0_ORG2_CA --name productcontract -c '{"function":"createProductRecord","Args":["2", "Colors", "22", "CD"]}'

peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile $ORDERER_CA -C mychannel -n p1 --peerAddresses $CORE_PEER_ADDRESS --tlsRootCertFiles $CORE_PEER_TLS_ROOTCERT_FILE --peerAddresses localhost:9051 --tlsRootCertFiles $PEER0_ORG2_CA --name productcontract -c '{"function":"createProductRecord","Args":["3", "iPhone", "03/29/2021", "mobile"]}'
```
## Task 2 - Get product by key
```
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile $ORDERER_CA -C mychannel -n p1 --peerAddresses $CORE_PEER_ADDRESS --tlsRootCertFiles $CORE_PEER_TLS_ROOTCERT_FILE --peerAddresses localhost:9051 --tlsRootCertFiles $PEER0_ORG2_CA --name productcontract -c '{"function":"getProductByKey","Args":["2", "Colors"]}'
```

##  Task 3 - Update Product quantity
```
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile $ORDERER_CA -C mychannel -n p1 --peerAddresses $CORE_PEER_ADDRESS --tlsRootCertFiles $CORE_PEER_TLS_ROOTCERT_FILE --peerAddresses localhost:9051 --tlsRootCertFiles $PEER0_ORG2_CA --name productcontract -c '{"function":"updateQuantity","Args":["2", "Colors", "22", "400"]}'

peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile $ORDERER_CA -C mychannel -n p1 --peerAddresses $CORE_PEER_ADDRESS --tlsRootCertFiles $CORE_PEER_TLS_ROOTCERT_FILE --peerAddresses localhost:9051 --tlsRootCertFiles $PEER0_ORG2_CA --name productcontract -c '{"function":"updateQuantity","Args":["3", "iPhone", "03/21/2021", "1059"]}'
```

## Task 4 - Query by Product Type
```
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile $ORDERER_CA -C mychannel -n p1 --peerAddresses $CORE_PEER_ADDRESS --tlsRootCertFiles $CORE_PEER_TLS_ROOTCERT_FILE --peerAddresses localhost:9051 --tlsRootCertFiles $PEER0_ORG2_CA --name productcontract -c '{"function":"queryByProductType","Args":["CD"]}'

peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile $ORDERER_CA -C mychannel -n p1 --peerAddresses $CORE_PEER_ADDRESS --tlsRootCertFiles $CORE_PEER_TLS_ROOTCERT_FILE --peerAddresses localhost:9051 --tlsRootCertFiles $PEER0_ORG2_CA --name productcontract -c '{"function":"queryByProductType","Args":["mobile"]}'
```

## Task 5 - Query by Mfg Date
```
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile $ORDERER_CA -C mychannel -n p1 --peerAddresses $CORE_PEER_ADDRESS --tlsRootCertFiles $CORE_PEER_TLS_ROOTCERT_FILE --peerAddresses localhost:9051 --tlsRootCertFiles $PEER0_ORG2_CA --name productcontract -c '{"function":"queryByMfgdate","Args":["22"]}'

 peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile $ORDERER_CA -C mychannel -n p1 --peerAddresses $CORE_PEER_ADDRESS --tlsRootCertFiles $CORE_PEER_TLS_ROOTCERT_FILE --peerAddresses localhost:9051 --tlsRootCertFiles $PEER0_ORG2_CA --name productcontract -c '{"function":"queryByMfgdate","Args":["03/29/2021"]}'
 ```

 ## Task 6 - Query by product type dual
 ```
 peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile $ORDERER_CA -C mychannel -n p1 --peerAddresses $CORE_PEER_ADDRESS --tlsRootCertFiles $CORE_PEER_TLS_ROOTCERT_FILE --peerAddresses localhost:9051 --tlsRootCertFiles $PEER0_ORG2_CA --name productcontract -c '{"function":"queryByProduct_Type_Dual","Args":["CD", "mobile"]}'
```

 ## Task 7 - Unknown transaction
 ```
 peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile $ORDERER_CA -C mychannel -n p1 --peerAddresses $CORE_PEER_ADDRESS --tlsRootCertFiles $CORE_PEER_TLS_ROOTCERT_FILE --peerAddresses localhost:9051 --tlsRootCertFiles $PEER0_ORG2_CA --name productcontract -c '{"function":"queryByProduct_Type_Dual3333","Args":["CD","mobile"]}'
 ```