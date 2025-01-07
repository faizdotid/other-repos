

To use **Cloudflare DNS** (1.1.1.1 and 1.0.0.1) instead of `systemd-resolved`, you need to update your DNS configuration in one of these ways:

---

### **Option 1: Modify `/etc/systemd/resolved.conf`**
1. Open `/etc/systemd/resolved.conf` for editing:
   ```bash
   sudo nano /etc/systemd/resolved.conf
   ```

2. Look for the lines starting with `DNS=` and `FallbackDNS=`. Uncomment and set them as:
   ```ini
   [Resolve]
   DNS=1.1.1.1 1.0.0.1
   FallbackDNS=8.8.8.8 8.8.4.4
   ```

3. Restart `systemd-resolved` to apply changes:
   ```bash
   sudo systemctl restart systemd-resolved
   ```

4. Verify the new DNS settings:
   ```bash
   resolvectl status
   ```

---

### **Option 2: Disable `systemd-resolved` and use manual `/etc/resolv.conf`**
If you prefer to manually configure `/etc/resolv.conf`:
1. Stop and disable `systemd-resolved`:
   ```bash
   sudo systemctl stop systemd-resolved
   sudo systemctl disable systemd-resolved
   ```

2. Remove the symlink for `/etc/resolv.conf`:
   ```bash
   sudo rm /etc/resolv.conf
   ```

3. Create a new `/etc/resolv.conf`:
   ```bash
   sudo nano /etc/resolv.conf
   ```

4. Add the following:
   ```bash
   nameserver 1.1.1.1
   nameserver 1.0.0.1
   ```

5. Save and close the file.

---

### **Option 3: Use `resolvectl` to set DNS on the fly**
This method works without disabling `systemd-resolved`:
1. Set Cloudflare DNS temporarily (until reboot):
   ```bash
   sudo resolvectl dns eth0 1.1.1.1 1.0.0.1
   ```

   Replace `eth0` with your active network interface (`ip a` will show your interface names).

2. Make it persistent (by using `resolved.conf` as in Option 1).

---

### **Final Verification**
Check your DNS configuration:
```bash
cat /etc/resolv.conf
```
You should see `1.1.1.1` and `1.0.0.1`.

To confirm DNS works, test with:
```bash
dig google.com @1.1.1.1
```
