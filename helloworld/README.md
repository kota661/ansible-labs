# helloworld

Ansibleをローカルにインストールし、Helloworldを表示してみよう！

※Ansibleがインストールできているか、動いているかの確認目的なので、実際に役立つものではないです



## ansibleのインストール

brewでインストールします

```
brew install ansible
```

実行できるか確認します

```
ansible --version
```

バージョンが表示されればOKです

```
ansible [core 2.13.4]
  config file = None
  configured module search path = ['/Users/kotasato/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/local/Cellar/ansible/6.4.0/libexec/lib/python3.10/site-packages/ansible
  ansible collection location = /Users/kotasato/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/local/bin/ansible
  python version = 3.10.7 (main, Sep 15 2022, 01:51:29) [Clang 14.0.0 (clang-1400.0.29.102)]
  jinja version = 3.1.2
  libyaml = True
```

## playbookの実行

ansible-playbookコマンドを使ってplaybookを実行します
成功すればログに`Hello World!!!`と表示されます
```
ansible-playbook ./playbook.yaml
```
