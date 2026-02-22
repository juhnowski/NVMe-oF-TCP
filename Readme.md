# About
NVMe-oF-TCP settings for NixOS

# Install
## Client
```bash
  cd client
  sudo cp configuration.nix /etc/nixos
  sudo nixos-rebuild switch
```

## Server
```bash
  cd server
  sudo cp configuration.nix /etc/nixos 
  sudo nixos-rebuild switch
```

# Первое подключение
## Форматируем раздел
```bash
sudo nix-shell -p parted --run "parted /dev/nvme0n1 mklabel gpt"
sudo nix-shell -p parted --run "parted /dev/nvme1n1 mkpart primary ext4 0% 100%"
```

# Проверка производительности
## Тест через dd
Чтобы убедиться, что TCP-протокол не режет скорость слишком сильно, можно запустить простой тест:
```bash
sudo nix-shell -p coreutils --run "dd if=/dev/zero of=/dev/nvme0n1 bs=1M count=1024 oflag=direct status=progress"
```
Здесь oflag=direct критически важен — он заставляет данные идти напрямую в NVMe, минуя кэш оперативной памяти.

## Тест через Fio
```bash
sudo nix-shell -p fio --run "fio --name=random-write --ioengine=libaio --rw=randwrite --bs=4k --numjobs=1 --size=1G --iodepth=64 --runtime=30 --time_based --do_verify=0 --direct=1 --group_reporting --filename=/dev/nvme0n1"
```
