**# --- Step 1: Check Location \& Create New Assets ---**



pwd                                  # 1. Confirm your current folder location

mkdir images                         # 2. Create an "images" folder

cd images                            # 3. Enter the "images" folder

touch hero-banner.png                # 4. Create a new file inside "images"

cd ..                                # 5. Return to the root project folder



**# --- Step 2: Organize \& Copy Existing Files ---**



cp logo.png images/                  # 6. Copy logo.png into the "images" folder

mv old-email.html archive/           # 7. Move old-email.html into "archive"

cp -r components templates/          # 8. Copy the "components" folder into "templates"



**# --- Step 3: Git Workflow (Check, Sync, Save, Push) ---**



git status                           # 9. Review all new, moved, and modified files

git pull origin <your-branch-name>   # 10. Fetch latest changes from GitHub to prevent conflicts

git add .                            # 11. Stage all project changes

git commit -m "Reorganize structure and add hero banner" # 12. Save changes locally

git push origin <your-branch-name>   # 13. Upload local commits to GitHub

