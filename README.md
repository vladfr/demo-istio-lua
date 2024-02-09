# Istio lua filter

[Please see the discussion for context.](https://github.com/istio/istio/discussions/49297)

Make sure you have Istio installed and working, and enable sidecar injection.

There are 3 namespaces in play:

- `default` for our sleep pod
- `flex` for httpbin
- `hello` holds a hello world service, and has the EnvoyFilter applied.

The lua filter checks for the header, and if it exists, it simply proxies the request to `httpbin`.

The expected behavior is that routes in the VirtualService are applied in the `httpbin` container.

Actual behavior:

```shell
# fails as expected
k exec sleep-9454cc476-ng9b7 -- curl -vvv httpbin.flex:8000/headers -H 'X-Flex-Env: bad'

# Should also fail but doesn't; httpbin responds
k exec sleep-9454cc476-ng9b7 -- curl -vvv helloflex.hello:5000/headers -H 'X-Flex-Env: bad'
```
