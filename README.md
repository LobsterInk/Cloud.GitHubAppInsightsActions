# App Insights for GitHub Actions
Sometimes a DevOps engineer wants to see changes to the deployment infrastructure on the observability dashboard, so that the changes can be correlated with resource usages or increases in errors. This [composite run steps GitHub Action](https://docs.github.com/en/actions/creating-actions/creating-a-composite-run-steps-action) is a rudimentary attempt to solve that challenge by allowing you to send events to Azure Application Insights. Each event will be sent with some [default GitHub environment variables](https://docs.github.com/en/actions/reference/environment-variables#default-environment-variables). Full example usage of this action can be seen in [this repository](https://github.com/syedhassaanahmed/test-app-insights-event-action).

## Usage
```yaml
      - name: 'AppInsights: Begin Terraform'
        uses: lobsterink/cloud.githubappinsightsactions@main
        with:
            instrumentation-key: '${{ secrets.APP_INSIGHTS_KEY }}'
            event-name: 'provision-azure'
            event-data: |
              {
                "type": "start"
              }

      - name: "Terraform Apply"
        run: terraform apply -auto-approve

      - name: 'AppInsights: End Terraform'
        uses: lobsterink/cloud.githubappinsightsactions@main
        with:
            instrumentation-key: '${{ secrets.APP_INSIGHTS_KEY }}'
            event-name: 'provision-azure'
            event-data: |
              {
                "type": "end"
              }
```
