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

### Resource Allocation & Server Provisioning

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


### Required Apps To Be Installed in Umbrel OS

Once Umbrel OS is running, you will install these three apps directly from its App Store.

*   **Bitcoin Node:** The core software for running a Bitcoin node.

*   **Electrs:** An efficient indexing server that allows wallets and apps to quickly query the blockchain.

*   **Tailscale:** A VPN service that creates a secure, private network between your devices and your cloud node, eliminating the need to expose ports to the public internet.

### Configuring the Firewall

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

### Installation and Configuration Steps

Now that the server is prepared, you can install and configure the software.


1.  **Install Umbrel OS:** Follow the official Umbrel documentation to install the OS on your Oracle instance. This typically involves running a single script from their site.


2.  **Access the Dashboard:** Once installed, access the Umbrel dashboard by navigating to `http://<your-reserved-public-ip>` in your web browser.


3.  **Install Apps:** From the Umbrel App Store, install **Bitcoin Node**, **Electrs**, and **Tailscale**.


4.  **Configure Bitcoin for Testnet4:**
    *   Open the Bitcoin Node app in Umbrel.
    *   Go to **Settings -> Advanced Settings**.
    *   Change the `Network` from `Mainnet` to **`Testnet4`**.
    *   Save the settings. The node will restart and begin syncing the Testnet4 blockchain, which is very fast


5.  **Check Electrs Status in Umbrel Dashboard**

After installing Electrs through the Umbrel App Store and switching Bitcoin to Testnet4, you must verify that Electrs is properly indexing the blockchain

5.1. Navigate to the Umbrel dashboard via your Tailscale IP (e.g., `http://100.x.x.x`)
5.2. Click on the **Electrs** app
5.3. Look for the status indicator:
   - **Green**: Electrs is running and synced
   - **Yellow**: Electrs is syncing/indexing
   - **Red**: Electrs has encountered an error

5.4. Check the logs for indexing progress:
   - Click **Show Logs** in the Electrs app
   - Look for messages like: `INFO - Electrs server RPC thread running`
   - Indexing progress: `INFO - indexed X blocks (Y%)`


6.  **Verify Electrs is Serving on Port 50001**

SSH into your Oracle instance and test that Electrs is listening:

```bash
# SSH into your instance
ssh ubuntu@<reserved-public-ip>

# Check if Electrs is listening on port 50001
sudo netstat -tulpn | grep 50001
```

**Expected output:**
```
tcp        0      0 0.0.0.0:50001           0.0.0.0:*               LISTEN      12345/electrs
```

If you don't see this, Electrs may not have started properly. Check the logs in the Umbrel dashboard.

7. **Test Electrs with a Direct Query**

You can send a test query to Electrs to verify it's responding correctly:

```bash
# Test the Electrs server version
echo '{"id":1,"method":"server.version","params":["test","1.4"]}' | nc localhost 50001
```

**Expected response:**
```json
{"id":1,"result":["ElectrumX 1.16.0","1.4"]}
```

If you see this, Electrs is working correctly!

8.  **Verify Network is Testnet4**

Confirm that Electrs is indexing the Testnet4 blockchain, not mainnet:

```bash
# Query the genesis block (block 0)
echo '{"id":2,"method":"blockchain.block.header","params":[0]}' | nc localhost 50001
```

**Parse the response to get the genesis block hash.**

The hash should be the **Testnet4 genesis hash**:
```
00000000da84f2bafbbc53dee25a72ae507ff4914b867c565be350b0da8bf043
```

If you see the mainnet genesis hash instead, Bitcoin Node may not have switched to Testnet4 properly. Return to the Bitcoin Node settings and verify the network selection.

9.  **Check Current Block Height**

Verify Electrs is synced to the latest Testnet4 block:

```bash
# Subscribe to headers to get current block height
echo '{"id":3,"method":"blockchain.headers.subscribe","params":[]}' | nc localhost 50001
```

