---
description: Easy bash script to change the emails used in a git repo
---

# Changing Git Commit Email Address

To change the email address for all commits in your Git repository to a new correct one, you can use the `git filter-branch` command. Here's a step-by-step process:

1.  First, ensure you have the correct email set for future commits:

    ```
    git config user.email "your-new-correct-email@example.com"
    ```
2.  Then, run the following command to rewrite the history:

    ```
    git filter-branch --env-filter '
    OLD_EMAIL="your-old-incorrect-email@example.com"
    CORRECT_EMAIL="your-new-correct-email@example.com"
    if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
    then
        export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
    fi
    if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
    then
        export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
    fi
    ' --tag-name-filter cat -- --branches --tags
    ```

    \
    Replace `your-old-incorrect-email@example.com` with the wrong email you used, and `your-new-correct-email@example.com` with your correct email.\

3. After running this command, Git will rewrite your repository's history, changing the email address for all commits.\

4.  If you've already pushed your commits to a remote repository, you'll need to force push the changes:

    ```
    git push --force --tags origin 'refs/heads/*'
    ```

Be cautious with this operation, especially if you're working on a shared repository, as it rewrites history. This can cause issues for other contributors if they've based work on the old history.





source: Claude sonnet 3.5 :)
