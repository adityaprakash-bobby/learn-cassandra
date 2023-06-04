# Getting Started

## Get image for Cassandra
```bash
docker pull cassandra:latest
```

## Create separate network for Cassandra
```bash
docker network create cassandra
```

## Spin up Cassandra container
```bash
docker run --rm -d --name cassandra --hostname cassandra --network cassandra cassandra
```

## Running the scripts
```bash
docker run --rm --network cassandra -v "$(pwd)/scripts/data.cql:/scripts/data.cql" -e CQLSH_HOST=cassandra -e CQLSH_PORT=9042 -e CQLVERSION=3.4.6 nuvo/docker-cqlsh
```
In this step, you might get an error regarding connection not being established:
```
Checking connection to cassandra...
Can't establish connection, will retry again in 1 sconds
Can't establish connection, will retry again in 2 sconds
Can't establish connection, will retry again in 3 sconds
Can't establish connection, will retry again in 4 sconds
Can't establish connection, will retry again in 5 sconds
Failed to connect to cassandra at cassandra:9042
```
This might happen due to the version mismatch of the CQL version. If so, you can run the next step to know the compactible version of CQL and decide on the CQL version to use.

## Interactive shell
```bash
docker run --rm -it --network cassandra nuvo/docker-cqlsh cqlsh cassandra 9042 --cqlversion='3.4.6'
```

Inside the CQLshell, run the below statements to playaround the data:
```cql
SELECT * FROM store.shopping_cart;
INSERT INTO store.shopping_cart (userid, item_count) VALUES ('4567', 20);
```

## Cleanup
```bash
docker kill cassandra
docker network rm cassandra
```