**Response will include the current height:**
```json
{"id":3,"result":{"height":45678,"hex":"..."}}
```

Compare this height to a public Testnet4 block explorer (e.g., mempool.space/testnet4) to confirm your node is fully synced.

10. **Clean Up Firewall**

For maximum security, you can now delete the Ingress Rule for Port 80 in the Oracle Console Security List, as all access will go through Tailscale.

---

## Workshop: Sharing Tailscale Access

Tailscale allows you to securely share your Umbrel node with workshop attendees without exposing it to the public internet. Here's how to set up network sharing.

### A. Understanding Tailscale Sharing

When you share your Tailnet (Tailscale network):
- Attendees can access your Umbrel node's Tailscale IP (100.x.x.x)
- They **cannot** access your personal devices
- They **can only** access the specific node you share
- Access can be revoked at any time

### B. Enable Sharing in Tailscale Admin Console

1. **Log in to Tailscale Admin Console**
   - Go to https://login.tailscale.com/admin/machines
   - Sign in with your Tailscale account

2. **Find Your Umbrel Node**
   - Look for your node in the Machines list (usually named `umbrel` or similar)
   - Click on the machine name

3. **Share the Machine**
   - Click the **Share** button (or the `...` menu â†’ **Share**)
   - You have two options:

**Option A: Share via Email (Best for Small Groups)**
   - Enter the email addresses of workshop attendees
   - They'll receive an invite to join your Tailnet
   - They can accept and access your Umbrel node

**Option B: Share via Shareable Link (Best for Workshops)**
   - Click **Create shareable link**
   - Set an expiration time (e.g., "Expires in 24 hours" for day-of-workshop)
   - Copy the link
   - Share the link with all attendees via:
     - Workshop registration email
     - QR code displayed at venue
     - Slack/Discord channel
     - Printed handouts

4. **Configure Sharing Permissions**
   - **Read-only access**: Recommended for workshops (attendees can query, not modify)
   - **Auto-approve**: Makes joining instant without your approval
   - **Expiration**: Set to automatically revoke access after the workshop (e.g., 48 hours)

---

## Workshop: Attendee Setup Instructions

Provide these instructions to workshop participants:

**Before the Workshop:**

1. **Install Tailscale**
   - Android: https://play.google.com/store/apps/details?id=com.tailscale.ipn
   - iOS: https://apps.apple.com/app/tailscale/id1470499037
   - Desktop: https://tailscale.com/download

2. **Accept the Invite**
   - Click the shareable link you provided
   - Log in or create a Tailscale account (free)
   - Accept the connection to your Tailnet

3. **Verify Connection**
   - Open the Tailscale app
   - You should see your Umbrel node listed (e.g., `umbrel`)
   - Note the IP address (e.g., `100.64.0.5`)

**At the Workshop:**

4. **Enable Tailscale VPN**
   - Open the Tailscale app
   - Toggle the VPN to **ON**
   - Confirm you're connected to the Tailnet

5. **Test Connection to Umbrel**
   - Ping the Umbrel Tailscale IP: `ping 100.64.0.5`
   - You should see responses (e.g., `64 bytes from 100.64.0.5: icmp_seq=1 ttl=64 time=45ms`)

6. **Configure BYOB Wallet**
   - Open BYOB Wallet app
   - Go to Settings â†’ Node Connection
   - Enter Tailscale IP: `100.64.0.5`
   - Port: `50001`
   - Tap **Test Connection**
   - Should show: âœ… **Connected to Umbrel node**

### Troubleshooting for Attendees

**Problem: "Cannot reach node"**
- **Check**: Is Tailscale VPN enabled on your device?
- **Check**: Can you ping the Umbrel IP? (`ping 100.64.0.5`)
- **Check**: Are you connected to WiFi or mobile data?

**Problem: "Tailscale not connecting"**
- **Solution**: Turn Tailscale OFF and back ON
- **Solution**: Restart your phone
- **Solution**: Check you're logged into the correct Tailscale account

