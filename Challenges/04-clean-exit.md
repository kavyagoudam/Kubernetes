In kubernetes, you can add a delay to pod termination to allow clean exit.

This is quite handy to release database connections to the app. how is this done? by using pre-stop hook (one good way!)

A pre-stop hook is a command that runs before the container is terminated. Here's how to use it in pod spec

spec:
 containers:
 - name: my-container
 image: my-image
 lifecycle:
 preStop:
 exec:
 command: ["sleep", "15"] 