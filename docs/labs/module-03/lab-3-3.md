# Lab 3.3: SSH Certificates & CA

**Goal:** Set up an SSH CA to avoid `known_hosts` prompts and key distribution.

## 1. Create CA Keys

```bash
mkdir ssh-ca
cd ssh-ca
ssh-keygen -t ed25519 -f user_ca -C "user_ca" -N ""
ssh-keygen -t ed25519 -f host_ca -C "host_ca" -N ""
```

## 2. Sign a User Key (for yourself)

Generate your personal key (if you don't have one, or make a temp one).

```bash
ssh-keygen -t ed25519 -f my-key -N ""
```

Sign it with the **User CA**.
*   `-I`: Key ID (e.g., user email)
*   `-n`: Principals (users you can log in as: root, ubuntu)
*   `-V`: Validity (+1h = 1 hour)

```bash
ssh-keygen -s user_ca -I "darshan@corp" -n root,ubuntu -V +1h my-key.pub
```

Inspect the signed cert:

```bash
ssh-keygen -L -f my-key-cert.pub
```

## 3. Configure Server to Trust User CA

Copy the `user_ca.pub` to the server (our docker container).

```bash
docker cp user_ca.pub ssh-lab:/etc/ssh/
```

Configure `sshd_config` to trust it.

```bash
docker exec ssh-lab sh -c 'echo "TrustedUserCAKeys /etc/ssh/user_ca.pub" >> /etc/ssh/sshd_config'
docker exec ssh-lab kill -HUP 1
```

## 4. Log in with Certificate

Use your signed key.

```bash
# Get IP
ip=$(docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' ssh-lab)

ssh -i my-key -o CertificateFile=my-key-cert.pub root@$ip
```

**Success!** You logged in without your public key being in `~/.ssh/authorized_keys` on the server!

## 5. Sign Host Key (Remove TOFU)

Now let's fix the "Unknown Host" prompt.

Get server's host pub key.

```bash
docker cp ssh-lab:/etc/ssh/ssh_host_ed25519_key.pub ./server_host.pub
```

Sign it with **Host CA**.

```bash
ssh-keygen -s host_ca -I "ssh-lab" -h -n $ip,ssh-lab -V +1y server_host.pub
```

Copy cert back to server.

```bash
docker cp server_host-cert.pub ssh-lab:/etc/ssh/ssh_host_ed25519_key-cert.pub
```

Configure sshd to present cert.

```bash
docker exec ssh-lab sh -c 'echo "HostCertificate /etc/ssh/ssh_host_ed25519_key-cert.pub" >> /etc/ssh/sshd_config'
docker exec ssh-lab kill -HUP 1
```

## 6. Configure Client to Trust Host CA

Add the Host CA to your `known_hosts` with a special marker `@cert-authority`.

```bash
echo "@cert-authority * $(cat host_ca.pub)" >> ~/.ssh/known_hosts
```

(Note: `*` means trust this CA for ALL hosts. In prod, you limit this to `*.corp.com`).

**Test:**
Clear your old known host entry for the IP.
Now SSH.

```bash
ssh -i my-key -o CertificateFile=my-key-cert.pub root@$ip
```

**Result:** No prompt! It just connects. You have achieved **Zero Trust SSH**.
