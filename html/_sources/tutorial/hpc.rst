High Performance Computing
===========================

For some homework assignments, your laptop or desktop computer may not be powerful enough to run the code in a reasonable amount of time. In these cases, you may want to use a high performance computing (HPC) cluster. This tutorial will show you how to run your code on the HPC cluster `Expanse <https://www.sdsc.edu/support/user_guides/expanse.html>`_ at the San Diego Supercomputer Center (SDSC).

.. note::
    This tutorial assumes that your account has been created on Expanse. Here is the full `Expanse User Guide <https://www.sdsc.edu/support/user_guides/expanse.html>`_. We will only need a small subset of the information in this guide, but you may find it useful to refer to the full guide if you have any questions.


Install SSH-related extensions for VS Code
------------------------------------------
To log in to Expanse, you will need to use SSH. If you are using Visual Studio Code, you need to install the following extensions:

- Remote - SSH
- Remote - SSH: Editing Configuration Files
- Remote - Tunnel

You can install these extensions by clicking on the Extensions icon on the left side of the VS code window, searching for the extension name, and clicking the Install button.


Add Expanse to your SSH configuration
-------------------------------------
To log in to Expanse, you need to add the following lines to your SSH configuration file. This file is located at `~/.ssh/config` on your computer. You could check if the file exists by open your home directory using VS code. If the file does not exist or the folder `~/.ssh` does not exist, create it in your home directory using VS code. Then add the following lines to the file `~/.ssh/config`:

.. code-block:: bash

    Host expanse
        HostName login.expanse.sdsc.edu
        User YOUR_USER_NAME

Replace `YOUR_USER_NAME` with your username on Expanse. Save the file and close it.

.. note::
    Your user name on Expanse might be different from your user name on `ACESS`. You can find your user name on Expanse in your ACESS account. After you log in to ACESS, find the page showing your project information. Your user name on Expanse is listed there.


Logging in to Expanse
---------------------
After adding Expanse to your SSH configuration, you can log in to Expanse using either the terminal or VS Code.

In a terminal, you could log in to Expanse by running the following command:

.. code-block:: bash

    ssh expanse

It will ask you for your password. After entering your password, you should be logged in to Expanse.

If you are using VS Code, you can log in to Expanse by clicking on the Remote Explorer icon on the left side of the VS Code window. Then click on the SSH Targets icon at the top of the Remote Explorer window. You should see `expanse` in the list of SSH targets. Click on `expanse` to log in to Expanse.

.. note::

    The node you log in to in the above steps is a login node. You can write your code and move files around on the login node, but you should not run your code on the login node. Instead, you should run your code on a compute node. The following sections will show you how to login to a compute node and run your code.


Logging in to a compute node
----------------------------
Compute nodes are shared resources on the HPC cluster and are managed by the job scheduler Slurm. To use a compute node, you need to request it from Slurm. When you request a compute node, you can specify the number of CPUs, the amount of memory, and the amount of time you need. The following is an example of how to request a compute node in terminal after you have logged in to a login node:

.. code-block:: bash

    srun --account=tft109 --pty -p compute --nodes=1 --ntasks-per-node=1 --mem=1G --time=1:00:00 --wait=0 --export=ALL /bin/bash

The above command requests a compute node with 1 CPU, 1GB of memory, and 1 hour of time. You can change the values of `--nodes`, `--ntasks-per-node`, `--mem`, and `--time` to request more resources. The `--account` flag specifies the project `tft109` that you are using. 

If there are available resources and your request is accepted by Slurm, you will be logged in to a compute node. You can run your code on the compute node.

To request a compute node with a GPU, you can use the following command:

.. code-block:: bash

    srun --account=tft109 --pty -p gpu --gpus=1 --nodes=1 --ntasks-per-node=1 --mem=1G --time=1:00:00 --wait=0 --export=ALL /bin/bash


.. note::
    Please be mindful of the resources you request, as you are sharing the 
    resources granted to this course with other students in the class. Once your requested resources are granted by Slurm, they are considered used, even if they are not used fully. For example, if you request a compute node with 10 hours of time and only use it for 1 hour, the remaining 9 hours are considered used.

Now you are assigned a compute node and can run your code on it via the terminal. What if you want to run your code in VS Code? For example, you may want to use the interactive debugger or run a Jupyter notebook in VS Code on the compute node. To do this, you could use ssh tunneling. This requires a few more steps.


1. Copy the vscode command line tool to your directory. You could do this on either the login node or the compute node by running the following command. You only need to do this once.

.. code-block:: bash

    ## cope the vscode command to your directory
    mkdir -p ~/apps
    cp /home/xding1/apps/code ~/apps

.. note::
    You only need to copy the vscode command once. The next time you want to login to a compute node, you can skip this step.


2. On a compute node that you have logged in to using the `srun` command, run the following command to start the ssh tunnel:

.. code-block:: bash

    ~/apps/code tunnel

This command will ask you what account to use for the tunnel. Select using Github account. It will then provide you with a URL and a passcode. Open the URL in your browser and enter the passcode. It might ask you to log in to your Github account if you are not already logged in. After you enter the passcode, the tunnel should be established.

3. Open the Remote Explorer in VS Code. Click on the Tunnels icon at the top of the Remote Explorer window. You should see a tunnel with the name of the compute node you are logged in to. Click on the tunnel to open a new VS Code window that is connected to the compute node.

Now you can run your code in VS Code on the compute node. You can use the interactive debugger, run Jupyter notebooks, and use other features of VS Code.





