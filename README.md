# Install
# Prometheus
Скачиваем отсюда https://prometheus.io/download/  
Распаковываем архив , удаляем все лишнее, оставляя только бинарный и yml файл  
делаем далее все по командам  
mv prometheus /usr/bin  
mkdir /etc/prometheus/data
mv prometheus.yml /etc/prometheus/  
useradd -rs /bin/false prometheus  
chown prometheus:prometheus /usr/bin/prometheus  
chown -R prometheus:prometheus /usr/bin/prometheus  
nano /etc/systemd/system/prometheus.service  
[Unit]  
Description=Prometheus Server  
After=network.target  

[Service]  
User=prometheus  
Group=prometheus  
Type=simple  
Restart=on-failure  
ExecStart=/usr/bin/prometheus \  
  --config.file         /etc/prometheus/prometheus.yaml \  
  --storage.tsdb.path   /etc/prometheus/data  

[Install]  
WantedBy=multi-user.target  
