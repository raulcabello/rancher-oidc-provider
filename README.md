# Rancher OIDC provider

![OIDC diagram](https://github.com/raulcabello/rancher-oidc-provider/blob/main/oidc.png?raw=true)

Rancher includes an OpenID Connect (OIDC) provider, offering the following endpoints:

- OIDC Configuration Endpoint

`$(HOST)/oidc/.well-known/openid-configuration`
This endpoint provides the OIDC configuration required for client setup.

- JSON Web Key Set (JWKS) Endpoint

`$(HOST)/oidc/.well-known/jwks.json`
Returns the public keys used to validate tokens by the kube-apiserver.

- Authorization Endpoint

`$(HOST)/oidc/authorize`
Acts as the entry point for the authorization code flow. Rancher detects the enabled provider (e.g., Keycloak, OpenLDAP, or SAML), redirects the user to authenticate with the chosen provider, and proxies the login process without storing credentials. Upon successful authentication, it returns an authorization code that can be exchanged for a token.

- Token Endpoint

`$(HOST)/oidc/token`
Returns a JSON Web Token (JWT) containing user and group information retrieved from the authentication provider. This token can be used for Kubernetes authentication, including local and downstream clusters, using tools like kubelogin.

### Configuring Kubernetes for OIDC Authentication

Kubernetes clusters must be configured to support OIDC authentication. This is achieved by enabling the appropriate flags in the kube-apiserver. For detailed instructions, refer to the Kubernetes documentation on OIDC authentication.

### Direct Cluster Communication:
Authentication tokens can be used directly to interact with downstream clusters without relying on the Rancher proxy. For details, see the Rancher Manager Architecture documentation.
This eliminates the need for impersonation resources in downstream clusters.

### Areas for Improvement:
- Limited Provider Support:
Currently, only Keycloak, OpenLDAP, and SAML are supported.

- Hardcoded OIDC Parameters:
OIDC parameters such as ClientID and redirectURL are hardcoded and need to be configurable.

- UI and Rancher Integration with JWT Tokens:
Adapt the Rancher UI and backend to natively support JWT tokens for seamless integration.

- Token Lifecycle Management:
Add functionality to refresh and revoke tokens as needed.

### Demo

<video src='https://github.com/raulcabello/rancher-oidc-provider/raw/refs/heads/main/video.mp4' width="300" />