Vultr module for Caddy
===========================

This package contains a DNS provider module for [Caddy](https://github.com/caddyserver/caddy). It can be used to manage DNS records with Vultr accounts.

## Caddy module name

```
dns.providers.vultr
```

## Config examples

To use this module for the ACME DNS challenge, [configure the ACME issuer in your Caddy JSON](https://caddyserver.com/docs/json/apps/tls/automation/policies/issuer/acme/) like so:

```
{
	"module": "acme",
	"challenges": {
		"dns": {
			"provider": {
				"name": "vultr",
				"api_token": "YOUR_VULTR_API_TOKEN"
			}
		}
	}
}
```

or with the Caddyfile:

```
tls {
	dns vultr {env.VULTR_API_TOKEN}
}
```

You can replace `{env.VULTR_API_TOKEN}` with the actual auth token if you prefer to put it directly in your config instead of an environment variable.


## Authenticating

See [the associated README in the libdns package](https://github.com/libdns/vultr) for important information about credentials.

## Troubleshooting `timed out waiting for record to fully propagate`

If the appropriate `_acme-challenge` TXT record has been created in the Vultr dashboard but the propagation check keeps timing out, using Vultr's nameservers with Caddy might help.

Add a custom `resolver` to the [`tls` directive](https://caddyserver.com/docs/caddyfile/directives/tls):

```
tls {
  dns vultr {env.VULTR_API_TOKEN}
  resolvers ns1.vultr.com
}
```
