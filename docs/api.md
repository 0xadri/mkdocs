# API
*TODO*

## ixo-Node API

The ixo node uses JSON-RPC to receive client requests.  The structure of all calls follow the same structure:

URI: `<node server>/api/<entity>`

Request type: `POST`

Structure: 
`{"jsonrpc": "2.0", "method": "<method name>", 	"id": <message id>, 	"params": <json data object> }`

| Variable             | Description            |
|----------------------|-----------------------|
| `<node server>`      | The URL of the server |
| `<entity>`           | The entity to send the method |
| `<method name>`      | The name of the method to call |
| `<message id>`       | The message ID, used to correlate asynchronous responses |
| `<json data object>` | The parameters that are passed to the method handler |

### Structure of params object

Everything in the payload section is signed to create a signature.  It shoul dbe packed using `JSON.stringify()` method before signing. 

```
{
    "payload": {
        "did": <did|publicKey of user>,
        "data": <request data>
    },
    "signature": {
        "type": <signature type ECDSA or E25519>,
        "created": <date of signature>,
        "creator": <did|publicKey of user>,
        "signature": <signature in hex>
    }
}
```

### Network

Example URI: https://ixo-node.herokuapp.com/api/network

#### Ping

Pings the network to ensure connectivity.

Request:

```
{"jsonrpc": "2.0", "method": "ping", "id": 1}
```

Response:

```
{"jsonrpc": "2.0", "id": 1, "result": "pong"}
```

### Project

Example URI: https://ixo-node.herokuapp.com/api/project

#### Get Template

Retrieves the project template from the template registry.

Request:

```
{"jsonrpc": "2.0", "method": "getTemplate", "params": {"payload":{"did":<did of user>,"data":{"name":<project template name>}}}, "id": 1}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "template": <the template>,
        "form": <template's corresponding form>
    }
}
```

#### Create Project

Creates a new project.

Request:

```
{
	"jsonrpc": "2.0", 
	"method": "create", 
	"id": 3, 
	"params": {
        "payload":{
            "did":<did of user>,
            "template": {
                "name": <template name>
            },
		    "data": <project data>
        },
		"signature": {
			"type": "ECDSA",
    		"created": "2016-02-08T16:02:20Z", 
    		"creator": <did of user>,
    		"signature": <signature>
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
        "created": "2018-01-23T07:13:31.060Z"
    }
}
```

#### List Projects

Lists all new projects.

Request:

```
{"jsonrpc": "2.0", "method": "list", "id": 1}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": <array of projects>
}
```

#### List Projects for a specific DID

Lists all projects owned by a specific DID.

Request:

```
{"jsonrpc": "2.0", "method": "listForDID", "params": {"payload":{"did":<did of user>,"data":{"did":<did of owner>}}}, "id": 1}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": <array of projects>
}
```

### Template

Example URI: https://ixo-node.herokuapp.com/api/template

#### Get Template

Retrieves a template from the template registry. Valid types are: 'project', 'agent', claim' or 'evaluation'

Request:

```
{"jsonrpc": "2.0", "method": "getTemplate", "params": {"payload":{"did":<did of user>,"data":{"type": <template type>, "name": <name of template>}}}, "id": 1}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "template": <the template>,
        "form": <template's corresponding form>
    }
}
```

### Agent

Example URI: https://ixo-node.herokuapp.com/api/agent

#### Get Template

Retrieves the agent template called "default" from the template registry.

Request:

```
{"jsonrpc": "2.0", "method": "getTemplate", "params": {"payload":{"did":<did of user>,"data":{"name": <name of agent template>}}}, "id": 1}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "template": <the template>,
        "form": <template's corresponding form>
    }
}
```

#### Create Agent

Creates a new agent.

Request:

