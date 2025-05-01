# Vector Scalingo Buildpack

⚠️ This project is a fork of the original
[Vector Heroku Buildpack](https://github.com/vectordotdev/vector-heroku-buildpack)
documented for Scalingo.

---

This buildpack has been designed to help us quickly deploy
[Vector](https://vector.dev) as a [Scalingo](https://scalingo.com) application.

## Quick start guide

Once you've created our Scalingo application, in the Git repository of your
application, add this buildpack to your buildpacks by running the following
command (at the root of the repository):

```sh
echo "https://github.com/jmaupetit/vector-scalingo-buildpack" >> .buildpacks
```

You should now create a `vector.toml` configuration file for Vector describing
your data flow. A test configuration file that takes everything from the
standard input and prints it to the standard output may look like:

```toml
# vector.toml

# We need a directory with write permission
data_dir = "/tmp"

[sources.stdin]
type = "stdin"

[sinks.debug]
type = "console"
inputs = ["stdin"]
target = "stdout"
```

Now create a `start.sh` bash script that will be used to run vector:

```sh
#!/usr/bin/env bash

# Run vector
bin/refresh-vector . . . && \
  bin/vector --config vector.toml
```

And finally, add the corresponding `Procfile` to your project to run Vector:

```Procfile
web: bash ./start.sh
```

Now your repository tree should look like:

```
.
├── .buildpacks
├── Procfile
├── start.sh
└── vector.toml

1 directory, 4 files
```

Once committed and pushed to your Git forge, you will be able to
[deploy your application to Scalingo](https://doc.scalingo.com/platform/deployment/deploy-with-git).

---

✋ Real work now begins: depending on your needs, you should update Vector's
configuration with required sources, transformations and sinks.

---

## Example integration

An example integration can be found in the
[QualiCharge Logs](https://github.com/MTES-MCT/qualicharge-logs) repository:
Vector is used a ELK-like Log drain for application logs. Vector is a
[Logstash](https://www.elastic.co/logstash) replacement and
[TimescaleDB](https://www.timescale.com) is used as a Postgres sink instead of
[ElasticSearch](https://www.elastic.co/elasticsearch). We invite you to check
this out and give us feedback!

## License

Documentation of this work is released under the [Unlicense](./LICENSE) terms,
and the buildpack original code is distributed under the [Apache 2](./LICENSE)
license terms ; see the [NOTICE](./NOTICE).
