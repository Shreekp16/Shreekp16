name: Generate Snake Animation

on:
  schedule:
    - cron: "0 0 * * *"      # Runs once a day at midnight UTC
  workflow_dispatch:          # Allows manual trigger from the Actions tab
  push:
    branches:
      - main                  # Regenerate whenever main is updated

permissions:
  contents: write

jobs:
  generate:
    runs-on: ubuntu-latest
    steps:
      - name: Generate Snake Contribution Animation
        uses: Platane/snk@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/github-contribution-grid-snake.svg
            dist/github-contribution-grid-snake-dark.svg?palette=github-dark

      - name: Push Generated SVGs to the "output" Branch
        uses: crazy-max/ghaction-github-pages@v4
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
