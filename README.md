# docker-pipes
Demonstrating the use of named pipes created through the `mkfifo` command for inter-container transfer of data through pipes.

## How It Works
To pipe data from one container to another, this example uses named pipes, a special kind of file that functions in the same way the pipe character `|` would in a `bash` command. As a minimal example:

`echo "test" | cat`

Uses `|` to send the standard output of the `echo "test"` command into the standard input of the `cat` command.

A similar thing can be accomplished using named pipes. To create a named pipe, run:
`mkfifo -m a=rw </path/to/named/pipe>`

The additional `-m a=rw` option sets the permissions of the pipe to be read/write for all users.

The instructions below will demonstrate using a named pipe created in this repo using `mkfifo -m a=rw ./pipes/pipe` and `bind` mounted onto two docker containers as `~/pipe` to transfer data between the containers. This is coordinated through the use of `docker-compose`.

## Building or Pulling from Dockerhub
(Paths in these commands relative to the root of this git repo)

To build yourself:  
`docker build --tag espinoza09/docker-pipes-source ./source`
`docker build --tag espinoza09/docker-pipes-dest ./dest`

If you are pulling from dockerhub, the images are pulled for you when running `docker-compose up -d` as described in the next section.

## Running
Run the containers in `detached` mode with:  
`docker-compose up -d`

You can verify the containers are running through:  
`docker ps -a | grep "docker-pipes"`

Attach to each container in a separate terminal with with a  
`docker attach docker-pipes-[source|dest]`

Now, you can output some data to the named pipe located at `~/pipe` in the `docker-pipes-source` container, and receive the data in the `docker-pipes-dest` container:
`[rocky@docker-pipes-source ~]$ echo "test" > pipe`  
`[rocky@docker-pipes-dest ~]$ cat < pipe`

Note that the order in which the commands are run doesn't matter, and you could just as well do:  
`[rocky@docker-pipes-dest ~]$ cat < pipe`
`[rocky@docker-pipes-source ~]$ echo "test" > pipe`

## Exiting
Exit the containers using the `exit` command, and/or run a `docker-compose down`.


## Additional Resources
[Install Docker](https://docs.docker.com/get-docker/)  
[Install Docker Compose](https://docs.docker.com/compose/install/)