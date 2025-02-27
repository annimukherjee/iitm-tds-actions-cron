# Hello GitHub Cron Jobs

`UPDATE 27th Feb 2025`:

I just realised that this is making commits on my behald belying my true commit history. I have stopped it.



### the main,yml


```
# Workflow name - identifies the workflow in GitHub Actions UI
name: Daily Commit

on:
  # Schedule the workflow to run daily at 20:30 UTC
  schedule:
    - cron: '30 20 * * *'  # Format: minute hour day_of_month month day_of_week
  
  # Allow manual triggering of the workflow from GitHub Actions UI
  workflow_dispatch:

permissions:
  contents: write  # ✅ Fixed indentation

jobs:
  # Define a job named 'daily_commit'
  daily_commit:
    # Specify the type of runner to use
    runs-on: ubuntu-latest
    
    steps:
      # Step 1: Check out the repository code
      - name: Checkout Repository
        uses: actions/checkout@v3  # Use GitHub's checkout action version 3
      
      # Step 2: Create or update the commit log file with current timestamp
      - name: Update commit file
        run: |
          echo "Commit made on $(date)" >> commit_log.txt
      
      # Step 3: Set up Git user configuration for the commit
      - name: Configure Git User
        run: |
          git config user.name "annimukherjee"
          git config user.email "mukh.aniruddha@gmail.com"
      
      # Step 4: Stage, commit, and push changes to the repository
      - name: Commit and Push Changes
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}  # ✅ Fixed indentation
        run: |
          git add commit_log.txt
          git commit -m "Daily commit: $(date)" || echo "No changes to commit"
          git push https://x-access-token:${GITHUB_TOKEN}@github.com/annimukherjee/iitm-tds-actions-cron.git HEAD:main
```
