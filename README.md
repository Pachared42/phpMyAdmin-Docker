# phpMyAdmin + MySQL (Docker Desktop)

โปรเจกต์นี้ใช้สำหรับรัน **MySQL และ phpMyAdmin ผ่าน Docker Desktop**
เพื่อใช้จัดการฐานข้อมูลผ่าน Web UI ได้ง่าย โดยไม่ต้องติดตั้ง MySQL
ลงเครื่องโดยตรง

------------------------------------------------------------------------

## คุณสมบัติ

-   Run MySQL 8.0 in Docker
-   Manage database via phpMyAdmin (Web UI)
-   Persistent data with Docker Volume (ข้อมูลไม่หาย)
-   Easy start / stop with Docker Compose
-   รองรับหลายฐานข้อมูล (ไม่จำกัด)

------------------------------------------------------------------------

## ความต้องการ

-   Docker Desktop (Windows / Mac / Linux)

ดาวน์โหลด Docker: https://www.docker.com/products/docker-desktop

------------------------------------------------------------------------

## การติดตั้ง

### 1. Clone หรือสร้างโฟลเดอร์โปรเจกต์

``` bash
mkdir phpmyadmin-docker
cd phpmyadmin-docker
```

------------------------------------------------------------------------

### 2. สร้างไฟล์ `docker-compose.yml`

``` yaml
services:
  mysql:
    image: mysql:8.0
    container_name: mysql_db
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: my_database
    ports:
      - "3306:3306"
    volumes:
      - mysql_data:/var/lib/mysql

  phpmyadmin:
    image: phpmyadmin:latest
    container_name: phpmyadmin
    restart: unless-stopped
    environment:
      PMA_HOST: mysql
      PMA_PORT: 3306
    ports:
      - "8080:80"
    depends_on:
      - mysql

volumes:
  mysql_data:
```

------------------------------------------------------------------------

### 3. เริ่มต้นคอนเทนเนอร์

``` bash
docker compose up -d
```

------------------------------------------------------------------------

## เข้าถึง phpMyAdmin

เปิดในเบราว์เซอร์:

    http://localhost:8080

------------------------------------------------------------------------

## เข้าสู่ระบบ

  Server     mysql
  
  Username   root
  
  Password   root

------------------------------------------------------------------------

## ฐานข้อมูล

`MYSQL_DATABASE=my_database` เป็นเพียง **ฐานข้อมูลเริ่มต้น**

สามารถสร้างฐานข้อมูลเพิ่มได้ไม่จำกัดผ่าน phpMyAdmin หรือ SQL

ตัวอย่าง:

``` sql
CREATE DATABASE stock_db;
CREATE DATABASE users_db;
```

------------------------------------------------------------------------

## คำสั่ง Docker

### Stop containers (หยุดชั่วคราว)

``` bash
docker compose stop
```

### Start อีกครั้ง

``` bash
docker compose start
```

### Remove containers (ข้อมูลยังอยู่)

``` bash
docker compose down
```

### Remove ทุกอย่างรวมฐานข้อมูล ⚠️

``` bash
docker compose down -v
```

------------------------------------------------------------------------

## การเชื่อมต่อ MySQL เริ่มต้น

ใช้สำหรับ PHP / API / XAMPP

    Host: 127.0.0.1
    Port: 3306
    User: root
    Password: root
    Database: my_database

------------------------------------------------------------------------

## หมายเหตุ

-   ถ้าใช้ XAMPP ให้เปิดเฉพาะ **Apache** และปิด MySQL ใน XAMPP
-   ถ้า Port 3306 ถูกใช้งาน ให้เปลี่ยนใน `docker-compose.yml`
-   ข้อมูล MySQL ถูกเก็บใน Docker Volume (`mysql_data`) จะไม่หายแม้
    container ถูกลบ

------------------------------------------------------------------------

## ใบอนุญาต

โครงการนี้จัดทำขึ้นเพื่อการพัฒนาและการศึกษาเท่านั้น

