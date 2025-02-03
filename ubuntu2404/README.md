# setup ubuntu2404

## プレイブック実行コマンド

```shell
# プレイブック実行(ドライラン)
ansible-playbook -i inventories/hosts.ini -l noble playbook.yaml --diff --check

# プレイブック実行
ansible-playbook -i inventories/hosts.ini -l noble playbook.yaml --diff 

# プレイブック実行(初回時のみ、このコマンド)
ansible-playbook -i inventories/hosts.ini -l noble playbook.yaml --diff --ask-become-pass

# 一部のタスクのみ実行(一例)
ansible-playbook -i inventories/hosts.ini -l noble playbook.yaml --diff --start-at-task="git config"
```
