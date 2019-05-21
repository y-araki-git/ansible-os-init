# OS初期設定 Ansible 自分用サンプル

### 対応OSとバージョン

- CentOS 6,7 Debian 8

- roles/init/taskのzabbix,td-agent.ymlはインストールするバージョンを都度確認すること。

### ディレクトリ構成

inventoryファイルは、環境ごと(prd, stg, dev)に作成しています。

環境ごとの変数ファイルは、vars配下にディレクトリ分けして(prd, stg, dev)定義します。

環境に依存しない共通の変数は、group_varsに定義します。

    ┣━ group_vars
    ┃　　┣━ all.yml
    ┃　　┗━ web.yml
    ┣━ vars
    ┃　　┣━ prd
    ┃  　　┣━ init.yml
    ┃　　┣━ stg
    ┃　　┗━ dev
    ┣━ README.md
    ┣━ prd
    ┣━ init.yml
    ┗━ roles
    　　　┣━ init
    　　　 ：


### inventory ファイル

inventoryには :vars を使うことで、変数を定義できるため、環境名を定義する変数stageを定義しておきます。

    [web]
    xxx.xxx.xxx.xxx

    [batch]
    xxx.xxx.xxx.xxx
    [all:vars]
    stage=stg


### roles ファイル

- init: インスタンス初期構築時の共通処理

    実行前に group_vars/all.yml の変数を定義してください。

### 実行例

本番環境のinitグループのインスタンスに、OSごとの初期設定処理を行います

→ ansibleコマンド実行前にパスフレーズをキャッシュさせます

    # eval `ssh-agent`
    # ssh-add 秘密鍵のパス

→ 実行ユーザとしてroot指定

    # ansible-playbook -i prd init.yml -u root --private-key=秘密鍵

### 参考

[Ansible Playbook Best Practice](http://docs.ansible.com/ansible/playbooks_best_practices.html)
