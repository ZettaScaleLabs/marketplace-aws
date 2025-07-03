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
curl http://{$IP}:9000/api/v1/plugins
```

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
     -d '{key_expr:"demo/example/**",strip_prefix:"demo/example",volume: {id: "fs",dir:"example"}}' \
     http://{$IP}:8000/@/local/router/config/plugins/storage_manager/storages/demo
```


### 4. Retrieve Configuration File
To verify the configuration, retrieve the zenoh.json file:
```bash
curl http://{$IP}:8000/demo/example/zenoh.json
```
