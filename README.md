# Zetta Plan
--- 
|  **Zetta**     |  **OSS**         | :cloud: **AMI Marketplace**  |  :cloud: **Zetta Kubernetes** | :cloud: **Zetta EC2** | **ZC2 Tools**
| :-------------:| :--------------: | :---------------------------:| :----------------------------:|:---------------------:|:---------------: | 
| Auto-Scaling   | Self Managed     |    Self Managed              |      :white_check_mark:       |     :no_entry_sign:   |:no_entry_sign: 
| Network isolation   | Self Managed     |    Self Managed         |      :no_entry_sign:       |      :white_check_mark:    |Self Managed
|  **Authentification** 
| Mtls   | :white_check_mark:     |   :white_check_mark:       |      :white_check_mark:     |      :white_check_mark:    |:white_check_mark:
| User/password   | :white_check_mark:     |   :white_check_mark:       |      :white_check_mark:     |      :white_check_mark:    |:white_check_mark: 
|  **Features** 
| ACL   | :white_check_mark:     |   :white_check_mark:       |     :white_check_mark:   |      :white_check_mark:   |:white_check_mark: 
| Admin   | :no_entry_sign:     |   :white_check_mark:       |      Partial     |      Partial    |:white_check_mark: 
| DDS   | :white_check_mark:     |   :white_check_mark:       |      :no_entry_sign:    |      :no_entry_sign:    |:no_entry_sign: 
| MQTT   | :white_check_mark:     |   :white_check_mark:       |      :white_check_mark:    |      :white_check_mark:    |:white_check_mark: 
| Record & replay   | :white_check_mark:     |   :white_check_mark:       |      :no_entry_sign:      |      :no_entry_sign:    |:no_entry_sign: 
| rocksdb   | :white_check_mark:     |   :white_check_mark:       |      :no_entry_sign:   |      :no_entry_sign:    |:no_entry_sign: 
| Stats   | :white_check_mark:     |   :white_check_mark:       |      :white_check_mark:    |      :white_check_mark:    |:white_check_mark: 
|  **Storage** 
| File system   | :white_check_mark:     |   :white_check_mark:       |      :no_entry_sign:    |     :no_entry_sign:   |:no_entry_sign: 
| Influxdbv1   | :white_check_mark:     |   :no_entry_sign:        |     :no_entry_sign:      |     :no_entry_sign:      |:no_entry_sign:  
| Influxdbv2   | :white_check_mark:     |   :white_check_mark:       |      :white_check_mark:    |      :white_check_mark:    |:white_check_mark: 
| Memory   | :white_check_mark:     |   :white_check_mark:       |      :white_check_mark:    |      :white_check_mark:    |:white_check_mark:
| Minio   | :white_check_mark:     |   :white_check_mark:       |      :white_check_mark:    |       :white_check_mark: |:white_check_mark:
| S3   | :white_check_mark:     |   :white_check_mark:       |       :white_check_mark:   |      :white_check_mark:   |:white_check_mark:
| Shared memory   | :white_check_mark:   |   :white_check_mark:       |    :no_entry_sign:   |    :no_entry_sign:   |:no_entry_sign: 


--- 
# Zenoh Plugin Configuration Guide

This guide provides usage instructions for configuring the **Zenoh** router with the `fs` storage plugin.

---
## Prerequisite

Ensure that ports 7447, 8000 and 9000 are accessible.

The Zenoh service must be running to apply these commands.

## 🔧 Usage Instructions

### 1. List Active Plugins

This command lists the plugins currently activated on the server:

```bash
curl http://{$IP}:9000/api/v1/config/plugins
```

#### Output : 
<html>
     <body>
          <pre style="word-wrap: break-word; white-space: pre-wrap;">["admin","remote_api","rest","rmw_zenoh"]</pre>
     </body> 
</html>

### 2.  Activate and Configure the fs Storage Plugin
Use the following command to activate and configure the filesystem-based (fs) storage plugin:
```bash
curl -X PUT \
     -H 'content-type:application/json' \
     -d '{}' \
     http://{$IP}:8000/@/local/router/config/plugins/storage_manager/volumes/fs -v
```


### 3. Map Keys to a Storage Volume
This command maps keys of type demo/example/** to volume fs:
```bash
curl -X PUT \
     -H 'content-type:application/json' \
     -d '{key_expr:"demo/example/**",strip_prefix:"demo/example",volume: {id: "fs",dir:""}}' \
     http://{$IP}:8000/@/local/router/config/plugins/storage_manager/storages/demo
```


### 4. Retrieve Configuration File
To verify the configuration, retrieve the zenoh.json file:
```bash
curl http://{$IP}:8000/demo/example/zenoh.json
```

#### Output :

<html>
  <body>
    <dl>
      <dt>demo/example/zenohd_env.conf</dt>
      <dd>RUST_LOG=z=info</dd>
      <dt>demo/example/zenoh.json</dt>
      <dd>
{
  "id": "fc2d814f3dcf42b081b8bc54e2c08ffd",
  "listen": {
    "endpoints": [
      "tcp/[::]:7447"
    ]
  },
  "metadata": {
    "zenoh_version": "1.4.0"
  },
  "scouting": {
    "timeout": 3000,
    "delay": 200,
    "multicast": {
      "enabled": false
    },
    "gossip": {
      "enabled": false,
      "multihop": false
    }
  },
  "adminspace": {
    "permissions": {
      "read": true,
      "write": true
    }
  },
  "plugins": {
    "rest": {
      "http_port": 8000
    },
    "remote_api": {
      "websocket_port": "10000"
    },
    "rmw_zenoh": {},
    "admin": {
      "__required__": true,
      "address": "[::]",
      "port": 9000,
      "configuration_file": "/opt/zenohd/zenoh.json"
    }
  }
}
      </dd>
    </dl>
  </body>
</html>


