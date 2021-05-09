# Environment #
* For this assignment i stood up a terraform server, and a jenkins server
* The flow is Jenkins pipeline grabs the latest code from this repo. 
* Then using the Ansible Jenkins plugin I run the ansible code. 
* On the jenkins server before you build you have to enter paramters. 
* These paramters are then used to be inserted into the ansible code.
* Also note I ran this on a terraform host. This could easily be converted to localhost as well if the terraform project is also located on the same server as jenkins. 
* During the demo I will walk through how I have the terraform project setup, the jenkins pipeline configuration, and lastly run the pipeline for the various tasks listed below.
* For my pipeline I call site/terraform_project.yml. This is the main playbook which will call the tasks stated below. Depending on the vars passed in from jenkins will determine what is ran from the playbook. 
# Functionality #
* Ability to set backend configuration during runtime
  * If you check roles/terraform/tasks/config_backend you can see how I used the built in terraform role
  * This config works both at runtime for existing projects or if you are deploying a new project it will configure the backend
* Ability to pass terraform variables through ansible vars
  * If you check roles/terraform/tasks/create_plan.yml you can see there are two options. One will use default variables the other will accept user inputted variables. 
  * I also created roles/terraform/tasks/update_terraform_vars.yml. This is not called but i wrote it as a way to show you could update variables and instnatly apply them. 
  * For this demo I picked specific variables just to demonstrate how to use the terraform ansible module. However this could easily be converted to a specific environment with other variables. 
* Only terraform plan and not execute it 
  * In roles/terraform/tasks/create_plan.yml using user input or default settings I will create a plan for a specific project. This uses the "planned" state as it wont apply unti leither manually applied or a user runs the apply job. 
* Just execute a plan that has been created earlier
  * The tasks roles/terraform/tasks/apply_plan.yml will apply a previous created plan. Either using the default plan name or user inputted plan name on jenkins. 
* Create the plan and execute it
  * To create and apply plan I simply call the both the create and apply plan tasks together. This will instantly create a plan then apply it. 
* Lastly I wanted the option to deploy new projects which currently have no state. 
  * There are twi different ways to do this. If the project has a backend/config file simply call the backend_config role.
  * The other method is to call roles/terraform/tasks/deploy_project. This will deploy a project which either uses local state or already has a pre-defined backend. 
