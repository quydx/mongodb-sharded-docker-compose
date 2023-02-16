# mongodb-sharded-docker-compose
Setup:  
1. Run  
`docker-compose up`
2. Setup configs servers
`mongosh mongodb://localhost:10001`  
`rs.initiate(
  {
    _id: "cfgrs",
    configsvr: true,
    members: [
      { _id : 0, host : "configs1:27017" },
      { _id : 1, host : "configs2:27017" },
      { _id : 2, host : "configs3:27017" }
    ]
  }
)`    
3. Setup shard servers  
`mongosh mongodb://localhost:20001`    
`rs.initiate(
  {
    _id: "shard1rs",
    members: [
      { _id : 0, host : "shard1s1:27017" },
      { _id : 1, host : "shard1s2:27017" },
      { _id : 2, host : "shard1s3:27017" }
    ]
  }
)`
4. Add Shards to the Cluster  
mongosh  mongodb://localhost:30000
sh.addShard("shard1rs/shard1s1:27017,shard1s2:27017,shard1s3:27017")
sh.status()
5. Enable Sharding for a Database and Collection  
`sh.enableSharding("testdb")`  
`sh.shardCollection("testdb.testtable", { name: "hashed" } )`
6. Try it out:D
