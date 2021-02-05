# Instalar Kubernetes (K8S-Minikube) con Docker versión 18.

Este GitHub describe la Shell que esta desarrolla para la instalación de Minikube(K8S) con Docker v.18 Localmente.

## Pre-Requisitos
* Oracle Linux Server release 7.8 o superior
* Docker versión 19.* 

## Ejemplos:
En el directorio YML, encontraras los siguientes ejemplos:
* Ejemplo 1: Creación de un Pod por línea de comando.
* Ejemplo 2: Un Pod con dos contenedores Nginx.
* Ejemplo 3: Aplicando Replicación de Pod’s.
* Ejemplo 4: Aplicando un Deployment en un ClusteIP.
* Ejemplo 5: Aplicando Deployment en un App Web. (backend)

## DESCRIPCIÓN SHELL

El objetivo es describir las acciones que realiza para la instalación de Minikube en Oracle Linux Server release 7.8 o superior.

### Actualizar el  Sistema
```sh
$ sudo yum -u update
```
### Validación para Soporte Virtual
```sh
$ grep -q ^flags.*\ hypervisor /proc/cpuinfo && echo "This machine is a VM"
$ sudo yum -y install epel-release
$ sudo yum -y install libvirt qemu-kvm virt-install 
$ sudo yum -y install virt-top libguestfs-tools bridge-utils
$ sudo systemctl start libvirtd
$ sudo systemctl enabled libvirtd
$ sudo systemctl status libvirtd
$ sudo usermod -a -G libvirt $(whoami)
```
### Agregar las siguientes lineas al archivo libvirtd.conf
```sh
$ sudo su -
$ echo 'unix_sock_group = "libvirt"' >> /etc/libvirt/libvirtd.conf
$ echo 'unix_sock_rw_perms = "0770"' >> /etc/libvirt/libvirtd.conf
$ exit
$ sudo systemctl restart libvirtd.service
```
## DOCKER
### Stop Docker si Existe.
```sh
$ sudo systemctl stop docker
```
## Docker-Engine (Oracle Linux)
Pre-Requisito: copiar docker.repo a /etc/yum.repos.d/
Usuario: root
### Oracle Linux 7.x
```sh
$ sudo yum -y install docker-engine
$ sudo docker version
$ sudo systemctl start docker
$ sudo systemctl enable docker
$ sudo usermod -aG docker root
$ sudo usermod -ag docker desa
```
### Configura usuarios.
```sh
$ sudo usermod -aG docker $USER
```
### Instalar Docker Service.
```sh
$ sudo systemctl start docker
$ sudo systemctl enable docker
$ sudo systemctl status docker
```
### Docker Versión.
```sh
$ sudo docker version
```
## INSTALAR KUBECTL.
### Descarga de kubectl.
```sh
$ curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
```
### Asigna Permiso de ejecucion.
```sh
$ chmod +x ./kubectl
```
### Mueve kubectl a /usr/local/bin.
```sh
$ if [ -f /usr/local/bin/kubectl ]
  then
    echo "### Existe kubectl, se elimina en /usr/bin. ###"
    sudo rm -rf /usr/local/bin/kubectl
  fi
$ sudo mv ./kubectl /usr/local/bin/kubectl
```
### Valida kubectl en /usr/bin.
```sh
$ ls -l /usr/local/bin/kubectl
```
### Versión kubectl.
```sh
$ kubectl version --client
$ kubectl version --client -o json
```
## INSTALAR MINIKUBE.
### Descarga minikube.
```sh
$ wget https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
```
### Asigna Permiso de ejecucion.
```sh
$ chmod +x minikube-linux-amd64
```
### Mueve minikube a /usr/bin.
```sh
$ if [ -f /usr/local/bin/minikube ]
$ then
    echo "### Existe minikube, se elimina en /usr/bin. ###"
    sudo rm -rf /usr/local/bin/minikube
  fi
$ sudo mv minikube-linux-amd64 /usr/local/bin/minikube
```
### Valida minikube en /usr/local/bin.
```sh
$ ls -l /usr/local/bin/minikube
```
### Versión minikube.
```sh
$ minikube version
```
### Iniciar  minikube.
```sh
$ minikube start --kubernetes-version=''
```
### Status minikube.
```sh
$ minikube status
```

SACACIngeniería

### SACACI Chile

