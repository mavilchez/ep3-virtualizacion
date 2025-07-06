# Despliegue de WordPress y MySQL en Kubernetes

Este repositorio contiene los manifiestos YAML necesarios para desplegar una aplicación WordPress conectada a una base de datos MySQL en un clúster Kubernetes.

## 📁 Estructura del repositorio

```
.
├── manual-pv.yaml              # PersistentVolumes para MySQL y WordPress (hostPath)
├── mysql-deployment.yaml       # Deployment + Service para MySQL
├── mysql-pvc.yaml              # PersistentVolumeClaim para MySQL
├── namespaces.yaml             # Namespaces para separar WordPress y MySQL
├── wordpress-deployment.yaml   # Deployment + Service para WordPress
├── wordpress-pvc.yaml          # PersistentVolumeClaim para WordPress
```

---

## 🚀 Requisitos previos

- Clúster Kubernetes funcional (al menos 2 nodos)
- Acceso a `kubectl`
- Carpeta `/mnt/data/mysql` y `/mnt/data/wordpress` creadas en los nodos
- Git instalado (si descargas desde el repositorio)

---

## 🔧 Paso a paso para desplegar

### 1. Clonar el repositorio

```bash
git clone https://github.com/mavilchez/ep3-virtualizacion.git
cd ep3-virtualizacion
```

### 2. Crear namespaces

```bash
kubectl apply -f namespaces.yaml
```

### 3. Crear volúmenes persistentes (hostPath)

```bash
kubectl apply -f manual-pv.yaml
```

> **IMPORTANTE**: Asegúrate de tener creados estos directorios en todos los nodos del clúster:

```bash
sudo mkdir -p /mnt/data/mysql /mnt/data/wordpress
sudo chown -R 1000:1000 /mnt/data/
```

### 4. Crear los PVC (PersistentVolumeClaims)

```bash
kubectl apply -f mysql-pvc.yaml
kubectl apply -f wordpress-pvc.yaml
```

### 5. Desplegar MySQL

```bash
kubectl apply -f mysql-deployment.yaml
```

### 6. Desplegar WordPress

```bash
kubectl apply -f wordpress-deployment.yaml
```

---

## 🔑 Credenciales de la base de datos

### MySQL

- Usuario: `wpuser`
- Contraseña: `wppass`
- Base de datos: `wordpress`
- Puerto: `3306`

---

## 🌐 Acceso a WordPress vía navegador

### 1. Obtener IP del nodo

```bash
kubectl get nodes -o wide
```

### 2. Verificar el puerto del `NodePort`

```bash
kubectl get svc -n wordpress
```

Deberías ver algo como:

```
wordpress   NodePort   10.x.x.x   <none>   80:30080/TCP
```

### 3. Abrir en el navegador

```http
http://<IP-del-nodo>:30080
```

Ejemplo:

```http
http://3.85.9.23:30080
```

---

## 🧪 Verificación de estado

- Verifica pods:

```bash
kubectl get pods -n mysql
kubectl get pods -n wordpress
```

Todos deben estar en estado `Running`.

- Verifica PVC:

```bash
kubectl get pvc -A
```

Todos deben estar en estado `Bound`.

---

## 📌 Autor

**Manuel Vilchez**  
Repositorio educativo para despliegue en Kubernetes — *Duoc UC*

---
