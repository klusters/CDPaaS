# Set env vars
```bash
export BWS_ACCESS_TOKEN=<token>
export GOOGLE_APPLICATION_CREDENTIALS=<path to service account json>
export GCLOUD_PROJECT=cdp-training
workon CDPaaS || mkvirtualenv CDPaaS
```
# Install requirements
```bash
cd ansible
pip install -r python_requirements.txt
ansible-galaxy install -n -r ansible_requirements.yml
```
# Create infra
```bash
ansible-playbook -i inventories/openvpn_lab_anis playbooks/infra/openvpn/site.yml
ansible-playbook -i inventories/freeipa_lab_anis playbooks/infra/freeipa/site.yml
ansible-playbook -i inventories/cdp_lab_anis playbooks/infra/cdp/site.yml
ansible-playbook -i inventories/cdp_lab_anis playbooks/deployment/site.yml
```

# Destroy infra
```bash
ansible-playbook -i inventories/freeipa_lab_anis playbooks/infra/common/destroy_infra.yml
ansible-playbook -i inventories/openvpn_lab_anis playbooks/infra/common/destroy_infra.yml
ansible-playbook -i inventories/cdp_lab_anis playbooks/infra/common/destroy_infra.yml
```
