Role Name
=========

This role allows a user to perform terraform tasks. these tasks include manipulating plans, restarting init, configuring variables, and updating backend configs. 

Requirements
------------
have the terraform plugin installed on your machine. can install with: ansible-galaxy collection install community.general

Role Variables
--------------
project_path_input: "/home/ubuntu/terraform_project" This is the path to the terraform project
plan_file_input: "/home/ubuntu/terraform_project/test.tfplan" This is the default file name for the terraform plan. 
ami_input: "ami-00399ec92321828f5" Default ami to use on ec2 instances

Dependencies
------------

N/A

Example Playbook
----------------

---
- name: create tfplan dont execute
  hosts: terraform
  tasks:
    - include_role: 
        name: terraform
        tasks_from: create_plan.yml
      when: state == "create only"

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
