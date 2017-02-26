# arbitrary-dns64-generator

A PowerDNS pipe backend which synthesizes IPv4-embedded AAAA records based on the hostname being queried. There is a live instance of this code serving `dns64.name` (e.g. look up `192.0.2.1.dns64.name` to get `64:ff9b::c000:201`)

## Example usage

A query of the form:

    dig 192.0.2.1.dns64.example.com ANY

Will produce the answer:

    192.0.2.1.dns64.example.com. 60 IN   AAAA    64:ff9b::c000:201

This can be useful for hosts on v6-only networks where it is necessary to connect to a v4 literal for testing, or at the behest of a customer.

## Configuration

Configure in `pdns.conf` like so:

    launch=pipe
    pipe-command=/srv/dns/arbitrary-dns64-generator dns64.example.com ns1.example.com dns@example.com
    pipe-regex=^(.*\.dns64\.example\.com|dns64\.example\.com)$

Where `dns64.example.com` is the zone to serve, `ns1.example.com` and `dns@example.com` are used to construct the SOA.
