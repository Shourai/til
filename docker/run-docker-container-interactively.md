# Run docker container interactively with bash

```
docker run -it --rm -v $(pwd):/tmp -w /tmp CONTAINER_NAME /bin/bash
```

```
-v : mount a certain directory
-w : set working dir inside the container
```

If you want to run a script when the container runs without running an interactive tty, you can use

```
docker run --rm -v ~/scripts:/tmp -w /tmp node /bin/bash -c "node index.js"
```

This command runs the `node` container and executes `index.js` using bash to execute `node index.js`
