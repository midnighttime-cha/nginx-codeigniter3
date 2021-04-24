# วิธีตั้งค่า NGINX เพื่อใช้งานกับ Codeigniter3 บน Ubuntu
ความรู้พื้นฐานที่ต้องใช้
- การตั้งค่า nginx เบื้องต้น
- Linux command line

### บทความที่เกี่ยวข้อง
- [วิธีติดตั้ง PHP หลาย Version ให้ใช้งานกับ NGINX บน Ubuntu 20.04](https://github.com/midnighttime-cha/nginx-multiple-php)

## สร้างไฟล์ `mywebsite.conf`
ให้ทำการสร้างไฟล์ในการตั้งค่าขึ้นมา ควรจะสร้างแยกเฉพาะแต่ละเว็บขึ้นมากรณีมีหลายเว็บรวมๆกัน
```bash
sudo nano /etc/nginx/sites-available/mywebsite.conf
```
สร้าง Shortcut link ไปที่ `sites-enabled`
```bash
sudo ln -s /etc/nginx/sites-available/mywebsite.conf /etc/nginx/sites-enabled/
```
ตั้งค่าตามตัวอย่างต่อไปนี้
```
# FileName: /etc/nginx/sites-available/mywebsite.conf
server {
  listen 80;
  server_name www.mywebsite.com subdomain.mywebsite.com;

  location / {
    root /var/www/mywebsite;
    index index.php;
    try_files $uri $uri/ /index.php;

    location ~ \.php$ {
      include snippets/fastcgi-php.conf;
      fastcgi_pass unix:/var/run/php/php7.1-fpm.sock;
      fastcgi_param SCRIPT_FILENAME $document_root/index.php;
    }

    location ~ /\.ht {
      deny all;
    }
  }
}
```
ทำการตรวจความถูกต้องของการตั้งค่า
```bash
sudo nginx -t
```
ถ้าระบบแสดงข้อความต่อไปนี้แสดงว่าทำการตั้งค่าถูกต้องแล้ว
```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```
จากนั้น `restart nginx` ด้วยคำสั่ง
```bash
sudo systemctl restart nginx
```
