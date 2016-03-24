# UP Queue Sender (up-queue-sender)
[![Circle CI](https://circleci.com/gh/Financial-Times/up-queue-sender/tree/master.png?style=shield)](https://circleci.com/gh/Financial-Times/up-queue-sender/tree/master)

__Consumes a JSON payload, wraps it in an FT message and sends it to a queue.__

## Installation

For the first time:

`go get github.com/Financial-Times/up-queue-sender`

or update:

`go get -u github.com/Financial-Times/up-queue-sender`

## Running

The values below also represent the default values: 

```
export|set ADDR=localhost:8080
export|set TOPIC=NativeCmsMetadataPublicationEvents
export|set QUEUE=kafka
$GOPATH/bin/up-queue-sender
```

With Docker:

`docker build -t coco/up-queue-sender .`
`docker run -ti coco/up-queue-sender`

## Endpoints
/message
### PUT
Example:
`curl -X PUT -H "Content-Type: application/json" localhost:8080/message --data '{"uuid":"bba39990-c78d-3629-ae83-808c333c6dbc","canonicalName":"Metals Markets"}'`

## Notes
* the payload MUST have a "uuid" named field on the top level
* the operation is not throttled

# Use Cases

## Bulk V1 metadata republishing
We may need to republish our V1 metadata in bulk, and this app can help with that.
Republishing the V1 metadata means taking the V1 metadata stored in the native store, wrapping it in an FT message and putting it on the kafka queue, on the topic on which the V1 metadata publish event enter our system.

### To do a republish: 

1. `fleetctl ssh up-queue-sender-v1-metadata@1.service`
2. `` export SENDER_PORT=`docker ps | grep up-queue-sender | awk '{print $1}' | xargs docker port | cut -d":" -f2` ``
3. `export MESSAGES_PER_SECOND=20`
4. ``` docker run coco/up-restutil /up-restutil dump-resources --throttle=$MESSAGES_PER_SECOND http://$HOSTNAME:8080/__nativerw/v1-metadata/ |  \
       docker run -i coco/up-restutil /up-restutil put-resources uuid http://$HOSTNAME:$SENDER_PORT/message`
   ```

### See 

* [up-restutil](https://github.com/Financial-Times/up-restutil)
* [nativerw](https://github.com/Financial-Times/nativerw)
