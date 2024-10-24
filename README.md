# synkronized-charts

Charts for Synkronized! Synkronized is a custom built deployment pipeline for auto applying helm charts based on simple YAML files in repo's root directories. These charts here are not intended to be used alone, but rather act as a middleman between ArgoCD and the Synkronized service. These charts leverage vault and the vault operator to pull in static secrets to the deployment, and it's inteded to deploy these charts via ArgoCD. 

## Single Container

This is a chart to use a single container with set resource limits and vault.

```shell
$ helm repo add synkronized-charts https://charts.vaughn.sh
```

An example `values.yaml` is as follows:

```
name: "example"
size: medium
image:
  repository: "ghcr.io/vaughnw128/example"
  tag: "sha-b7bb738"

vaultSecrets:
- DISCORD_TOKEN: "example/discord-token"
- FAKETOKEN: "test/test"
env:
- MAPS_ADDRESS: "https://maps.example.com"
- MC_SERVER_IP: "example.com"
  ```
And apply the chart with:

```shell
$ helm upgrade --install -n example --create-namespace synkronized-charts/single-container .
```