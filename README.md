# synkronized-charts

Charts for Synkronized! Synkronized is a custom built deployment pipeline for auto applying helm charts based on simple YAML files in repo's root directories. These charts here are not intended to be used alone, but rather act as a middleman between ArgoCD and the Synkronized service. These charts leverage vault and the vault operator to pull in static secrets to the deployment, and it's expected to deploy these charts via ArgoCD. 

## Prelude

These charts require your cluster be in a pretty specific state. I do urge those interested to mirror these charts and edit them as need-be for base values to work out, but if you're uninterested in that tedium, there's a [Terraform repository](https://github.com/vaughnw128/immanent-grove) straight-from-the-source. This will let you build your lab to the exact specificiations of yours truly, and permit the use of both synkronized and synkronized-charts.

## Single Container

This is a chart to use a single container with set resource limits, vault, and possibility to use the Cilium gateway API and cloudflare tunnels.

```shell
$ helm repo add synkronized-charts https://charts.vaughn.sh
```

An example `values.yaml` is as follows:

```
name: "mc-status-rs"
size: medium
image: "ghcr.io/vaughnw128/mc-status-rs:sha-b7bb738"

gateway:
  public: true
  hostname: "maps.example.com"
  port: 80

vaultSecrets:
- name: "DISCORD_TOKEN"
  path: "mc-status-rs/discord-token"
env:
- name: "MAPS_ADDRESS"
  value: "https://maps.example.com"
- name: "MC_SERVER_IP"
  value: "mc.example.com"
  ```

Additional examples can be found in the `examples/` directory.

And apply the chart with:

```shell
$ helm upgrade --install -n example --create-namespace synkronized-charts/single-container .
```
