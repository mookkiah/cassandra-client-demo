# Cassandra Java Client Demonstration

## Setting up cassandra for development

### Using docker

Starting up a single node cassandra cluster
```bash
docker run -it --rm --name cassandra-node1 -p7000:7000 -p7001:7001 -p9042:9042 -p9160:9160 cassandra
```

Connect using cassandra client
```bash
docker run -it --rm -e CQLSH_HOST=$(docker inspect --format='{{ .NetworkSettings.IPAddress }}' cassandra-node1) --name cassandra-client --entrypoint=cqlsh cassandra
```
This brings up a cqlsh command line `cqlsh>`

Note this also tells the versions of client and cassandra.
```
Connected to Test Cluster at 172.17.0.2:9042.
[cqlsh 5.0.1 | Cassandra 3.11.9 | CQL spec 3.4.4 | Native protocol v4]
```


Create a keyspace
```cassandraql
create keyspace userkeysabase with replication = {'class':'SimpleStrategy','replication_factor' : 2};
```

Telling future commands to use this keyspace
```cassandraql
cqlsh> use userkeysabase;
```
It changes the prompt to `cqlsh:userkeysabase>`

Create a table
```cassandraql
cqlsh:userkeysabase> create table usertable (userid int primary key, usergivenname varchar, userfamilyname varchar, userprofession varchar);
```

Insert some rows(records) into the table

```cassandraql
cqlsh:userkeysabase> insert into usertable (userid, usergivenname, userfamilyname, userprofession) values (1, 'John', 'Smith', 'Software Engineer');
```

There are many [cassandra client drivers](https://cassandra.apache.org/doc/latest/getting_started/drivers.html) in java.
In this project we use [Datastax Java driver](https://github.com/datastax/java-driver).

At the time of this project creation [latest com.datastax.oss:java-driver-core](https://mvnrepository.com/artifact/com.datastax.oss/java-driver-core) library version is 4.10.0



