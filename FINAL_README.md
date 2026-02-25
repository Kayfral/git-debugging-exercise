**Task 1 — finding problematic commits**
```bash
git log --oneline --all --graph
git log --stat
git show <hash>
```

**Task 2 — splitting a “quick fix” commit**
```bash
git rebase -i <hash>
git reset --mixed HEAD^
git add <file>
git commit -m "<type>: <message>"
```

**Task 3 — safe fix**
```bash
git revert <hash>
```

**Task 4 — conflict resolution**
```bash
git merge feature/login
git add notes.txt
git commit -m "merge: resolve notes conflict"
```

**Task 5 — removing a secret**
```bash
rg -n "API_KEY|SECRET|TOKEN" -g"*"
git add secrets.txt
git commit -m "chore: remove hardcoded secret"
```

**Task 6 — syncing a branch**
```bash
git checkout feature/login
git rebase main
```
I chose rebase because it moves the changes from feature/login on top of the updated main and keeps the history linear and clean, without an extra merge commit. It’s easier to review and doesn’t clutter the history.

**Secrets cleaning algorithm**
1. Revoke the secret - Reissue the token / change the password / delete the key.
2. Rewrite the history - Use git filter-repo (or BFG) to remove the file or replace the string in all commits.
3. Force push the new history
4. Do not use the old commit history anymore

**Task 7 - Recover Lost Commit (Bonus)**
```bash
git checkout feature/login
git reflog show feature/login --date=iso | head -n 30
git cherry-pick <hash>
```
**Regarding the difficulties**  
In Task 2, I had to split one commit into three. I successfully did it in the main branch, but at that moment I didn’t notice that the same issue existed in feature/login. Because of that, during the next steps, the “quick fix” commit was moved from feature/login to main.

I noticed this problem only when I was pushing to GitHub. I had to spend some time cleaning up the branches to remove the “quick fix” commit that had already been deleted earlier.