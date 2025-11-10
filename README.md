# Yet Another One To-Do App!

This **To-Do App** is a user-friendly task management tool designed to help individuals organize their daily activities efficiently. 
With a clean and intuitive interface, users can easily add and delete tasks, ensuring that nothing important is overlooked.

Key Features:
- Task Management: Users can create tasks with titles and descriptions, allowing for detailed tracking of responsibilities.
- Customizable Appearance: The application offers settings to customize background colors, button colors, and secondary colors, enabling users to personalize their experience.
- Persistent Storage: Tasks are saved to a local file, ensuring that users' lists are preserved even after closing the application.
- User-Friendly Interface: The layout is designed for ease of use, with clearly labeled buttons and input fields for quick task entry.
- Settings Management: Users can easily access a settings window to adjust the application's appearance or restore default settings.
- Visual Feedback: The application provides visual cues and warnings to guide users, such as alerts for invalid task entries or reminders to select a task before deletion.

Whether you're managing personal errands, work projects, or study schedules, the To-Do List Application is a versatile tool that helps you stay organized and productive.

# How to use

1) Download the code:
```
git clone https://github.com/triple0ero/todo_app.git
```
3) Install dependencies:
```
pip install -r requirements.txt
```
3) Start the app:
```
python3 main.py
```
4) Enjoy!

# Краткая инструкция: прозрачный Squid proxy с ICAP DLP

## Настройка Squid на RedOS

**Установка:**
```bash
dnf install squid
sed -i 's/^SELINUX=.*/SELINUX=disabled/g' /etc/selinux/config
reboot
```

**Сертификаты:**
```bash
mkdir -p /etc/squid/ssl_cert /var/lib/squid/ssl_db
chown -R squid:squid /etc/squid/ssl_cert /var/lib/squid

openssl req -new -newkey rsa:2048 -days 3650 -nodes -x509 \
  -keyout /etc/squid/ssl_cert/iwproxy.key \
  -out /etc/squid/ssl_cert/iwproxy.crt \
  -subj "/C=RU/ST=Moscow/L=Moscow/O=IWProxy/CN=iwproxy.demo.lab" \
  -addext "subjectAltName = DNS:iwproxy.demo.lab, DNS:iwproxy"

openssl dhparam -out /etc/squid/ssl_cert/iwproxy_dhparam.pem 2048
chown squid:squid /etc/squid/ssl_cert/iwproxy*
chmod 400 /etc/squid/ssl_cert/iwproxy*

systemctl stop squid
rm -rf /var/lib/squid/ssl_db
/usr/lib64/squid/security_file_certgen -c -s /var/lib/squid/ssl_db -M 20MB
```

**Конфиг `/etc/squid/squid.conf`:**
```
visible_hostname iwproxy.demo.lab
http_port 3130
acl intermediate_fetching transaction_initiator certificate-fetching
http_access allow intermediate_fetching

http_port 3128 transparent ssl-bump \
  tls-cert=/etc/squid/ssl_cert/iwproxy.crt \
  tls-key=/etc/squid/ssl_cert/iwproxy.key \
  tls-dh=/etc/squid/ssl_cert/iwproxy_dhparam.pem

https_port 3129 intercept ssl-bump \
  tls-cert=/etc/squid/ssl_cert/iwproxy.crt \
  tls-key=/etc/squid/ssl_cert/iwproxy.key \
  tls-dh=/etc/squid/ssl_cert/iwproxy_dhparam.pem

sslproxy_cert_error allow all
ssl_bump stare all

sslcrtd_program /usr/lib64/squid/security_file_certgen -s /var/lib/squid/ssl_db -M 20MB

icap_enable on
icap_send_client_ip on
icap_service service_req reqmod_precache bypass=1 icap://192.168.88.101:1344/request
adaptation_access service_req allow all

http_access allow all
cache_dir ufs /var/spool/squid 100 16 256
```

**Запуск:**
```bash
systemctl enable squid
systemctl start squid
```

## Перенаправление трафика
```bash
echo 1 > /proc/sys/net/ipv4/ip_forward
iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 3128
iptables -t nat -A PREROUTING -p tcp --dport 443 -j REDIRECT --to-port 3129
iptables -t nat -I PREROUTING -d 192.168.88.0/24 -j RETURN
iptables -t nat -I PREROUTING -p udp --dport 53 -j RETURN
iptables -t nat -I PREROUTING -p tcp --dport 53 -j RETURN
```

## На клиентах
Импортировать `/etc/squid/ssl_cert/iwproxy.der` в доверенные корневые сертификаты.

**Порты:**
- ICAP: 1344 (DLP)
- HTTP: 3128 (Squid)
- HTTPS: 3129 (Squid)
