<!--
title: "Docker Engine monitoring with Netdata"
custom_edit_url: https://github.com/netdata/netdata/edit/master/collectors/python.d.plugin/dockerd/README.md
sidebar_label: "Docker Engine"
-->

# Docker Engine monitoring with Netdata

Collects docker container health metrics.

**Requirement:**

-   `docker` package, required version 3.2.0+

Following charts are drawn:

1.  **running containers**

    -   count

2.  **healthy containers**

    -   count

3.  **unhealthy containers**

    -   count

## Configuration

Edit the `python.d/dockerd.conf` configuration file using `edit-config` from the Netdata [config
directory](/docs/configure/nodes.md), which is typically at `/etc/netdata`.

```bash
cd /etc/netdata   # Replace this path with your Netdata config directory, if different
sudo ./edit-config python.d/dockerd.conf
```

```yaml
 update_every : 1
 priority     : 60000
```




### Troubleshooting

To troubleshoot issues with the `dockerd` module, run the `python.d.plugin` with the debug option enabled. The 
output will give you the output of the data collection job or error messages on why the collector isn't working.

First, navigate to your plugins directory, usually they are located under `/usr/libexec/netdata/plugins.d/`. If that's 
not the case on your system, open `netdata.conf` and look for the setting `plugins directory`. Once you're in the 
plugin's directory, switch to the `netdata` user.

```bash
cd /usr/libexec/netdata/plugins.d/
sudo su -s /bin/bash netdata
```

Now you can manually run the `dockerd` module in debug mode:

```bash
./python.d.plugin dockerd debug trace
```

