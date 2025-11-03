# Upgrade all packages with pip

```
pip list --outdated | awk 'NR>2 {print $1}' | xargs -n1 pip install -U
```

The explanation is that pip list --outdated outputs a list of all the outdated packages in this format:

```
Package   Version Latest Type
--------- ------- ------ -----
fonttools 3.31.0  3.32.0 wheel
urllib3   1.24    1.24.1 wheel
requests  2.20.0  2.20.1 wheel
```

In the AWK command, `NR>2` skips the first two records (lines) and `{print $1}` selects the first word of each line.

<https://stackoverflow.com/questions/2720014/how-to-upgrade-all-python-packages-with-pip>
