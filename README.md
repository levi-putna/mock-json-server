# Simple Static Mock Json Server

Create a simple mock JSON API server using a single json file to use when developing or automated testing against an predictable response.

This service extends the work by [@ajoelp](https://www.npmjs.com/~ajoelp) on the [mock-json-server](https://www.npmjs.com/package/mock-json-server) to add support to allow for customisation of the response headers and status code.

##Installation

To install you need to have NodeJS and NPM installed on your system [https://nodejs.org/en/](https://nodejs.org/en/)
Install the mock-json-server package by running `yarn add static-api-mock --dev`

**Thats it!**

## Example

To run the server adding `"start:mock": "static-api-mock db.json --port=3000"` to your package.json scripts.

```json
"scripts": {
    "start": "echo \"Some server start script\" && exit 1",
    "test": "echo \"Some test start script\" && exit 1",
    "start:mock": "mock-json-server db.json --port=3000"
  }
```

then run `yarn start:mock` this starts a server on http://localhost:3000/

## Config

You need to specify a config file (defaults to `db.json`). Use the `db.json` to mock out the desired responses. The file uses the following format.

```json
{
  "path": {

    "request method": {
      "status": 200,
      "header": {
        "header name": "header value"
      },
      "body": {
        "json":"body content"
      }
    }
  }
}
```

### Example db.json

```json
{
  "/user": {

    "get": {
      "status": 200,
      "header": {
        "Authorization": "Bearer 123ABC"
      },
      "body": [{
        "id": 1,
        "given_name": "Jo",
        "family_name": "Active",
        "active": true
      },{
        "id": 2,
        "given_name": "Sam",
        "family_name": "Inactive",
        "active": false
      }]
    },

    "put": {
      "status": 200,
      "body": {
        "id": 3,
        "given_name": "New",
        "family_name": "User",
        "active": true
      }
    }
  },

  "/user/1": {

    "get": {
      "status": 200,
      "header": {
        "example": "123ABC"
      },
      "body": {
        "id": 1,
        "given_name": "Jo",
        "family_name": "Active",
        "active": true
      }
    },

    "post": {
      "status": 200,
      "body": {
        "id": 1,
        "given_name": "Jo",
        "family_name": "Update",
        "active": false
      }
    },

    "delete": {
      "status": 201
    }

  }
}
```

A Get Request to http://localhost:8000/user will return.

```json
    [{
      "id": 1,
      "given_name": "Jo",
      "family_name": "Active",
      "active": true
    },{
      "id": 2,
      "given_name": "Sam",
      "family_name": "Inactive",
      "active": false
    }]
```

A Post Request to http://localhost:8000/user will return.

```json
    {
      "id": 3,
      "given_name": "New",
      "family_name": "User",
      "active": true
    }
```