```
{
	"jsonrpc": "2.0", 
	"method": "create", 
	"id": 3, 
	"params": {
        payload: {
            "template": {
                "name": <template name>
            },
            "did": <did of user>,
		    "data": <agent data>
        },
		"signature": {
			"type": "ECDSA",
    		"created": "2016-02-08T16:02:20Z", 
    		"creator": <did of user>,
    		"signature": <signature>
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
        "created": "2018-01-23T07:13:31.060Z"
    }
}
```

#### Update Agent Status

Update Agent Status

Request:

```
{
	"jsonrpc": "2.0", 
	"method": "updateAgentStatus", 
	"id": 3, 
	"params": {
        payload: {
            },
            "did": <did of user>,
		    "data": {
                agentTx: <tx of the agent to update>,
                status: <Approved|NotApproved|Revoked|Pending>
            }
        },
		"signature": {
			"type": "ECDSA",
    		"created": "2016-02-08T16:02:20Z", 
    		"creator": <did of user>,
    		"signature": <signature>
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
        "created": "2018-01-23T07:13:31.060Z"
    }
}
```

#### List Agents for a specific DID

Lists all agents for a specific DID.

Request:

```
{"jsonrpc": "2.0", "method": "listForDID", "params": {"payload":{"did":<did of user>,"data":{"did":<did of owner>}}}, "id": 1}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": <array of agents>
}
```

#### List Agents for a specific Project ID

Lists all agents for a specific project id.

Request:

```
{"jsonrpc": "2.0", "method": "listForProject", "params": {"payload":{"did":<did of user>,"data":{"projectTx":<project TX>}}}, "id": 1}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": <array of agents>
}
```

### Claim

Example URI: https://ixo-node.herokuapp.com/api/claim

#### Get Claim Template

Retrieves the claim template called "default" from the template registry.

Request:

```
{"jsonrpc": "2.0", "method": "getTemplate", "params": {"payload":{"did":<did of user>,"data":{"name": <name of claim template>}}}, "id": 1}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "template": <the template>,
        "form": <template's corresponding form>
    }
}
```

#### Create Claim

Creates a new claim.

Request:

```
{
	"jsonrpc": "2.0", 
	"method": "create", 
	"id": 3, 
	"params": {
        payload: {
            "template": {
                "name": <template name>
            },
            "did": <did of user>,
		    "data": <claim data>
        },
		"signature": {
			"type": "ECDSA",
    		"created": "2016-02-08T16:02:20Z", 
    		"creator": <did of user>,
    		"signature": <signature>
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
        "did": <creator's did>
        "_id": "5a66e09b38f45f01d90d122a",
        "created": "2018-01-23T07:13:31.060Z"
    }
}
```

#### Evaluate Claim

Evaluate a new claim.

Request:

```
{
	"jsonrpc": "2.0", 
	"method": "evaluateClaim", 
	"id": 3, 
	"params": {
        payload: {
            "template": {
                "name": <template name>
            },
            "did": <did of user>,
		    "data": {
                claimTx: <tx of the claim>
                <other evaluation data>
            }
        },
		"signature": {
			"type": "ECDSA",
    		"created": "2016-02-08T16:02:20Z", 
    		"creator": <did of user>,
    		"signature": <signature>
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
        "did": <creator's did>
        "_id": "5a66e09b38f45f01d90d122a",
        "created": "2018-01-23T07:13:31.060Z"
        "latestEvaluation": "Approved",
        "evaluations": [{
            <evaluation data>
        }]
    }
}
```

#### List Claims for a specific Project ID

Lists all claims for a specific agent did.

Request:

```
{"jsonrpc": "2.0", "method": "listForDID", "params": {"payload":{"did":<did of user>,"data":{"did":<agent did>}}}, "id": 1}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": <array of claims>
}
```

#### List Claims for a specific Project ID

Lists all claims for a specific project id.

Request:

```
{"jsonrpc": "2.0", "method": "listForProject", "params": {"payload":{"did":<did of user>,"data":{"projectTx":<project TX>}}}, "id": 1}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": <array of claims>
}
```

#### Lists all claims for a specific project ID and status.

Request:

