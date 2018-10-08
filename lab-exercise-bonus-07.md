# 7. Run your Docker image

After your image builds successfully, you can run the container with [`docker run`](https://docs.docker.com/engine/reference/commandline/run/).

You can use `docker run --help` to see a list of options, but it's a long and unfiltered list. For now, you just need to know the basics.

## Running inside a box

The first parameter to `docker run` is the name of your image. This is the name shown in the `REPOSITORY` column in the output of `docker image ls`, with the value from the `TAG` column appended after a colon if the tag is anything other than `latest`.

For your purposes, you'll just need to use:

```powershell
docker run mywebapi
```

Run this command and you'll see output from your app running inside the container you built. It should also specify that it's listening on port 80. This is different from port 5000 and port 5001 that it ran on locally - long story short, the `microsoft/dotnet:2.1-aspnetcore-runtime` image is configured to run the server on port 80 by default.

Because it's in a container, you can't actually reach it at port 80 just yet. Press <kbd>CTRL</kbd>+<kbd>C</kbd> to break out of the container and get back to the command line.

## Cleaning up

One thing before you move on. Run [`docker ps`](https://docs.docker.com/engine/reference/commandline/ps/) to see all the containers you have running on your computer. If you ran `docker run` and exited with <kbd>CTRL</kbd>+<kbd>C</kbd>, you should see at least one item on this list.

Find the container you started up and stop it with [`docker stop [CONTAINER ID]`](https://docs.docker.com/engine/reference/commandline/stop/). You'll see the container ID printed as confirmation that it stopped.

```powershell
> docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
56c78de88c88        mywebapi            "/app/WebAPI --publi…"   4 minutes ago       Up 4 minutes        80/tcp              hardcore_volhard

> docker stop 56c78de88c88
56c78de88c88
```

You can also specify a container by its name (from the `NAMES` column, not the image name) or by a partial container ID.

```powershell
> docker stop hardcore_volhard
hardcore_volhard

> docker stop 56c
56c
```

Then, use [`docker rm`](https://docs.docker.com/engine/reference/commandline/rm/) to get rid of the stopped container completely. If you need to see the name or ID of a stopped container, run `docker ps --all` first.

```powershell
> docker ps --all
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS                          PORTS               NAMES
56c78de88c88        mywebapi            "/app/WebAPI --publi…"   4 minutes ago        Exited (0) About a minute ago                       hardcore_volhard

> docker rm 56c78de88c88
56c78de88c88
```

Or take a shortcut and use [`docker rm --force`](https://docs.docker.com/engine/reference/commandline/rm/#force-remove-a-running-container) to stop and remove the running container all in one step.

```powershell
> docker rm --force 56c78de88c88
56c78de88c88
```

## Poking a hole

To get to port 80 inside the container, you'll need to use the [`--publish` option](https://docs.docker.com/engine/reference/commandline/run/#publish-or-expose-port--p---expose) (often shortened to `-p`) of `docker run`. This maps from a port on your local PC to a port inside the container, letting it communicate freely on that port. To specify the port mapping, provide a port number on your local PC followed by a port number inside the container, separated by a colon.

Let's map local port `5000` to container port `80`. Make sure you specify your `docker run` options **before** the image name, or else they'll get passed through as parameters to the app running inside the container!

```powershell
docker run --publish 5000:80 mywebapi
```

Now you can communicate with your API at <http://localhost:5000>. Note that this is different port (5000 instead of 5001) and protocol (HTTP instead of HTTPS) from what you were using before, so make sure you update your URL in Postman.

## Exercise 7

* Run your app with `docker run`
  * Map local port `5000` to container port `80`

## Verify

* Use Postman to interact with your API at <http://localhost:5000/>.