---
layout: post
title: Planet 如何使用自定义域名
date: 2022-09-23 08:41:59 +0800
tags:
  - web3
  - 区块链
  - ipfs
categories:
  - 学习笔记
---

![planet](/assets/planet-follow.png)

目前 Planet（9.0.2）的订阅功能支持 .eth 和 .bit 这两种基于区块链的域名系统。

它们实际上是将 IPNS 写入了区块链，IPFS 客户端已经集成了 .eth 的解析，所以 Planet 自然也就支持 .eth 了。而 .bit 则需要自行集成，Planet 先通过 [indexer](https://github.com/dotbitHQ/das-account-indexer) 节点查询区块链数据，得到 IPNS 记录，完成订阅。

除此之外，其实 IPFS 支持任意域名作为 IPNS。因为 DNS 本身就是一个解析服务，只要使用 TXT 记录指向 IPNS，IPFS 客户端就可以通过域名解析得到 IPNS。

```
$ ipfs resolve -r /ipns/laijiawei.com
/ipfs/bafybeid4lvdhridsvwbruokzbmlnlj7n2rpifo5z53fpie3b7vuzw2hqeu
```

这个标准称为 [DNSLink](https://dnslink.io/)，同时一些公共 Gateway 也[支持 DNSLink](https://docs.ipfs.tech/concepts/ipfs-gateway/#dnslink) ，比如

`https://www.cloudflare-ipfs.com/ipns/laijiawei.com/`

重点来了，支持 DNSLink 的 Gateway 还可以接受 CNAME 解析，于是 `laijiawei.com` 就可以直接使用了。

我在实现 [bit.cc Gateway](https://github.com/paicha/daslink) 的时候为了最简化（偷懒），也是使用 DNSLink，只需要维持两条记录：

```
laijiawei.com CNAME cloudflare-ipfs.com
_dnslink.laijiawei.com TXT "dnslink=/ipns/k51qzi5uqu5dicntuik6dvxxkjyvz4sya18svfe15z3vor3or65yttiase3np7"
```

That’s all.
