---
safe-outputs:
  jobs:
    update-project-board:
      description: "Update GitHub Project item fields"
      runs-on: ubuntu-latest
      output: "Project board updated"
      inputs:
        project_id:
          description: "GitHub project node id"
          required: true
          type: string
        item_id:
          description: "Project item node id"
          required: true
          type: string
        status:
          description: "Target status value"
          required: true
          type: string
      steps:
        - name: Update Project Field
          uses: actions/github-script@v8
          env:
            GH_TOKEN: "${{ secrets.GITHUB_TOKEN }}"
          with:
            script: |
              const projectId = core.getInput('project_id')
              const itemId = core.getInput('item_id')
              const status = core.getInput('status')
              core.info(`Would update project ${projectId}, item ${itemId} to ${status}`)
---

# Update Project Board Hook

Use this hook to keep GitHub Projects in sync after workflow actions. Only update fields required by the team workflow.
