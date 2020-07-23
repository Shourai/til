# Sweep pings in a certain subnet
```
for i in {1..254} ;do (ping -c 1 192.168.1.$i | grep "bytes from" &) ;done
```

What this does is a for loop from 1 to 254, $i takes the value of the current
iteration so in the first one it will be 1 then 2, 3… and so on, then we tell it
to call the ping command with the -c option which means only ping once otherwise
it would ping forever after that we pipe the output to grep so we only see the
hosts that actually responded and the & at the end send it to the background so
it will launch all the pings in parallel. If we only want the ip address and not
the whole line we can further filter this using cut.

see also:
https://www.rubyguides.com/2012/02/cli-ninja-ping-sweep/
