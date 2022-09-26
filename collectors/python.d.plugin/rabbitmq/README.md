<!--
title: "RabbitMQ monitoring with Netdata"
custom_edit_url: https://github.com/netdata/netdata/edit/master/collectors/python.d.plugin/rabbitmq/README.md
sidebar_label: "RabbitMQ"
-->

# RabbitMQ monitoring with Netdata

Collects message broker global and per virtual host metrics.


Following charts are drawn:

1.  **Queued Messages**

    -   ready
    -   unacknowledged

2.  **Message Rates**

    -   ack
    -   redelivered
    -   deliver
    -   publish

3.  **Global Counts**

    -   channels
    -   consumers
    -   connections
    -   queues
    -   exchanges

4.  **File Descriptors**

    -   used descriptors

5.  **Socket Descriptors**

    -   used descriptors

6.  **Erlang processes**

    -   used processes

7.  **Erlang run queue**

    -   Erlang run queue

8.  **Memory**

    -   free memory in megabytes

9.  **Disk Space**

    -   free disk space in gigabytes


Per Vhost charts:

1.  **Vhost Messages**

    -   ack
    -   confirm
    -   deliver
    -   get
    -   get_no_ack
    -   publish
    -   redeliver
    -   return_unroutable

2. Per Queue charts:

    1. **Queued Messages**

        - messages
        - paged_out
        - persistent
        - ready
        - unacknowledged

    2. **Queue Messages stats**

        -   ack
        -   confirm
        -   deliver
        -   get
        -   get_no_ack
        -   publish
        -   redeliver
        -   return_unroutable

## Configuration

Edit the `python.d/rabbitmq.conf` configuration file using `edit-config` from the Netdata [config
directory](/docs/configure/nodes.md), which is typically at `/etc/netdata`.

```bash
cd /etc/netdata   # Replace this path with your Netdata config directory, if different
sudo ./edit-config python.d/rabbitmq.conf
```

When no configuration file is found, module tries to connect to: `localhost:15672`.

```yaml
socket:
  name     : 'local'
  host     : '127.0.0.1'
  port     :  15672
  user     : 'guest'
  pass     : 'guest'
```



### Per-Queue Chart configuration

RabbitMQ users with the "monitoring" tag cannot see all queue data. You'll need a user with read permissions. 
To create a dedicated user for netdata:

```bash
rabbitmqctl add_user netdata ChangeThisSuperSecretPassword
rabbitmqctl set_permissions netdata "^$" "^$" ".*"
```

See [set_permissions](https://www.rabbitmq.com/rabbitmqctl.8.html#set_permissions) for details.

Once the user is set up, add `collect_queues_metrics: yes` to your `rabbitmq.conf`:

```yaml
local:
  name                   : 'local'
  host                   : '127.0.0.1'
  port                   :  15672
  user                   : 'netdata'
  pass                   : 'ChangeThisSuperSecretPassword'
  collect_queues_metrics : 'yes'
```
### Troubleshooting

To troubleshoot issues with the `rabbitmq` module, run the `python.d.plugin` with the debug option enabled. The output
will give you the output of the data collection job or error messages on why the collector isn't working.

First, navigate to your plugins directory, usually they are located under `/usr/libexec/netdata/plugins.d/`. If that's not the 
case on your system, open `netdata.conf` and look for the setting `plugins directory`. Once you're in the plugin's directory, switch
to the `netdata` user.

```bash
cd /usr/libexec/netdata/plugins.d/
sudo su -s /bin/bash netdata
```

Now you can manually run the `rabbitmq` module in debug mode:

```bash
./python.d.plugin rabbitmq debug trace
```

