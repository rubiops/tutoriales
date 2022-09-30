## Levantando k3s en un ambiente local con Ansible

Este es el código que permite levantar un cluster k3s local con un nodo control plane y multiples nodos worker.
Para salir a internet se utilizará cloudflare tunnel ya que encripta la conexión y además permite utilizar redes hogareñas con IP dinámica.

Para configurar el software de las VM se debe ejecutar Ansible en todos los nodos del cluster.
- Este playbook de Ansible hace lo siguiente:
    - Configura el server SSH para evitar login con password y con usuario root.
    - Habilita UFW (Uncomplicated Firewall)
    - Habilita puerto 22 (ssh)
    - Habilita puerto 443 y 6443 (k3s API server)
    - Habilita puerto 10250 (metricas de Kubelet)
    - Habilita puerto 8472 (Flannel VXLAN, OPCIONAL)
    - Habilita puerto 51820 (Flannel Wireguard backend, OPCIONAL)
    - Habilita puerto 51821 (Flannel Wireguard backend con IPv6, OPCIONAL)
    - Habilita puertos 2379:2380 (HA con etcd embebido, OPCIONAL)
    - Instala cloudflared para utilizar cloudflare tunnel con Kubernetes

# Pasos previos
Lo primero que necesitamos es tener acceso por ssh a los nodos del cluster.
La manera mas sencilla es copiando nuestra llave ssh pública a los nodos. Para esto la manera mas sencilla es 
ejecutar este comando.
```bash
ssh-copy-id -i ~/.ssh/mykey.pub user@host-ip
```
Esto nos permitirá acceder a los nodos sin password.

Otro paso muy importante es agregar las IPs de nuestros nodos k3s al archivo inventory, el que será utilizado por Ansible para saber a que nodos debe aplicar las configuraciones.

El ultimo paso previo es agregar una lista de variables de nuestros nodos en el archivo playbook.yaml para que estos nodos sean agregados al `/etc/hosts` de cada vm para que sean visibles entre ellos.
```yaml
cluster_nodes:
    - '192.168.54.166 k3smaster01'
    - '192.168.54.155 k3sworker01'
```

# Ejecución de Ansible
Para ejecutar el playbook de Ansible debes ejecutar el siguiente comando
```bash
ansible-playbook -u rubiops --private-key ~/.ssh/id_rsa -i ./kubernetes/001/inventory ./kubernetes/001/playbook.yaml -K
```
Es importante que apuntes a la llave privada ssh que se utilizó en los `pasos previos`.
Tambien debes apuntar a los archivos `inventory` y `playbook.yaml` para que Ansible sepa a que VM llegar y que playbook ejecutar.
Por ultimo la opción -K te solicitará la password de root para poder ejecutar los playbook en cada VM.

# Configuración de cloudflared
`Estos pasos se deben realizar en un worker, no el control plane`
Para obtener el certificado de cloudflared se debe ejecutar el siguiente comando y realizar los pasos necesarios
```bash
cloudflared tunnel login
```

Una vez obtenido el certificado es necesario crear un tunel, el que será mas adelante utilizado por Kubernetes
```bash
cloudflared tunnel create example-tunnel
```



