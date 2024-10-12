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
chown -R prometheus:prometheus /etc/prometheus  
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
  --config.file         /etc/prometheus/prometheus.yml \  
  --storage.tsdb.path   /etc/prometheus/data  

[Install]  
WantedBy=multi-user.target  
systemctl enable prometheus  
systemctl start prometheus  
systemctl status prometheus  
  
Готовим сервера которые будем мониторить  
качаем на сервер wget https://github.com/prometheus/node_exporter/releases/download/v1.8.2/node_exporter-1.8.2.linux-amd64.tar.gz 
распаковываем, затем mv node_exporter /usr/bin/  
создаем системного пользователя useradd -rs /bin/false node_exporter  
chown node_exporter:node_exporter /usr/bin/node_exporter  
nano /etc/systemd/system/node_exporter.service  
[Unit]
Description=Prometheus Node Exporter
After=network.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
Restart=on-failure
ExecStart=/usr/bin/node_exporter

[Install]
WantedBy=multi-user.target
systemctl enable node_exporter   
systemctl start node_exporter   
systemctl status node_exporter 
Перезапускаем сервер systemctl restart prometheus  




