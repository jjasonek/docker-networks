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