# dehydrated [![Donate](https://img.shields.io/badge/Donate-PayPal-green.svg)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=23P9DSJBTY7C8)

![](docs/logo.jpg)

Dehydrated is a client for signing certificates with an ACME-server (e.g. Let's Encrypt) implemented as a relatively simple (zsh-compatible) bash-script.
This client supports both ACME v1 and the new ACME v2 including support for wildcard certificates!

It uses the `openssl` utility for everything related to actually handling keys and certificates, so you need to have that installed.

Other dependencies are: cURL, sed, grep, awk, mktemp (all found pre-installed on almost any system, cURL being the only exception).

Current features:
- Signing of a list of domains (including wildcard domains!)
- Signing of a custom CSR (either standalone or completely automated using hooks!)
- Renewal if a certificate is about to expire or defined set of domains changed
- Certificate revocation

Please keep in mind that this software, the ACME-protocol and all supported CA servers out there are relatively young and there might be a few issues. Feel free to report any issues you find with this script or contribute by submitting a pull request,
but please check for duplicates first (feel free to comment on those to get things rolling).

## Getting started

For getting started I recommend taking a look at [docs/domains_txt.md](docs/domains_txt.md), [docs/wellknown.md](docs/wellknown.md) and the [Usage](#usage) section on this page (you'll probably only need the `-c` option).

Generally you want to set up your WELLKNOWN path first, and then fill in domains.txt.

**Please note that you should use the staging URL when experimenting with this script to not hit Let's Encrypt's rate limits.** See [docs/staging.md](docs/staging.md).

If you have any problems take a look at our [Troubleshooting](docs/troubleshooting.md) guide.

## Config

dehydrated is looking for a config file in a few different places, it will use the first one it can find in this order:

- `/etc/dehydrated/config`
- `/usr/local/etc/dehydrated/config`
- The current working directory of your shell
- The directory from which dehydrated was run

Have a look at [docs/examples/config](docs/examples/config) to get started, copy it to e.g. `/etc/dehydrated/config`
and edit it to fit your needs.

## Usage:

```text
Usage: dehydrated [-h] [command [arg]] [parameter [arg]] [parameter [arg]] ...

Default command: help

Commands:
  -v/--version                        Print version information
  --register                          Register account key
  --account                           Update account contact information
  -c/--cron                           Sign/renew
non-existent/changed/expiring
                                      certs
  -s/--signcsr path/to/csr.pem        Sign a given CSR, output CRT on stdout
                                      (advanced usage)
  -r/--revoke path/to/cert.pem        Revoke specified certificate
  -gc/--cleanup                       Move unused cert files to archive dir
  -h/--help                           Show this help text
  -e/--env                            Output config variables for use in
other
                                      scripts

Parameters:
  --accept-terms                      Accept CA's terms of service automatically
  -fc/--full-chain                    Print full chain when using -s/--signcsr
  -4/--ipv4                           Resolve names to IPv4 addresses only
  -6/--ipv6                           Resolve names to IPv6 addresses only
  -d/--domain domain.tld              Use specified domain name(s) instead of
                                      domains.txt entry (one cert!)
  --alias certalias                   Use specified name for cert dir (and
                                      per-cert config) instead of the primary
                                      domain (only used if -d/--domain is
                                      specified)
  -g/--keep-going                     Keep going after errors while
                                      creating/renewing multiple certs in cron
                                      mode
  -x/--force                          Force renewal of cert even if it is valid
                                      for more than RENEW_DAYS days
  -n/--no-lock                        Don't use a lockfile (dangerous!)
  --lock-suffix example-com           Suffix lockfile name with a string (useful
                                      for running with -d/--domain)
  --ocsp                              Set option in CSR indicating OCSP stapling
                                      is mandatory
  -p/--privkey path/to/key.pem        Use specified private key instead of
                                      account key (useful for revocation)
  -f/--config path/to/config          Use specified config file
  -k/--hook path/to/hook.sh           Use specified script for hooks
  -o/--out certs/dir                  Output certs into the specified dir
  -t/--challenge http-01|dns-01       The challenge to use
  -a/--algo rsa|prime256v1|secp384r1  The public key algorithm to use
  -V/--verbose                        Enable light debug tracing
  -D/--debug                          Enable heavy debug tracing
```

## Donate

(From the original README)

> I'm a student hacker with a few (unfortunately) quite expensive hobbies
> (self-hosting, virtualization clusters, routing, high-speed networking,
> embedded hardware, etc.).  I'm really having fun playing around with hard-
> and software and I'm steadily learning new things.  Without those hobbies
> I probably would never have started working on dehydrated to begin with :)

> I'd really appreciate if you could [donate a bit of > money](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=23P9DSJBTY7C8)
> so I can buy cool stuff (while still being able to afford food :D).  

> If you have hardware laying around that you think I'd enjoy playing with
> (e.g. decommissioned but still modern-ish servers, 10G networking
> hardware, enterprise grade routers or APs, interesting ARM/MIPS boards,
> etc.) and that you would be willing to ship to me please contact me at
> `donations@dehydrated.io` or on Twitter
> [@lukas2511](https://twitter.com/lukas2511).

> If you want your name to be added to the [donations
> list](https://dehydrated.io/donations.html) please add a note or send me
> an email `donations@dehydrated.io`. I respect your privacy and won't
> publish your name without permission.

> Other ways of donating:
>  - [My Amazon Wishlist](http://www.amazon.de/registry/wishlist/1TUCFJK35IO4Q)
>  - Monero: 4Kkf4tF4r9DakxLj37HDXLJgmpVfQoFhT7JLDvXwtUZZMTbsK9spsAPXivWPAFcDUj6jHhY8hJSHX8Cb8ndMhKeQHPSkBZZiK89Fx8NTHk
>  - Bitcoin: 12487bHxcrREffTGwUDnoxF1uYxCA7ztKK

Of course, if you want to give me (@therealbstern) money too, you're welcome
to!  :-P
