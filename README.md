# Membuat-tools-monitoring-Mikrotik-menggunakan-Prometheus-dan-Grafana

### Requirements
  1. [Prometheus](https://prometheus.io/download/)
  2. [Grafana](https://grafana.com/)
  3. [Snmp_exporter](https://github.com/prometheus/snmp_exporter)
  4. [Blackbox Exporter](https://prometheus.io/download/)

### Download Prometheus
```Shell
wget https://github.com/prometheus/prometheus/releases/download/v2.53.4/prometheus-2.53.4.linux-amd64.tar.gz
tar xvf prometheus-2.53.4.linux-amd64.tar.gz
cd prometheus-2.53.4.linux-amd64/
```
### Pindahkan File Prometheus dan Promtool
```Shell
sudo mv prometheus promtool /usr/local/bin/
```
### Buat User dan Group khusus untuk Prometheus
```Shell
sudo groupadd --system prometheus
sudo useradd --system -s /sbin/nologin -g prometheus prometheus
```
### Pindahkan console_libraries, console, dan prometehus.yml ke directory /etc/prometheus/
```Shell
sudo mkdir /etc/prometheus
sudo mv consoles/ console_libraries/ prometheus.yml /etc/prometheus/
```
### Membuat Directory prometheus di /var/lib/ dan ubah ownership menjadi user dan grub yang sudah dibuat sebelumnya
```Shell
sudo mkdir /var/lib/prometheus
sudo chown -R prometheus:prometheus /var/lib/prometheus/
```
### Membuat File Unit .service untuk Menjalankan Prometheus sebagai Layanan
```Shell
sudo nano /etc/systemd/system/prometheus.service
```
### Konfigurasi File .service
Pada ExecStart pastikan path sudah sesuai yang dibuat sebelumnya /usr/local/bin/prometheus
```Service
[Unit]
Description=Prometheus
Documentation=https://prometheus.io/docs/introduction/overview/
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
--config.file /etc/prometheus/prometheus.yml \
--storage.tsdb.path /var/lib/prometheus/ \
--web.console.templates=/etc/prometheus/consoles \
--web.console.libraries=/etc/prometheus/console_libraries

[Install]
WantedBy=multi-user.target
```












