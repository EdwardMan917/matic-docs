---
id: archive-node-binaries
title: Set up an Archive Node with Binaries
sidebar_label: Set up an Archive Node with Binaries
description: "Using binaries to set up an archive node."
keywords:
  - erigon
  - archive
  - node
  - binary
image: https://matic.network/banners/matic-network-16x9.png
---

:::tip

To set the Archive Node, you need to follow the same process for a [<ins>deploying a full node with binaries</ins>](https://docs.polygon.technology/docs/develop/network-details/full-node-binaries). However, it requires a minor config change. You should include the following parameter at the `start.sh` file:

```makefile
--gcmode 'archive'
```