name: Weekly Pull Request Bot

on:
  schedule:
    - cron: '0 13 * * 6' # هر شنبه ساعت ۱۳ اجرا میشه
  workflow_dispatch:

jobs:
  create-pull-request:
    runs-on: ubuntu-latest
    permissions:
      contents: write
      pull-requests: write

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Git
        run: |
          git config --global user.email "vikram.singh13672000@gmail.com"
          git config --global user.name "ThomasBakker-hub"

      - name: Create a new branch and change
        run: |
          BRANCH_NAME="maintenance-$(date +%s)"
          git checkout -b $BRANCH_NAME
          echo "Weekly maintenance log: $(date)" >> maintenance.log
          git add maintenance.log
          git commit -m "Routine maintenance"
          git push origin $BRANCH_NAME
          
          # اینجا از ابزار خود گیت‌هاب (gh) برای ساخت PR استفاده می‌کنیم
          gh pr create --title "Weekly Maintenance Report" --body "Automated weekly update." --base main --head $BRANCH_NAME
        env:
          GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}

      - name: Merge Pull Request
        run: |
          # پیدا کردن آخرین PR ساخته شده توسط خودمان و ادغام آن
          gh pr merge --auto --merge --delete-branch
        env:
          GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
