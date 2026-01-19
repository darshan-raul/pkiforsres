# Lab 3.1: Keys & known_hosts

**Goal:** Understand where keys live and how TOFU works.

## 1. Inspect your client keys

Look at your existing keys (if any).

```bash
ls -l ~/.ssh/
```

*   `id_rsa` / `id_ed25519`: Private Key (Keep secret!)
*   `id_rsa.pub`: Public Key (Copy to servers)

## 2. Inspect known_hosts

This file maps Hostnames/IPs to Public Keys.

```bash
cat ~/.ssh/known_hosts
```

It often looks like gibberish because it might be hashed (for privacy). Use `ssh-keygen -F` to find a host.

```bash
ssh-keygen -F github.com
```

## 3. SSH into a local container

Let's start a fresh ssh server container.

```bash
docker run -d -P --name ssh-lab lscr.io/linuxserver/openssh-server
```

Find the port:

```bash
port=$(docker port ssh-lab 2222 | head -1 | awk -F: '{print $2}')
echo "SSH Port: $port"
```

Connect (default user usually `linuxserver` or set via env, but for this generic alpine image, let's assume we need to handle auth. Actually, `linuxserver/openssh-server` requires setup. Let's use a simpler one or just localhost if enabled. Better yet, let's use a standard alpine and install openssh).

**Simpler Approach:** Use a standard ubuntu container.

```bash
docker rm -f ssh-lab
docker run -d --name ssh-lab ubuntu:latest sh -c "apt-get update && apt-get install -y openssh-server && mkdir /var/run/sshd && echo 'root:password' | chpasswd && /usr/sbin/sshd -D"
```

Find IP:
```bash
ip=$(docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' ssh-lab)
```

Connect:
```bash
ssh root@$ip
```

1.  Observe the "The authenticity of host... can't be established." prompt.
2.  Type "yes".
3.  Password is "password".
4.  Exit.

## 4. Check known_hosts again

You will see a new entry for that IP.

## 5. Re-key the server (Simulate "Hack" or Reinstall)

Regenerate the server's host keys.

```bash
docker exec ssh-lab rm /etc/ssh/ssh_host_rsa_key
docker exec ssh-lab ssh-keygen -A
docker exec ssh-lab kill -HUP 1 # Reload sshd (PID 1 usually)
```

## 6. Connect again

```bash
ssh root@$ip
```

**WARNING:** "REMOTE HOST IDENTIFICATION HAS CHANGED!"

This is the most common SSH error. It means the key we trusted has changed. Either:
1.  The server was reinstalled (common).
2.  You are being MITM'd (rare but critical).

## 7. Fix it

Remove the old key.

```bash
ssh-keygen -R $ip
```

Connect again.
