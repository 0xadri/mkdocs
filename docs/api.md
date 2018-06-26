# API

The ixo platform has a number of different components that each has a set of APIs

## ixo NPM module API

The ixo NPM module makes it easier to interact withthe project data stores and the ixo blockchain

### Usage
#### Install using npm

`npm install --save ixo-module`

#### Create new Ixo Object (Without provider)

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

## Keysafe Browser Extension API

* Todo *

## Project Datastore API

### Introduction
The project data store holds the data relating to projects.  The API is split into a public API and a non public API. The public API requests do not require cryptographic signatures, while all other requests must be signed and adher to the capabilities that have been granted to the signer.

### Health check

URI: `<pds server>/`

Request type: `GET`

Response:

```
API is running
```

The ixo project data store (pds) uses JSON-RPC to receive client requests.  The structure of all calls follow the same structure:

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

### Structure of params object

These are unsigned requests for publicly available information. A key is generated and sent back to the client, to be used in retrieval of information. 
Data is stored in binary and can handle any of the following encodings: "ascii" | "utf8" | "utf16le" | "ucs2" | "base64" | "latin1" | "binary" | "hex"
 

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

### Structure of params object

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

## ixo Blockchain API

* TODO *

## ixo Explorer API

Returns a the publicly available data pertaining to projects

<a name="explorer-listProjects"></a>
### List Projects

Lists all the projects

<a name="explorer-getProject"></a>
### Get Project

Retrieves a project by project DID

