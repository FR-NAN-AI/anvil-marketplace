---
safe-outputs:
  jobs:
    slack-notify:
      description: "Send a Slack notification"
      runs-on: ubuntu-latest
      output: "Slack notified"
      inputs:
        channel:
          description: "Slack channel (e.g. #eng-alerts)"
          required: true
          type: string
        message:
          description: "Message to send"
          required: true
          type: string
      steps:
        - name: Post to Slack
          uses: actions/github-script@v8
          env:
            SLACK_WEBHOOK_URL: "${{ secrets.SLACK_WEBHOOK_URL }}"
          with:
            script: |
              const channel = core.getInput('channel')
              const message = core.getInput('message')
              if (!process.env.SLACK_WEBHOOK_URL) {
                throw new Error('Missing Slack webhook')
              }
              core.info(`Would notify ${channel}: ${message}`)
---

# Slack Notify Hook

Use this hook to notify a team channel when a workflow finishes or needs attention. Keep messages short and include links to relevant issues or pull requests.
