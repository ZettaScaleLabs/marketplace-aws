 Zenoh Log Level Configuration Guide

This guide provides usage instructions for configuring the **Zenoh** router with the `fs` storage plugin.

> ℹ️  **Info**  
> By default, the logging level is set to ‘info’. 

---
## Prerequisite

Ensure that ports 7447, 8000 and 9000 are accessible.

The Zenoh service must be running to apply these commands.

> ⚠️ **Careful:** All configurations defined by the administration port will be deleted when the zenoh service is restarted.

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
     -d '{key_expr:"demo/example/**",strip_prefix:"demo/example",volume: {id: "fs",dir:""}}' \
     http://{$IP}:8000/@/local/router/config/plugins/storage_manager/storages/demo
```

### 3. Create a Configuration File Template

#### File:
zenohd_env.conf

```
RUST_LOG=z=debug
```

> ⚠️ **Careful:**  Zenoh service will be automatically restarted to process the new value

### 4. Push the configuration

```bash
curl -X PUT -d @zenohd_env.conf  http://{$IP}:8000/demo/example/zenohd_env.conf
``` 
