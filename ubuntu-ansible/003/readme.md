## Configuración base e instalación de paquetes con Ansible

Este proyecto de Ansible instala y configura Cloudflared.

- Este playbook de Ansible realiza lo siguiente:
    - Instala cloudflared
    - Configura cloudflared
    - Crea servicio cloudflared

# Pasos previos
Lo primero que necesitamos es tener 
Este playbook esta configurado para ejecutarse en `localhost` por lo que solo necesitamos acceder al server por ssh y que nuestro usuario tenga permisos de root.

Una vez dentro de la maquina tenemos que instalar cloudflared con [estas instrucciones](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/tunnel-guide/local/#set-up-a-tunnel-locally-cli-setup) y obtener el `certificado` de cloudflared con este comando:
```
cloudflared tunnel login
```

Luego crear un tunel con este comando
```
cloudflared tunnel create <NAME>
```

Y finalmente crear un configuration file como se indica [aquí](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/tunnel-guide/local/#4-create-a-configuration-file).

Una vez obtenidos los 3 archivos se deben dejar en el directorio `./ubuntu-ansible/003/roles/cloudflared/files/`

## Como manejar archivos con Ansible
Dentro del directorio `roles/cloudflare/files` se deben adjuntar todos los archivos que serán utilizados por Ansible.

## Como manejar secretos
Ansible tiene una herramienta para encriptar archivos y variables llamado `ansible vault` que permite manejar secretos y guardanlos directamente en el repositorio sin que nadie pueda descifrar el contenido.

Se puede encriptar un archivo con el siguiente comando:
```
ansible-vault encrypt ./ubuntu-ansible/003/roles/cloudflared/files/example.yaml
```

IMPORTANTE!!!!\
El password de encriptación debes guardarlo en un lugar seguro ya que lo necesitaras para desencriptar o para ejecutar los playbooks

# Ejecución de Ansible
Para ejecutar el playbook de Ansible debes ejecutar el siguiente comando
```bash
ansible-playbook -u <user> -c local -i localhost, --vault-password-file ./ubuntu-ansible/003/vault_devops_secret.txt ./ubuntu-ansible/003/playbook.yaml -K
```
Para que la ejecución sea mas sencilla te recomiento que el password que utilizaste para encriptar en el paso anterior ahora lo guardes en un archivo en el siguiente directorio `./ubuntu-ansible/003/vault_devops_secret.txt`\
Por último debes indicar la ruta del pplaybook y la opción -K te solicitará la password de root para poder ejecutar el playbook en el sistema.




