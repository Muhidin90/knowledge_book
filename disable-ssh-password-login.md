# How to Disable SSH Password Login on Ubuntu

This guide shows you how to securely disable SSH password login on Ubuntu and force key-based authentication only.

## ⚠️ Critical Warning

**Before disabling password login, make sure you already have SSH keys working, or you will lock yourself out.**

Check if you have SSH keys configured:
```bash
ls ~/.ssh/authorized_keys
```

If this file doesn't exist or is empty, set up SSH keys first before proceeding.

## Step 1: Edit SSH Configuration

Open the SSH daemon configuration file:

```bash
sudo nano /etc/ssh/sshd_config
```

Find and set these lines (uncomment if needed):

```
PasswordAuthentication no
ChallengeResponseAuthentication no
UsePAM no
```

### Optional but Recommended

Also disable root login for additional security:

```
PermitRootLogin no
```

## Step 2: Restart SSH Service

After making changes, restart the SSH service to apply them:

```bash
sudo systemctl restart ssh
```

## Step 3: Verify Configuration

From another terminal window (keep your current session open), try to connect:

```bash
ssh user@server-ip
```

If no SSH key is configured, login will fail — that's expected and confirms the configuration is working.

## Quick Hardening Combo (Recommended)

For best security practices, use these settings in `/etc/ssh/sshd_config`:

```
PasswordAuthentication no
PermitRootLogin no
PubkeyAuthentication yes
```

These settings ensure:
- **PasswordAuthentication no**: Disables password-based login
- **PermitRootLogin no**: Prevents direct root login
- **PubkeyAuthentication yes**: Enables SSH key-based authentication

## Important Notes

1. **Keep a terminal session open** while testing to avoid lockout
2. **Test SSH key login first** before disabling password authentication
3. **Have physical or console access** to the server as a backup
4. Make sure your SSH keys are properly set up in `~/.ssh/authorized_keys`
5. Restart SSH service after any configuration changes

## Troubleshooting

If you get locked out:
- Use physical access or cloud provider console to regain access
- Revert changes in `/etc/ssh/sshd_config`
- Restart SSH service: `sudo systemctl restart ssh`
