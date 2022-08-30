# Release Plan

## Here is a standardized release flow 
1. Fetch the latest code from `dev` branch
2. Create a `release` branch on top of `dev` branch
3. Update the **version tag** in `ver.txt` file
4. Run build command
    - For the frontend Vue project, run:
        ```shell
        npm run build
        ```
    - For the backend Laravel project, run:
        ```shell
        npm run prod
        ```

5. Commit the `release` branch with a commit message and publish the branch, please read more about the commit message format at: [Version control](../collaboration/version-control.md)
6. Merge the `release` branch into the `main` branch and tag with a version number 
7. Merge the `release` branch into the `dev` branch
8. Delete the `release` branch
