# workflow-action-token-wise-testing

1. Create Repo

2. Create/import production branch ruleset

3. Create/add  workflow action file in .github/workflows/deploy-main-to-production-<xyz>.yml

name: Deploy main → production (XYZ)

on:
  workflow_dispatch: # Allows manual trigger

permissions:
  contents: write

jobs:
  deploy:
    name: Push main to production
    runs-on: ubuntu-latest

    steps:
      - name: Validate actor (restrict who can run)
        if: github.actor != 'pushpesh-zuzu'
        run: |
          echo "❌ Unauthorized user: ${{ github.actor }}"
          exit 1

      - name: Checkout repository
        uses: actions/checkout@v4
        with:
          ref: main
          token: ${{ secrets.PERSONAL_GITHUB_TOKEN }}

      - name: Push main to production
        run: |
          git config user.name "Pushpesh Sharma"
          git config user.email "pushpeshsh@zuzucodes.com"
          git fetch origin
          git checkout main
          git push origin main:production --force


4. add PERSONAL_GITHUB_TOKEN token under Repository secrets from repo -> Settings -> Secrets and variables -> Actions
