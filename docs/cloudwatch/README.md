 Zenoh Plugin Configuration Guide

This guide provides usage instructions for configuring the **Zenoh** router with the `fs` storage plugin.

---
## Prerequisite

Ensure that ports 7447, 8000 and 9000 are accessible.

The Zenoh service must be running to apply these commands.


## 🔧 Push logs to Cloudwatch

> ℹ️  **Info**  
> This AMI is preconfigured to push logs to a cloudwatch log group with the name zenoh.
> Configuration file is available ZENOH_HOME (/) 

### 1. Attach instance profile to EC2

Your EC2 must be linked to an instance-profile role with the policy:
(https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/QuickStartEC2Instance.html)
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents",
                "logs:DescribeLogGroups",
                "logs:DescribeLogStreams"
            ],
            "Resource": "*"
        }
    ]
}
```

## 🔧 Configure Cloudwatch Agent


> ⚠️ **Careful:**  All configurations defined by the administration port will be deleted when the zenoh service is restarted.


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

### 3. Create a CloudWatch Agent Configuration File Template

#### File:
cloudwatch.json

```
{
  "logs": {
    "logs_collected": {
      "files": {
        "collect_list": [
          {
            "file_path": "/var/log/zenoh/zenohd.log",
            "log_group_name": "custom_name",
            "log_stream_name": "zenoh.log",
            "timezone": "UTC"
          },
          {
            "file_path": "/var/log/zenoh/zenohd.err",
            "log_group_name": "custom_name",
            "log_stream_name": "zenoh.err",
            "timezone": "UTC"
          }
        ]
      }
    }
  }
}
```

### 4. Push the configuration

```bash
curl -X PUT -d @cloudwatch.json  http://{$IP}:8000/demo/example/cloudwatch.json
```

