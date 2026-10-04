# Bai 4 - Nginx Virtual Hosts tren nhieu cong

Bai nay cau hinh Nginx chay song song hai website tinh tren cung mot Droplet:

- Beta App: cong `8080`, root `/var/www/beta-app/html/`
- Internal App: cong `8090`, root `/var/www/internal-app/html/`

## Cau truc file nop bai

```text
homework/session_03/ex4/
├── multi-port.conf
├── beta-app/
│   └── index.html
└── internal-app/
    └── index.html
```

## Trien khai tren Droplet

Chay cac lenh sau tu thu muc project tren server:

```bash
sudo mkdir -p /var/www/beta-app/html
sudo mkdir -p /var/www/internal-app/html

sudo cp homework/session_03/ex4/beta-app/index.html /var/www/beta-app/html/index.html
sudo cp homework/session_03/ex4/internal-app/index.html /var/www/internal-app/html/index.html
sudo cp homework/session_03/ex4/multi-port.conf /etc/nginx/sites-available/multi-port.conf

sudo ln -s /etc/nginx/sites-available/multi-port.conf /etc/nginx/sites-enabled/multi-port.conf
sudo nginx -t
sudo systemctl reload nginx
```

Neu symbolic link da ton tai thi bo qua lenh `ln -s`.

## Mo cong tren UFW

```bash
sudo ufw allow 8080/tcp
sudo ufw allow 8090/tcp
sudo ufw status
```

Tren DigitalOcean Cloud Firewall, mo them inbound rule:

- TCP `8080` tu `0.0.0.0/0` va `::/0`
- TCP `8090` tu `0.0.0.0/0` va `::/0`

## Kiem tra

Thay `<IP_ADDRESS_DROPLET>` bang IP cua Droplet:

```bash
curl http://<IP_ADDRESS_DROPLET>:8080
curl http://<IP_ADDRESS_DROPLET>:8090
```

Ket qua mong doi:

- Cong `8080` hien thi trang `Beta App`.
- Cong `8090` hien thi trang `Internal App`.