**Problem: "Node is syncing"**
- **This is normal** if the blockchain is still catching up
- Check the current block height vs. expected height

### Post-Workshop Cleanup

After the workshop:

1. **Revoke Access**
   - Go to Tailscale Admin Console â†’ Machines â†’ Your Umbrel
   - Click **Shared with** to see list of users
   - Click **Remove** next to each attendee (or let auto-expire handle it)

2. **Or: Disable Sharing Entirely**
   - Click the **Share** button again
   - Click **Stop sharing**
   - All attendee access is immediately revoked

---

## Workshop: Testing Your Complete Setup

Before the workshop, perform these tests to ensure everything works correctly.

### A. Pre-Workshop Testing Checklist

**24 Hours Before Workshop:**

1. **Verify Oracle Instance is Running**
   ```bash
   # SSH into your instance
   ssh ubuntu@<reserved-public-ip>
   
   # Check uptime
   uptime
   
   # Expected: Instance has been running without restart
   ```

2. **Verify Umbrel Dashboard is Accessible**
   - Open browser on your laptop/phone
   - Enable Tailscale VPN
   - Navigate to: `http://100.x.x.x` (your Umbrel Tailscale IP)
   - You should see the Umbrel dashboard

3. **Check Bitcoin Node Sync Status**
   - In Umbrel dashboard â†’ Bitcoin Node
   - Verify:
     - **Network**: Testnet4
     - **Status**: Synced
     - **Block height**: Matches current Testnet4 height
   - Compare to: https://mempool.space/testnet4

4. **Check Electrs Sync Status**
   - In Umbrel dashboard â†’ Electrs
   - Verify:
     - **Status**: Running
     - **Indexed**: 100%
     - **No errors** in logs

5. **Check Tailscale Status**
   - In Umbrel dashboard â†’ Tailscale
   - Verify:
     - **Status**: Connected
     - **IP address**: Shows your 100.x.x.x address
     - **No warnings** about disconnection


### B. Connection Tests from Your Device

Perform these tests from the device you'll use during the workshop:

**Test 1: Ping Test**
```bash
# Enable Tailscale on your phone/laptop
# Then ping your Umbrel
ping 100.x.x.x

# Expected: Successful responses
# 64 bytes from 100.x.x.x: icmp_seq=1 ttl=64 time=45ms
```

**Test 2: Electrs Connection Test**
```bash
# Test TCP connection to Electrs
telnet 100.x.x.x 50001

# Expected: Connection established
# Connected to 100.x.x.x.
# Escape character is '^]'.
```

**Test 3: Electrs Query Test**

In the telnet session, type:
```json
{"id":1,"method":"server.version","params":["test","1.4"]}
```
Then press Enter.

**Expected response:**
```json
{"id":1,"result":["ElectrumX 1.16.0","1.4"]}
```

If you see this, Electrs is working perfectly!

To exit telnet: Press `Ctrl + ]`, then type `quit`

**Test 4: Balance Query Test (Using a Known Testnet4 Address)**

You can test with a known testnet4 address to verify full functionality:

```bash
# Example: Query balance for a testnet4 address
# First, convert address to scripthash (you can use a tool or library)
# For demonstration, using an example scripthash:

echo '{"id":2,"method":"blockchain.scripthash.get_balance","params":["<example_scripthash>"]}' | nc 100.x.x.x 50001
```

**Expected response:**
```json
{"id":2,"result":{"confirmed":0,"unconfirmed":0}}
```

Even if balance is 0, getting a valid response means Electrs is working!


### C. BYOB Wallet Integration Test

**Test the actual BYOB Wallet connection:**

1. **Install BYOB Wallet** on your test device (Android phone/tablet)

2. **Enable Tailscale** on the test device

3. **Open BYOB Wallet** and go to Settings

4. **Configure Node Connection:**
   - Tailscale IP: `100.x.x.x`
   - Port: `50001`
   - Network: Testnet4

