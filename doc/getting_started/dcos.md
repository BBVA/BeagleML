# Getting started: DCOS (DatIO-Google)

***1 - Set up the infrastructure:***

In ```deploy/dcos``` folder, there is a JSON file for each beagleml component.
Just enter DCOS UI, and deploy one after another in the following order:
```
kafka > mongo > beagleml-monitor > scheduler > beagleml-front
```

Wait until each healtcheck gets green before deploying the following component (excepts monitor, that doesn't have healtcheck).

***---Common UI steps---***

***2 - Upload the model template and the experiment definition.***

To access the UI, type ``` http://<dcos_ip>/services/beagleml-front``` .

Once in the beagleml-front UI, just click ```Add new project``` and click:
-  ```Add Model Template``` to select the ```json``` experiment definition file of your choice.
You must have it in your computer (try ```models/dcos_model_template_neural-networks.json```).

-  ```Add Experiment Definition``` to select the ```yml``` experiment definition file of your choice.
You must have it in your computer (try ```models/dcos_experiment_definition_neural-networks.yml```).

With these files, you are configuring the experiment to be executed in the beagleml system.

Once done, in the home page it should appear the name of the project and the number of experiments to run.

***3 - Launch the experiments.***

Execute "Play" ('play/pause' icon) from the UI.

You should see how "Running" experiments number increases and "Queue" decreases.

***4 - Check results.***

Check experiments results by clicking into the project's Actions 'info ('i')'' button in the UI.

***5 - Stop experiments.***

To finish the demo and get rid of queued experiments, execute the following steps from UI:
-  To stop running experiments, execute "Stop" ('play/pause' icon) from the UI.
-  To delete all projects, execute "Reset ('garbage collector' icon)" from the UI.
