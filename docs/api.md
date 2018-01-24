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
| `<json data object>` | The parametors that are passed to the method |

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
{"jsonrpc": "2.0", "method": "getTemplate", "params": {"name":<project template name>}, "id": 1}
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
    "template": {
      name: <template name>
    },
		"data": <project data>,
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
{"jsonrpc": "2.0", "method": "listForDID", "params": {"did":<did>}, "id": 1}
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
{"jsonrpc": "2.0", "method": "getTemplate", "params": {"type": <template type>, "name": <name of template>}, "id": 1}
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
{"jsonrpc": "2.0", "method": "getTemplate", "params": {"name": <agent template name>}, "id": 1}
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
    "template": {
      name: <template name>
    },
		"data": <agent data>,
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
        "did": <cedator's did>
        "_id": "5a66e09b38f45f01d90d122a",
        "created": "2018-01-23T07:13:31.060Z"
    }
}
```

#### List Agents for a specific DID

Lists all agents for a specific DID.

Request:

```
{"jsonrpc": "2.0", "method": "listForDID", "params": {"did":<did>}, "id": 1}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": <array of agents>
}
```

#### List Agents for a specific DID

Lists all agents for a specific DID.

Request:

```
{"jsonrpc": "2.0", "method": "listForProject", "params": {"projectTx":<project TX>}, "id": 1}
```

Response:

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": <array of agents>
}
```

## ixo-Module API

#### Install using npm

`npm install --save ixo-module`

####Create new Ixo Object
```js
import Ixo from 'ixo-module';
var ixo = new Ixo('ixo_node_url')
```

#### CryptoUtil

**Generate Mnemonic**

Request:
```js
ixo.cryptoUtil.generateMnemonic()
```

Response:
```js
dilemma allow swamp hedgehog client reject mistake spell involve index panda course
```
**Generate SovrinDID**

Request:
```js
ixo.cryptoUtil.generateSovrinDID(mnemonic)
```

Response:
```js
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
```js
ixo.cryptoUtil.getDocumentSignature(sdid.secret.signKey, sdid.verifyKey, JSON.stringify(testJson))
```

Response:
```js
DjoT6XqQ53J2kR4zd1shB17qFuM9DM5A2DAxQ3jjtgacvvpafWefHx54kkHewMVMTAsZm61wDCtzMV2TwkL7Fc5YdTc898X4tQ8SepthqyFBdMVs8fAt3fWDGD1fiVe5cymPCDcHwB6hP34DpQB3UAcfZSoPP2wxCbCLhTAF25RywqEWmcMDqF42pEqa9RonpF6AYGxYQt2tUKT9383HR6RhCkkbrkJSBwYQ6b4jsnysz23p4TPfahPKGWinGahXFwtZKD69SSjipzQNHWFXb5YuoqQcCToTFcEteQ3dtkDQdCcWFZ9N1
```

**Validate Signature**

Request:
```js
ixo.cryptoUtil.verifyDocumentSignature(signature, sdid.verifyKey)
```

Response:
```js
true/false
```

#### Network
   
**Ping ixo Server Node**
   
Request:
   ```js
   
   ixo.network.pingIxoServerNode().then((result) => {
       console.log('Ping Results: ' + result)
   })
   
   ```
   
Response:
 ```js
Ping Results: {
             "jsonrpc": "2.0",
             "id": 1,
             "result": "pong"
     }
```
     
#### Projects
   
   **Gets project template**
   
   Request:
   ```js
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
   
   ```js
   ixo.project.listProjects().then((result) => {
       console.log('Project List:  ' + result)
   })
   
   ```
   
Response:

```js
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
   
 