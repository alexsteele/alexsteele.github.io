---
layout: post
title:  "Consensus Algorithms in the Wild"
permalink: /:title/
date:   2022-03-03 00:00:00 -0800
---

I've been getting back into distributed consensus algorithms lately.
One thing I've found interesting is that, despite all the talk in the
literature, you can't easily find a solid paxos implementation out in
the open source jungle.

There are a whole bunch of implementations on github. But most of
them are one-off play things. Not much really solid out there,
except maybe [phxpaxos](https://github.com/Tencent/phxpaxos) by
tencent.

Considering all the interest, I was a little surprised by this. But I
guess it makes sense. Not many systems directly use
consensus primitives. And even if they did, how many paxos
implementations do we need anyways? Even fewer. There are
probably just a small handful of solid, well-tested implementations in
the biggest commercial systems.  We know Google has one, or used to
for the Chubby lock system. I'm sure Amazon has or had one, and
Microsoft probably built its own at some point over the years too.
Maybe a few other tech giants.

Another reason: Raft. [Raft](https://raft.github.io/) is a
consensus algorithm that aims to be easier to understand and implement
than Paxos. It came out in 2014 or so, and there's a few solid
implementations out there in different languages. etcd, the key-value
store used in kubernetes, uses it, and if you look around, you can
spot a bunch of other systems that do too. I particularly like
HashiCorp's implementation in Go, which looks really clean and easy
to work with.

And finally: Apache ZooKeeper. ZooKeeper is a coordination service
based on Google's Chubby. It provides a nice directory-based model for
consensus. Everyone and their dog uses it, or so it seems. It's used
in most commercial distributed systems I've studied over the past few
years. But interestingly, ZooKeeper uses neither paxos nor raft. It
has its own implementation of atomic broadcast based on its specific
needs for high throughput messaging. The
[docs](https://zookeeper.apache.org/doc/r3.4.13/zookeeperInternals.html#sc_atomicBroadcast)
are fairly minimal but still interesting. And the implementaiton is
nestled inside the zookeeper server
[package](https://github.com/apache/zookeeper/tree/master/zookeeper-server/src/main/java/org/apache/zookeeper/server/quorum).
Given its wide usage in big open source projects, including systems
like elasticsearch and (older) versions of kafka, I wouldn't be
surprised if virtually every tech company that runs distributed
systems depends on ZooKeeper somewhere in the stack. Talk about reach.
And it looks like only a small handful of contributors built out the
core pieces.  Brings to mind the classic
[xkcd](https://xkcd.com/2347/).
