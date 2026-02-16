# NODE

### Free, Private Bitcoin Testnet Node on Oracle Cloud

This guide provides a complete, step-by-step blueprint for running a private Bitcoin Testnet4 node with an indexer using Umbrel OS on the Oracle Cloud Free Tier. 

This setup is perfect for developers, enthusiasts, or anyone wanting to experiment with Bitcoin applications without any hardware cost or risk to real funds. 

By using Testnet4, the entire blockchain is small enough to fit comfortably within the free tier's storage limits, allowing for fast setup and experimentation.

Here is the high-level process:

1.  **Provision Server:** Create a cost-optimized ARM-based virtual machine and assign it a static public IP.

2.  **Configure Firewall:** Open the necessary port in the cloud firewall and clear the local OS firewall rules.

3.  **Install Umbrel OS:** Deploy the user-friendly Umbrel operating system.

4.  **Install Apps:** Use the Umbrel App Store to install the Bitcoin Node, Electrs, and Tailscale.

5.  **Configure & Connect:** Switch Bitcoin to Testnet4 and use Tailscale for secure, private access.

---

#### 1. Resource Allocation & Server Provisioning

The key to success on the free tier is using resources efficiently. Start with the minimum and scale up only if needed.

*   **Shape:** Use the **Ampere A1 Flex** shape (ARM-based).

*   **CPU:** Start with **1 OCPU**. This is sufficient for Testnet4.

*   **RAM:** Start with **6 GB RAM**. This is enough to run Umbrel, Bitcoin Core, and Electrs smoothly on testnet.

*   **Storage:** Create a **50 GB Block Volume**. This provides ample space for the OS, Umbrel, and the ~10 GB Testnet4 blockchain, leaving plenty of room for growth.

**Critical Step: Create a Reserved Public IP**

This ensures your server's address never changes if you reboot it. This is free and essential for stability.

1.  After creating your instance, navigate to **Compute -> Instances** and click your instance name.

2.  In the Resources section, click **Attached VNICs**, then click your Primary VNIC.

3.  Under the IP Addresses section, click the **...** menu and select **Create Reserved Public IP**.

4.  Give it a name like `umbrel-static-ip` and click **Create**.


#### 2. Required Apps To Be Installed in Umbrel OS

Once Umbrel OS is running, you will install these three apps directly from its App Store.

*   **Bitcoin Node:** The core software for running a Bitcoin node.

*   **Electrs:** An efficient indexing server that allows wallets and apps to quickly query the blockchain.

*   **Tailscale:** A VPN service that creates a secure, private network between your devices and your cloud node, eliminating the need to expose ports to the public internet.

#### 3. Configuring the Firewall

Oracle Cloud has a "double firewall" that often blocks access. You must configure both layers.

**A. Oracle Cloud Console (Network Firewall)**

1.  Navigate to **Networking -> Virtual Cloud Networks**.
2.  Click into your VCN, then into your **Subnet**.
3.  Go to the **Security Lists** tab and click on the Default Security List.
4.  Click **Add Ingress Rules** and create a rule to allow temporary HTTP access for the initial Umbrel setup:
    *   **Source Type:** CIDR
    *   **Source CIDR:** `0.0.0.0/0`
    *   **IP Protocol:** TCP
    *   **Destination Port Range:** `80`
5.  Click **Add Ingress Rules** again.


**B. OS Terminal (Local Firewall)**

The default Oracle OS image has aggressive `iptables` rules that block traffic even if the cloud firewall is open. You must flush them.

1.  SSH into your new instance using its Reserved Public IP.

2.  Run the following commands to clear the existing rules and save the change:
    ```bash
    sudo iptables -F
    sudo netfilter-persistent save
    ```

#### 4. Installation and Configuration Steps

Now that the server is prepared, you can install and configure the software.

1.  **Install Umbrel OS:** Follow the official Umbrel documentation to install the OS on your Oracle instance. This typically involves running a single script from their site.

2.  **Access the Dashboard:** Once installed, access the Umbrel dashboard by navigating to `http://<your-reserved-public-ip>` in your web browser.

3.  **Install Apps:** From the Umbrel App Store, install **Bitcoin Node**, **Electrs**, and **Tailscale**.

4.  **Configure Bitcoin for Testnet4:**
    *   Open the Bitcoin Node app in Umbrel.
    *   Go to **Settings -> Advanced Settings**.
    *   Change the `Network` from `Mainnet` to **`Testnet4`**.
    *   Save the settings. The node will restart and begin syncing the Testnet4 blockchain, which is very fast.

5.  **Configure Tailscale:**
    *   Open the Tailscale app in Umbrel.
    *   It will provide an authentication URL. Click it to log in with your Tailscale account and authorize your new node.
    *   Once connected, you can access your Umbrel dashboard securely using its private Tailscale IP address (e.g., `100.x.x.x`).

6.  **Clean Up Firewall:** For maximum security, you can now delete the Ingress Rule for Port 80 in the Oracle Console Security List, as all access will go through Tailscale.

#### 5. Why This Setup Works Well

*   **Zero Cost:** You get a powerful, private development server for $0/month using the Oracle Free Tier.

*   **Fast & Efficient:** Testnet4 blocks are small, so the Initial Block Download (IBD) finishes in minutes, not days. The minimal resource allocation keeps it lean.

*   **Secure & Private:** Tailscale ensures your node and its data are never exposed to the public internet, accessible only from devices you authorize.




This is the first draft of a step-by-step blueprint for running a full Umbrel node with Electrs and Tailscale on the Oracle Cloud Free Tier, specifically optimized for Testnet4. It's designed to be a zero-cost, secure, and fast way to spin up a personal development environment. 

I've done my best to make it clear and accurate, but I'm sure the collective 
knowledge here could make it even better. If you follow this guide, please share your experience - what worked, what didn't, and any optimizations you discovered. 

Your feedback is invaluable for turning this into a truly community-vetted resource.





