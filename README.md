# synkronized-charts

Charts for Synkronized! Synkronized is a custom built deployment pipeline for auto applying helm charts based on simple YAML files in repo's root directories. These charts here are not intended to be used alone, but rather act as a middleman between ArgoCD and the Synkronized service. These charts leverage vault and the vault operator to pull in static secrets to the deployment, and it's expected to deploy these charts via ArgoCD. 

## Single Container

This is a chart to use a single container with set resource limits and vault.

```shell
$ helm repo add synkronized-charts https://charts.vaughn.sh
```

An example `values.yaml` is as follows:

```
synkronized:
  name: mc-status-rs
  template: single-container
config:
  size: medium
  vaultSecrets:
    - name: "DISCORD_TOKEN"
      path: "mc-status-rs/discord-token"
  env:
    - name: "MAPS_ADDRESS"
      value: "https://maps.vaughn.sh"
    - name: "MC_SERVER_IP"
      value: "mc.vaughn.sh"
  ```
And apply the chart with:

```shell
$ helm upgrade --install -n example --create-namespace synkronized-charts/single-container .
```
