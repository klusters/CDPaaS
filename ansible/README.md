# Set env vars
```bash
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
# Create secrets file
```bash
gcloud secrets versions access latest --secret=bw_secrets --out-file inventories/freeipa_lab_anis/group_vars/all/secrets.yml
cp inventories/freeipa_lab_anis/group_vars/all/secrets.yml inventories/cdp_lab_anis/group_vars/all/secrets.yml
```
# Create infra
```bash
ansible-playbook -i inventories/openvpn_lab_anis playbooks/infra/openvpn/site.yml
ansible-playbook -i inventories/freeipa_lab_anis playbooks/infra/freeipa/site.yml
ansible-playbook -i inventories/cdp_lab_anis playbooks/infra/cdp/site.yml
ansible-playbook -i inventories/cdp_lab_anis -i inventories/freeipa_lab_anis playbooks/deployment/site.yml
```
# Local Config
```bash
ansible-playbook -i inventories/cdp_lab_anis -i inventories/freeipa_lab_anis playbooks/tools/local_etc_hosts.yml
ansible-playbook -i inventories/cdp_lab_anis -i inventories/freeipa_lab_anis playbooks/tools/local_trust_ca.yml
```

# Destroy infra
```bash
ansible-playbook -i inventories/freeipa_lab_anis playbooks/infra/common/destroy_infra.yml
ansible-playbook -i inventories/openvpn_lab_anis playbooks/infra/common/destroy_infra.yml
ansible-playbook -i inventories/cdp_lab_anis playbooks/infra/common/destroy_infra.yml
```
