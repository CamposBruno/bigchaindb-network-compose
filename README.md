# Bigchaindb Compose Network

In order to setup this three node network you have to execute these steps:

### first:

Make sure you have docker-compose installed and run:

```
docker-compose up
```

### Second:

Once the compose finishes to build the network you have to figure out the tendermint node id for each node. You can run this comand in each of the nodes. 

```
docker exec -it bigchaindb-one tendermint show_node_id 
``` 

Remember to replace `bigchaindb-one` with the node name before run this comand to other nodes.

### Third:

Once you have all the three node ids you must edit the `config.toml` file located under `bigchaindb_one/tendermint/config` and replace the ids of each node ids listed in the `persistent_peers` config

```
persistent_peers = "a0688b7cd3ad7419658bbad67be0b238524ce0f5@bigchaindb-one:26656, 7a16cbd536ba3a070a917e4d529cb435fe61beb4@bigchaindb-two:26656, 891e728004223c440016a2ff92e80913523d9047@bigchaindb-three:26656, "
```

Remember once again to execute this step for every node folder.

### Forth:

You also have to figure out the public key of every node. for this simply look the contents of the file `priv_validator.json` under the folder `bigchaindb_one/tendermint/config` and copy the `value` of the public key.

```
// priv_validator.json
"pub_key": {
    "type": "tendermint/PubKeyEd25519",
    "value": "8tHk1oyUSxLqt3pw6JePSpTnwYTff0k/hJp8SZHGCtA="
  },
```

### Fifth:

Once you have copied all public keys of all three nodes, open the file `genesis.json`under `bigchaindb_one/tendermint/config` and replace the values under the `validators`list

```
/// genesis.json
{
    "pub_key":{
        "type":"tendermint/PubKeyEd25519",
        "value":"8tHk1oyUSxLqt3pw6JePSpTnwYTff0k/hJp8SZHGCtA="
    },
    "power": "10",
    "name":"bigchaindb-one"
},
```

### Sixth:

Finally, once all the three genesis files are updated, simply reload the network

```
docker-compose down && docker-compose up
```