```
{"jsonrpc": "2.0", "method": "listForProjectAndDID", "params": {"payload":{"did":<did of user>,"data":{"projectTx":<project tx>,"status":<claim status>}}}, "id": 1}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": <array of claims>
}
```

#### Lists all claims for a specific project ID and agent DID.

Request:

```
{"jsonrpc": "2.0", "method": "listForProjectAndDID", "params": {"payload":{"did":<did of user>,"data":{"projectTx":<project tx>,"did":<agent did>}}}, "id": 1}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": <array of claims>
}
```


## ixo-Module API

#### Install using npm

`npm install --save ixo-module`

####Create new Ixo Object

```
import Ixo from 'ixo-module';
var ixo = new Ixo('ixo_node_url')
```

#### CryptoUtil

**Generate Mnemonic**

Request:

```
ixo.cryptoUtil.generateMnemonic()
```

Response:

```
dilemma allow swamp hedgehog client reject mistake spell involve index panda course
```

**Generate SovrinDID**

Request:

```
ixo.cryptoUtil.generateSovrinDID(mnemonic)
```

Response:

```
{
    "did": "LuEoT1EkTVT7vaYP1ibvfw",
        "verifyKey": "Br6jjwiPNBgDod4hHAKNP5AfA6ViV39eVX3UV4t9uADC",
            "secret": {
                     "seed": "b3cad338b23d0e58583ca243481262ee5f8632b14d713245e8b91be87daff073",
                     "signKey": "D6qMjQCdjB8gHkEPyxXoyJTxkk5WK9egYQAEtJ6RX5fx"
    }
}
```

**Sign Document**

Request:

```
ixo.cryptoUtil.getDocumentSignature(sdid.secret.signKey, sdid.verifyKey, JSON.stringify(testJson))
```

Response:

```
DjoT6XqQ53J2kR4zd1shB17qFuM9DM5A2DAxQ3jjtgacvvpafWefHx54kkHewMVMTAsZm61wDCtzMV2TwkL7Fc5YdTc898X4tQ8SepthqyFBdMVs8fAt3fWDGD1fiVe5cymPCDcHwB6hP34DpQB3UAcfZSoPP2wxCbCLhTAF25RywqEWmcMDqF42pEqa9RonpF6AYGxYQt2tUKT9383HR6RhCkkbrkJSBwYQ6b4jsnysz23p4TPfahPKGWinGahXFwtZKD69SSjipzQNHWFXb5YuoqQcCToTFcEteQ3dtkDQdCcWFZ9N1
```

**Validate Signature**

Request:

```
ixo.cryptoUtil.verifyDocumentSignature(signature, sdid.verifyKey)
```

Response:

```
true/false
```

#### Network
   
**Ping ixo Server Node**
   
Request:

```
ixo.network.pingIxoServerNode().then((result) => {
    console.log('Ping Results: ' + result)})
   
```
   
Response:

```
Ping Results: {
             "jsonrpc": "2.0",
             "id": 1,
             "result": "pong"
     }
```
     
#### Projects
   
**Gets project template**
   
Request:
```
   ixo.project.getProjectTemplate().then((result) => {
       console.log('Project Template: ' + result)
   })
   
```

Response:
```
   Project Template:
   {
      "jsonrpc":"2.0",
      "id":1,
      "result":{
         "template":{
            "@context":"http://ixo.foundation/schema",
            "@type":"Project",
            "so":"http://schema.org/",
            "name":"so:name",
            "about":"so:about",
            "country":"so:country",
            "thumbnail":{
               "@type":"ImageObject",
               "contentUrl":"so:contentUrl"
            },
            "owner":{
               "@type":"Person",
               "email":"so:email",
               "name":"so:name"
            }
         },
         "form":{
            "fields":[
               {
                  "label":"Project Name",
                  "name":"name",
                  "type":"text"
               },
               {
                  "label":"About",
                  "name":"about",
                  "type":"textarea"
               },
               {
                  "label":"Country",
                  "name":"country",
                  "type":"country"
               },
               {
                  "label":"Thumbnail",
                  "name":"thumbnail",
                  "type":"image"
               },
               {
                  "label":"Owner Name",
                  "name":"owner.name",
                  "type":"text"
               },
               {
                  "label":"Owner email",
                  "name":"owner.email",
                  "type":"text"
               }
            ]
         }
      }
   }
```
     
