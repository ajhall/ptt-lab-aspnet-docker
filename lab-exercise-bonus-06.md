# 6. Make a Docker image

To make an effective Docker image for your application, start by picking the environment you want to run your app in. You built your app with ASP&#46;NET Core 2.1, so you'll need an environment that can run that framework.

Microsoft provides [`dotnet` Docker images](https://hub.docker.com/r/microsoft/dotnet/) on Docker Hub that you can use as a base. These images give you a minimal Linux environment that's fully prepared to run .NET Core apps.

Many other maintainers provide similar images for their own applications, too. For example, you might use an [nginx](https://hub.docker.com/_/nginx/) image to start with a webserver that's ready to go, [MySQL](https://hub.docker.com/_/mysql/) for a database, or [golang](https://hub.docker.com/_/golang/) to build apps with the Go programming language.

Take a look at the common tags listed on the [`dotnet` repository](https://hub.docker.com/r/microsoft/dotnet/). One of them is `2.1-aspnetcore-runtime`, which is exactly what you'll need to run your ASP&#46;NET Core app. To specify a specific image by its tag, add a colon after the repository name (`microsoft/dotnet:2.1-aspnetcore-runtime`). If you don't specify a tag, Docker will automatically try to use the `latest` tag.

Now that you've picked `microsoft/dotnet:2.1-aspnetcore-runtime` as your base image, you can start preparing a Dockerfile.

## The Dockerfile

A Dockerfile is a set of instructions to prepare a Docker image. It starts from a base, follows a set of commands, and then takes a snapshot of the environment after the commands are run. The result of this process is the Docker image. When you run an image, it "wakes up" the snapshot, turns it into a container, and starts running.

To build a Docker image for your app, you'll want to:

1. Build your project with Linux as your target runtime.
2. Copy the output of your build into a Docker image.
3. Configure the image to run your app when it starts up.

The following is an example of a Dockerfile that builds a Python project. Use it as reference to help you build an image for your ASP&#46;NET Core project. The official [Dockerfile reference](https://docs.docker.com/engine/reference/builder) page is also helpful.

```Dockerfile
# Use an official Python runtime as a base image
FROM python:2.7-slim

# Copy the current directory contents into the container at /pythonapp
ADD . /pythonapp

# Define an environment variable
ENV PYTHON_VERSION 2.7

# Set the working directory to /pythonapp
WORKDIR /pythonapp

# Run app.py when the container launches
ENTRYPOINT ["python", "app.py"]
```

Add a new file at the root of your project named `Dockerfile`. Follow the above example, but use `microsoft/dotnet:2.1-aspnetcore-runtime` as a base and build a set of instructions to copy your app into the image and run it.

**Note:** When your app starts up, it searches the **current working directory** for `appsettings.json` files - **not** the directory the app lives in. Make sure you set the right working directory in your Dockerfile so your app will know where to look for its configuration files.

## Building your image

Run [`docker build .`](https://docs.docker.com/engine/reference/commandline/build/) from the folder containing your Dockerfile to build your Docker image. You can (and should) add the [`--tag` parameter](https://docs.docker.com/engine/reference/commandline/build/#tag-an-image--t) (often shortened to `-t`) to name your image.

```powershell
docker build . --tag mywebapi
```

When the build starts, you'll see it go through your Dockerfile line by line, displaying each instruction as it progressively builds the image. When it succeeds, you'll see `Successfully built xxxxxxxxx` and `Successfully tagged mywebapi` in the output.

## Finding your image

After you build an image, you can see it (and any other Docker images on your machine) by running [`docker image ls`](https://docs.docker.com/engine/reference/commandline/image_ls/). The newest images will show at the top of the list, so the one you just built should be at the very top with the tag you specified displayed in the `REPOSITORY` column.

You can also filter the list of images to your specific image by adding the repository name as an extra parameter to the `docker image ls` command.

```powershell
docker image ls mywebapi
```

## Exercise 6

* Use `dotnet publish` to generate an executable Linux version of your app
  * Set the build configuration to `Release`
  * Set the runtime to `linux-x64`
  * Set the output directory to `./output`
* Create a Dockerfile that:
  * Starts from `microsoft/dotnet:2.1-aspnetcore-runtime`
  * Copies the content of `./output` into the `/app` folder inside the image
  * Sets the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`
  * Sets the working directory to `/app` so the app will be able to find its configuration files
  * Starts running your app, which should be located at `/app/WebAPI` inside the image
* Use the Dockerfile to build an image

## Tips

* Use the [Dockerfile reference page](https://docs.docker.com/engine/reference/builder) to help make sense of the different Dockerfile instructions.

## Verify

* Run `docker image ls` and find the image you created.
* Make sure the image is tagged as `mywebapi`.