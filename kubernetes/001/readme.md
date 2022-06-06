## Levantando k3s en un ambiente local con Ansible

Este es el código que permite levantar un cluster k3s local con un nodo control plane y multiples nodos worker.

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


```bash
# This is the command to run the playbook the first time (with the original account Ubuntu)
ansible-playbook -u ubuntu --private-key ~/.ssh/id_automation -i 75.101.213.76, --vault-password-file ./ansible/vault_devops_secret.txt ./ansible/playbook.yaml

# This is the command to run the playbook normally (with the user Ansible)
ansible-playbook -u ansible --private-key ~/.ssh/id_automation -i 75.101.213.76, --vault-password-file ./ansible/vault_devops_secret.txt ./ansible/playbook.yaml
```



