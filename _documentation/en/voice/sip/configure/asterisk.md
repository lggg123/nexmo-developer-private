---
title: Asterisk
description: How to setup Asterisk with Vonage SIP
---

# Asterisk

Below we provide example configurations for using Vonage's SIP service with [Asterisk](http://www.asterisk.org).

> Visit the [Vonage Knowledge Base](https://help.nexmo.com/hc/en-us/articles/115004859247-Which-IP-addresses-should-I-whitelist-in-order-to-receive-voice-traffic-from-Nexmo) to obtain the current list of IP addresses.

## Inbound configuration

```text
[nexmo-sip]
fromdomain=sip.nexmo.com
type=peer
context=nexmo
insecure=port,invite
nat=no
;Add your codec list here.
; Note: Use "ulaw" for US only, "alaw" for the rest of the world.
allow=ulaw
allow=alaw
allow=G729
dtmfmode=rfc2833

[nexmo-sip-01](nexmo-sip)
host=5.10.112.121

[nexmo-sip-02](nexmo-sip)
host=5.10.112.122

[nexmo-sip-03](nexmo-sip)
host=119.81.44.6

[nexmo-sip-04](nexmo-sip)
host=119.81.44.7

[nexmo-sip-05](nexmo-sip)
host=169.60.141.29

[nexmo-sip-06](nexmo-sip)
host=169.60.141.30
```


## Outbound configuration

```text
[general]
    register => <key>:<secret>@sip.nexmo.com
[nexmo]
  username=<key>
  host=sip.nexmo.com
  defaultuser=<key>
  fromuser=<long_virtual_number>
  fromdomain=sip.nexmo.com
  secret=<secret>
  type=peer
  context=nexmo
  insecure=very
  qualify=yes
  nat=no
  ;Add your codec list here.
  ; Note: Use "ulaw" for US only, "alaw" for the rest of the world.
  allow=ulaw
  allow=alaw
  allow=G729
  dtmfmode=rfc2833
```

### Asterisk version 12+ with chan_pjsip

```text
; Basic UDP only endpoint.
[transport-udp]
  type=transport
  protocol=udp
  bind=0.0.0.0
[nexmo]
  type = endpoint
  aors = nexmo
  outbound_auth = nexmo-auth
  context = inbound
  transport=transport-udp
  from_user = <long_virtual_number> ; This is optional. CLI can be set in the dialplan
  allow=alaw
  allow=ulaw
  allow=g729
[nexmo]
  type = aor
  contact = sip:sip.nexmo.com
  qualify_frequency = 120
[nexmo-auth]
  type = auth
  auth_type = userpass
  username = <key>
  password = <secret>
[nexmo-reg]
  type = registration
  outbound_auth = nexmo-auth
  server_uri = sip:sip.nexmo.com
  client_uri = sip:<key>@sip.nexmo.com
[nexmo-identify]
  type = identify
  endpoint = nexmo
  match = 5.10.112.122
  match = 5.10.112.121
  match = 119.81.44.6
  match = 119.81.44.7
  match = 169.60.141.29
  match = 169.60.141.30
```
