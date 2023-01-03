Usage
===========

To use MMRTracker you need to setup three things: GitHub SSH access, Docker and VSCode.

.. _github-setup:

GitHub SSH setup
--------------------
*You can choose to do this in several different ways, using whichever Windows SSH client you
prefer. This guide will use the OpenSSH client built into Windows 10/11*

#. Open an admin Powershell terminal as administrator

   * Right-click the Start/Windows button and select ``Windows PowerShell (Admin)``

#. Execute
  
    .. code-block:: console

      $ Set-Service ssh-agent -StartupType Automatic
      $ Start-Service ssh-agent
      $ Get-Service ssh-agent

#. Generate an ``ssh`` key (no need to change default prompts)

    .. code-block:: console

      $ ssh-keygen

*You can see your newly created key by executing ``cat ~/.ssh/id_rsa.pub`` in a local terminal*

#. Open/Create ``C:\Users\<User>\.ssh\config`` in any text editor. Add the following contents
 
    .. code-block:: text
   
      Host *
      AddKeysToAgent yes
      IdentityFile ~/.ssh/id_rsa

#. Add the GitHub server public SSH host keys to your computer:
  
    .. code-block:: console
 
      $ ssh-keyscan github.com >> ~/.ssh/known_hosts

**Configure GitHub keys and tokens**

#. Login into GitHub, then go to the `SSH settings <https://github.com/settings/ssh/new>`_ page and add your key
#. Create a GitHub personal access token `here <https://github.com/settings/tokens/new>`_ with ``repo``, ``workflow``, ``write:packages`` and ``delete:packages`` permissions

**Setup environment variables**

#. Open the windows Environment Variables editor
#. Add the following to the ``User`` section, by clicking the ``[New...]`` button:
  
    .. code-block:: text
    
      GITHUB_ACTOR="your-github-username"
      GITHUB_TOKEN="your-github-token"


**Login into GitHub Container Registry (GHCR)**

#. Make sure your environment variables are usable. If ``echo %GITHUB_ACTOR%`` doesn't output your environment variable value, restart your computer.
#. Login by running:

    .. code-block:: console
    
      echo %GITHUB_TOKEN% | docker login ghcr.io -u %GITHUB_ACTOR% --password-stdin


.. _docker-setup:

Docker setup
------------

#. Install `Docker Desktop <https://www.docker.com/products/docker-desktop/>`_
   
   * Use WSL 2 (recommended) when prompted

#. Start Docker
   
   * Might need to install some WSL 2 prerequisites to get it working. See `this guide <https://learn.microsoft.com/ro-ro/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package>`_.
  
#. Check docker is working, by opening a new terminal window and running

    .. code-block:: console
    
      docker --version
      # Docker version 18.09.2, build 6247962

#. Download the container

    .. code-block:: console
    
      docker pull ghcr.io/luke-95/mmrtracker:latest

#. Create a shared docker volume

    .. code-block:: console
    
      docker volume create mmrtracker-state


.. _vscode-setup:

VSCode setup
----------------

#. Install the `Dev Containers Extension <https://code.visualstudio.com/docs/devcontainers/tutorial#_install-the-extension>`_.

    .. note:: Add some default container extensions for VSCode, by adding something like this to settings.json (feel free to use your favorite extensions):
        
        .. code-block:: text

            "dev.containers.defaultExtensions": [
                "eamodio.gitlens",
                "GitHub.vscode-pull-request-github",
                "VisualStudioExptTeam.vscodeintellicode",
            ]

#. Run the project in a container, using VSCode:

   #. Open Docker
   #. Open VSCode
   #. In VSCode:

      #. Open the command pallette by pressing ``[CTRL] + [SHIFT] + [P]``
      #. Select ``Dev Containers: Clone Repository in Container Volume``
      #. Select Git, then select the MMRTracker_ repository from the dropdown

This should prompt VSCode to load the latest MMRTracker_ container image from `ghcr.io <ghcr.io>`_, build it locally, run it in docker, clone the MMRTracker repository inside it, and then connect VSCode to it.

Congratulations! You can now work on the project

.. _MMRTracker: https://github.com/ErkanFRT/MMRTracker