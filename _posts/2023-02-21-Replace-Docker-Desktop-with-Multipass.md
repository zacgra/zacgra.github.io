---
title: "Replace Docker Desktop with Multipass"
author: zacgra
layout: post
categories: Code
tags: [vm, multipass, docker]
---

Multipass is a multi-platform tool used to set up and run Ubuntu VMs on your local
machine, while using some of the cloud scripting functionality.

1. Install Multipass

   - install from https://multipass.run/install

2. Review the `docker` workflow
   We will be using the `docker` workflow, a cloud-init yaml file

   - docker workflow `https://github.com/canonical/multipass-blueprints/blob/main/v1/docker.yaml`

3. Launch a new instance using the `docker` workflow
   Here we are creating a new Ubuntu VM using the docker workflow. By passing only
   `docker`, we are specifying to use the workflow. We could also use a generic
   Ubuntu VM by omitting `docker`. Also, add a custom name with `-n my-custom-name`

   ```
   multipass launch docker -c 4 -m 12G -d 100G
   ```

   Note: we've assigned 4 cpu cores, 12gb of memory, and 100gb of disk space.
   Adjust accordingly.

4. View Logs
   If your launch fails, you may need to look over the logs to figure out what happened.

   - view instance logs at `/Library/Logs/Multipass/*.log`

5. Access `docker` instance
   The instance should be availabe now, so we can load it up and try things out.

   - view available VMs with `multipass ls`. Should see `docker`.
   - access new instance with `multipass shell docker`
   - run `docker ps`
   - notice that there is already a container running called `portainer`

6. Set up Portainer
   This is a browser-based docker admin tool, also handy for monitoring
   container resource usage and performance.

   - To log in, you'll need to know your instance's address, which you can find with
     `multipass info --all`
   - go to `<instance's ip address>:9000` in your browser
   - without a certificate, you may need to "continue anyway"
   - set your password and

7. Set aliases
   This step is the secret sauce, and it lets you type `docker` or `docker compose`
   on your local machine and run it in the VM.

   - `docker` workflow sets two aliases by default
   - remove both with `multipass unalias docker && multipass unalias docker-compose`
   - set `multipass alias docker:docker`

8. Mount projects in Multipass

   - If we have a project at ~/docker-project, then we'll need to mount that using
     `multipass mount ~/docker-project docker`
     where docker is the name of the multipass instance.
   - view that it worked with `multipass info --all`

9. Optional: Set up Docker in VS Code

   - after we've aliased docker, we should be able to see our containers and images in VSCode
   - we'll need to modify the `docker-compose` path either in the UI or in the user
     settings.json and update it to `"dev.containers.dockerComposePath": "docker compose"`
