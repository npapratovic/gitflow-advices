# gitflow-advices

Additional notes: 

- Use rebases on feature branches to keep a clean, readable timeline. During development run frequent `git rebase main` 
- Use merges between shared branches (main to production, hotfix to main) to preserve history integrity.
- Always open PRs when merging commits from feature branch to main or from main to production for controlled review, testing, and audit trail.
- Hotfix are branched from production → merge back to both
- Feature branches are branched from main
- Its safe to do commits on main and on feature branches at the same time, just be carefull to rebase main on feature branches

- Never push on production branch. **Rule of the thumb for commiting code:**
1) push to hotfix branch if it is urgent and must go on live environment. Hotfix branch is made from production branch and merged back to all branches. 
2) push to main branch if it is not urgent and will go to live when merged into production
3) push to feature branch if it is related to feature and not yet ready for merging to main
   
- Add semantic versioning or release tags  Tag production branch on every deployment:  `git tag -a v2025.10.31 -m "October release"` This helps rollback, CI tracking, and client communication. Tags are like "snapshot label"

Every time production branch is deployed (whether it’s a full release or a quick hotfix), create a tag on that exact commit:

Typical Flow
# Checkout production branch after deploy

```
git checkout production
git pull origin production
```

# Tag the commit currently deployed

``` 
git tag -a v2025.10.31 -m "October release with bugfixes"
git push origin v2025.10.31
```
Benefits of tagging: 

| Reason                           | Explanation                                                                                     |
| -------------------------------- | ----------------------------------------------------------------------------------------------- |
| **Auditing & traceability**      | You know *exactly* what code version was deployed on any given day.                             |
| **Rollback safety**              | You can `git checkout v2025.10.31` to restore your app to that precise version.                 |
| **Deployment reproducibility**   | CI/CD can redeploy or rebuild that same release any time in the future.                         |
| **Release changelog generation** | Tools like `git log v2025.10.24..v2025.10.31` automatically list what changed between releases. |
| **Hotfix management**            | Each hotfix can be tagged incrementally (e.g., `v2025.10.31.1`, `v2025.10.31.2`).               |
| **Confidence for team**          | Everyone knows what’s live — no guessing, no "what commit is in prod?" confusion.               |

Let’s say v2025.10.31.1 (your latest hotfix) broke something.

Rollback is instant and risk-free:
```
git checkout production
git reset --hard v2025.10.31
git push origin production --force
```
Now your production branch again points to the previous stable tag (v2025.10.31).

You’ve just rolled back production to a known-good state with zero guesswork.

- **Feature flag strategy** When long-running features are half-done, use flags so you can still deploy main frequently.
- Document branch lifecycle Example: Create → Develop → PR → Merge → Delete after merge Keeps repository clean.
- Use the PRs: 

| Benefit                             | Explanation                                                                                                                                   |
| ----------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| **Code Review & Knowledge Sharing** | PRs ensure every line of code gets seen by another developer before merging. Prevents bugs, promotes consistent patterns, and shares context. |
| **Automated Checks**                | CI/CD can automatically test, lint, and validate before merging — enforcing code quality.                                                     |
| **Clean Commit History**            | PR merges can be squashed into a single logical commit (`feat: add disposal location import`) keeping the history readable.                   |
| **Change Traceability**             | GitHub links each PR to an issue. You can always see *why* something was added and who approved it.                                           |
| **Protected Branches**              | You can lock `main` and `production` so they only accept merges through reviewed PRs, preventing accidental direct pushes.                    |
| **Release Audit Trail**             | Each PR acts as a “changelog entry” — makes release notes generation automatic.                                                               |

_A PR is not just a merge tool — it’s a controlled gateway for code quality, testing, and traceability._

I have created gitGraph in mermaid just to show how branching strategy should work: 

https://mermaid.ink/svg/pako:eNqtVstymzAU_RWNusUZS-CYYddpZ7rqotNdh42CrkFjpOsIKU3q8b9XGDvxAwNJywqkcx_nnCtgSwuUQDNaKvfNik2VGxKuArVW7mP3D1aYoiI55cl8tgLhZkZoyOkBWUGxRu9u7t-qcIzTQpluSYMtYTgPUTILCAuPHhoHkoCpRAEajCPsBrTB-ikgf3wmqmlC2BvwhBkbYcb-EzP2EWZ8KrMAJGfUNhalL5xCc8G4QrdSz7N-zbpNwsgK7T7FEIpfo44CXFbvZHgtPazWOezEKj5iFX-HVVPurxscdyzuVaxBDcT9BrFupnbeaXFZeqDLMcf7Z-nRq2LduvmJLxbv85FPk0lIGSR68M6FLH0tFJUwIWuNpTJhp0Z7rsFFuT7uk90_lkTjlPFAJDxBjZsr4m9cTuYvHjnC7a63sD8WGqWoe1HCoKvggDgZnWk9JCM9dGp3WoY-dC_Ib6Rw0IcaeoMNHb_-QTmZiKHE8a3pv0X4H6r2WnFuQn6A5oZGNKQMwTJ8V7ftak5DQNtJGy5hJXy9j9kFqPAOf76YgmbOeoioRV9WNFuJuglPneZflSit0K-rG2F-IepjSHik2ZY-02zGFpzdJeGLuEzSJLnnPKIvNGNpesdZzOP5fMk4S5e7iP7ZJ-B38ZwtGLuP03kaL0NkREEqh_Z791-w_z2IaGlbNofyFowE-wW9cTRLWLLc_QUsb6kL

