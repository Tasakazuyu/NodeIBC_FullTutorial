### CARA UPGRADE NODE MENGGUNAKAN INSTALASI COSMOVISOR ATAUPUN SERVICE
**DI SINI SAYA CONTOHKAN UPGRADE QUICKSILVER MAINNET MENGGUNAKAN COSMOVISOR DAN SERVICE, TINGGAL GANTI LINK GITCLONE DAN NAMA PROJECTNYA**

## UPGRADE NODE MENGGUNAKAN COSMOVISOR
```
# Clone project repository
cd $HOME
rm -rf quicksilver
git clone https://github.com/ingenuity-build/quicksilver.git
cd quicksilver
git checkout v1.2.4

# Build binaries
make build

# Prepare binaries for Cosmovisor
mkdir -p $HOME/.quicksilverd/cosmovisor/upgrades/v1.2.4/bin
mv build/quicksilverd $HOME/.quicksilverd/cosmovisor/upgrades/v1.2.4/bin/
rm -rf build
```

## UPGRADE NODE MENGGUNAKAN SERVICE
```
cd $HOME
rm -rf quicksilver
git clone https://github.com/ingenuity-build/quicksilver.git
cd quicksilver
git checkout v1.2.4
make install

systemctl restart quicksilverd && journalctl -fu quicksilverd -o cat
```

# UPGRADE NODE MENGGUNAKAN SERVICE VERSION 2
```
sudo systemctl stop quicksilverd
cd $HOME && rm $HOME/quicksilver -rf
git clone https://github.com/ingenuity-build/quicksilver.git
git checkout v1.2.4
make install
sudo systemctl restart quicksilverd && journalctl -fu quicksilverd -o cat
```
