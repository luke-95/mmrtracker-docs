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

1. Login into GitHub, then go to the `SSH settings`_ page and add your key
2. Create a GitHub personal access token here_ with ``repo``, ``workflow``, ``write:packages`` and ``delete:packages`` permissions

.. _SSH settings: https://github.com/settings/ssh/new\
.. _here: https://github.com/settings/tokens/new

**Setup environment variables**

1. Open the windows Environment Variables editor
2. Add the following to the ``User`` section, by clicking the ``[New...]`` button:
  
  .. code-block:: text
   
    GITHUB_ACTOR="your-github-username"
    GITHUB_TOKEN="your-github-token"


**Login into GitHub Container Registry (GHCR)**

1. Make sure your environment variables are usable. If ``echo %GITHUB_ACTOR%`` doesn't output your environment variable value, restart your computer.
2. Login by running:

  .. code-block:: console
   
    echo %GITHUB_TOKEN% | docker login ghcr.io -u %GITHUB_ACTOR% --password-stdin


.. _docker-setup:

Docker setup
----------------

.. _vscode-setup:

VSCode setup
----------------