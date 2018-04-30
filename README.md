## TiKV is a distributed Key-Value database powered by Rust and Raft


[![Build Status](https://circleci.com/gh/pingcap/tikv.svg?style=shield&circle-token=36bab0a8e43edb0941b31c38557d2d9d0d58f708)](https://circleci.com/gh/pingcap/tikv) [![Coverage Status](https://coveralls.io/repos/github/pingcap/tikv/badge.svg?branch=master)](https://coveralls.io/github/pingcap/tikv) ![GitHub release](https://img.shields.io/github/release/pingcap/tikv.svg)

TiKV (The pronunciation is: /'taɪkeɪvi:/ tai-K-V, etymology: titanium) is a distributed Key-Value database which is based on the design of Google Spanner and HBase, but it is much simpler without dependency on any distributed file system. With the implementation of the Raft consensus algorithm in Rust and consensus state stored in RocksDB, it guarantees data consistency. Placement Driver which is introduced to implement sharding enables automatic data migration. The transaction model is similar to Google's Percolator with some performance improvements. TiKV also provides snapshot isolation (SI), snapshot isolation with lock (SQL: `SELECT ... FOR UPDATE`), and externally consistent reads and writes in distributed transactions.

TiKV has the following primary features:

- **Geo-Replication:** TiKV uses Raft and Placement Driver to support Geo-Replication.

- **Horizontal scalability:** With Placement Driver and carefully designed Raft groups, TiKV excels in horizontal scalability and can easily scale to 100+ TBs of data.

- **Consistent distributed transactions:** Similar to Google's Spanner, TiKV supports externally-consistent distributed transactions.

- **Coprocessor support:** Similar to Hbase, TiKV implements a coprocessor framework to support distributed computing.

- **Cooperates with [TiDB](https://github.com/pingcap/tidb):** Thanks to the internal optimization, TiKV and TiDB can work together to be a compelling database solution with high horizontal scalability, externally-consistent transations, and support for RDMBS and NoSQL design patterns.


## The TiKV Software Stack

![The TiKV software stack.](images/tikv_stack.png)

- **Placement Driver:** Placement Driver (PD) is the cluster manager of TiKV. PD periodically checks replication constraints to balance load and data automatically.
- **Store:** There is a RocksDB within each Store and it stores data into local disk.
- **Region:** Region is the basic unit of Key-Value data movement. Each Region is replicated to multiple Nodes. These multiple replicas form a Raft group.
- **Node:** A physical node in the cluster. Within each node, there are one or more Stores. Within each Store, there are many Regions.

When a node starts, the metadata of the Node, Store and Region are registered into PD. The status of each Region and Store is reported to PD regularly.


## Your First Test Drive

We have a [Docker Compose](https://github.com/pingcap/tidb-docker-compose/) you can use to test out [TiKV](https://github.com/pingcap/tikv) and [TiDB](https://github.com/pingcap/tidb).

```bash
git clone https://github.com/pingcap/tidb-docker-compose/
cd tidb-docker-compose
docker-compose up -d
```

Shortly after you will be able to connect with `mysql -h 127.0.0.1 -P 4000 -u root` and view the cluster metrics at [http://localhost:3000/](http://localhost:3000/).


## Setting Up a Development Workspace

In order to get started developing on TiKV we suggest you follow the [development guide](https://github.com/pingcap/docs/blob/master/dev-guide/development.md) as you will need to (at least) have `pd-server` working alongside `tikv-server` in order to do integration level testing.

## Deploying To Production

**Use Ansible?** The official [Ansible playbooks](https://github.com/pingcap/tidb-ansible) are the reccomended way of deploying a cluster of TiKV nodes alongside the other components of a cluster.

**Prefer Docker?** You can find a guide on deploying a cluster with [Docker](https://github.com/pingcap/docs/blob/master/op-guide/docker-deployment.md) to run the TiKV and the rest of the cluster.

**Want to roll your own?** That works too! We have written a [Binary Deployment Guide](https://github.com/pingcap/docs/blob/master/op-guide/binary-deployment.md) which might help you as you piece together your own way of doing things.


## Using TiKV

TiKV is a component in the TiDB project. To run TiKV you must build and run it with PD, which is used to manage the cluster.

Currently the only interface to TiKV is the [TiDB Go client](https://github.com/pingcap/tidb/tree/master/store/tikv) and the [TiSpark Java client](https://github.com/pingcap/tispark/tree/master/tikv-client/src/main/java/com/pingcap/tikv).

**We would love it if you developed drivers in other languages.**


### Configuration

Read our configuration guide to learn about the various [configuration options](https://github.com/pingcap/docs/blob/master/op-guide/configuration.md).


## Contributing

See [CONTRIBUTING](./CONTRIBUTING.md) for details on submitting patches and the contribution workflow.


## License

TiKV is under the Apache 2.0 license. See the [LICENSE](./LICENSE) file for details.


## Acknowledgments

- Thanks [etcd](https://github.com/coreos/etcd) for providing some great open source tools.
- Thanks [RocksDB](https://github.com/facebook/rocksdb) for their powerful storage engines.
- Thanks [mio](https://github.com/carllerche/mio) for providing metal IO library for Rust.
- Thanks [rust-clippy](https://github.com/Manishearth/rust-clippy). We do love the great project.
