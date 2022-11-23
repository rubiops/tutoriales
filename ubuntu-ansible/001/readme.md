## Configuración base e instalación de paquetes con Ansible

Este proyecto de Ansible realiza configuraciones básicas de seguridad.

- Este playbook de Ansible realiza lo siguiente:
    - Configura el nombre del hostname.
    - Crea el usuario '<user>'.
      - Se crea el usuario con grupo sudo y se configura el password del usuario.
      - Se crea el archivo authorized keys y se agrega la llave pública SSH al archivo. 
    - Configura el server SSH para evitar login con password y con usuario root.
    - Copia el archivo .bashrc al home del usuario '<user>'
    - Habilita UFW (Uncomplicated Firewall)
    - Habilita puerto 22 (ssh)
    - Habilita puerto 80 y 443 (Web)
    - Habilita puertos 60001 a 60999 (Mosh)
    - Habilita puerto 51820 (Wireguard)

# Pasos previos
Este playbook esta configurado para ejecutarse en `localhost` por lo que solo necesitamos acceder al server por ssh y que nuestro usuario tenga permisos de root.\

# Modificaciones a ciertos archivos
Hay ciertos archivos que deben modificarse con datos reales de nuestro host, dominios, etc.
- En el archivo `playbook.yaml` se debe agregar una llave pública real para poder ingresar al host y al usuario que será creado.
- Reemplazar en todo el proyecto el string `<user>` por el nombre real del usuario a crear.
- Reemplazar en main.yml el string `<hostname>` por el nombre real con el que se llamará el host.

## Como manejar archivos con Ansible
Dentro del directorio `roles/base/files` se deben adjuntar todos los archivos que serán utilizados por Ansible.
En este caso son 2 archivos, `.bashrc` y `sshd_config`.
estos archivos serán utilizados por el playbook para configurar la configuración ssh y .bashrc en nuestro servidor.\

## Como manejar el password para el usuario <user>
Ansible tiene una herramienta para encriptar archivos y variables llamado `ansible vault` que permite manejar secretos y guardanlos directamente en el repositorio sin que nadie pueda descifrar el contenido.\
Se debe crear un archivo en el siguiente directorio `./ubuntu-ansible/001/roles/base/vars/main.yaml` para que pueda ser reconocido por Ansible.\

El archivo debe tener el siguiente formato:
```
var_name: password

# Por ejemplo algo así:
user_password: 123456qwerty
```

Luego se debe encriptar el archivo con el siguiente comando:
```
ansible-vault encrypt ./ubuntu-ansible/001/roles/base/vars/main.yaml
```

IMPORTANTE!!!!\
El password de encriptación debes guardarlo en un lugar seguro ya que lo necesitaras para desencriptar o para ejecutar los playbooks

# Ejecución de Ansible
Para ejecutar el playbook de Ansible debes ejecutar el siguiente comando
```bash
ansible-playbook -u <user> -c local -i localhost, --vault-password-file ./ubuntu-ansible/001/vault_devops_secret.txt ./ubuntu-ansible/001/playbook.yaml -K
```
Para que la ejecución sea mas sencilla te recomiento que el password que utilizaste para encriptar en el paso anterior ahora lo guardes en un archivo en el siguiente directorio `./ubuntu-ansible/001/vault_devops_secret.txt`\
Por último debes indicar la ruta del pplaybook y la opción -K te solicitará la password de root para poder ejecutar el playbook en el sistema.




