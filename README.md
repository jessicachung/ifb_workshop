# IFB workshop

Ansible playbook to install software for the IFB workshop (Summer School Genopole 2017).

### Steps:

In a GVL instance, install the workshop software that can be localised to
the filesystem. Create an archive then upload it to the object store
([documentation](ifb_software_archive.md)).

Launch as many machines you require for the workshop using the [
launch_multiple_gvls](https://github.com/gvlproject/gvl.utilities/tree/master/launch_multiple_gvls) 
script.

```bash
# Set environment variables
# WORKSHOP_PASSWORD
# EC2_ACCESS_KEY
# EC2_SECRET_KEY

virtualenv -p /usr/bin/python venv
source venv/bin/activate
pip install -r requirements.txt

python launch_gvl.py \
    -a ${EC2_ACCESS_KEY} \
    -s ${EC2_SECRET_KEY} \
    -i ami-00c0fcea \
    -z NCI \
    -t m1.large \
    -p ${WORKSHOP_PASSWORD} \
    -u sample_data.txt \
    -c ifb-workshop \
    -n 6 \
    --num_start 1
```

Add the IP addresses to the `hosts` file.

Run the Ansible playbook.

```
# Ignore SSH authenticity check
export ANSIBLE_HOST_KEY_CHECKING=False

ansible-playbook -i hosts playbook.yml | tee ansible_log_$(date +%F_%s).log
```
