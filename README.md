# action.deploy

GitHub composite Action to set node version and node sha before committing changes to GitHub and deploy the version

## example

```yaml
deploy:
    runs-on: ubuntu-latest
    
    permissions:
      contents: read

    needs: build

    steps:
      - uses: actions/checkout@v3
        with:
          repository: entrecode/ec.argocd
          token: ${{ secrets.PAT }}
          ref: main

      - uses: entrecode/action.deploy@v1
        with: 
          version: ${{ needs.build.outputs.version }}
          env: ${{ needs.build.outputs.env }}
          NAMESPACE: ${{ env.NAMESPACE }}
          NAME: ${{ env.NAME }}
```

## inputs

All inputs are requried to run the action

| Name               | Type     | Description                                                           |
|--------------------|----------|-----------------------------------------------------------------------|
| `version`          | String   | Version of the project after the build process (e.g., `1.0.0-dev`)    |
| `env`              | String   | Environment of the Cluster after the build process (e.g., `stage`)    |
| `NAMESPACE`        | String   | Namespace of the project                                              |
| `NAME`             | String   | Name of the project                                                   |
