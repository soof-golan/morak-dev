---
title: "Scale is a hard problem"
author: Gepetto Quattro
date: 2023-09-27T21:29:07+03:00
draft: false
---

I have heavily relied on MongoDB in the past, running several hundred shards spanning several thousand servers. The database was handling billions of writes per day and storing petabytes of data.

While MongoDB has a lot of virtues, the experience also entailed some severe pain points. To start, I'll talk about what worked well. MongoDB is quite straightforward to "get up and running". Its document model allows for extremely flexible data storage, and you can horizontally scale relatively easily with sharding. When we first started up, MongoDB's ability to elastically scale and handle a mix of structured and unstructured data was a game changer for us.

However, once our data had grown into the petabyte realm, "the cracks started to show" so to speak. One key issue we ran into was a lag in the replication. In an environment where real-time, or near real-time data is a requirement, we couldn't afford a slave delay of 40 seconds or more. Theoretically, the voting system for electing the primary node among the replica set should work well, however, in practice, when you encounter network partitions often, nodes may be down and up which causes the constant switching of primaries, hence the lag time in replication.

Another problem was data corruption. We had a couple of instances where we encountered unexplained data loss and corruption. After several postmortems, we concluded that the loss was due to MongoDB's default write concern, which acknowledged a write operation after it had been sent to the primary server, but before it had been replicated to secondary nodes. We solved this by changing the write concern to "majority" to ensure that data was replicated before a write was acknowledged.

However, the largest battle we consistently fought was around scaling and performance. As MongoDB grew, the balancer, which is responsible for managing shard distribution, could not keep up with the volume of data we were dealing with. The auto-balancing feature struggled to work correctly with our large datasets and high traffic volume. In addition, the more we sharded, the more complex it became to manage the database. There simply became too many moving parts which made managing the system as a whole incredibly complex.

Moreover, when we needed to perform maintenance or upgrades on the database, this often resulted in increasing lag in the oplog. It would take hours, sometimes even days, to catch up, which severely impacted our real-time data delivery.

Memory usage was another problem. MongoDB uses the system’s memory to store frequently accessed data and indexes. As our documents increased in complexity and quantity, the native memory-mapped storage engine’s performance dropped.

Despite these issues, MongoDB provided an excellent platform for us to scale and perform at high levels. As long as we continued to push through the pain points with creative problem-solving and constant system adjustments. It taught me a whole lot about managing databases at scale, and for that, I wouldn't change my choice to use MongoDB.

In retrospect, many of the issues we faced were not at fault of MongoDB itself but rather the massive scale at which we were trying to operate. Today, there are managed MongoDB services such as MongoDB Atlas that perhaps could offload some of these issues we experienced. However, for our level of control, self-managing was the best decision.