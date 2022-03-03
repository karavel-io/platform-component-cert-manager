# Configuration

```hcl
component "cert-manager" {
  namespace = "cert-manager"

  # Params default values

  letsencrypt = {
    email = ""              # required, the email to associate with the Let's Encrypt account

    route53 = {
      enable = false        # enable Route53 DNS01 challenge
      region = "eu-west-1"  # AWS region of the Route53 zone
      domains = []          # optional, domains that match this list will use the DNS01 challenge
      zoneId = ""           # optional, the Route53 zone ID to match
      eksRole = ""          # When running on EKS, the IAM role cert-manager will use to invoke the Route53 API
      iamRole = ""          # When running on EC2, the IAM role cert-manager will use to invoke the Route53 API
    }

    cloudflare = {
      enable = false        # enable CloudFlare DNS01 challenge
      email = ""            # required, CloudFlare email associated with the account
      domains = []          # optional, domains that match this list will use the DNS01 challenge

      # External Secrets configuration for pulling the CloudFlare API token from the storage service
      secret = {
        # override this section only if you are not using the default store from the external-secrets component
        store = {
          name = "default"
          kind = "ClusterSecretStore"
        }
        key = ""            # required, should be the store-specific key to the secret, e.g. the Vault or AWS Secrets Manager key
        property = ""       # optional, should be the store-specific property inside the secret containing the token if the secret is structured (e.g. a JSON document)
      }
    }
  }
}
```

## Route53

See [cert-manager docs](https://cert-manager.io/docs/configuration/acme/dns01/route53/) for more information on the permissions required to perform the DNS01 challenge.

## CloudFlare

See [cert-manager docs](https://cert-manager.io/docs/configuration/acme/dns01/cloudflare/) for more information on the permissions required to perform the DNS01 challenge.