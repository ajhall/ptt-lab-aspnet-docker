# 8. Build inside a Docker container

The above process works fine if you know the details of the environment in which your code is being built - you can compile your code on your developer PC, copy it into a container, and go on your merry way. But what if your build is running on a build server (maybe VSTS or Bamboo) where you don't control which version of the .NET Core SDK is installed?

To solve this, you can take advantage of Docker's ability to conjure up a pre-prepared environment and compile your code inside the `docker build` process itself. Let's try out a feature called [multi-stage builds](https://docs.docker.com/develop/develop-images/multistage-build/#use-multi-stage-builds).

## Multi-stage Dockerfile

A multi-stage Dockerfile uses the `FROM` instruction multiple times, allowing you to choose the right environment for each step of the job. This lets you end up with a minimal image containing exactly the output you want.

The following is an example of a Dockerfile that builds a Go project in one environment and then copies the compiled output into another environment. The final stage in the Dockerfile is the only one that gets captured in the resulting image.

```Dockerfile
# Start from an official Golang image, naming it 'builder'
FROM golang:1.7.3 as builder

# Set the working directory
WORKDIR /go/src/fakegit.com/go-app/

# Copy all .go files to the working directory
COPY *.go .

# Build the code in the working directory and output a binary named 'compiled-go-app'
RUN go build -o compiled-go-app .



# Start a new stage, this time using the Alpine image
FROM alpine:latest

# Set the working directory
WORKDIR /go-app/

# Copy the 'compiled-go-app' file from the builder image to the working directory in the Alpine image
COPY --from=builder /go/src/fakegit.com/go-app/compiled-go-app .

# Start the compiled program
CMD ["./compiled-go-app"]
```

Notice the line that says `COPY --from=builder`? This allows you to move files between images during the build process, where `builder` is the name specified on the `FROM golang:1.7.3 as builder` line. You can name each stage anything you want.

Use this example to help you design a Dockerfile that builds your app in one environment and adds the compiled output to an output image.

## Exercise 8

First, delete the `./output` and `./bin` folders in your project. This will clear out any output left over from previous executions of `dotnet publish` and prove that this new build is happening entirely inside the Docker build process.

Create a multi-stage Dockerfile for your ASP&#46;NET Core app.

* Use `microsoft/dotnet:2.1-sdk` as a build environment
* Use `microsoft/dotnet:2.1-aspnetcore-runtime` as the environment for your output image
* Only copy across the output of the build, not the source code

## Tips

* Build and run the image the same way you did in sections 6 and 7.

## Verify

* Use Postman to interact with your API at <http://localhost:5000/>.
