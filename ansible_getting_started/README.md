Wow, first day at Unicorn.Rentals. I found the orientation pretty cringeworthy, but someone else in my group didn't laugh at the jokes and they were fired. Yikes! Still though, the chowder taps and ball pit are the best perks I've ever had at a startup.

The old team left something running and I think my first task will be to figure out what it is doing. I think it would be cool to use Ansible to get an inventory of what's running in the AWS account. The Ansible stuff here probably needs some work, but hopefully this will be a good starting point for my team or someone else that is in the same situation!

From a quick glance I don't feel great about what is running there. There is some guy here who likes to tinker with the network stuff and insists that I call him "The Plague." He seems to hate documentation. I've been trying to ask him about what he did but he keeps muttering something about DaVinci and playing on his VR headset.

### Ansible - what is it?

We found some Ansbile scripts that can help us automate the launch of the infrastructure in AWS.

[Ansible](http://docs.ansible.com/ansible/intro_installation.html) is an open-source automation engine that automates infrastructure creation, software provisioning, configuration management, and application deployment.

In our environment, we can use it to create compute resources like VPCs, EC2 instances, autoscaling groups, launch configurations, Elastic Load Balancers, S3 buckets, and more!

Here are the Ansible playbooks I have been working on. Hopefully these will be useful to set up instance and networks.
Here's what's in this zip file:

- facts.yml: provides you some information as to what is running in your AWS environment.

- vars.yml: use this to fill out the variables you will use in the Ansible playbooks

- site.yml: run the ansible-playbook script against this playbook. It identifies which 'tasks' you want to run

- tasks/
	server.yml: actions within your AWS account including creates EC2 and security groups
- templates
	userdata.txt.j2: describes the commands that the EC2 instance will run after it has been launched

### How to install

If you have a Linux or Mac:
- Pip install [Ansible](http://docs.ansible.com/ansible/intro_installation.html) either on an EC2 instance or locally ```pip install ansible```

If you have Windows or want to have a central location for Ansible:

- you can launch our Ansible-ready AMI in an exiting VPC. AMI ID: ```ami-515cf23e``` for eu-central-1, make sure to open port 22 in the Security Group for SSH access - [Launch Instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-515cf23e)
	_NOTE: associate the EC2 instance with the IAM Player Instance Profile_

- To be able to SSH into the instance, use [Putty](http://www.putty.org/) or [Bitvise Tunnelier](https://www.bitvise.com/download-area)

- Use the UserName of **Fedora**, if using commandline do: ```ssh -i <SSH KEY LOCATION> fedora@<SERVER ADDRESS>```

### How to use

- Download the zip file containing the Ansible artifacts to the machine that has Ansible installed

- If using Ansible locally:

	- Go to the Player dashboard and click the link **Request Credentials/Console URL**

	- Copy the displayed temporary API crednetials to your clipboard

	- Paste the credentials into your terminal **Note: these credentials expire in 1 hour**

- Now that Ansible has been installed, you can run any YAML file playbook to interact with your AWS environment. Try it!
	Run this to view what is running in your account: ```ansible-playbook -vv facts.yml```
        
        **This is a Read-Only acction, does not launch resources in your account**

	Make note of the information it returns to you. This can show you what EC2 instances are running in your account.

#### To run the site.yml Ansible playbook

- Fill in the correct values for the variables in the vars.yml file

- Grab the credentials from the GameDay [dashboard](https://dashboard.cash4code.net/?tid=<YOUR_TEAMS_API_TOKEN)

- Run the command to create the EC2 assets ```ansible-playbook site.yml```

- I think the other thing - the networks.yml - it probably should be in the sites.yml somewhere as well. And then you probably have to change something in the server.yml to refer to the name of the new VPC you're creating.

For more information, visit the [Ansible documentation](http://docs.ansible.com/ansible/index.html) and [cloud module](http://docs.ansible.com/ansible/list_of_cloud_modules.html) pages

