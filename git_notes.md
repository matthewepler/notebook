#Git

## Learning Resources
[Atlassian Guide](https://www.atlassian.com/git/tutorials/saving-changes)

## Rebase
[Atlassian Tutorial](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)  
*The Golden Rule of Rebasing*: never use rebase on public branches.  

So, before you run git rebase, always ask yourself, “Is anyone else looking at this branch?” If the answer is yes, take your hands off the keyboard and start thinking about a non-destructive way to make your changes (e.g., the git revert command). Otherwise, you’re safe to re-write history as much as you like.

