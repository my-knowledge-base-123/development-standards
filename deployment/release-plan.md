# Release Plan

## Here is a standardized release flow 
1. Fetch the latest code from `dev` branch
2. Create a `release` branch on top of `dev` branch
3. Update the **version tag** in `ver.txt` file
4. Commit the `release` branch with a commit message and publish the branch, please read more about the commit message format at: [Version Control](../collaboration/version-control.md)
5. Merge the `release` branch into the `main` branch and tag with a version number 
6. Merge the `release` branch into the `dev` branch
7. Delete the `release` branch
