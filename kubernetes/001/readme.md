# Instructions to create a Gitlab-runner instance in AWS EC2.

Gitlab-runner is a service that talks with Gitlab CI/CD and execute the pipelines directly in our infrastructure instead of the cloud.
Having a private runner increases performance and security notably comparing to cloud services.

## Creating the Gitlab-runner instance in AWS EC2.

- The creation of the instance (Ubuntu) is fully automated with Terraform.
    - As we still don`t have a Gitlab-runner, this steps needs to be executed in a local machine (laptop)
    - This is the software that you need to install prior to run Terraform
        - Terraform 0.14 or greater
        - Ansible 2.7 or greater
        - Openssh
    - These are the steps to run Terraform
    ```bash
    # first we need to create our Terraform proyect
    terraform init

    # Second we need to check the syntaxis of the files
    terraform fmt

    # Third, we can run a plan to see the object that Terraform will create
    terraform plan

    # Finally, if Terraform plan looks good, we can run apply command and confirm with "yes"
    terraform apply
    ````

## Configuring the instance with Ansible

Now we need to configure the software for the instance (Ubuntu), for this we will use Ansible.
- This Ansible playbook will do the following:
    - Create a `devops` user with password to sudo as root
    - Create an `ansible` user with passwordless root.
    - Adding an `automation ssh public key` to the ansible user.
    - Adding a `human ssh keys` to the devops user.
    - Install Docker and docker-compose.
    - Install gitlab-runner with the root user.

The passwords for `devops` and `ansible` users are encrypted, to run the Ansible playbook we need to create a password file in the path `./ansible/vault_devops_secret.txt`. This password is available in `Doppler`.

We also need to add the `id_automation` ssh private key to be able to run the playbook since the second time.

```bash
# This is the command to run the playbook the first time (with the original account Ubuntu)
ansible-playbook -u ubuntu --private-key ~/.ssh/id_automation -i 75.101.213.76, --vault-password-file ./ansible/vault_devops_secret.txt ./ansible/playbook.yaml

# This is the command to run the playbook normally (with the user Ansible)
ansible-playbook -u ansible --private-key ~/.ssh/id_automation -i 75.101.213.76, --vault-password-file ./ansible/vault_devops_secret.txt ./ansible/playbook.yaml
```



