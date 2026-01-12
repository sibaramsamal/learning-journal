# PostgreSQL Startup Failure – `postmaster.pid` Lock Issue (macOS + Homebrew)

## Context

* OS: macOS
* PostgreSQL: **14.20 (Homebrew)**
* Port: **5432**
* Usage: Local development

---

## Initial Symptom (Application Level)

```
connection failed: connection to server at "::1", port 5432 failed: Connection refused
connection failed: connection to server at "127.0.0.1", port 5432 failed: Connection refused
Is the server running on that host and accepting TCP/IP connections?
```

**Interpretation:**

* Application attempted IPv6 and IPv4
* Nothing was listening on port `5432`
* Indicates PostgreSQL server was **not running**

---

## Service Startup Failure (Homebrew)

While starting PostgreSQL via Homebrew:

```
Bootstrap failed: 5: Input/output error
Failure while executing:
/bin/launchctl bootstrap gui/502 \
/Users/arunkumar/Library/LaunchAgents/homebrew.mxcl.postgresql@14.plist
```

**Key Insight:**

* This is a `launchctl` failure
* Not an application / JDBC / driver issue

---

## Critical Log Output (Root Cause)

PostgreSQL log showed:

```
FATAL:  lock file "postmaster.pid" already exists
HINT:  Is another postmaster (PID 502) running in data directory "/opt/homebrew/var/postgresql@14"?
```

**Meaning:**

* PostgreSQL detected an existing lock file
* Server assumes another instance is running
* In reality, the previous instance crashed or was killed

---

## Verification Step

Checked PostgreSQL version:

```
postgres --version
postgres (PostgreSQL) 14.20 (Homebrew)
```

✔ Version matched the data directory (`postgresql@14`)
✔ No version incompatibility

---

## Resolution Steps (Actual Steps Performed)

### Step 1: Stop PostgreSQL service

```
brew services stop postgresql@14
brew services list
```

Confirmed that PostgreSQL was not running.

---

### Step 2: Remove the stale lock file

```
rm /opt/homebrew/var/postgresql@14/postmaster.pid
```

> `postmaster.pid` was left behind from an unclean shutdown and blocked startup.

---

### Step 3: Verify database availability

```
pg_isready
psql postgres
```

PostgreSQL responded successfully, confirming the issue was resolved.

---

After that restart the DB. If multiple DBs are present, stop and restart one by one. PG admin will use one of them which is having proper files.

```
brew services start postgresql@14
```

## Final Conclusion

* ❌ Not an application issue
* ❌ Not a dependency or driver issue
* ❌ Not a port or firewall issue
* ✅ Root cause was a **stale `postmaster.pid` lock file**
* ✅ PostgreSQL failed to start due to crash/interrupted shutdown

---

## Prevention Notes

* Always stop PostgreSQL before macOS or Homebrew upgrades:

```
brew services stop postgresql@14
```

* Avoid force shutdowns while DB is running
* Consider Dockerized PostgreSQL for isolated local development

---

## Key Takeaway

> **`Connection refused` almost always means the database server never started.**

Fix the server first — the application will follow.
