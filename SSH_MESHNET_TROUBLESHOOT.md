# SSH Meshnet Troubleshooting Guide

## Summary of Fix

1. The main SSH issue was that the SSH daemon (`sshd`) was not running on ben-chisos5016.nord (proxmox) after running it in debug mode. This caused SSH connections from m75q to hang or be refused.
2. Once you restarted sshd with `sudo systemctl start sshd`, SSH connections from m75q to proxmox worked immediately.
3. SSH from m75q to all meshnet and LAN devices was tested. All online Linux hosts were accessible; offline or Windows hosts failed as expected.

## How to Repair If It Happens Again

1. **If SSH hangs or is refused:**
   - Log in locally or via another method to the target host.
   - Check if sshd is running:
     ```bash
     sudo systemctl status sshd
     ```
   - If not running, start or restart it:
     ```bash
     sudo systemctl start sshd
     ```
   - Confirm it’s listening:
     ```bash
     sudo ss -tlnp | grep :22
     ```
2. **If you still have issues:**
   - Check firewall rules to ensure port 22 is open.
   - Restart NordVPN meshnet if using meshnet:
     ```bash
     sudo systemctl restart nordvpnd
     ```
   - Ping the host to verify network connectivity.
3. **After fixing, test SSH again from m75q:**
   ```bash
   ssh -vvv punkpar@<hostname or IP>
   ```

If you follow these steps, you’ll quickly restore SSH access between your meshnet and LAN hosts.
