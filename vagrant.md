# INFRAESTRUCTURA CON VAGRANT

## Levantar usando Vagrant varias máquinas virtuales con SO Debian sin entorno gráfico. El número de máquinas virtuales dependerá de los recursos libres del sistema donde se ejecuten (aprox. 2GB HDD y 256MB RAM).

Lo que haremos en primer lugar será abrir una terminal en **Powershell** y descargar **Vagrant** y **VirtualBox**. Verificaremos que los tenemos instalados con los comandos 
```shell
vagrant -v
virtualbox -v
```
Luego crearemos un directorio donde almacenaremos el proyecto de Vagrant:
```shell
mkdir D:\vagrant
cd vagrant
```
e inicializaremos Vagrant con el comando 
```shell
vagrant init
```

Una vez hecho esto editaremos nuestro **Vagrantfile** escribiendo las siguientes líneas:

```python
Vagrant.configure("2") do |config|
  config.vm.box = "debian/bookworm64"

  (1..5).each do |i|
    config.vm.define "debian_vm#{i}" do |node|
      node.vm.hostname = "debian-vm#{i}"
      
      # Asignar IPs estáticas para evitar conflictos de DHCP
      node.vm.network "private_network", type: "static", ip: "192.168.56.1#{i}"

      node.vm.provider "virtualbox" do |vb|
        vb.memory = "256"  # 256MB de RAM
        vb.cpus = 1        # 1 núcleo de CPU
      end
    end
  end
end
```
Este código desplegará cinco máquinas virtuales que usarán la imagen base de **Debian Bookworm (64 bits)**, cada una con **256MB de RAM** y **1 núcleo de CPU**. A cada una de ellas se asignará una **IP estática** para evitar conflictos de DHCP.

Guardamos los cambios en dicho archivo y finalmente ejecutamos el comando
```shell
vagrant up
```
para levantar nuestras máquinas.




