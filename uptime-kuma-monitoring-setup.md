
## Uptime Kuma Monitoring Setup

This guide will help you set up Uptime Kuma, a self-hosted monitoring tool, on MikroTik CHR using containers.

### Steps

1. **Create a bridge for the containers:**

    ```shell
    /interface/bridge/add name=dockers
    ```

2. **Assign an IP address to the bridge:**

    ```shell
    /ip/address/add interface=dockers address=172.17.0.1/16
    ```

3. **Add a virtual Ethernet (veth) interface:**

    ```shell
    /interface/veth/add address=172.17.0.6/16 gateway=172.17.0.1
    ```

4. **Add the veth interface to the bridge:**

    ```shell
    /interface/bridge/port/add bridge=dockers interface=vethX
    ```

5. **Set up container mounts for Uptime Kuma data:**

    ```shell
    /container/mounts add name=uptime-kuma-data src=uptime-kuma/ dst=/app/data
    ```

6. **Add the Uptime Kuma container:**

    ```shell
    /container/add remote-image=louislam/uptime-kuma:latest interface=vethX workdir=uptime-kuma/ hostname=uptime-kuma mounts=uptime-kuma-data
    ```
7. **Set up port forwarding for Uptime Kuma:**

    ```shell
    /ip firewall nat add action=dst-nat chain=dstnat dst-port=3001 in-interface=ether1 protocol=tcp to-addresses=172.17.0.6 to-ports=3001
    ```
    
## License 📄

Licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

Contributions and suggestions are welcome. Happy configuring! 🎉🚀
