## 1. Memiliki Domain
![image(1)](https://user-images.githubusercontent.com/109075185/223764571-9a0eea31-3eca-4d03-95d0-1ffcdf2175ee.png)

1. Belilah Domain Di [idcloudhost.com](https://idcloudhost.com/) atau dimanapun itu

## 2. Mengoneksikan domain ke cloudflare
### a. Add Site
![image(2)](https://user-images.githubusercontent.com/109075185/223764856-154a5d68-80dd-4a18-81e5-788160e8a719.png)
- **Masuk Kedalam https://cloudflare.com/**
- **Lalu daftarkan akun kalian ke cloudflare**
- **lalu tekan add a site**

## b. Register domain
![image(3)](https://user-images.githubusercontent.com/109075185/223765680-580983c9-a768-4719-a0b2-d863d5bb255a.png)

- Masukan domain yang kalian sudah beli dengan format seperti di atas tanpa http
- Lalu tekan add site

## c. Select Plan
![image(4)](https://user-images.githubusercontent.com/109075185/223765982-e2a9feb7-bacb-4762-b270-681d2f79f22e.png)
- Pada bagian pembayaran pilih lah yang free
- Lalu tekan continue

## d. Review Dns Record
![image(5)](https://user-images.githubusercontent.com/109075185/223766455-92354db5-bfd4-435d-b57d-1d1d956d80a0.png)
- Pada bagian ini langsung continue saja


## e. Change your nameservers
![image(6)](https://user-images.githubusercontent.com/109075185/223766923-335110cf-7427-41a6-bc9a-6ed3197df071.png)

- Pertama buka provider tempat kamu membeli domain lalu
- tekan manage pada domain anda
- lalu tekan nameservers
- copy name server yang telah di berikan cloudflare lalu pindakan ke provider anda
- seperti gambar diatas 
- dan tunggulah beberapa saat hingga connect

## f. Mengatur DNS
![image(7)](https://user-images.githubusercontent.com/109075185/223766988-857cb013-da77-4adb-a7a7-ad823aac9f78.png)
- tekan dns
- lalu tekan record

## g. Membuat Subdomain
 
#### 1. Membuat dns TYPE A
![image(8)](https://user-images.githubusercontent.com/109075185/223767191-45cdbd6b-cf5e-40cb-83b3-57cf5134d1e6.png)
- Tekan Add Record
- Pilih Type A 
- Isi Name planq
- IPV4 ADDRES Adalah ip addres vps kamu 
- matikan proxy status
- Lalu Save

## 2. Membuat Subdomain API
![image(9)](https://user-images.githubusercontent.com/109075185/223767457-eb8fd55f-797f-4054-a938-c288a91e14c8.png)
- Tekan add record lagi
- pilih type CNAME
- Name isi dengan api.planq 
- target isi dengan planq-namadomainkamu.xyz
- Matikan Proxy Status
- Tekan Save

## 3. Membuat Subdomain RPC
![image(10)](https://user-images.githubusercontent.com/109075185/223767845-d9bd99d4-9c1e-4d4d-b812-31d1a4e069ca.png)
- Tekan add record lagi
- pilih type CNAME
- Name isi dengan rpc.planq 
- target isi dengan planq-namadomainkamu.xyz
- Matikan Proxy Status
- Tekan Save

## 3. Konfigurasi Di Vps

#### a. Instal alat dan bahan

USAHAKAN INSTAL 1 1 COMMAND JANGAN LANGSUNG TIMPA

```
sudo apt update && sudo apt upgrade -y
sudo apt install nginx certbot python3-certbot-nginx -y
curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -
sudo apt-get update && apt install -y nodejs git
curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | gpg --dearmor | sudo tee /usr/share/keyrings/yarnkey.gpg >/dev/null
echo "deb [signed-by=/usr/share/keyrings/yarnkey.gpg] https://dl.yarnpkg.com/debian stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update && sudo apt-get install yarn -y
```

## Mengaktifkan Server Api Dan Mencari Portnya
```
nano $HOME/.planqd/config/app.toml
```
![image(11)](https://user-images.githubusercontent.com/109075185/223768429-e071ef27-ef0d-47f3-8984-0d7b9cca9a48.png)
- Masukan Command Diatas Ke Vps Anda lalu scrool sampai bawah dan cari seperti gambar diatas ubah enable = false menjadi enable = true
- simpan baik baik address = "tcp://0.0.0.0:1317" 
- lalu restart node kalian dengan command berikut:
```
sudo systemctl restart planqd
```

## c. Mencari Port RPC
```
nano $HOME/.planqd/config/config.toml
```
![image(12)](https://user-images.githubusercontent.com/109075185/223768685-6abb228a-aef8-424e-ac17-2077e2e7c149.png)
- Simpan Baik Baik laddr = "tcp://127.0.0.1:14657"
- tekan pada keyboard ctrl+x untuk keluar

## d. Membuat Config API
Mohon Ikuti Instruksi Berikut

- nano /etc/nginx/sites-enabled/<api.planq-namadomainkamu.xyz>.conf
- **CONTOH YANG BENAR SEPERTI DIBAWAH**
- nano /etc/nginx/sites-enabled/api.planq.spt-node.xyz.conf

#### Masukan script berikut dan ubah sedikit seperti gambar di bawah ini
```
server {
    server_name api.planq.namadomainkamu.xyz;
    listen 80;
    location / {
        add_header Access-Control-Allow-Origin *;
        add_header Access-Control-Max-Age 3600;
        add_header Access-Control-Expose-Headers Content-Length;

	proxy_set_header   X-Real-IP        $remote_addr;
        proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
        proxy_set_header   Host             $host;

        proxy_pass http://ip-node-kamu-yang-saya-suruh-simpan-baik-baik:1337;

    }
}
```
![image(13)](https://user-images.githubusercontent.com/109075185/223769080-e623457a-5b19-49e9-8620-011f49764f7c.png)

## e. Membuat Config RPC

#### Mohon Ikuti Instruksi Berikut

- nano /etc/nginx/sites-enabled/<rpc.planq-namadomainkamu.xyz>.conf
**CONTOH YANG BENAR SEPERTI DIBAWAH**
- nano /etc/nginx/sites-enabled/rpc.planq.spt-node.xyz.conf

**Masukan script berikut dan ubah sedikit seperti gambar di bawah ini**
```
server {
    server_name rpc.planq.namadomainkamu.xyz;
    listen 80;
    location / {
        add_header Access-Control-Allow-Origin *;
        add_header Access-Control-Max-Age 3600;
        add_header Access-Control-Expose-Headers Content-Length;

	proxy_set_header   X-Real-IP        $remote_addr;
        proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
        proxy_set_header   Host             $host;

         proxy_pass http://ip-node-kamu-yang-saya-suruh-simpan-baik-baik:1337;

    }
}
```
![image(14)](https://user-images.githubusercontent.com/109075185/223769381-6e4c7e10-fe7f-472a-a679-9cae6c9c86db.png)

## f. Memasang ssl
```
sudo certbot --nginx --register-unsafely-without-email
sudo certbot --nginx --redirect
```
![image(15)](https://user-images.githubusercontent.com/109075185/223769507-08e2e8de-1d7b-4800-bb5f-2697940ebf9f.png)
1. lakukan pemasangan ssl kepada dua subdomain di atas dengan cara memasukan command diatas

## g. Mengecek api dan rpc

Masuk ke browser anda lalu cek subdomain yang kalian buat tadi seperti
https://api.planq.spt-node.xyz/
![image(16)](https://user-images.githubusercontent.com/109075185/223770068-496a4ace-ac9c-4de6-b4f9-dcf57c39b86c.png)
https://rpc.planq.spt-node.xyz/
![image(17)](https://user-images.githubusercontent.com/109075185/223770127-7e76ac8c-9abe-4c7a-b33b-ab8fb7a6d00c.png)


# NOTE: Rubah nama "planq" dan "planqd" menjadi nama project yang kamu sedang deploy RPC, GRPC, API nya. 

Thanks To SPT-NODE.
Credit: SPT-NODE
