5. **Tap "Test Connection"**

**Expected result:**
```
âœ… Connected to Umbrel node
Block height: 45678
Network: Testnet4
Status: Synced
```

6. **Scan a Test Membership Card** (testnet4 tpub)
   - Wallet should load
   - Balance should display (even if 0 sats)
   - Transaction history should load (even if empty)

**If successful: You're ready to run the first BYOB workshop!**



### D. Load Testing

Simulate multiple users connecting simultaneously:

**From Multiple Devices:**
1. Connect 3-5 devices to Tailscale
2. Open BYOB Wallet on each
3. Configure all to connect to `100.x.x.x:50001`
4. Have all devices scan membership cards simultaneously

**Monitor Oracle Cloud Resources:**
1. Go to Oracle Cloud Console â†’ Compute â†’ Instances
2. Click your instance â†’ Metrics
3. Watch:
   - **CPU Utilization**: Should stay under 70%
   - **Memory Utilization**: Should stay under 80%
   - **Network Bandwidth**: Should stay under limits

**If resources spike too high:**
- Consider increasing OCPUs (1 â†’ 2) or RAM (6GB â†’ 8GB)
- Oracle Free Tier allows up to 4 OCPUs / 24GB RAM total across instances

### E. Backup Internet Connection Test

**Test your mobile hotspot backup:**

1. **Disconnect venue WiFi** (simulate internet failure)

2. **Enable mobile hotspot** on your phone

3. **Connect your laptop/tablet** to the hotspot

4. **Verify Tailscale still works** over mobile data

5. **Test BYOB Wallet connection** to Umbrel

**Expected:** Everything should work normally, just slower

### F. Day-of-Workshop Final Checks

**2 Hours Before Start:**

- [ ] Oracle instance is running (check cloud console)
- [ ] Umbrel dashboard is accessible via Tailscale
- [ ] Bitcoin Node is synced (block height current)
- [ ] Electrs is synced (100% indexed)
- [ ] Tailscale is connected
- [ ] Test connection from your phone works
- [ ] Sharing link is active and not expired
- [ ] Mobile hotspot is charged and ready
- [ ] You have Tailscale IP written down: `100.x.x.x`
- [ ] You have a testnet4 faucet ready for funding test wallets

**If any check fails:**
- You still have 2 hours to troubleshoot
- Worst case: Restart the Oracle instance (takes ~5 mins)
- Nuclear option: Redeploy Umbrel (takes ~15 mins, if you have a backup)

### G. Success Criteria

Your setup is workshop-ready when:

âœ… Umbrel accessible via Tailscale from multiple devices
âœ… Bitcoin Node synced to current Testnet4 height
âœ… Electrs responding to queries within 1-2 seconds
âœ… BYOB Wallet successfully connects and loads test wallet
âœ… Multiple simultaneous connections don't crash the system
âœ… Mobile hotspot backup tested and working

---

## Quick Reference Card for Workshop Day

Print this and keep it handy:

**Umbrel Tailscale IP:** `100.x.x.x`
**Electrs Port:** `50001`
**Network:** Testnet4

**Troubleshooting:**
- Attendee can't connect? â†’ Check Tailscale is ON
- Node not responding? â†’ SSH and check `uptime`
- Slow queries? â†’ Check Oracle metrics for resource limits

**Emergency Contacts:**
- Oracle Support: (if instance down)
- Tailscale Support: (if VPN issues)
- Your backup plan: (mobile hotspot, local testnet, etc

---

## Why This Setup Works Well

*   **Zero Cost:** You get a powerful, private development server for $0/month using the Oracle Free Tier.

*   **Fast & Efficient:** Testnet4 blocks are small, so the Initial Block Download (IBD) finishes in minutes, not days. The minimal resource allocation keeps it lean.

*   **Secure & Private:** Tailscale ensures your node and its data are never exposed to the public internet, accessible only from devices you authorize.