- For new feature request, start with creating new branch from main, then do commits on that branch. Deploy that branch to staging env. after QA, merge branch to main so that it can be checked on mirror environment. Repeat endlessly. 
- If client requests enhacements, commit them only on main branch and merge to production if needed. If there is need for hotfix, branch out production branch, make commits, and merge hotfix branch to main and to production branch. **Enhacements are something that needs to be added to app but not deployed instantly, hotfixes need to be deployed instantly!**
- Whever hotfix is merged into main or enhacements is added to main, checkout all feature branches and rebase main into them to avoid future merge conflicts 

Mermaid live code example: 

https://mermaid.live/edit#pako:eNqtVstymzAU_RWNusWeIMA27DrtTFdddLrrsFHQNWiCdB0hpUk9_vcKYyd-YCBpGRYgnfs451zZbGmBAmhGS2m_Gb6pck38VaBS0n7s-d5wXVQkpyy-m62B25nmCnJ6QFZQPKCzN_dvVTjGKS51t6TAlDCch0iReYSBRweNBUFAV7wABdqS8Aa0wfrJI398JrJpfNgb8IRZOMIs_E_Mwo8wY1OZeSA5o7YxKFxhJeoLxhXatXye9WvWbZKQrNHsUwyh2DXqKMBl9U6G19LDap3DTqxiI1axd1g15fm6wXHHol7FGlRA7G_gD83UzjstLksPdDnmeP8sPTpZPLRufmJJ8j4f2TSZuBBeontnrc_S10JRce2z1lhK7XdqNOcaXJTr4z7Z_WNJ1FZqB0TAE9S4uSL-xuVk_qKRI9zuOgP7Y6FQ8LoXxTXaCg6Ik9GZ1kM80kOndqel70P1gtxGcAt9qKFfsKHj1z8oJxMxlDi6Nf23CP9D1V4rzk3ID9Bc04D6lD5Y-P_VbbuaUx_QdtKGC1hzV-9jdh7KncWfL7qgmTUOAmrQlRXN1rxu_Fun-VfJS8PV6-qG61-I6hjiX2m2pc80m6VpMk9YuEwW4SpepkkS0BeasSies2jl72Wapqso2QX0zz5BOF-lIUvYIo2Wd-EyjtOAgpAWzffuu2D_eRDQ0rRsDuUNaAHmCzptaRYtFmz3F0niqV8

``` 
gitGraph
    commit
    commit
    commit
    commit
    commit
    branch "240-feat-name"
    checkout "240-feat-name"
    commit
    commit
    checkout main
    merge "240-feat-name"
    commit id: "requested enhacement 1"
    commit id: "resolved QA issues 1"
    branch "241-feat-name"
    checkout "241-feat-name"
    commit
    commit
    checkout main
    merge "241-feat-name"
    commit id: "requested enhacement 2"
    commit id: "resolved QA issues 2" 
    branch production
    branch "hotfix-1"
    commit id: "hotfix 1 for prod"
    commit id: "hotfix 2 for prod"
    checkout production
    merge hotfix-1
    checkout main
    merge hotfix-1
    branch "242-feat-name"
    checkout "242-feat-name"
    commit
    commit
    commit
    commit
    commit
    checkout main
    commit id: "requested enhacement 3"
    commit id: "some tweaks"
    checkout "242-feat-name"
    merge main
    commit
    commit
    checkout production
    branch "hotfix-2"
    commit id: "quickfix #255"
    checkout production
    merge hotfix-2
    checkout main
    commit id: "added button"
    commit id: "change login color"
    merge hotfix-2
    commit
    checkout "242-feat-name"
    commit
    commit id: "continue develop"
    checkout main
    branch "243-feat-name"
    commit id: "feature for modal"
    commit id: "another modal enhacement"
    checkout main
    branch "244-feat-name"
    commit id: "added login form"
    commit id: "update login form"
    checkout main
    merge "242-feat-name"
    checkout production
    merge main
    checkout main
    merge "243-feat-name"
    merge "244-feat-name"
    checkout production
    merge main
    checkout main
    commit id: "another enhacement"
```

    <img width="2360" height="2348" alt="mermaid-diagram-2025-10-31-150239" src="https://github.com/user-attachments/assets/49ab78e4-746a-4445-93d0-745079d6ecf6" />


