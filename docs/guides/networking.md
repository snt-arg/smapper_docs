# Networking

Proper network configuration is essential for reliable and high-throughput communication, especially when working with data-intensive sensors like LiDAR and multiple cameras. To address this, we follow the network setup recommended by the [Autoware Foundation](https://autowarefoundation.github.io/autoware-documentation/main/), which is tailored for ROS 2 systems with demanding real-time communication needs.

We use **CycloneDDS** as our ROS 2 DDS middleware implementation, with additional system-level tuning to handle the high bandwidth requirements of our sensors.

## Autoware-Based Configuration

!!! info

    The steps below are based on the Autoware documentation. For the original guide, refer to the official [Autoware DDS settings page](https://autowarefoundation.github.io/autoware-documentation/main/installation/additional-settings-for-developers/network-configuration/dds-settings/).

### Install and Configure CycloneDDS

1. **Install CycloneDDS for ROS 2 (Humble):**

```bash
sudo apt install ros-humble-rmw-cyclonedds-cpp
```

2. **Create a CycloneDDS configuration file** at `~/cyclonedds.xml` with the following contents:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<CycloneDDS xmlns="https://cdds.io/config" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://cdds.io/config https://raw.githubusercontent.com/eclipse-cyclonedds/cyclonedds/master/etc/cyclonedds.xsd">
  <Domain Id="any">
    <General>
      <Interfaces>
        <NetworkInterface autodetermine="false" name="lo" priority="default" multicast="default" />
      </Interfaces>
      <AllowMulticast>default</AllowMulticast>
      <MaxMessageSize>65500B</MaxMessageSize>
    </General>
    <Internal>
      <SocketReceiveBufferSize min="10MB"/>
      <Watermarks>
        <WhcHigh>500kB</WhcHigh>
      </Watermarks>
    </Internal>
  </Domain>
</CycloneDDS>
```

3. **Set CycloneDDS as the default RMW implementation**:

```bash
echo "export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp" >> ~/.bashrc
echo "export CYCLONEDDS_URI=file:///home/$(whoami)/cyclonedds.xml" >> ~/.bashrc
```

### System-Wide Network Configuration

To support high-throughput data transmission, the following system settings must be adjusted. These changes increase buffer sizes and reduce packet fragmentation delays.

**Temporary (session-based) configuration:**

```bash
# Increase the maximum receive buffer size for network packets
sudo sysctl -w net.core.rmem_max=2147483647  # 2 GiB, default is 208 KiB

# IP fragmentation settings
sudo sysctl -w net.ipv4.ipfrag_time=3        # in seconds, default is 30 s
sudo sysctl -w net.ipv4.ipfrag_high_thresh=134217728  # 128 MiB, default is 256 KiB
```

**Permanent configuration:**

Create or edit `/etc/sysctl.d/10-cyclone-max.conf` with the following:

```conf
# Increase the maximum receive buffer size for network packets
net.core.rmem_max=2147483647  # 2 GiB, default is 208 KiB

# IP fragmentation settings
net.ipv4.ipfrag_time=3  # in seconds, default is 30 s
net.ipv4.ipfrag_high_thresh=134217728  # 128 MiB, default is 256 KiB
```

**Validate the settings:**

```bash
sysctl net.core.rmem_max net.ipv4.ipfrag_time net.ipv4.ipfrag_high_thresh

# Output
# net.core.rmem_max = 2147483647
# net.ipv4.ipfrag_time = 3
# net.ipv4.ipfrag_high_thresh = 134217728
```

### Enable Multicast on Loopback

**Temporary (until reboot):**

```bash
sudo ip link set lo multicast on
```

**Permanent via systemd:**

1. Create a systemd unit file at `/etc/systemd/system/multicast-lo.service`:

```toml
[Unit]
Description=Enable Multicast on Loopback

[Service]
Type=oneshot
ExecStart=/usr/sbin/ip link set lo multicast on

[Install]
WantedBy=multi-user.target
```

2. Apply and enable the service:

```bash
sudo systemctl daemon-reload
sudo systemctl enable multicast-lo.service
sudo systemctl start multicast-lo.service
```

## Setup Ouster (Needed for PTP Support)

```bash
sudo apt install dnsmask
```

edit file on `/etc/dnsmask.d/ouster.conf` and add the following

```bash
port=0
interface=eth0
bind-interfaces
dhcp-range=192.168.100.10,192.168.100.100,12h
log-dhcp
```

Configure the link `eth0` ipv4 to have manual, ip `192.168.100.1` and 24 as the netmask, or `255.255.255.0`

```bash
sudo systemctl restart dnsmasq
```

connect the ouster, and use `arp -a` to find ouster IP. The usual os-id.local should also work.
