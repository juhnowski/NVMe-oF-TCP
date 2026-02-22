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

sudo dd if=/dev/zero of=/dev/nvme0n1 bs=1M count=512 conv=fdatasync
sudo nix-shell -p parted --run "parted /dev/nvme0n1 mklabel gpt mkpart primary ext4 0% 100%"
sudo nix-shell -p e2fsprogs --run "mkfs.ext4 -L remote-nvme /dev/nvme0n1p1"
sudo mount /dev/nvme0n1p1 /mnt/nvme-remote
sudo ouch /mnt/nvme-remote/test_file && ls /mnt/nvme-remote
```

