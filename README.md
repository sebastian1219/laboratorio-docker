# Kubernetes WordPress + MariaDB + phpMyAdmin Lab

Este repositorio contiene un laboratorio en Kubernetes desplegado en Docker Desktop que implementa:
- **WordPress** (expuesto en `NodePort 30088`)
- **phpMyAdmin** (expuesto en `NodePort 30080`)
- **MariaDB** (servicio ClusterIP, accesible solo dentro del clúster)
- **Almacenamiento persistente** con PV y PVC para MariaDB

## 📂 Estructura de Archivos
docker-desktop-wordpress-lab/
│
├── manifests/
│ ├── namespace.yaml
│ ├── mariadb-pv-pvc.yaml
│ ├── mariadb-secret.yaml
│ ├── mariadb-deployment.yaml
│ ├── mariadb-service.yaml
│ ├── wordpress-pv.yaml
│ ├── wordpress-phpmyadmin.yaml
│ ├── phpmyadmin-deployment.yaml
│ ├── networkpolicy-mariadb.yaml
│ ├── check-pv.yaml # (opcional, para debug)
│ ├── debug-pod.yaml # (opcional, para debug)
│ └── debug-host.yaml # (opcional, para debug)
└── README.md
## 🚀 Despliegue

```bash
# Crear namespace
kubectl create namespace wordpress-app

# Aplicar todos los manifiestos
kubectl apply -f manifests/ -n wordpress-app

# Verificar que los pods están corriendo
kubectl get pods -n wordpress-app

🌐 Acceso a la Aplicación

WordPress: http://localhost:30088

phpMyAdmin: http://localhost:30080

# Escalar WordPress
kubectl scale deployment wordpress-deployment --replicas=3 -n wordpress-app

# Verificar servicios y puertos
kubectl get svc -n wordpress-app -o wide

# Probar conectividad interna
kubectl exec -it -n wordpress-app deploy/wordpress -- curl -I http://localhost:8080
🛡 Seguridad

MariaDB se expone solo como ClusterIP

Namespace dedicado para aislar recursos
