## Configuración base e instalación de paquetes con Ansible

Este proyecto de Ansible instala y configura Cloudflared.

- Este playbook de Ansible realiza lo siguiente:
    - Instala openvscode-server
    - Configura openvscode-server como servicio

# Pasos previos
Lo primero que necesitamos es tener 
Este playbook esta configurado para ejecutarse en `localhost` por lo que solo necesitamos acceder al server por ssh y que nuestro usuario tenga permisos de root.

# Modificaciones a ciertos archivos
Hay ciertos archivos que deben modificarse con datos reales de nuestro host, dominios, etc.
- Reemplazar en todo el proyecto el string `<user>` por el nombre real del usuario que se utilizará.

## Como manejar archivos con Ansible
Dentro del directorio `roles/cloudflare/files` se deben adjuntar todos los archivos que serán utilizados por Ansible.

# Ejecución de Ansible
Para ejecutar el playbook de Ansible debes ejecutar el siguiente comando
```bash
ansible-playbook -u <user> -c local -i localhost, ./ubuntu-ansible/004/playbook.yaml -K
```
Para que la ejecución sea mas sencilla te recomiento que el password que utilizaste para encriptar en el paso anterior ahora lo guardes en un archivo en el siguiente directorio `./ubuntu-ansible/004/vault_devops_secret.txt`\
Por último debes indicar la ruta del pplaybook y la opción -K te solicitará la password de root para poder ejecutar el playbook en el sistema.




