# check-site-up

A simple shell script to check that a site is up and loading expected content. Requires only `curl` and `grep` as dependencies.

Useful for adding to a crontab (use the `-c` flag).

Also useful for monitoring less stable .onion addresses (wrap it through `torify`, and set a reasonable retry with `-r` in case of intermittent Tor instabilities).

# Options

```
  -u    URL of the site to check
  -k    Key word on the page to look for
  -r    Amount of retries before alerting (default: 3)
  -t    Time to wait in between retries (default: 5)
  -s    Silent mode (you can check the return code for success/failure)
  -c    Cron mode (only errors get output)
  -h    This help message.
```

# Examples

### Check from the command line that the site mig5.net is loading and shows the term 'Sysadmin'

```
~# ./check-site-up -u https://mig5.net -k Sysadmin
Attempt #1 to check https://mig5.net for Sysadmin
OK - found Sysadmin on https://mig5.net
```

### Try 10 times with a delay of 2 seconds

```
~# ./check-site-up -u https://mig5.net/blah -k Sysadmin -r 10 -t 2
Attempt #1 to check https://mig5.net/blah for Sysadmin
CRITICAL - couldn't find Sysadmin on https://mig5.net/blah
Attempt #2 to check https://mig5.net/blah for Sysadmin
CRITICAL - couldn't find Sysadmin on https://mig5.net/blah
Attempt #3 to check https://mig5.net/blah for Sysadmin
CRITICAL - couldn't find Sysadmin on https://mig5.net/blah
Attempt #4 to check https://mig5.net/blah for Sysadmin
CRITICAL - couldn't find Sysadmin on https://mig5.net/blah
Attempt #5 to check https://mig5.net/blah for Sysadmin
CRITICAL - couldn't find Sysadmin on https://mig5.net/blah
Attempt #6 to check https://mig5.net/blah for Sysadmin
CRITICAL - couldn't find Sysadmin on https://mig5.net/blah
Attempt #7 to check https://mig5.net/blah for Sysadmin
CRITICAL - couldn't find Sysadmin on https://mig5.net/blah
Attempt #8 to check https://mig5.net/blah for Sysadmin
CRITICAL - couldn't find Sysadmin on https://mig5.net/blah
Attempt #9 to check https://mig5.net/blah for Sysadmin
CRITICAL - couldn't find Sysadmin on https://mig5.net/blah
Attempt #10 to check https://mig5.net/blah for Sysadmin
CRITICAL - couldn't find Sysadmin on https://mig5.net/blah
```

### Check a Tor .onion site (hint: wrap it through `torify`)

```
~# /usr/bin/torify ./check-site-up -u http://yvhz3ofkv7gwf5hpzqvhonpr3gbax2cc7dee3xcnt7dmtlx2gu7vyvid.onion/ -k Sysadmin
Attempt #1 to check http://yvhz3ofkv7gwf5hpzqvhonpr3gbax2cc7dee3xcnt7dmtlx2gu7vyvid.onion/ for Sysadmin
OK - found Sysadmin on http://yvhz3ofkv7gwf5hpzqvhonpr3gbax2cc7dee3xcnt7dmtlx2gu7vyvid.onion/
```

### Silent mode (no output, but you can review the return code)

```
~# ./check-site-up -u https://mig5.net -k Sysadmin -s
~# echo $?
0
```

### Example crontab (Similar to silent mode, but the `-c` flag prints errors, which can be mailed to you with MAILTO in your cron)

```
MAILTO="you@example.com"
*/15 * * * www-data /usr/bin/torify /usr/local/bin/check-site-up -u http://yvhz3ofkv7gwf5hpzqvhonpr3gbax2cc7dee3xcnt7dmtlx2gu7vyvid.onion/ -k Sysadmin -c
```
