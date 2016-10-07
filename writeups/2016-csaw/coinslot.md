## 2016 CSAW challenges: Coinslot

I tried a few other challenges, and was getting frustrated, so I grabbed an easy one to build momentum.

We don't have a binary, just a host and a port:  misc.chal.csaw.io 8000

netcat gives us a nice text interface, where we're given an amount of currency, then we iterate through bills and coins ($10,000 down to $0.01):

```bash
root@kali:~# nc misc.chal.csaw.io 8000
$0.06
$10,000 bills: 0
$5,000 bills: 0
$1,000 bills: 0
$500 bills: 0
$100 bills: 0
$50 bills: 0
$20 bills: 0
$10 bills: 0
$5 bills: 0
$1 bills: 
root@kali: #
```
If you skip an input or enter a non-numeric value, the connection is immediately closed.

```bash
root@kali:~# nc misc.chal.csaw.io 8000
$0.02
$10,000 bills: 0
$5,000 bills: 0
$1,000 bills: 0
$500 bills: 0
$100 bills: 0
$50 bills: 0
$20 bills: 0
$10 bills: 0
$5 bills: 0
$1 bills: 0
half-dollars (50c): 0
quarters (25c): 0
dimes (10c): 0
nickels (5c): 0
pennies (1c): 2
correct!
$0.07
$10,000 bills: ^C
root@kali# 
```

If you enter the correct number of coins and bills to make the amount given, you see `Correct!` and a new problem is assigned.

If you screw up, you'll be told what was expected of you.

```bash
root@kali:~# nc misc.chal.csaw.io 8000
$0.02
$10,000 bills: 0
$5,000 bills: 0
$1,000 bills: 0
$500 bills: 0
$100 bills: 0
$50 bills: 0
$20 bills: 1
$10 bills: 0
$5 bills: 0
$1 bills: 0
half-dollars (50c): 0
quarters (25c): 0
dimes (10c): 0
nickels (5c): 0
pennies (1c): 2
Incorrect number of $20 bills: expected 0, found 1
root@kali:~# 
```

I wrote a script to extract the amount given, then iterate through the prompts and calculate the correct number of coins/bills. (edited)

```python
#!/usr/bin/python

import socket
from math import floor

values = [10000,5000,1000,500,100,50,20,10,5,1,.5,.25,.1,.05,.01]

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect(("misc.chal.csaw.io",8000))
s.settimeout(5)

counter = 0

while True:
  data = s.recv(1024)
  for line in data.split("\n"):
    if line.find(".") > 0:				# look for a line like:  "$13.37"
      counter += 1
      total = float(line[1:])				# skipping the $ character
      print "\r  TOTAL:", total, "(#%d)" % counter,
      for value in values:
        num = int(total/value)
        s.send(str(num)+"\n")
        total = round((total - value * num),2)
  if data.find("flag") > 0:
    print "\n" + data[data.find("flag"):] 
    break
```

Runs in about a minute, depending on the network connection to the server.  Here's a sample run, and the flag:

```bash
root@kali:~# time python coinslot2.py 
  TOTAL: 19144.83 (#400) 
flag{started-from-the-bottom-now-my-whole-team-fucking-here}



real    0m53.411s
user    0m0.028s
sys    0m0.108s
root@kali:~#
```