**Gets list of projects**

Request:
   
```
   ixo.project.listProjects().then((result) => {
       console.log('Project List:  ' + result)
   })
   
```
   
Response:

```
Project List:
{
   "jsonrpc":"2.0",
   "id":1,
   "result":[
      {
         "_id":"5a61abec670a09001abf0560",
         "name":"Water Saving",
         "country":"ZA",
         "tx":"f249310d2582927fc2019a94f96ca9df9a1c069bf3826bec89a4226a8201df4e",
         "__v":0,
         "owner":{
            "email":"joe@bloggs.com",
            "name":"Joe Blogs",
            "did":"0x92928b5135d8dbad88b1e772bf5b8f91bfe41a8d"
         },
         "created":"2018-01-19T08:27:24.669Z"
      },
      {
         "_id":"5a61abeb670a09001abf055e",
         "name":"Water Saving",
         "country":"ZA",
         "tx":"77fc5cbdc2985aab2bda1754e36f9a879c29d98c0eb49394df07d40f306c3b65",
         "__v":0,
         "owner":{
            "email":"joe@bloggs.com",
            "name":"Joe Blogs",
            "did":"0x92928b5135d8dbad88b1e772bf5b8f91bfe41a8d"
         },
         "created":"2018-01-19T08:27:23.679Z"
      },
      {
         "_id":"5a61abea670a09001abf055c",
         "name":"Water Saving",
         "country":"ZA",
         "tx":"a843e51f7e8787a669463bb0eeda7c2f4578fb501388ab2bc0ae3493012b086e",
         "__v":0,
         "owner":{
            "email":"joe@bloggs.com",
            "name":"Joe Blogs",
            "did":"0x92928b5135d8dbad88b1e772bf5b8f91bfe41a8d"
         },
         "created":"2018-01-19T08:27:22.685Z"
      },
      {
         "_id":"5a61abe8670a09001abf055a",
         "name":"Water Saving",
         "country":"ZA",
         "tx":"02382076c5ae1b0478d32bf48f8e8f9b39049ae98d7fb573e346e72cd4d8242e",
         "__v":0,
         "owner":{
            "email":"joe@bloggs.com",
            "name":"Joe Blogs",
            "did":"0x92928b5135d8dbad88b1e772bf5b8f91bfe41a8d"
         },
         "created":"2018-01-19T08:27:20.833Z"
      },
      {
         "_id":"5a61abe7670a09001abf0558",
         "name":"Water Saving",
         "country":"ZA",
         "tx":"d164ffd4d18add95eea273783c4b98e3046618d3c2009b01a19f935ab9f734ca",
         "__v":0,
         "owner":{
            "email":"joe@bloggs.com",
            "name":"Joe Blogs",
            "did":"0x92928b5135d8dbad88b1e772bf5b8f91bfe41a8d"
         },
         "created":"2018-01-19T08:27:19.124Z"
      }
   ]
}

```

#### Auth

**Get Credential Provider**

Request:
   
```
ixo.auth.getCredentialProvider(provider).then((result) => {
    console.log('Provider:  ' + result);
})
```
   
Response:

```
Credential Provider Object
```
   
**Sign Data**
 
Request:
    
```
ixo.auth.sign(provider, dataToSign).then((signature: string) => {
        console.log(signature);
    }).catch((error) => {
        console.log(error);
    });
```
    
Response:
 
 ```
 0xa51b768f35a9d02151590c419cd32b072fbb3a92871dfcc24021da828f0846e94fd1f24c1a987699e06479262a414d6739f0add1387d6276d7dd8b9099a306501c
 ```