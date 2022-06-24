# Local ScylladB Cluster with Docker Compose

Sets up a local ScyllaDB v4.5 cluster with 3 nodes.

To add new nodes, just modify the `docker-compose.yml` file and add another ScyllaDB instance,
then point the new instance name to the `--seeds` command value on other nodes. Then you are
good to go.

To run it:

```sh
docker compose up -d
```

There is no authentication being set up. If you want to enable authentication, change `scylla.yaml`
file on line 237 to `PasswordAuthenticator` and line 249 to `CassandraAuthorizer`. You should read
Cassandra or ScyllaDB documentation about creating a username-password combination.

Obviously don't use it on a production environment.

To connect with Go:

```go
package main

import (
    "time"
    
    "github.com/gocql/gocql"
)

func main() {
    session := gocql.NewSession(gocql.ClusterConfig{
        Hosts:          []string{"localhost:9042", "localhost:9043", "localhost:9044"},
        Timeout:        3 * time.Second,
		ConnectTimeout: 3 * time.Second,
        Consistency:    gocql.Quorum,
    })
    defer session.Close()

    // do things with session
}
```