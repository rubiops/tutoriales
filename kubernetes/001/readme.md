## Configuración de las VM con Ansible

Para configurar el software de las VM se utilizará Ansible.
- Este playbook de Ansible hace lo siguiente:
    - Configura el server SSH para evitar login con password e ingresar con usuario root.
    - Habilita UFW (Uncomplicated Firewall)
    - Habilita puerto 22 (ssh)
    - Habilita puerto 6443 (k3s API server)
    - Habilita puerto 10250 (metricas de Kubelet)
    - Habilita puerto 8472 (Flannel VXLAN, OPCIONAL)
    - Habilita puerto 51820 (Flannel Wireguard backend, OPCIONAL)
    - Habilita puerto 51821 (Flannel Wireguard backend con IPv6, OPCIONAL)
    - Habilita puertos 2379:2380 (HA con etcd embebido, OPCIONAL)


```bash
# This is the command to run the playbook the first time (with the original account Ubuntu)
ansible-playbook -u ubuntu --private-key ~/.ssh/id_automation -i 75.101.213.76, --vault-password-file ./ansible/vault_devops_secret.txt ./ansible/playbook.yaml

# This is the command to run the playbook normally (with the user Ansible)
ansible-playbook -u ansible --private-key ~/.ssh/id_automation -i 75.101.213.76, --vault-password-file ./ansible/vault_devops_secret.txt ./ansible/playbook.yaml
```



