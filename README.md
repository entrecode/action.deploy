# action.deploy

GitHub composite action for deploying to argoCD

## example

```yaml
deploy:
    runs-on: ubuntu-latest
    
    permissions:
      contents: read

    needs: build

    steps:
      - uses: entrecode/action.deploy@latest
        with:
          PAT: ${{ secrets.PAT }} 
          NAMESPACE: ${{ vars.NAMESPACE }}
          NAME: ${{ vars.NAME }}
          version: ${{ needs.build.outputs.version }}
          env: ${{ needs.build.outputs.env }}
```

## inputs

All inputs are requried to run the action

| Name               | Type     | Description                                                           |
|--------------------|----------|-----------------------------------------------------------------------|
| `PAT`              | String   | Personal Access Token as GitHub secret                                |
| `NAMESPACE`        | String   | Namespace of the project                                              |
| `NAME`             | String   | Name of the project                                                   |
| `SERVICES`         | String   | Comma separated list of services to deploy, can be used to use dynamic yq paths |
| `version`          | String   | Version of the project after the build process (e.g., `1.0.0-dev`)    |
| `env`              | String   | Environment of the Cluster after the build process (e.g., `stage`)    |

`NAMESPACE` and `NAME` have to be defined in GitHub as action-variables. `version` and `env` are build-outputs.

## dynamic yq paths

The action defaults to the path `.node.version` and `.node.gitsha` in `version.yaml` to update the versions. If you want to use a different path in `version.yaml`, you can use `SERVICES` variable. Simply add your list of services as a comma separated list and add your custom path seperated by colon. For example: `cronjobA:.my.custom.path,cronjobB:.my.custom.path2`. If you omit the path for any service, the default path will be used.