---
aliases:
- /notes/2018/07/08/docker-run-etcd/
categories: notes
date: "2018-07-08T00:00:00Z"
tags:
- etcd
- docker
title: Deploying an etcd cluster as a standalone cluster on docker
---

docker host ip addr `192.168.5.170`

```bash

docker run \
  -p 2379:2379 \
  -p 2380:2380 \
  --volume=/dir/for/etcd/data:/etcd-data \
  --name etcd --rm -d quay.io/coreos/etcd:latest \
  /usr/local/bin/etcd \
  --data-dir=/etcd-data --name node1 \
  --initial-advertise-peer-urls http://192.168.5.170:2380 \
  --listen-peer-urls http://0.0.0.0:2380 \
  --advertise-client-urls http://192.168.5.170:2379 \
  --listen-client-urls http://0.0.0.0:2379 \
  --initial-cluster node1=http://192.168.5.170:2380

```

<!--more-->

with tls certificate.

```bash

docker run --name etcd -d -p 2379:2379 -p 2380:2380 \
 -v /home/tian/workspace/certs:/etcd-ssl-certs-dir \
 -v /home/tian/workspace/etcd:/etcd-data \
  quay.io/coreos/etcd:latest /usr/local/bin/etcd \
    --name my-name \
    --data-dir /etcd-data \
    --listen-client-urls "https://0.0.0.0:2379" \
    --advertise-client-urls "https://hostname:2379" \
    --listen-peer-urls "https://0.0.0.0:2380" \
    --initial-advertise-peer-urls "https://hostname:2380" \
    --initial-cluster "my-name=https://hostname:2380" \
    --initial-cluster-token "tkn" \
    --initial-cluster-state "new" \
    --client-cert-auth \
    --trusted-ca-file "/etcd-ssl-certs-dir/ca.pem" \
    --cert-file "/etcd-ssl-certs-dir/server.pem" \
    --key-file "/etcd-ssl-certs-dir/server-key.pem" \
    --peer-client-cert-auth \
    --peer-trusted-ca-file "/etcd-ssl-certs-dir/ca.pem" \
    --peer-cert-file "/etcd-ssl-certs-dir/server.pem" \
    --peer-key-file "/etcd-ssl-certs-dir/server-key.pem"

```

check

```bash

docker exec -it etcd etcdctl member list

```

test with golang

{% gist 211bd79f3ce5a6e1fde607086a75c46b %}

create golang client with certificate.

```go

//load client certificate.
cert, err := tls.LoadX509KeyPair("./client.pem", "./client-key.pem")
if err != nil {
  panic(err)
}
//load ca certificate.
rootCAPem, err := ioutil.ReadFile("ca.pem")
if err != nil {
  panic(err)
}
caPool := x509.NewCertPool()
ok := caPool.AppendCertsFromPEM(rootCAPem)
if !ok {
  panic("append ca cert failed.")
}

cfg := client.Config{
  Endpoints: []string{"https://hostname:2379"},
  Transport: &http.Transport{
    TLSClientConfig: &tls.Config{
      Certificates: []tls.Certificate{cert},
      RootCAs:      caPool,
    },
  },
  HeaderTimeoutPerRequest: time.Second,
}

```