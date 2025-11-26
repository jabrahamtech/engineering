---
cssclasses: wide-mermaid
---
### Adding locally hosted code to GitHub

```mermaid
%%{init: {
  "sequence": { "useMaxWidth": true },
  "themeVariables": {
    "fontSize": "12px",
    "sequenceDiagramActorFontSize": "12px"
  }
}}%%
sequenceDiagram
    box Local
    participant WD as Working Directory
    participant SA as Staging Area
    participant LR as Local Repository
    end

    box Remote
    participant RR as Remote Repository
    end

    WD ->> SA: git add
    SA ->> LR: git commit

    LR ->> RR: git push
    RR -->> LR: git pull

    LR -->> WD: git checkout
    LR -->> WD: git merge

```
#### Initiate
```
git init -b main
```

#### Stage
```
git add .
```

#### Commit
```
git commit -m "first commit"
```

### Create Remote Repo
#### Create 
```
gh repo create
```

**Follow CLI instructions**
