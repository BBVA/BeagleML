# Add on: Docker Compose in remote machines.

***1 - Set up the infrastructure:***

...
Similar to [local steps](../dockercompose.md)
...

***1.5 - Create SSH tunnels to remote machine:***

Redirect front to your local machine:
```
ssh -L 8080:localhost:8080  <REMOTE_IP>
```
Redirect scheduler to your local machine:
```
ssh -L 5000:localhost:5000  <REMOTE_IP>
```
Redirect tensorboard UI to your local machine:
```
ssh -L 6006:localhost:6006  <REMOTE_IP>
```

***2 - Upload the model template and the experiment definition.***

...
Similar to [local steps](../dockercompose.md)
...
