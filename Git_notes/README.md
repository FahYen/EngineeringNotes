Initial setup of Git on a new system
```bash
# Configure user information
git config --global user.name "[Firstname Lastname]"
git config --global user.email "[Valid-email]"

# Setup ssh key
# Check if there is an existing ssh key
ls ~/.ssh
# If not, generate key type Ed25519
ssh-keygen -t ed25519 -C "your_email@example.com"
# Print public key
cat ~/.ssh/id_ed25519.pub
# Copy public key to your GitHub profile's ssh key
```

