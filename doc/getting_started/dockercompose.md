# Getting started: Docker Compose

***PRE-REQUISITES:***

- Install [docker](https://docs.docker.com/engine/installation/) and [docker-compose](https://docs.docker.com/compose/install/) in your machine.

- BeagleML docker images are stored in AWS ECR docker registry, so it is mandatory to log in before downloading any image.

 In short, follow these steps to
[configure AWS CLI](http://docs.aws.amazon.com/es_es/cli/latest/userguide/cli-chap-getting-started.html)
, and then to
[log in to AWS ECR](http://docs.aws.amazon.com/cli/latest/reference/ecr/get-login.html).
It should be something like this:
```
...
$ aws configure
...
$ aws ecr get-login --region eu-west-1 --no-include-email | bash
...
```

***1 - Set up the infrastructure:***

So...as every container is launched locally, docker-compose takes care of everything.

Just type:
```
$ cd <path>/<to>/<beagleml>/<repo>
$ cd deploy
$ docker-compose up -d
```
5 services will be started in order: mongo, kafka, beagleml-monitor, beagleml-scheduer, and finally beagleml-front.
Check that these services are up and running by typing ```docker ps```.

Moreover, a network called ```deploy_beagleml``` will be created. All containers will be attached to it.

***---Common UI steps---***

***2 - Upload the model template and the experiment definition.***

To access the UI, type ``` http://localhost:8080``` .

Once in the beagleml-front UI, just click ```Add new project``` and click:
-  ```Add Model Template``` to select the ```yml``` experiment definition file of your choice.
You must have it in your computer (try ```models/dockercompose_model_template_neural-networks.yml```).

-  ```Add Experiment Definition``` to select the ```yml``` experiment definition file of your choice.
You must have it in your computer (try ```models/dockercompose_experiment_definition_neural-networks.yml```).

With these files, you are configuring the experiment to be executed in the beagleml system.

Once done, in the home page it should appear the name of the project and the number of experiments to run.

***3 - Launch the experiments.***

Execute "Play" ('play/pause' icon) from the UI.

You should see how "Running" experiments number increases and "Queue" decreases.

***4 - Check results.***

Check experiments results by clicking into the project's Actions 'info ('i')'' button in the UI.

***5 - Stop experiments.***

To finish the demo and get rid of queued experiments, execute the following steps from UI:
-  To stop running experiments, execute "Stop" from the UI.
-  To delete all projects, execute "Reset ('garbage collector' icon)" from the UI.

***6 - Tensorboard.***

To access the tensorboard UI, type ``` http://localhost:6006``` .
