# Cài đặt nfs
### Phía server
```
sudo apt install nfs-server
mkdir /data
sudo chown -R nobody:nogroup /data
sudo chmod -R 777 /data/

vi /etc/exports

/data *(rw,sync,no_subtree_check,no_root_squash)

exportfs -rav
sudo systemctl restart nfs-kernel-server
```

## Phía client
```
sudo apt install nfs-common
```

# Cài đặt ArgoCd
