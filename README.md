# Raspberry Pi Cluster Setup and Deployment Guide

## 1. Set Up Your Raspberry Pis

1. Flash the micro SD card for each Raspberry Pi.
2. SSH into each Raspberry Pi and edit the `cmdline.txt` file:
   ```bash
   sudo nano /boot/firmware/cmdline.txt
   ```
3. Add the following line to the end of the file:
   ```
   cgroup_memory=1 cgroup_enable=memory
   ```
4. Reboot each Raspberry Pi:
   ```bash
   sudo reboot
   ```

## 2. Set Up the Router

1. Connect your notebook to the router (either wireless or wired).
2. Open a web browser and navigate to `192.168.0.1`. (The IP address may vary; check your network settings for the correct address.)s
3. Use the following credentials to log in:
   - **Username:** `admin`
   - **Password:** `admin`

## 3. Set Up Your VM and Master Node Using VirtualBox

1. **Create a New Virtual Machine:**

   - Open VirtualBox and create a new virtual machine.
   - Choose a name for your VM, select the type as **Linux**, and choose the version based on your distribution (e.g., Ubuntu 64-bit).
   - Allocate memory (RAM) according to your system's available resources (recommend at least 2GB).
   - Create a virtual hard disk (VDI) and set its size (at least 20GB recommended).

2. **Install an Operating System:**

   - Attach the installation ISO for your chosen Linux distribution to the VM and start it.
   - Follow the installation steps to install the OS on the virtual machine.

3. **Update the System:**

   - Once the installation is complete, log in to the VM and update the system:
     ```bash
     sudo apt update && sudo apt upgrade -y
     ```

4. **Install K3s on the Master Node:**

   - To set up the VM as the master node, run the following command:
     ```bash
     curl -sfL https://get.k3s.io | sh -
     ```
   - This will install K3s and set up the node as the master (server) node by default.

5. **Verify K3s Installation:**

   - Check the status of the K3s service to ensure it is running:
     ```bash
     sudo systemctl status k3s
     ```
   - If it is not running, you can start it with:
     ```bash
     sudo systemctl start k3s
     ```

6. **Retrieve the Node Token:**

   - You will need the token to add worker nodes to the cluster. Retrieve it using:
     ```bash
     sudo cat /var/lib/rancher/k3s/server/node-token
     ```
   - Save this token for use when configuring worker nodes.

7. **Verify the Master Node:**

   - Use `kubectl` to check the status of the cluster and ensure the master node is up:
     ```bash
     kubectl get nodes
     ```
   - You should see the master node listed as "Ready."

8. **Check the IP Address of the Master Node:**
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
   curl -sfL https://get.k3s.io | K3S_TOKEN="xxxxx::server:xxxxx" K3S_URL="https://192.168.0.105:6443" K3S_NODE_NAME="pi1" sh -
   ```

   - **Note:** `pi1` is the name of the worker node, and `192.168.0.105` is the IP address obtained from the `ip a` command.
   - The `K3S_TOKEN` can be obtained from:
     ```bash
     sudo cat /var/lib/rancher/k3s/server/node-token
     ```

4. Change the WiFi connection of the Raspberry Pi to the router's network and wait for a moment.

5. On the VM, verify the worker node is added:

   ```bash
   kubectl get nodes
   ```

   You should see the worker node `pi1` listed.

6. Repeat these steps for each Raspberry Pi until all 4 worker nodes are added.

## 5. Deploy

- [Details to be added later]
