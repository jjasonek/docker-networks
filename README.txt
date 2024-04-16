## Using 3rd party lib "axios" to send HTTPS requests

## following fails because of localhost. Of course, there si no DB in the container

docker run --rm --name favorites -p 3000:3000 favorites-node
(node:1) [MONGODB DRIVER] Warning: Current Server Discovery and Monitoring engine is deprecated, and will be removed in a future version. To use the new Server Discover and Monitoring engine, pass option { useUnifiedTopology: true } to the MongoClient constructor.
(Use `node --trace-warnings ...` to show where the warning was created)
MongoNetworkError: failed to connect to server [localhost:27017] on first connect [Error: connect ECONNREFUSED 127.0.0.1:27017
    at TCPConnectWrap.afterConnect [as oncomplete] (node:net:1605:16) {
  name: 'MongoNetworkError'
}]
    at Pool.<anonymous> (/app/node_modules/mongodb/lib/core/topologies/server.js:441:11)

## Lets try without the DB

docker build -t favorites-node .
run -d --rm --name favorites -p 3000:3000 favorites-node

$ curl http://localhost:3000/people | jq

## with Mongo DB installed on localhost

curl -X POST -H "Content-Type: application/json" -d '{"name": "Obi-Wan Kenobi", "type": "character", "url": "https://swapi.dev/api/people/10/"}' http://localhost:3000/favorites
curl http://localhost:3000/favorites | jq


## now let's stop the local MongoDB and let's use a Mongo image.

docker run -d --name mongodb mongo

docker container inspect mongodb
[
    {
        "Id": "a20c3a014bfdfb24786462bbac8e72c563b48b1355b07f5cef97ae1b32ec8128",
        "Created": "2024-04-16T16:51:35.363639887Z",
        "Path": "docker-entrypoint.sh",
        "Args": [
            "mongod"
        ],
       ...
        "NetworkSettings": {
            ...
            "IPAddress": "172.17.0.2",
            ...
        }
    }
]

## That works but it's not convenient.
