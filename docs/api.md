# API

The ixo platform has a number of different components that each has a set of APIs

## ixo NPM module API

The ixo NPM module makes it easier to interact withthe project data stores and the ixo blockchain

`npm install --save ixo-module`

To Create new Ixo Object (Without provider)

```
import Ixo from 'ixo-module';
var ixo = new Ixo('ixo_node_url')
```

### Project Functions
Functions pertaining to projects. These calls take the form `ixo.project.<functionName>`

#### List Projects
Returns a list of all projects

Request:
```
ixo.project.listProjects().then((result) => {
    console.log('Project List: ' + result)
})   
```

Response: [ixo Explorer: listProjects](#explorer-listProjects)


#### Get Project
Retrieves public project details by DID
```
let projectDid = 'did:ixo:TknEju4pjyRQvVehivZ82x';
ixo.project.getProjectByDid(projectDid).then((result) => {
    console.log('Project Details: ' + result)
})   
```

Response: [ixo Explorer: getProject](#explorer-getProject)

#### Create Project
```
ixo.project.createProject(projectData, signature, PDSUrl).then((result) => {
    console.log('Project Details: ' + result)
})   
```

Response: [PDS: createProject](#pds-createProject)


#### Upload a Document
Function to upload a dataUrl to the project.  It returns a unique reference to the data so it can be retrieved at a later stage. It is used for uploading images and json templates, but it could countain any other project specific public data.

The `dataUrl` takes the form of `data:<mediatype>;<encoding>,<data>`

```
// Upload an image
let dataUrl = 'data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAAUA AAAFCAYAAACNbyblAAAAHElEQVQI12P4//8/w38GIAXDIBKE0DHxgljNBAAO 9TXL0Y4OHwAAAABJRU5ErkJggg==';
ixo.project.createPublic(dataUrl, PDSUrl) {
    console.log('Document hash: ' + result)
})   
```

Response: [PDS: createPublic](#pds-createPublic-image)

#### Retrieve a Document
Retrieves a previously uploaded document using the document hash

```
ixo.project.fetchPublic(documentHash, PDSUrl) {
    console.log('Document hash: ' + result)
})   
```

Response: [PDS: fetchPublic](#pds-fetchPublic-image)

### Agent Functions
Functions pertaining to agents on projects.  These calls take the form `ixo.agent.<functionName>`

#### List Agents on a Project
Returns a list of all agents on a project

Request:
```
ixo.agent.listAgentsForProject(data, signature, PDSUrl).then((result) => {
    console.log('Agent List: ' + result)
})   
```

Response: [PDS: listAgents](#pds-listAgents)


#### Create an Agent on a Project
Create an agents on a project

Request:
```
ixo.agent.createAgent(agentData, signature, PDSUrl).then((result) => {
    console.log('Create Agent: ' + result)
})   
```

Response: [PDS: createAgent](#pds-createAgent)

#### Update Agent Status
Update an agent's status on a project

Valid statuses are:
 
| Status  | Value |
| ------- | ----- |
|Pending  | 0     |
|Approved | 1     |
|Revoked  | 2     |

Request:
```
ixo.agent.createAgent(agentData, signature, PDSUrl).then((result) => {
    console.log('Update Agent Status: ' + result)
})   
```

Response: [PDS: updateAgentStatus](#pds-updateAgentStatus)

### Claim Functions
Functions pertaining to agents on projects.  These calls take the form `ixo.claim.<functionName>`

#### List Claims on a Project
Returns a list of all claims on a project

Request:
```
ixo.claim.listClaimsForProject(data, signature, PDSUrl).then((result) => {
    console.log('Claim List: ' + result)
})   
```

Response: [PDS: listClaimsForProject](#pds-listClaimsForProject)

#### Create a Claim on a Project
Create a claim on a project

Request:
```
ixo.agent.createClaim(agentData, signature, PDSUrl).then((result) => {
    console.log('Create Claim: ' + result)
})   
```

Response: [PDS: createClaim](#pds-createClaim)

#### Evaluate a Claim
Create an evaluation for a claim

Valid statuses are:
 
| Status  | Value |
| ------- | ----- |
|Pending  | 0     |
|Approved | 1     |
|Rejected | 2     |

Request:
```
ixo.agent.evaluateClaim(evaluationData, signature, PDSUrl).then((result) => {
    console.log('Create Evaluation: ' + result)
})   
```

Response: [PDS: evaluateClaim](#pds-evaluateClaim)

### User Functions

#### Register DID Doc to Blockchain

Registers a new user with the supplied DID document to the blockchain

Request:
```
ixo.user.registerUserDid().then((result) => {
    console.log('Register DID: ' + result)
})   
```
Response: [ixo Blockchain: registerUserDid](#blockchain-registerUserDid)

#### Get the DID Doc from Blockchain

Retrieves the DID Doc from the blockchain for the specified DID

Request:
```
Let did = 'did:sov:2p19P17cr6XavfMJ8htYSS';
ixo.user.getDidDoc(did).then((result) => {
    console.log('DID Doc: ' + result)
})   
```
Response: [ixo Blockchain: getDidDoc](#blockchain-getDidDoc)

### Metrics
Returns the global statistics for all projects

Request:
```
ixo.stats.getGlobalStats().then((result) => {
    console.log('Statistics: ' + result)
})   
```
Response: [ixo Explorer: getGlobalStats](#explorer-getGlobalStats)

### Health Check Functions

#### Heath Check to Blockchain node

Request:
```
ixo.network.pingIxoBlockchain().then((result) => {
    console.log('Health Check: ' + result)
})   
```

Response: [ixo Blockchain: healthCheck](#blockchain-health)

#### Heath Check to Explorer node

Request:
```
ixo.network.pingIxoExplorer().then((result) => {
    console.log('Health Check: ' + result)
})   
```

Response: [ixo Explorer: ping](#explorer-ping)

## Keysafe Browser Extension API

When the keysafe extension is added to the browser it injects a globale on the the `window` object.  It can be accessed as follows:

```
const IxoInpageProvider = window['ixoKs'];
let keysafe = new IxoInpageProvider();
```

### Info Functions

#### Get User Information
Returns a javascript object contain the user name and the DID Doc

Request:
```
keysafe.getInfo().then((error, result) => {
	if (result) {
        console.log('User Info: ' + result)
    } else {
        console.log(error)
    }
})   
```

Response: 
```
{
    name: "John",
    didDoc: {
        "did": "did.sov.EvBFmtyRaBuMNMnwjHNVgn",
        "pubKey": "8awT75ZgZttei45J52bcXC2q8isMRATLcdgbmx4FHyFf"
    }
}
```

#### Get User DID Doc
Returns a javascript object contain the user's DID Doc

Request:
```
keysafe.getDidDoc().then((error, result) => {
	if (result) {
        console.log('User DID Doc: ' + result)
    } else {
        console.log(error)
    }
})   
```

Response: 
```
{
  "did": "2HQrdvfjqZwRQCapLDPZzY",
  "pubKey": "hZHiC5kPgiADRXnuiktvmsNSPH1D4c96NxMSjjNLVTY",
}
```

### Signing Functions

#### Request Signing
Request the user to sign some data using the keys in the keysafe

```
keysafe.requestSigning(JSON.stringify(data), (error, signature) => {	
    if (!error) {
        console.log("Signature: " + signature);
    } else {
        console.log(error);
    }
});
```

Response: 

`A011D11A2D91A9CB03ECFFB7D9AFC1001DB56B3DABF42BDD0F4D00352A9B8E0E73E85F0B4586DA2934696C0A78602EEB047EA6B3D9096C1A0C3FB144E6A51C09`


## Project Datastore API

The project data store holds the data relating to projects.  The API is split into a public API and a non public API. The public API requests do not require cryptographic signatures, while all other requests must be signed and adher to the capabilities that have been granted to the signer.

### Public API

URI: `<pds server>/api/public`

Request type: `POST`

Structure: 
`{"jsonrpc": "2.0", "method": "<method name>", 	"id": <message id>, 	"params": <json data object> }`

| Variable             | Description            |
|----------------------|-----------------------|
| `<node server>`      | The URL of the server |
| `<entity>`           | The entity to send the method |
| `<method name>`      | The name of the method to call defined in the config file |
| `<message id>`       | The message ID, used to correlate asynchronous responses |
| `<json data object>` | The parameters that are passed to the method handler |

#### Structure of params object

These are unsigned requests for publicly available information. A key is generated and sent back to the client, to be used in retrieval of information. 
Data will accept any of the following encodings: "ascii" | "utf8" | "utf16le" | "ucs2" | "base64" | "latin1" | "binary" | "hex".
contentType should reference a MIME type. See https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types/Complete_list_of_MIME_types
 

```
{
	"jsonrpc":"2.0", 
	"method":"createPublic",
	"id": 123,
	"params": {
		"data": "bob public message", 
		"contentType": "text"
		}
}

```
<a name="pds-createPublic-image"></a>
#### Upload an image
 
 Request:

```
{
	"jsonrpc": "2.0", 
	"method": "createPublic", 
	"id": 3, 
	"params": 
		{
		"data": "<base64 encoded image>", 
		"contentType": "image/png"
		}
}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 3,
    "result": "<string>"
}
```

<a name="pds-fetchPublic-image"></a>
#### Fetch image
 
Request:

```
{
    "jsonrpc": "2.0", 
     "method": "fetchPublic", 
     "id": 3, 
     "params": {"key": <string>}
}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 3,
    "result": {
        "data": "<base64 encoded image>",
        "contentType": "image/png"
    }
}
```

<a name="pds-createPublic-json"></a>
#### Upload a Json file

Request:

```
{
     "jsonrpc": "2.0", 
     "method": "createPublic", 
     "id": 3, 
     "params": 
	{
	   "data": "<JSON string>", 
	   "contentType": "application/json"
	}
}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 3,
    "result": "<string>"
}
```

<a name="pds-fetchPublic-json"></a>
#### Fetch Json file

Request:

```
{
    "jsonrpc": "2.0", 
     "method": "fetchPublic", 
     "id": 3, 
     "params": {"key": <string>}
}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 3,
    "result": {
        "data": "<JSON string>",
        "contentType": "application/json"
    }
}
```

### Private API

URI: `<pds server>/api/request`

Request type: `POST`

Structure: 
`{"jsonrpc": "2.0", "method": "<method name>", 	"id": <message id>, 	"params": <json data object> }`

| Variable             | Description            |
|----------------------|-----------------------|
| `<node server>`      | The URL of the server |
| `<entity>`           | The entity to send the method |
| `<method name>`      | The name of the method to call defined in the config file |
| `<message id>`       | The message ID, used to correlate asynchronous responses |
| `<json data object>` | The parameters that are passed to the method handler |

#### Structure of params object

Everything in the payload section is signed to create a signature.  It should be packed using `JSON.stringify()` method before signing. 

```
{
	"jsonrpc":"2.0", 
	"method":"createAgent",
	"id": 123,
	"params": {
		"payload": {
			
            "template": {
                "name": "create_agent"
            },
            "data": {"projectDid": "did:ixo:TknEju4pjyRQvVehivZ82x",
            		 "name": "Brennon",
            		 "surname": "Hampton",
            		 "email": "brennon@me.com",
            		 "agentDid": "did:sov:64",
            		 "role": "SA"}
        },
        "signature": {
            "type": "ed25519-sha-256",
            "created": "2018-06-27T16:02:20Z", 
            "creator": "did:sov:2p19P17cr6XavfMJ8htYSS",
            "signatureValue": "A011D11A2D91A9CB03ECFFB7D9AFC1001DB56B3DABF42BDD0F4D00352A9B8E0E73E85F0B4586DA2934696C0A78602EEB047EA6B3D9096C1A0C3FB144E6A51C09"
        }
    }
}

```
<a name="pds-createProject"></a>
#### Create Project

Creates a new project.

Request:

```
{
	"jsonrpc": "2.0", 
	"method": "createProject", 
	"id": 3, 
	"params": {
        "payload": {        
            "template": {
                "name": "<template to validate>"
            },
            "data": {
                <create project data>
            }
        },
        "signature": {
            "type": <signature type ECDSA or E25519>,
            "created": <date of signature>, 
            "creator": <user did>,
            "signatureValue":  <signature in hex>
        }
    }
}
```

Example:

```
{
    "jsonrpc": "2.0",
    "method": "createProject",
    "id": 123,
    "params": {
        "payload": {
            "template": {
                "name": "create_project"
            },
			"data": {
				"title": "Test Water project",
		        "ownerName": "Don",
		        "ownerEmail": "don@gmail.com",
		        "shortDescription": "Project for water",
		        "longDescription": "project to save water for areas with drought",
		        "impactAction": "litres of water saved",
		        "projectLocation": "ZA",
		        "sdgs": [
		          "12.2",
		          "3",
		          "2.4"
		        ],
		        "requiredClaims": 30,
		        "templates": {
		          "claim": {
		            "schema": "af175axcn6ejiuds0sh",
		            "form": "1v6v8a6woabjiuds3i9"
		          }
		        },
		        "evaluatorPayPerClaim": "0",
		        "socialMedia": {
		          "facebookLink": "https://www.facebook.com/ixofoundation/",
		          "instagramLink": "",
		          "twitterLink": "",
		          "webLink": "https://ixo.foundation"
		        },
		        "serviceEndpoint": "http://35.192.187.110:5000/",
		        "imageLink": "pc16l7yk62ejiudrox5",
		        "founder": {
		          "name": "Nic",
		          "email": "nic@test.co.za",
		          "countryOfOrigin": "ZA",
		          "shortDescription": "primary description for founder",
		          "websiteURL": "www.water.com",
		          "logoLink": ""
		        }
			}
        },
         "signature": {
            "type": "ed25519-sha-256",
            "created": "2018-06-05T12:35:02Z", 
            "creator": "did:sov:2p19P17cr6XavfMJ8htYSS",
            "signatureValue": "23EED2462B11B94C9F63A509B39F15CB9C0B2DB8C16A52A22115B755BF3F6BDF7ABB8881697AA7DB6F4AFBD7C5DE4618B403AB43B738841BB89E72C8792AC401"
        }
    }
}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 3,
    "result": {
        "__v": 0,
        <project data>
        "tx": "b51cd2665d146d0a0240fd2756beb4c9e9c1948275ac5d37a7ae405ab2d71a7a",
        "_id": "5a66e09b38f45f01d90d122a",
    }
}
```

Example:

```
{
    "jsonrpc": "2.0",
    "id": 123,
    "result": {
        "_id": "5b32094f05aa3f0011405957",
        "title": "Test Water project",
        "ownerName": "Don",
        "ownerEmail": "don@gmail.com",
        "shortDescription": "Project for water",
        "longDescription": "project to save water for areas with drought",
        "impactAction": "litres of water saved",
        "projectLocation": "ZA",
        "sdgs": [
            "12.2",
            "3",
            "2.4"
        ],
        "requiredClaims": 30,
        "templates": {
            "claim": {
                "schema": "af175axcn6ejiuds0sh",
                "form": "1v6v8a6woabjiuds3i9"
            }
        },
        "evaluatorPayPerClaim": "0",
        "socialMedia": {
            "facebookLink": "https://www.facebook.com/ixofoundation/",
            "instagramLink": "",
            "twitterLink": "",
            "webLink": "https://ixo.foundation"
        },
        "serviceEndpoint": "http://35.192.187.110:5000/",
        "imageLink": "pc16l7yk62ejiudrox5",
        "founder": {
            "name": "Nic",
            "email": "nic@test.co.za",
            "countryOfOrigin": "ZA",
            "shortDescription": "primary description for founder",
            "websiteURL": "www.water.com",
            "logoLink": ""
        },
        "txHash": "a09c8bc12a3e7cc1f859f0fc98cd37880d8c894826e0f1fa7a3f824db37941f5",
        "__v": 0
    }
}
```

<a name="pds-createAgent"></a>
#### Create Agent

Creates a new agent.

Request:

```
{
	"jsonrpc": "2.0", 
	"method": "createAgent", 
	"id": 3, 
	"params": {
        "payload": {        
            "template": {
                "name": "<template to validate>"
            },
            "data": {
                <create agent data>
            }
        },
        "signature": {
            "type": <signature type ECDSA or E25519>,
            "created": <date of signature>, 
            "creator": <user did>,
            "signatureValue":  <signature in hex>
        }
    }
}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 3,
    "result": {
        "__v": 0,
        <agent data>
        "tx": "b51cd2665d146d0a0240fd2756beb4c9e9c1948275ac5d37a7ae405ab2d71a7a",
        "_id": "5a66e09b38f45f01d90d122a",
        "version": 1
    }
}
```

<a name="pds-updateAgentStatus"></a>
#### Update Agent Status

Update Agent Status

Request:

```
{
	"jsonrpc": "2.0", 
	"method": "updateAgentStatus", 
	"id": 3, 
	"params": {
        "payload": {        
            "template": {
                "name": "<template to validate>"
            },
            "data": {
                <update agent data>
            }
        },
        "signature": {
            "type": <signature type ECDSA or E25519>,
            "created": <date of signature>, 
            "creator": <user did>,
            "signatureValue":  <signature in hex>
        }
    }
}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 3,
    "result": {
        "__v": 0,
        <agent data>
        "tx": "b51cd2665d146d0a0240fd2756beb4c9e9c1948275ac5d37a7ae405ab2d71a7a",
        "did": <creator's did>
        "_id": "5a66e09b38f45f01d90d122a",
        "version": 1
    }
}
```
<a name="pds-listAgents"></a>
#### List Agents

List claims and latest status.

Request:

```
{
	"jsonrpc": "2.0", 
	"method": "listAgents", 
	"id": 3, 
	"params": {
        "payload": {        
            "template": {
                "name": "<template to validate>"
            },
            "data": {
                <data to filter>
            }
        },
        "signature": {
            "type": <signature type ECDSA or E25519>,
            "created": <date of signature>, 
            "creator": <user did>,
            "signatureValue":  <signature in hex>
        }
    }
}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 3,
    "result": [
        {
            <agent data>,
            "currentStatus": {
                <agent status data>
            }
        }
    ]
}
```

<a name="pds-createClaim"></a>
#### Submit Claim

Creates a new claim.

Request:

```
{
	"jsonrpc": "2.0", 
	"method": "submitClaim", 
	"id": 3, 
	"params": {
        "payload": {        
            "template": {
                "name": "<template to validate>"
            },
            "data": {
                <submit claim data>
            }
        },
        "signature": {
            "type": <signature type ECDSA or E25519>,
            "created": <date of signature>, 
            "creator": <user did>,
            "signatureValue":  <signature in hex>
        }
    }
}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 3,
    "result": {
        "__v": 0,
        <claim data>
        "tx": "b51cd2665d146d0a0240fd2756beb4c9e9c1948275ac5d37a7ae405ab2d71a7a",
        "_id": "5a66e09b38f45f01d90d122a"
    }
}
```

<a name="pds-evaluateClaim"></a>
#### Evaluate Claim

Evaluate a new claim.

Request:

```
{
	"jsonrpc": "2.0", 
	"method": "evaluateClaim", 
	"id": 3, 
	"params": {
        "payload": {        
            "template": {
                "name": "<template to validate>"
            },
            "data": {
                <evaluate claim data>
            }
        },
        "signature": {
            "type": <signature type ECDSA or E25519>,
            "created": <date of signature>, 
            "creator": <user did>,
            "signatureValue":  <signature in hex>
        }
    }
}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 3,
    "result": {
        "__v": 0,
        <claim data>
        "tx": "b51cd2665d146d0a0240fd2756beb4c9e9c1948275ac5d37a7ae405ab2d71a7a",
        "_id": "5a66e09b38f45f01d90d122a",
        "version": 1
    }
}
```

#### List Claim

List claims and latest status.

Request:

```
{
	"jsonrpc": "2.0", 
	"method": "listClaims", 
	"id": 3, 
	"params": {
        "payload": {        
            "template": {
                "name": "<template to validate>"
            },
            "data": {
                <data to filter>
            }
        },
        "signature": {
            "type": <signature type ECDSA or E25519>,
            "created": <date of signature>, 
            "creator": <user did>,
            "signatureValue":  <signature in hex>
        }
    }
}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 3,
    "result": [
        {
            <claim data>,
            "evaluations": {
                <claim status data>
            }
        }
    ]
}
```
### Heath Check Functions
#### Health Check

URI: `<pds server>/`

Request type: `GET`

Response:

```
API is running
```

The ixo project data store (pds) uses JSON-RPC to receive client requests.  The structure of all calls follow the same structure:

## ixo Blockchain API

### DID Functions

<a name="blockchain-registerUserDid"></a>
#### Register DID Doc
Registers the DID Doc for the specified DID.  The DID Doc must contain the DID and the public key which can be used to verify signatures sign by this DID.

Request:

|         |       |
| ------- | ----- |
| Server: | Blockchain TX Server |
| Method: | `GET` |
| URI: | `/broadcast_tx_sync?tx=` |
| Parameters: | *&lt;uppercase hex of the DID Doc with its signature preceded with Ox&gt;* |

Example Request:
`http://localhost:46657/broadcast_tx_sync?tx=0x7B227...355430383A34363A31372B30323030227D7D`

Example Parameter (pre hex encoding):
```
{"payload":[10,{"didDoc":{"did":"did:sov:398fM9kMgHuNbCtRncYrwh","pubKey":"2Af4UzgUAgQk8Wt5xEkfJrjQSxWgxsuD8bzDQJSNfMSw","credentials":[]}}],"signature":{"signatureValue":[1,"211678D20C70292668C47D6220ED648F868DFE0CBB848EDB0E163F7EE35467F938CFB000FCCF1AD00A0AB67F6EEB6C02E4FE48D793247A7092D5B5613C87C405"],"created":"2018-06-05T08:46:17+0200"}}
```

Response:
```
{
  "did": "did.sov.EvBFmtyRaBuMNMnwjHNVgn",
  "pubKey": "8awT75ZgZttei45J52bcXC2q8isMRATLcdgbmx4FHyFf",
  "credentials": [
    {   
        "credential":{
            "type": ["Credential","ProofOfKYC"],
            "issuer": "DHHeFW9G17McBUk45ty7Jn",
            "issued": "2018-07-16T15:51:44+02:00",
            "claim": {
                "id": "did.sov.EvBFmtyRaBuMNMnwjHNVgn",
                "KYCValidated": true
            }
        }
    }
  ]
}
```

<a name="blockchain-addDidCredential"></a>
#### Add a Credential to a DID Doc
Adds a signed credential to the DID Doc for the specified DID.  The Credential must be signed by the DID of the credential issuer and the credential issuer's DID must already be registered.

Request:

|         |       |
| ------- | ----- |
| Server: | Blockchain TX Server |
| Method: | `GET` |
| URI: | `/broadcast_tx_sync?tx=` |
| Parameters: | *&lt;uppercase hex of the Add Credential Message with its signature preceded with Ox&gt;* |

Example Request:
`http://localhost:46657/broadcast_tx_sync?tx=0x7B227...355430383A34363A31372B30323030227D7D`

Example Parameter (pre hex encoding):
```
{"payload":[24,{"credential":{"type":["Credential","ProofOfKYC"],"issuer":"DHHeFW9G17McBUk45ty7Jn","issued": "2018-07-16T15:51:44+02:00","claim":{"id":"DHHeFW9G17McBUk45ty7Jn","KYCValidated":true}}}],"signature":{"signatureValue":[1,"8EA7D3D45C95863E5C7CD1A4043D5F618E32F41CA72FAE75B7C09377D2B6AFC9AE8844AF0B621216339F025C67428B1838C8A1BBCD48E761655EBA9CCF114502"],"created":"2018-07-16T14:39:31Z"}}
```

Response:
```
{
    jsonrpc: "2.0",
    id: "",
    result: {
        code: 0,
        data: "0116444848654657394731374D6342556B34357479374A6E012C3768446F346A724675713846727566666164595A6A7971374C39534E4536327765616E74534D7A466A78585801030102010A43726564656E7469616C010A50726F6F664F664B59430116444848654657394731374D6342556B34357479374A6E0119323031382D30372D31365431353A33393A34302B30323A30300116444848654657394731374D6342556B34357479374A6E010102010A43726564656E7469616C010A50726F6F664F664B59430116444848654657394731374D6342556B34357479374A6E0119323031382D30372D31365431353A35313A34342B30323A30300116444848654657394731374D6342556B34357479374A6E010102010A43726564656E7469616C010A50726F6F664F664B59430116444848654657394731374D6342556B34357479374A6E0119323031382D30372D31365431353A35313A34342B30323A30300116444848654657394731374D6342556B34357479374A6E01",
        log: "",
        hash: "91C033E74E27E7778BD0FE3481F82F839C92C5BC"
    }
}
```

<a name="blockchain-getDidDoc"></a>
#### Get Did Doc
Returns the Did Doc for the specified DID.  This contains the public key which can be used to verify signatures sign by this DID.

Request:

|         |       |
| ------- | ----- |
| Server: | Blockchain REST Server |
| Method: | `GET` |
| URI: | `/did` |
| Parameters: | *&lt;did&gt;* |

Example:
`http://localhost:1317/did/did.sov.EvBFmtyRaBuMNMnwjHNVgn`

Response:
```
{
  "did": "did.sov.EvBFmtyRaBuMNMnwjHNVgn",
  "pubKey": "8awT75ZgZttei45J52bcXC2q8isMRATLcdgbmx4FHyFf"
  "credentials": [
    {
      "type": "KYC",
      "data": "KYC Authentication Service",
      "signer": "DHHeFW9G17McBUk45ty7Jn"
    }
  ]
}
```

### Health Check Functions
<a name="blockchain-health"></a>
#### Heath Check

Check whether the blockchain node is available

Request:

|         |       |
| ------- | ----- |
| Server: | Blockchain TX Server |
| Method: | `GET` |
| URI: | `/heath` |

Example:
`http://localhost:46657/health`

Response:
```
{
jsonrpc: "2.0",
id: "",
result: { }
}
```

## ixo Explorer API

Returns a the publicly available data pertaining to projects

###Project Functions

<a name="explorer-listProjects"></a>
#### List Projects

Lists all the projects

<a name="explorer-getProject"></a>
#### Get Project

Retrieves a project by project DID

<a name="explorer-getGlobalStats"></a>
#### Get Global Stats

Retrieves the global statistics and metrics for all projects

### Health Check Functions
<a name="explorer-ping"></a>
#### Heath Check

Check that the explorer node is available