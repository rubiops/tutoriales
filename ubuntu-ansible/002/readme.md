## Configuración base e instalación de paquetes con Ansible

Este proyecto de Ansible instala software base.

- Este playbook de Ansible realiza lo siguiente:
    - Instala software base como gnupg, curl, ansible, wireguard, etc.
    - Instala Mosh shell
    - Instala Terraform
    - Instala Kubectl
    - Instala GCloud CLI
    - Instala Wireguard
    - Instala Docker
    - Instala Kubectx
    - Instala Kube-ps1

# Pasos previos
Lo primero que necesitamos es tener 
Este playbook esta configurado para ejecutarse en `localhost` por lo que solo necesitamos acceder al server por ssh y que nuestro usuario tenga permisos de root.

# Modificaciones a ciertos archivos
Hay ciertos archivos que deben modificarse con datos reales de nuestro host, dominios, etc.
- Reemplazar en todo el proyecto el string `<user>` por el nombre real del usuario que se utilizará.

## Como manejar archivos con Ansible
Dentro del directorio `roles/software/files` se deben adjuntar todos los archivos que serán utilizados por Ansible.

## Como manejar secretos
Ansible tiene una herramienta para encriptar archivos y variables llamado `ansible vault` que permite manejar secretos y guardanlos directamente en el repositorio sin que nadie pueda descifrar el contenido.

Se puede encriptar un archivo con el siguiente comando:
```
ansible-vault encrypt ./ubuntu-ansible/001/roles/base/vars/main.yaml
```

IMPORTANTE!!!!\
El password de encriptación debes guardarlo en un lugar seguro ya que lo necesitaras para desencriptar o para ejecutar los playbooks

# Ejecución de Ansible
Para ejecutar el playbook de Ansible debes ejecutar el siguiente comando
```bash
ansible-playbook -u <user> -c local -i localhost, --vault-password-file ./ubuntu-ansible/002/vault_devops_secret.txt ./ubuntu-ansible/002/playbook.yaml -K
```
Para que la ejecución sea mas sencilla te recomiento que el password que utilizaste para encriptar en el paso anterior ahora lo guardes en un archivo en el siguiente directorio `./ubuntu-ansible/002/vault_devops_secret.txt`\
Por último debes indicar la ruta del pplaybook y la opción -K te solicitará la password de root para poder ejecutar el playbook en el sistema.




