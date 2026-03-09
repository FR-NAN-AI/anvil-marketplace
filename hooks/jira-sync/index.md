---
safe-outputs:
  jobs:
    jira-sync:
      description: "Sync workflow results to Jira"
      runs-on: ubuntu-latest
      output: "Jira updated"
      inputs:
        issue_key:
          description: "Jira issue key (e.g. PROJ-123)"
          required: true
          type: string
        status:
          description: "Target Jira status"
          required: true
          type: string
      steps:
        - name: Update Jira
          uses: actions/github-script@v8
          env:
            JIRA_TOKEN: "${{ secrets.JIRA_TOKEN }}"
            JIRA_URL: "${{ secrets.JIRA_URL }}"
          with:
            script: |
              const issueKey = core.getInput('issue_key')
              const status = core.getInput('status')
              if (!process.env.JIRA_TOKEN || !process.env.JIRA_URL) {
                throw new Error('Missing Jira configuration')
              }
              core.info(`Would update ${issueKey} to ${status}`)
---

# Jira Sync Hook

Use this hook after a workflow completes to update Jira status. Provide the issue key and the desired status. Ensure the workflow output is summarized in the Jira comment when possible.
