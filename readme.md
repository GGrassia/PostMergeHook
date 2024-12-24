# Post-Merge-Hook

This Git hook automatically synchronizes changes from your base branch to all matching feature branches after a merge. It helps maintain feature branches up-to-date with the latest changes from your main development branch.

The main idea is to incrementally merge the main branch into all working branches, so you'll never find yourself with a branch 700 commits behind main (like it happened to me). You can now rest easy, knowing the feature branch that is in testing since 1989 is all caught up when the client finally tests the feature, and also avoid massive rebase headaches.

## Overview

When installed as a post-merge hook:
1. Checks if you're on the configured base branch
2. Finds all branches matching your specified pattern
3. Merges the base branch into each matching branch automatically
4. Reports any conflicts that need manual resolution
5. Switches back to the base branch if everything went fine

## Installation

Copy the script to your repository's hooks directory (```.git/hooks``` normally) and if you're on Unix/Linux give it executable permissions:
```bash
chmod +x .git/hooks/post-merge
```
Keep in mind the script needs to be called "post-merge" without extension.

## Usage

The hook runs automatically after any successful merge into your base branch. No manual intervention is required unless merge conflicts occur.

### Example Workflow:

1. You merge a PR into your base branch
2. The hook automatically runs
3. All matching branches are updated with the latest changes
4. If any conflicts occur, the script stops and leaves you on the conflicting branch

### Error Handling

The script will:
- Exit if you're not on the base branch
- Skip if no matching branches are found
- Stop and notify you if it encounters merge conflicts
- Leave you in a state where you can resolve any issues

## Configuration

Modify these placeholders at the top of the script to match your workflow:

```bash
BASE_BRANCH # The branch others merge into
BRANCH_PATTERN # Pattern to identify branches to update
```

## Troubleshooting

Common issues and solutions:

1. **Hook not executing**
   - Check file permissions (should be executable)
   - Verify the file is in `.git/hooks/post-merge`
   - If you are using Visual Studio, set it so it uses your system's Git, not its internal one.
   (Thanks Microsoft!) You can find a guide [here](https://stackoverflow.com/questions/43879501/configure-visual-studio-to-use-system-installed-git-exe).

2. **Merge conflicts**
   - The script will stop and leave you on the conflicting branch
   - Resolve conflicts manually and push them
   - Continue your workflow as normal

## Limitations

- Only works with branches that follow your naming pattern
- Requires write access to all affected branches
- No handling of complex merge strategies
- Potentially bothering if you have more than a couple of branches with merge issues
- Won't run on pull, only on merge, to re-trigger it's easiest to make a hard reset and pull again on the main branch

## Contributing

Feel free to submit issues and enhancement requests!

## License

[MIT License](LICENSE) - Feel free to use and modify as needed.
