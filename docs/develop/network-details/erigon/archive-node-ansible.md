---
id: archive-node-ansible
title: Set up an Archive Node with Ansible
sidebar_label: Set up an Archive Node with Ansible
description: "Using Ansible to set up an archive node."
keywords:
  - erigon
  - archive
  - node
  - ansible
image: https://matic.network/banners/matic-network-16x9.png
---

:::tip

To set the Archive Node, you need to follow the same process for a [<ins>full node deployment</ins>](https://docs.polygon.technology/docs/develop/network-details/full-node-deployment). However, it requires a minor config change. You should include the following parameter at the `start.sh` file:

```makefile
--gcmode 'archive'
```