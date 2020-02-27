# Let's Talk - Docker Manifests & Travis

<p align="center">
  <img width="40%" height="40%" src="images/docker-travis-wide.png"><br>
  Written By : <a href="https://github.com/bendermIBM">@Max Bender</a><br>
  <a href="https://travis.ibm.com/Lets-Talk/docker-travis-manifest"><img src="https://travis.ibm.com/Lets-Talk/docker-travis-manifest.svg?token=JXc6pEaq6GzQHxPLG6xU&branch=master"></a>
</p>

This is an example to work off of for building [Docker Manifests](https://docs.docker.com/engine/reference/commandline/manifest/) based off a Docker image. By building a manifest it provides Docker the ability to support multi-arch containers. 

See the presentation charts at [docs/docker-travis-manifests.pptx](https://github.ibm.com/Lets-Talk/docker-travis-manifest/blob/master/docs/docker-travis-manifests.pptx)

## Build Details

### Travis Environment Variables

We have to set a few environment variables on Travis in order to run our builds. See below for some methods of adding these environment variables to your build.

| Variable        | Required | Description                             |
| --------------- | -------- | --------------------------------------- |
| DOCKER_IMAGE    | Y        | Name of the Docker Image                |
| DOCKER_REGISTRY | Y        | Docker Registry to push DOCKER_IMAGE to |
| DOCKER_USER     | Y        | User to log into DOCKER_REGISTRY        |
| DOCKER_PASS     | Y        | Password to login into DOCKER_REGSITRY  |

###### VIA .travis.yml

If you want to add these environment variables into your `.travis.yml` you can do so in the `env` section. For security however we are going to want to encrypt `DOCKER_USER` and `DOCKER_PASS` as these don't need to be publically available to other people. To encrypt these inside of `.travis.yml` we are going to need the [travis gem](https://github.com/travis-ci/travis.rb) which is a utility client to interact w/ Travis. Once you have the travis gem installed you can follow the following steps to encrypt those variables.

```
travis encrypt DOCKER_USER=<docker_user> --add env.global
travis encrypt DOCKER_PASS=<docker_pass> --add env.global
```

This will append these two encrypted values into the global environment section in the `.travis.yml`. After executing both of these commands you should see two secure entries, along with your `DOCKER_IMAGE` and `DOCKER_REGISTRY` variables.

```
env:
  global:
  - DOCKER_REGISTRY=sys-ltic-docker-local.artifactory.swg-devops.com
  - DOCKER_IMAGE=example-docker
  - secure: pB+r9KUtajS14VC4bB66LLcrymjhtsNbTsyVW3eStswNtV5FAcrBU2+0gmALXIf+cCeL0B9JhRBLSQlEs135wiaAoailwAAnniyLgdSRoi90593ImHGbnGnB6kJDy5fIEExI5kTV1FoJtnFWBThh1vYGoPlXqYPdr/SZrUR++BbHBcSlAI5+4Q51nfgDhVrxnKpGLXCLX3d4o6qS7baHJJVJVqPygCixQZ6zkkIkanrMNacZsZfnk5Y2ztEMwHVOGCEn5JU34TmU7Msmt6DER/epQrOUeIWb158GTQOuvpRkP9gid/5TbZ6BaXdYiKSrosEK1sGkyGO/HOK3QiO8u65+3RCi/pHcFmtjSuvHLkN7qqemTRTuIKr0XP+J1nEaKHewcQTBsBURaTkFdMKc5LguNx8ax1p9Ban/f7F+7x3Trde9NQ6izNSmjmYXpO7LICDjBCmp1+JGasS092fGmAqk6Tj0y5aCnmETtn4xBsLThPVerONQWvZtxNajGuJS84mfCMui3dCsCfJ/cn3lGHK+4d4k6tf4afxFOr5A4WGLNnKumUY0EQM/QnKzH6lSqtTFCbQak6QVLrslT6iaK1sFjJQazAC/Xz+s6HfftU2LVsoF9VrOxcGwC9j08fPM3nIkfR2lNYVvywkuB9FE7OnzJQYI5mkQdQ3xAxfmi9Y=
  - secure: yIQB4JJkJ/Dqk7Gm6H3XPP7eW3UhJuNTfDuHURu9drIZYadlKZDIgVLK4m0uVB/leD0KezF9cZyfF3FRAbDTqwDF32AyDDgNmHk1IFcxZOwX9BHsPDsPnoct9gF8LVtafsXJqgDuTZgRMJyLUnDLZV7urbyPY7QNZn9DpepdrKkpSElzD7gWeXeCpVSOghoN0+0xENZopmzp8wGkP+eY6+iDDvjJnQ4MflRK0uHZsEoxmpJodMaF2x7WYriaWtKR/ptkDOLZ1GUAQQxaybYqJ7BGldb5aY9nfzIAor5fTWzoWk78JhnLvhac74Ci5F2VnANqcDoJKICQiDfpFB5+1PAnEGiXGKcvqKpvuoMC90zbEaCMc58zA18IqvaZByLlZ6876Xl87VwqI8I/+tCp1kAUVqN3wd4v6RNTeaZ7+O5OXSINl0kMwE3CRF+Vy3VIfjz8eXYmrr2puAdUMcEC83Vloeav5Wq5dw1SJSpBmQ30FM4XB3SijRfGwmHyzVaT1u4F9bFKgsu5PrIUpLNuZj93qTL8xcAwi+i+oorpBBMBG9mbM1/RewescymOYOKFU6jLKm7Oe4QHdMs9OYRytjeVFalkT6HVfGZjtOPEjQYzWRBoZGvZp4oPZAG3LHhzmwgHeOWCXwjSn6M7el+oPV5MU1pU9WyaBI2FyUOLHNU=
```

###### VIA Web-UI

Setting it via the Web-UI will still encrypt the values that you want to keep secret so functionally it's the same. See the [description here](https://docs.travis-ci.com/user/web-ui/#environment-variables) for info about setting the environment-variables via the UI.