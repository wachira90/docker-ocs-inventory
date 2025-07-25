When the **OCS Inventory NG Agent** is installed but does **not appear in the server**, there are several areas to check to troubleshoot the issue. Here's a systematic approach:

---

### ✅ 1. **Verify Agent Configuration**

Check the agent configuration file on the client machine:

* **Linux**: `/etc/ocsinventory/ocsinventory-agent.cfg`
* **Windows**: `C:\ProgramData\OCS Inventory NG\Agent\ocsinventory-agent.cfg`

Ensure these settings are correct:

* `server` – correct URL or IP of the OCS server (e.g., `http://ocs-server/ocsinventory`)
* `tag` – optional, but if used on the server, tag filtering may hide agents
* `logfile` – check logs for clues (Linux: `/var/log/ocsinventory-agent/ocsinventory-agent.log`, Windows: similar path in `ProgramData`)

You can also run the agent manually to test:

```bash
ocsinventory-agent --debug --server=http://<ocs-server>/ocsinventory
```

---

### ✅ 2. **Check Firewall / Network**

Make sure the agent can communicate with the server:

* Port **80** (HTTP) or **443** (HTTPS) must be open
* DNS resolution for the server works
* You can test with:

  ```bash
  curl -v http://<ocs-server>/ocsinventory
  ```

---

### ✅ 3. **Verify Apache Logs on Server**

OCS server uses Apache. Check these logs:

* **Access log**: `/var/log/apache2/access.log`
* **Error log**: `/var/log/apache2/error.log`

Look for entries from the client IP. If nothing appears, the request is not reaching the server.

---

### ✅ 4. **Server Configuration**

Ensure the server is properly set up:

* Apache + mod\_perl is installed
* The web interface is working
* The OCS server daemon is running
* Inventory reception endpoint is reachable:
  `http://<ocs-server>/ocsinventory`

Also, verify:

```bash
sudo systemctl status apache2
sudo systemctl status ocsinventory-server
```

---

### ✅ 5. **Check Agent Version Compatibility**

Make sure the **agent version** is compatible with the **server version**.

You can find the agent version with:

```bash
ocsinventory-agent --version
```

Update the agent or server if needed.

---

### ✅ 6. **Check SSL / HTTPS Config**

If using HTTPS:

* Ensure the server's certificate is valid
* On the agent, if using self-signed certs, use:

  ```bash
  ocsinventory-agent --no-ssl-check
  ```

---

### ✅ 7. **MySQL Backend**

Check if the agent is sending data but the **server is not storing it**.

On the server, log in to MySQL and check:

```sql
USE ocsweb;
SELECT * FROM devices ORDER BY last_seen DESC LIMIT 10;
```

See if the agent is in the DB but not visible in the UI.

---

### ✅ 8. **Check GUI Filters**

In the OCS web interface:

* Ensure you aren’t filtering by TAG, IP, or other criteria
* Clear filters to see all agents

---

### ✅ 9. **Force Re-registration**

Sometimes, re-registering helps:

```bash
ocsinventory-agent --force
```

---

### Summary Table

| Step            | What to Check                          |
| --------------- | -------------------------------------- |
| Agent Config    | Server URL, log file, manual run       |
| Network         | Port 80/443 open, DNS, curl test       |
| Server Logs     | Apache logs for agent hits             |
| Server Status   | Apache and OCS services running        |
| Agent Version   | Compatibility with server              |
| SSL Issues      | Use `--no-ssl-check` if needed         |
| Database        | Check if agent is in MySQL DB          |
| GUI Filters     | Clear filters in web interface         |
| Force Inventory | Use `--force` or `--debug` for details |

---

