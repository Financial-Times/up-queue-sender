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