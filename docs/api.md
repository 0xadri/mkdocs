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
{"jsonrpc": "2.0", "method": "getTemplate", "params": {"name":<project name>}, "id": 1}
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
{"jsonrpc": "2.0", "method": "getTemplate", "params": {"name":<template name>}, "id": 1}
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



