This document aims to answer the following questions:
1. What is this?
2. Why are we installing it?
3. What alternatives exist?
4. How do we verify it installed correctly?

1. What is Docker:
    - To understand Docker you must understand what a Container is:
        - A container is an isolated environment that packages an application together with everything it needs to run.
        - It provides an isolated environment that behaves like its own machine from the application's perspective, while actually sharing the host operating system.
        - Lightweight, isolated software packages that hold the application and it's dependencies. They share the host machine's OS kernel rather than having their own OS.
    - Docker allows applications to be packaged into isolated, reproducible environments that are easy to deploy, update and scale.
        - Dockers build, download, runs, networks, stores images, and manages volumes
    - Ex with no Docker: 
        - Ubuntu
            - MySQL
            - PostgreSQL
            - Python
            - etc..

        PostgreSQL gets updated and breaks because it's expecting the older version.

    - Ex. with Docker:
       Your Fantasy architecture example:
            Azure VM
            │
            ├── Docker
            │
            ├── PostgreSQL Container
            │      Stores:
            │      Accounts
            │      Characters
            │      Inventory
            │
            ├── Login API Container
            │      Handles:
            │      Login
            │      Registration
            │      Authentication
            │
            ├── Redis Container (later)
            │      Stores:
            │      Sessions
            │      Cache
            │
            └── Unreal Dedicated Server
        Everything is isolated and runs with their own dependencies.
    Docker Container vs Image:
        - Image: A blueprint or template for an application
            - Contains the application, its dependencies, and instructions on how to run it
            - Stored locally or in a registry like Docker Hub
            - One Image can create many Containers
        - Container: A running (or stopped) instance created from a docker image.
            - Has its own isolated filesystem, processes and network.
            - Multiple contianers can be created from the same image.
            
2. Use Case

    Docker is commonly used to package and deploy services independently. For Your Fantasy Game Docker will be used to run services such as
    - PostgreSQL Database
    - Login API
    - Redis Cache
    - Monitoring Tools
    - Admin dashboard
    - Other backend microservices.

    Note: The UE dedicated server will initially run  directly on vm for simplicity, but later may be containerized if scaling requirements change.

    Benefits of isolation: Updating services do not affect eachother and can be manage more controllably. (Easier debugging, deployments, services can be restarted individually, etc)


3. Alternatives to Docker:
    - Alternatives include: Containerd, Podman

4. Installation and verification:

    Download Docker installation script:

    Command: curl -fsSL https://get.docker.com -o get-docker.sh
        - Curl : Downloads something from the internet
        - -f : fail silently if something goes wrong. Instead of downloading an HTML Error page, it exits
        - -s : Silent mode (no progress bar)
        - -S : If there is an error, show it
        - -L : If Docker redirects us to another URL, follow it
        - -o get-docker.sh : Saves it at "get-docker.sh"
        - Verification: head get-docker.sh
            -This checks the first 10 lines of the installer script to verify it's contents.
    This command downloads the docker installation script and saves it.

    Run installation script:

    Command: There are two ways to do it. You can run it as a shell or turn it into an executable and run it that way.
        - Run it as SH: sudo sh get-docker.sh
            - sudo = run as admin (Super User Do)
            - sh = execute this file using the shell
        - Make it executable first: chmod +x get-docker.sh 
            - chmod +x = gives the file execute permission
        - After giving the script executre permissions, it cane be ran directly via: sudo ./get-docker.sh
            - ./ = run the file in the current directory

        Entire Script:

        curl -fsSL https://get.docker.com -o get-docker.sh

        head get-docker.sh

        chmod +x get-docker.sh

        sudo ./get-docker.sh
    
    Verification:

        Check Version:
        Command: docker --version
            - Output: Docker's current version and tells us the Docker CLI is installed.

        Check if Docker Compose is installed:
        Command: docker compose version
            - Output: Docker Commposer version

        Check if Docker Service is Running:
            - Docker is two things
                - CLI
                - Daemon (The background service that creates containers)
                    - User > Docker Command > Docker Daemon > Docker Container
        Command: sudo systemctl status docker
            - Output: Returns status update of Docker Service. Press Q to exit afterwards.

        Check if Docker can create Containers:
        Command: sudo docker run hello-world
            - The following happens after running this command:
                1. Docker looks for hello-world image locally
                2. It doesn't find it
                3. It downloads it from the Docker Hub
                4. Creates Container
                5. The Container prints a success message
                6. The Container exits.
            - Output: Lists the images in Docker

        Check for changes:
        Command: docker images
            - Asks Docker what images it has downloaded
            - Output: Shows Repository, Tag and Image ID
        Command: docker ps -a
            - Lists containers
            - Output: Lists out containers and it's property data
