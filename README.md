# Raspberry Pi Cluster Setup And Deploy Guide

## 1. Set Up Your Raspberry Pis

1. Flash the micro SD card for each Raspberry Pi.
2. SSH into each Raspberry Pi and edit the `cmdline.txt` file:
   ```bash
   sudo nano /boot/firmware/cmdline.txt
   ```
3. Add the following to the end of the file:
   ```
   cgroup_memory=1 cgroup_enable=memory
   ```
4. Reboot each Raspberry Pi:
   ```bash
   sudo reboot
   ```

## 2. Set Up the Router

1. Connect a LAN cable from your notebook to the router.
2. Open a web browser and navigate to `192.168.0.1`.
3. Use the following credentials to log in:
   - **Username:** `admin`
   - **Password:** `admin`

## 3. Set Up Your VM Using VirtualBox

- [Details to be added later]
- [Assume the master node is up and running]
- Use the following command to check the IP address of the master node:
  ```bash
  ip a
  ```
- Note the IP address of the master node.

## 4. Set Up Worker Nodes (Raspberry Pis)

1. Connect each Raspberry Pi to a WiFi network that has internet access (not the router). You can do this using:
   ```bash
   sudo nmtui
   ```
2. Test the connection by pinging a public address:

   ```bash
   ping google.com
   ```

   If the ping is successful, proceed to the next step.

3. Join the worker node to the K3s cluster using the following command (replace values as needed):

   ```bash
   curl -sfL https://get.k3s.io | K3S_TOKEN="K10f27456d828aa2f7c3efb681edafd461f1a39ba8a5c8ab65f23e28a1d2f1dce79::server:cee0b7e3a6502d31920c471af04875f8" K3S_URL="https://192.168.0.105:6443" K3S_NODE_NAME="pi1" sh -
   ```

   - **Note:** `pi1` is the name of the worker node, and `192.168.0.105` is the IP address obtained from the `ip a` command.

4. Change the WiFi connection of the Raspberry Pi to the router's network and wait for a moment.

5. On the VM, verify the worker node is added:

   ```bash
   kubectl get nodes
   ```

   You should see the worker node `pi1` listed.

6. Repeat these steps for each Raspberry Pi until all 4 worker nodes are added.

## 5. Deploy

- [Details to be added later]
