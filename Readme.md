# About
NVMe-oF-TCP settings for NixOS

# Install
## Client
```bash
  cd client
  sudo cp configuration.nix /etc/nixos
```

## Server
```bash
  cd server
  sudo cp configuration.nix /etc/nixos 
```

# Первое подключение
## Форматируем раздел
```bash
sudo nix-shell -p parted --run "parted /dev/nvme0n1 mklabel gpt"
sudo nix-shell -p parted --run "parted /dev/nvme1n1 mkpart primary ext4 0% 100%"
```
