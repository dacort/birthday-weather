# Lambda Support

Now that [Lambda supports container images](https://aws.amazon.com/blogs/aws/new-for-aws-lambda-container-image-support/), I was curious what it would take to get this up and runnig there.

## Usage

- In this directory, build the Docker image:

```shell
docker build -t weather .
```

- Start the container

```shell
docker run -p 9000:8080 weather_bot:latest
```

- Test the endpoint!

```shell
curl -XPOST "http://localhost:9000/2015-03-31/functions/function/invocations" -d '{"year": 1987, "month": 12, "day": 2}'
```

You'll see a big long response with a base64-encoded image. It'll take about a minute to run locally.