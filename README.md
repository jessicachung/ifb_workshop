# IFB workshop

Ansible playbook to install software for the IFB workshop (Summer School Genopole 2017).

Steps:
 - In a GVL instance, install the workshop software that can be localised to
   the filesystem. Create an archive then upload it to the object store
   ([documentation](ifb_software_archive.md))
 - Launch as many machines you require for the workshop and add the IP
   addresses to the `hosts` file
 - Run the Ansible playbook
