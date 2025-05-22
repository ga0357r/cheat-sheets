# Configuring Smart Merge for Unity Project Asset Conflicts in Git

This guide will help you set up smart merge for your custom Unity project to resolve asset conflicts more efficiently when using Git, focusing on Git config commands.

## Prerequisites

- Git installed on your system
- Unity installed on your system
- A custom Unity project under Git version control

## Steps to Configure Smart Merge

### 1. Locate Unity's SmartMerge Tool

First, find the path to Unity's SmartMerge tool on your system. The location depends on your operating system and Unity installation:

- Windows: `C:\Program Files\Unity\Hub\Editor\2022.3.23f1\Editor\Data\Tools\UnityYAMLMerge.exe`
- macOS: `/Applications/Unity/Unity.app/Contents/Tools/SmartMerge`
- Linux: `/opt/unity/Editor/Data/Tools/SmartMerge`

### 2. Configure Git to Use SmartMerge

Open your terminal or command prompt, navigate to your Unity project directory, and run the following Git commands:

```bash
# using the git bash terminal
# navigate to unity project directory for example "cd C:\Users\ayode\Documents\Repos\GitHub\evon-ddt-game-template\evon-ddt-client"
cd <path-to-project-directory>

# Set the merge tool to unityyamlmerge
git config merge.tool unityyamlmerge

# Configure the unityyamlmerge tool
git config mergetool.unityyamlmerge.trustExitCode false

## for windows
git config mergetool.unityyamlmerge.cmd "\"C:/Program Files/Unity/Hub/Editor/2022.3.10f1/Editor/Data/Tools/UnityYAMLMerge.exe\" merge -p \\\"\$BASE\\\" \\\"\$REMOTE\\\" \\\"\$LOCAL\\\" \\\"\$MERGED\\\""
```

Replace `<path-to-unity>` with the actual path to your Unity installation. For example, on Windows, it might look like this:

```bash
git config mergetool.unityyamlmerge.cmd ' "C:\Program Files\Unity\Hub\Editor\2022.3.23f1\Editor\Data\Tools\UnityYAMLMerge.exe"  merge -p "$BASE" "$REMOTE" "$LOCAL" "$MERGED" '
```

### 3. Verify the Configuration

To verify that the configuration has been set correctly, you can use the following commands:

```bash
git config --get merge.tool
git config --get mergetool.unityyamlmerge.trustExitCode
git config --get mergetool.unityyamlmerge.cmd
```

These commands should display the values you've just set.

## Fixing Conflicts in Your Pull Request

When you're working on a feature branch and need to merge changes from the main branch, you might encounter conflicts, especially with Unity-specific files. Here's how to handle these conflicts effectively:

### Using GitHub Desktop

1. Open GitHub Desktop and switch to your feature branch.
2. Click on "Branch" in the top menu, then select "Rebase current branch".
3. Choose the branch you want to rebase onto (usually the main or master branch).
4. If conflicts are detected, GitHub Desktop will notify you.

### Resolving Unity-Specific Conflicts

If you see conflicts in `.unity`, `.asset`, `.prefab`, or other Unity-specific files, follow these steps:

1. Open a terminal or command prompt.
2. Navigate to your Unity project's root directory.
``` bash
# navigate to unity project directory for example "cd C:\Users\ayode\Documents\Repos\GitHub\evon-ddt-game-template\evon-ddt-client"

cd <path-to-project-directory>
```

3. Run the following command:

``` bash
git mergetool
```

4. The Unity SmartMerge tool will open for each conflicting file and will try to automatically fix them.
5. Use the SmartMerge interface to resolve conflicts visually.
6. Save your changes and close the SmartMerge tool.

### Completing the Rebase

1. After resolving all conflicts, return to GitHub Desktop.
2. You should see the option to "Continue rebase".
3. Click on "Continue rebase" to finish the process.

### Pushing Your Changes

1. Once the rebase is complete, you may need to force push your changes.
2. In GitHub Desktop, you'll see a notification that your branch is ahead of the remote.
3. Click on "Push origin" and select "Force push" when prompted.

**Note:** Be cautious with force pushing, especially on shared branches. Make sure you understand the implications before proceeding.

### Final Steps

1. After successfully pushing your changes, your pull request should automatically update.
2. Review the changes in your pull request on GitHub to ensure everything looks correct and there are no conflicts.
3. If necessary, ask a team member to review your changes again.

By following these steps, you can effectively resolve conflicts in your Unity project, even when dealing with complex asset files. Remember to communicate with your team throughout this process, especially when working on shared branches or critical project files.

## Tips for Effective Merging

- Always pull the latest changes before starting work on a new feature.
- Commit and push your changes frequently to minimize large merge conflicts.
- Communicate with your team to avoid working on the same assets simultaneously.
- Use Unity's collaboration features in conjunction with Git for better workflow management.

By following this guide, you should now have smart merge configured for your custom Unity project, making it easier to resolve asset conflicts when using Git.
