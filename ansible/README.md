# Set env vars
```bash
export GOOGLE_APPLICATION_CREDENTIALS=<path to service account json>
export GCLOUD_PROJECT=primeval-pixel-403806
workon CDPaaS || mkvirtualenv CDPaaS
```
# Install requirements
```bash
cd ansible
pip install -r python_requirements.txt
ansible-galaxy install -n -r ansible_requirements.yml
```
# Create secrets file
```bash
gcloud secrets versions access latest --secret=bw_secrets --out-file inventories/lab_ses/freeipa/group_vars/all/secrets.yml
cp inventories/lab_ses/freeipa/group_vars/all/secrets.yml inventories/lab_ses/ecs/group_vars/all/secrets.yml
```
# Create infra
```bash
ansible-playbook -i inventories/lab_ses/network playbooks/infra/common/init_infra.yml
ansible-playbook -i inventories/lab_ses/fw_rules playbooks/infra/common/init_infra.yml
terraform -chdir=tf-cdp-cluster-${USER,,}/layers/fw_rules init
terraform -chdir=tf-cdp-cluster-${USER,,}/layers/fw_rules workspace new ${USER,,}
ansible-playbook -i inventories/lab_ses/my_fw_rules playbooks/infra/common/init_infra.yml
ansible-playbook -i inventories/lab_ses/vpn playbooks/infra/openvpn/site.yml
ansible-playbook -i inventories/lab_ses/freeipa playbooks/infra/freeipa/site.yml
ansible-playbook -i inventories/lab_ses/ecs playbooks/infra/cdp/site.yml
ansible-playbook -i inventories/lab_ses/ecs -i inventories/lab_ses/freeipa playbooks/deployment/site.yml
```
# Local Config
```bash
ansible-playbook -i inventories/lab_ses/ecs -i inventories/lab_ses/freeipa playbooks/tools/local_etc_hosts.yml
ansible-playbook -i inventories/lab_ses/ecs -i inventories/lab_ses/freeipa playbooks/tools/local_trust_ca.yml
```

# Destroy infra
```bash
ansible-playbook -i inventories/lab_ses/vpn playbooks/infra/common/destroy_infra.yml
ansible-playbook -i inventories/lab_ses/freeipa playbooks/infra/common/destroy_infra.yml
ansible-playbook -i inventories/lab_ses/ecs playbooks/infra/common/destroy_infra.yml
```
