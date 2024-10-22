```mermaid
---
title: Git diagram
---
gitGraph
   commit
   commit
   branch develop
   checkout develop
   commit
   commit
   checkout main
   merge develop
   commit
   commit

```

![](https://www.youtube.com/watch?v=Uszj_k0DGsg&t=1036s)


```mermaid
flowchart LR
	init --> add --> commit-1 ---|edit|commit-2 --> tag --> push
	commit-1 ---| new branch|edit --> commit-3
```

