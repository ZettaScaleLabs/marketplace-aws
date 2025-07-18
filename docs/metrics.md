---
## Prerequisite

Ensure that port 9000 is accessible.

## 🔧 Metrics

Zenoh's metrics are available at the following url:

```
http://{$IP}:9000/api/v1/metrics
```

## Exposed Metrics

| Metrics                            | Type              | Description                                                                 |
|----------------------------------|-------------------|-----------------------------------------------------------------------------|
|zenoh_build |string | zenoh_build Information about zenoh|
| tx_bytes                         | counter           | Counter of sent bytes.                                                     |
| tx_t_msgs                        | counter           | Counter of sent transport messages.                                        |
| tx_n_msgs                        | counter           | Counter of sent network messages.                                          |
| tx_n_dropped                     | counter           | Counter of dropped network messages.                                       |
| tx_z_put_msgs                    | counter           | Counter of sent zenoh put messages.                                        |
| tx_z_put_pl_bytes               | counter           | Counter of sent bytes in zenoh put message payloads.                       |
| tx_z_del_msgs                    | counter           | Counter of sent zenoh del messages.                                        |
| tx_z_del_pl_bytes               | counter           | Counter of received bytes in zenoh del message attachments.                |
| tx_z_query_msgs                  | counter           | Counter of sent zenoh query messages.                                      |
| tx_z_query_pl_bytes             | counter           | Counter of sent bytes in zenoh query message payloads.                     |
| tx_z_reply_msgs                  | counter           | Counter of sent zenoh reply messages.                                      |
| tx_z_reply_pl_bytes             | counter           | Counter of sent bytes in zenoh reply message payloads.                     |
| rx_bytes                         | counter           | Counter of received bytes.                                                |
| rx_t_msgs                        | counter           | Counter of received transport messages.                                    |
| rx_n_msgs                        | counter           | Counter of received network messages.                                      |
| rx_n_dropped                     | counter           | Counter of dropped network messages.                                       |
| rx_z_put_msgs                    | counter           | Counter of received zenoh put messages.                                    |
| rx_z_put_pl_bytes               | counter           | Counter of received bytes in zenoh put message payloads.                   |
| rx_z_del_msgs                    | counter           | Counter of received zenoh del messages.                                    |
| rx_z_del_pl_bytes               | counter           | Counter of received bytes in zenoh del message attachments.                |
| rx_z_query_msgs                  | counter           | Counter of received zenoh query messages.                                  |
| rx_z_query_pl_bytes             | counter           | Counter of received bytes in zenoh query message payloads.                 |
| rx_z_reply_msgs                  | counter           | Counter of received zenoh reply messages.                                  |
| rx_z_reply_pl_bytes             | counter           | Counter of received bytes in zenoh reply message payloads.                 |
| rx_downsampler_dropped_msgs     | counter           | Counter of messages dropped by ingress downsampling.                       |
| tx_downsampler_dropped_msgs     | counter           | Counter of messages dropped by egress downsampling.                        |
| rx_low_pass_dropped_bytes       | counter           | Counter of bytes dropped by ingress low-pass filter.                       |
| tx_low_pass_dropped_bytes       | counter           | Counter of bytes dropped by egress low-pass filter.                        |
| rx_low_pass_dropped_msgs        | counter           | Counter of messages dropped by ingress low-pass filter.                    |
| tx_low_pass_dropped_msgs        | counter           | Counter of messages dropped by egress low-pass filter.                     |

