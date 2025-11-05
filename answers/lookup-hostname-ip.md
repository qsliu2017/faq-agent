# Query the IP address of a hostname

Resolve a hostname to its current IP addresses using DNS-aware utilities that ship with most Unix systems.

## Use dig for precise output

`dig` prints DNS answers in a script-friendly format and is available via the `bind` tools package on most platforms.

```bash
# Return all address records for example.com using the default resolver
dig +short example.com

# Ask for the IPv4 address only
dig +short example.com A

# Query Google's public resolver if the default server looks stale
dig +short example.com @8.8.8.8
```

The `+short` flag trims the verbose headers and leaves just the answer section, which is ideal when you only need the IPs.
Add `-t AAAA` or `-t MX` to fetch other record types when troubleshooting DNS.

## Fall back to nslookup or host

Older systems keep `nslookup`, which also relies on your resolver configuration.

```bash
nslookup example.com
nslookup -query=AAAA example.com
```

The pared-down `host` command works similarly and emits one line per answer.

```bash
host example.com
host -t AAAA example.com
```

## Check the system view with getent

If you need the address that your libc resolver will return (respecting `/etc/hosts` and nsswitch rules), use `getent`.

```bash
getent hosts example.com
```

This command prints all matching entries from `/etc/hosts`, cached name service lookups, and DNS, mirroring what most applications see.
