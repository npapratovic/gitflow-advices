# gitflow-advices

Mermaid live code example: 
 
`gitGraph
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
`

    

