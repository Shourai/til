# Send SMS with termux

```
termux-sms-send -h
Usage: termux-sms-send -n number[,number2,number3,...] [-s slot] [text]
Send a SMS message to the specified recipient number(s). The text to send is either supplied as arguments or read from stdin if no arguments are given.
  -n number(s)  recipient number(s) - separate multiple numbers by commas
  -s slot       sim slot to use - silently fails if slot number is invalid or if missing READ_PHONE_STATE permission
```

using cron to send a sms every first day of the month at noon:
```
00 12 1 * * termux-sms-send -n [number] -s 0 "$(date)"
```
