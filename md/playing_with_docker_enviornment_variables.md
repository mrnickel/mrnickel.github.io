---
date: 2016-03-02T09:52:18-08:00
draft: false
title: Playing With Docker Enviornment Variables
---

Managing various enviornments can always be a challenging task. Things have changed substantially over the years. With the introduction of AWS and other PaaS providers introducing their own solutions and the vast differences that can occur between developers own machines, staging, QA and production, we as developers and solution architects have to be ever vigilant in our quest to keep things simple, and stable.

I recently read an article by Chris Stump titled [Docker for an Existing Rails Application](http://chrisstump.online/2016/02/20/docker-existing-rails-application/) which is what got me thinking about this topic.

Now I’m not a Ruby developer, but I am a docker enthusiast and I’m always interested in how people are using it for their
various environments, which is why I found Chris' post so intriguing.

I found one point in Chris' post that reminded me of Part III of writing a twelve-factor app.
> Docker strips out environment variables from its build process in order to keep builds consistent across all the different platforms it supports.

I'm currently exploring the various implementation details for utilizing docker with twelve-factor apps.

Part III is concerned with storing configurations in the environment.

> Apps sometimes store config as constants in the code. This is a violation of twelve-factor, which requires strict
> separation of config from code. Config varies substantially across deploys, code does not.

It also has the added benefit of ensuring your credentials aren't thrown into your repos.

This post is more about wrapping my head around managing environments in docker, and hopefully help others with their
options.

The code for this experiment is posted on [GitHub](https://github.com/mrnickel/Experiments-DockerEnvs)

Here, I am going to explore five different ways of managing environment vars.

- Managed in the Dockerfile
- Override Environment Vars When Starting The Container
- Override Environment Vars With a File
- Environment Vars with Docker Compose
- Environment File with Docker Compose

## Managed in the Dockerfile

As I mentioned above, Chris' post lead me to believe that hosting env vars in the dockerfile isn't a possibility, but I
figured I'd put this to the test. I added the following line to my dockerfile:

```
ENV TEST_ENV_ITEM="this is my stuff"
```

I then built and ran the container
```
docker build -t experiments-dockerenvs .
docker run experiments-dockerenvs app
```

The output for this will be:
```
this is my stuff
```

It seems as though Docker has changed their stance on this.

The cons to this are obviously the possibility of your secret credentials leaking into your repos. A big no-no. It does however have the benefit of making things easier for new developers to get up and running.

I'd recommend **not** having any env vars in your Dockerfile. Don't give your developers the opportunity to make this very simple mistake. We're all lazy and will, in a moment of weakness, opt for the "easy answer".

## Override Environment Vars When Starting The Container

This is a nice way to play with different configurations quickly. Docker allows you to specify env vars when running the container.

Run the docker container overriding the default env value:
```
docker run -e 'TEST_ENV_ITEM=Override from run cmd' experiments-dockerenvs app
```

The output for this will be:
```
Override from run cmd
```

The problem with this solution is that your run command can get unwieldly too long if you have more than one or two. The solution to this would be to have an override file that we can add to the run command.

## Override Environment Vars With a File

The docker run command has a `--env-file` flag that we can specify.

```
docker run --env-file=main.env experiments-dockerenvs app
```

Result output:
```
"Override from file"
```

Note that by reviewing the `main.env` file, it's not formatted the same way a bash file would be.

Instead you have a simpler:
```
TEST_ENV_ITEM="Override from file"
```

As opposed to:
```
export TEST_ENV_ITEM="Override from file"
```

Now this seems like a maintainable way to manage various environments cleanly and easily. This is a solution I will use going forward for my own docker containers.

## Environment Vars with Docker Compose

Docker compose is a fantastic new utility offered by the docker team. It allows you to define your stack in a single file, thus making things more defined.

You can specify environment variables in the `.yml` file in the same way you can in the dockerfile.

```
  environment:
    - TEST_ENV_ITEM="overridden from compose"
```

Running the following:
```
docker-compose -f docker-compose.yml build
docker-compose -f docker-compose.yml up
```

Will return the output:
```
envtest_1 | + exec app
envtest_1 | "overridden from compose"
experimentsdockerenvs_envtest_1 exited with code 0
```

Great! But we're back in the same position we were earlier. We have the potential again for us to leak secret credentials in the repos. As I stated earlier, I don't recommend this as a solution.

## Environment File with Docker Compose

The last piece I want to look at, is the possibility of forcing environmental file overrides in the `.yml` file. I'm not surprised that the great people at Docker have a resolution for this as well. The property we're interested in is the `env_file` property.

More specifically:
```
  env_file:
    - ./compose-main.env
```

Running the following:
```
docker-compose -f docker-compose-env.yml build
docker-compose -f docker-compose-env.yml up
```

Results in the following output:
```
envtest_1 | + exec app
envtest_1 | "Override from compose-main file"
experimentsdockerenvs_envtest_1 exited with code 0
```

Fantastic!

## Conclusion

There are a lot of options for us using docker when it comes to managing enviornments. The most convenient option is to specify your docker environment vars directly into the Dockerfile, or docker-compose.yml file.

Note: Using this allows the possibility of secure credentials leaking into your repos.

Personally I'm going to refrain from using that option. I'd be more inclined to use the .env solutions outlined above (while remembering to add the .env files to your .gitignore file!)

Feel free to reach me on twitter [@rnickel] (https://twitter.com/rnickel) for any input on the post!