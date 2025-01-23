# Atlas-X Gateway MVD

This is a demonstration of a minimal data space built with [AtlasX-Gateway](https://github.com/chainstep/atlasx-gateway/).
In this example a provider and a consumer are started.

The provider provides a simple "Hello Service", that returns a text message.

## Policy configuration

The "Hello Service" is secured by a policy, so that only a customer with role ADMIN and legal registration number "HRB 0815" can access it.
The policy is set in line 15ff. in the [docker-compose.yml](docker-compose.yml).
The role and registration number in the policy match the Consumer configured in [Consumer GaiaXCredentialSD.json](consumer\vc-templates\GaiaxCredentialSD.json).
For more information consult the [Atlas-X Gateway Readme](https://github.com/chainstep/atlasx-gateway/blob/main/README.md) for more details.

## Reverse Proxy configuration

For this setup to work, you have to make sure that the Consumer- and the Provider AtlasX-Gateway configured in the [docker-compose.yml](docker-compose.yml)
are avaible from the internet over an HTTPS connection.
The provider container is configured to listen on port 8081 and the consumer is listening on port 8082.
You can create a publicly available Reverse Proxy that forwards all incoming traffic for the provider to the provider container port 8081
and the incoming traffic for the consumer to port 8082.

Before startup, you have to set the domain name of the provider and the consumer in the [.env](.env) file.

## Startup

The setup can be started with

```sh
docker compose up -d
```

If everything is started, you can access the "Hello Service" from the consumer side.
In the following command, replace the <CONSUMER_DOMAIN> placeholder with the consumer domain that you configured in the .env file:

```sh
curl -X GET http://localhost:8082/internal-proxy --header "X-Service-Url: https://<CONSUMER_DOMAIN>/proxy/helloservice" --header "X-Api-Key: 1234567890"
```
