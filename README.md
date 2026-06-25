# 📰 Proyecto #14 - FreshRSS - Sistemas Operativos I

**Universidad:** ESAN  
**Curso:** Sistemas Operativos I - 2026-1  
**Proyecto:** #14 - Lector de noticias RSS (FreshRSS)  
**Tema de SO:** Tareas programadas y procesos (cron)  
**Integrantes:** [Abdiel Ramos y Abel Flores]

## 🌐 Aplicación en producción
[https://freshrss.xyz](https://freshrss.xyz)

## 🛠️ Stack tecnológico
- AWS EC2 (Ubuntu 24.04)
- Podman 4.9.3 (rootless)
- FreshRSS 1.29.1
- Nginx (reverse proxy)
- Let's Encrypt (SSL)
- Cron (tareas programadas)
- Dynadot (dominio)

## Comandos de despliegue

### 1. Instalar Podman
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y podman
```

### 2. Desplegar FreshRSS con límites de cgroups
```bash
podman run -d \
  --name freshrss \
  -p 8080:80 \
  -e TZ=America/Lima \
  -e CRON_MIN=1,31 \
  --cpus 0.5 \
  --memory 256m \
  --memory-swap 512m \
  -v freshrss_data:/var/www/FreshRSS/data \
  -v freshrss_extensions:/var/www/FreshRSS/extensions \
  --restart unless-stopped \
  lscr.io/linuxserver/freshrss:latest
```

### 3. Configurar cron
```bash
crontab -e
# Agregar:
*/30 * * * * podman exec freshrss php /var/www/FreshRSS/app/actualize_script.php >> /home/ubuntu/freshrss-cron.log 2>&1
```

##  Evidencias de SO
- `podman ps` - contenedor corriendo
- `podman stats --no-stream freshrss` - CPU y RAM
- `podman top freshrss` - procesos
- `free -h` - memoria del sistema
- `crontab -l` - tareas programadas
- `cat /proc/[PID]/status` - info del proceso
