## ANSIBLE REFACTORING AND STATIC ASSIGNMENTS (IMPORTS AND ROLES)

We shall continue working with the ` ansible-config-mgt ` repository for thsi project

Now you need to refactor your Ansible code, create assignments, and learn how to use the imports functionality. Imports allow to effectively re-use previously created playbooks in a new playbook – it allows you to organize your tasks and reuse them when needed.

### Code Refactoring

`Refactoring` is a general term in computer programming. It means making changes to the source code without changing expected behaviour of the software. The main idea of refactoring is to enhance code readability, increase maintainability and extensibility, reduce complexity, add proper comments without affecting the logic.

In this project, we will move things around a little bit in the code, but the overal state of the infrastructure remains the same.

### Step 1 – Jenkins job enhancement

Before we begin, let us make some changes to our Jenkins job – now every new change in the codes creates a separate directory which is not very convenient when we want to run some commands from one place. Besides, it consumes space on Jenkins serves with each subsequent change. Let us enhance it by introducing a new Jenkins project/job – we will require `Copy Artifact` plugin.

1. Go to your `Jenkins-Ansible` server and create a new directory called `ansible-config-artifact` – we will store there all artifacts after each build using below code:

`sudo mkdir /home/ubuntu/ansible-config-artifact`

2. Change permissions to this directory, so Jenkins could save files there:

` sudo chmod -R 0777 /home/ubuntu/ansible-config-artifact`

3. Go to Jenkins web console -> Manage Jenkins -> Manage Plugins -> on `Available` tab search for `Copy Artifact` and install this plugin without restarting Jenkins

4. Create a new Freestyle project (you have done it in Project 9) and name it `save_artifacts`.

5. This project will be triggered by completion of your existing `ansible` project. Configure it accordingly as follows:


![config1](./Images/config1.png)

![config2](./Images/config2.png)

Note: You can configure number of builds to keep in order to save space on the server, for example, you might want to keep only last `2` or 5 build results. You can also make this change to your ansible job. For this project we shall select `2`

6. The main idea of `save_artifacts` project is to save artifacts into `/home/ubuntu/ansible-config-artifact` directory. To achieve this, create a `Build` step and choose `Copy artifacts` from other project, specify `ansible` as a source project and `/home/ubuntu/ansible-config-artifact` as a target directory.

![config3](./Images/config3.png)

7. Test your set up by making some change in README.MD file inside your `ansible-config-mgt` repository (right inside `master` branch).

If both Jenkins jobs have completed one after another – you shall see your files inside `/home/ubuntu/ansible-config-artifact` directory and it will be updated with every commit to your `master` branch.

Now your Jenkins pipeline is more neat and clean.

###  REFACTOR ANSIBLE CODE BY IMPORTING OTHER PLAYBOOKS INTO SITE.YML



![new-tasks](./Images/new-tasks.png)