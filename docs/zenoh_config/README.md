 Zenoh  Configuration Guide

This guide provides usage instructions for configuring the **Zenoh** router with the `fs` storage plugin.

> ℹ️  **Info**  
> By default, the logging level is set to ‘info’. 

---
## Prerequisite

Ensure that ports 7447, 8000 and 9000 are accessible.

The Zenoh service must be running to apply these commands.

### 1.  Activate and Configure the fs Storage Plugin
Use the following command to activate and configure the filesystem-based (fs) storage plugin:
```bash
curl -X PUT \
     -H 'content-type:application/json' \
     -d '{}' \
     http://{$IP}:8000/@/local/router/config/plugins/storage_manager/volumes/fs
```

### 2. Map Keys to a Storage Volume
This command maps keys of type demo/example/** to volume fs:
```bash
curl -X PUT \
     -H 'content-type:application/json' \
     -d '{key_expr:"demo/config/**",strip_prefix:"demo/config",volume: {id: "fs",dir:""}}' \
     http://{$IP}:8000/@/local/router/config/plugins/storage_manager/storages/demo
```

### 3. Create a Configuration File Template

#### File:
zenoh.json

```
{
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
    },
    "storage_manager": {
      "volumes": {
        // configuration of a "fs" volume (the "zenoh_backend_fs" backend library will be loaded at startup)
        "fs": {}
      },
      "storages": {
        "demo": {
          "key_expr": "demo/config/**",
          "strip_prefix": "demo/config",
          "volume": {
            "id": "fs",
            "dir": ""
          }
        }
      }
    }
  }
}
```

### 4. Push the configuration

```bash
curl -X PUT -d @zenoh.json  http://{$IP}:8000/demo/example/zenoh.json
```

### 5. Push the configuration

Restart the service to load the new config

```bash
curl -X PUT  http://{$IP}:9000/api/v1/restart
```

### 6. Retrieve Configuration File

To verify the configuration, retrieve the zenoh.json file:

```bash
curl http://{$IP}:8000/demo/config/zenoh.json
```
