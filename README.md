# Kubetail

Bash script that enables you to aggregate (tail/follow) logs from multiple pods into one stream.
This is the same as running "kubectl logs -f <pod>" but for multiple pods.

## Usage

First find the names of all your pods:

	$ kubectl get pods

This will return a list looking something like this:

```bash
NAME                   READY     STATUS    RESTARTS   AGE
app1-v1-aba8y          1/1       Running   0          1d
app1-v1-gc4st          1/1       Running   0          1d
app1-v1-m8acl  		   1/1       Running   0          6d
app1-v1-s20d0  		   1/1       Running   0          1d
app2-v31-9pbpn         1/1       Running   0          1d
app2-v31-q74wg         1/1       Running   0          1d
my-demo-v5-0fa8o       1/1       Running   0          3h
my-demo-v5-yhren       1/1       Running   0          2h
```

To tail the logs of the two "app2" pods in one go simply do:

	$ kubetail app2

If the pods are using multiple containers specify the container like this:

	$ kubetail app2 -c container1

Supply `-h` for help and addtional options:

	$ kubetail -h

## Colors

By using the `-k` argument you can specifiy how kubetail makes use of colors.

| Value   |     Description  |
|----------|---------------|
| pod | Only the pod name is colorized but the logged text is using the terminal default color |
| line | The entire line is colorized (default) |
| false | Don't colorize the output at all |
   
Example:

	$ kubetail app2 -k false
	
	
## Filtering / Highlighting etc

kubetail itself doesn't have filitering or highlighting capabilities built-in. If you're on MacOSX I recommend using [iTerm2](https://www.iterm2.com/) which allows for continuous highlighting of search terms, good scrolling capabilities and multitab arrangements. Another useful feature of iTerm2 is the "timeline" (`cmd` + `shift` + `e`) which lets you display a timeline in your own local timezone next to the logs (that are typically in UTC). 

If you're not using iTerm2 or think that kubetail is lacking in features there's a [fork](https://github.com/aks/kubetail) of kubetail made by [Alan Stebbens](https://github.com/aks) that allows for richer configuration and uses [multitail](https://www.vanheusden.com/multitail/) and [bash-lib](https://github.com/aks/bash-lib). Alan has been kind enough to provide a pull request but my current thinking is that I'd like kubetail to stay simple and small and not use any dependencies.

## Known issues

When you press "ctrl+c" to end the log session you may end up with errors like this:

```bash
error: write /dev/stdout: broken pipe
```

I'm not quite sure why this happens, pull requests are welcome :)

See also: http://code.haleby.se/2015/11/13/tail-logs-from-multiple-pods-simultaneously-in-kubernetes/
