## 出会ったエラーたち

### 目次
---
- error: failed to push some refs to '<repository>'
- 
  
---
  
### push した時にerror: failed to push some refs to '<repository>'
  
git push すると下記のようなエラーがでた。

```
! [rejected]        main -> main (fetch fast)
error: failed to push some refs to '<repository>'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
  
#### 解決方法

とりあえず、fetch、mergeをしないといけないみたい。
  
1. `git fetch origin`
1. `git merge --allow-unrelated-histories origin/main`
1. `git push origin main`
  



