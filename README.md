# Archlinux PRO Install 2026

### Verificar suporte a criptografia AES
```sh
grep -m1 aes /proc/cpuinfo
```

### Adicionar Autologin no Lightdm
1. **Adicione o seu usuario ao grupo autologin**
```sh
sudo groupadd -r autologin
sudo gpasswd -a $USER autologin
```
2. **Crie o arquivo 99-autologin.conf**
```
sudo mkdir -p /etc/lightdm/lightdm.conf.d
sudo nano /etc/lightdm/lightdm.conf.d/99-autologin.conf
```

3. **Adicione no 99-autologin.conf**
```
[Seat:*]
autologin-user = seu_nome_de_usuario
autologin-user-timeout = 0
```

4. **Reinicie o sistema**
```sh
reboot
```

### Instale o fishshell e o defina como default
```sh
sudo pacman -Sy fish
chsh 
## Depois de digitar a senha
/usr/bin/fish
```

### Deixe o seu ~/.config/fish/config.fish assim
```sh
alias snapshot="sudo btrfs subvolume snapshot -r / ~/.snapshots/(date +%Y-%m-%d_%H-%M)"
function restore_snapshot --arhument-names snapshot
    set snapid (sudo btrfs subvolume show /$sumapastahome/.snapshots/$snapshot)
    sudo btrfs subvoluume set-default $snapid
    sudo reboot
end
function delete_snapshot --argument-names snapshot
    sudo btrfs subvolume delete ~/.snapshots/$snapshot
end

if status is-interactive
    set fish_greeting
end
```
### Para criar uma nova snapshot:
```sh
snapshot
```
### Para deletar uma snapshot:
```sh
delete_snapshot nome_snapshot
```
### Para restaurar uma snapshot:
```sh
restore_snapshot nome_snapshot
```

### Criar arquivo config do o Sway
```sh
mkdir -p ~/.config/sway
cp /etc/sway/config ~/.config/sway
```

### Deixe o pacman igual um 🚀

1. **Instale o reflector**
```sh
sudo pacman -Sy reflector
```

2. **Configure os melhores espelhos para sua localidade**
```sh
sudo reflector --verbose --country 'Brazil','United States' --age 12 --protocol https --sort rate --save /etc/pacman.d/mirrorlist
```

3. **Sincronize o pacman**
```sh
sudo pacman -Syyu
```