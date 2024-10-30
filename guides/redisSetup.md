# Redis Setup

## Install Redis

```bash
sudo apt-get update
sudo apt-get install redis-server
```

---

## Open Redis CLI

To access the Redis CLI, run:
```bash
redis-cli
```

---

## Required Configuration Changes

Edit the Redis configuration file (usually located at `/etc/redis/redis.conf`) to allow external connections and set a password.

1. **Bind to IP Address**  
   By default, Redis only allows connections from `localhost`. To allow external connections, configure Redis to bind to the server’s IP address or to `0.0.0.0` (for all interfaces). Be cautious when allowing external access, as it can expose Redis to security risks if not properly secured.

   Open the configuration file:
   ```bash
   sudo nano /etc/redis/redis.conf
   ```

   Modify the `bind` line as follows:
   ```bash
   bind 0.0.0.0
   ```

2. **Set a Password**  
   To add an extra layer of security, set a password in the same configuration file. Locate and uncomment (or add) the following line, replacing `your_redis_password` with a strong password:

   ```bash
   requirepass "your_redis_password"
   ```
3. **open port**
   To ensure Redis can be accessed via the internet, port **6379/TCP** must be allowed through your firewall.
---

## Restart Redis to Apply Changes

Once you have made the necessary changes, restart the Redis service:

```bash
sudo systemctl restart redis
```


