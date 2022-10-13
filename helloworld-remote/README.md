Ansibleを使ってNginxを構築してみよう


## 事前準備
CentOSやRHELなどRedhat系のサーバーを準備する
* 到達可能なIPアドレス
* ssh key file



## 今回のゴール

* 用意したサーバーにNginxを構築してhelloworldを表示させる



## Ansibleの実行

まずはじめにPlaybookがあるフォルダに移動します

```
cd helloworld-remote
```



接続情報を持っているinventoryファイルを作成します

```
cp ./inventory.sample ./inventory
```



inventoryファイルを開きサーバーのIPアドレスと、Private Keyのパスを設定します

```
[web]
127.0.0.1   <<< こちらのIP

[wewb:vars]
ansible_port=22
ansible_user=root
ansible_ssh_private_key_file=~/.ssh/xxxxxxxx <<< こちらのSSH Key
```



次にNetworkの疎通確認をします

```
ansible web -i ./inventory -m ping
```

疎通できているとこちらの用意Successと表示されます

```
The authenticity of host '163.68.87.0 (163.68.87.0)' can't be established.
ED25519 key fingerprint is SHA256:LKJ3TraaepLeaKlmqdTXppn5k14rEdmrXv6LA+MfZY4.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
163.68.87.0 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/libexec/platform-python"
    },
    "changed": false,
    "ping": "pong"
}
```



Playbookを適用前に問題なく処理ができるかチェックをします

```
ansible-playbook -i ./inventory ./playbook.yaml --check
```

今の段階ではnginxが未インストールなので、TASK [Start Nginx service] は失敗してしまいますが、それ以外は成功することを確認します

```
PLAY [web server] ************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************
ok: [163.68.87.0]

TASK [Install Nginx] *********************************************************************************************************
changed: [163.68.87.0]

TASK [Create Contents] *******************************************************************************************************
changed: [163.68.87.0]

TASK [Start Nginx service] ***************************************************************************************************
fatal: [163.68.87.0]: FAILED! => {"changed": false, "msg": "Could not find the requested service nginx: host"}

PLAY RECAP *******************************************************************************************************************
163.68.87.0                : ok=3    changed=2    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0   
```



それでは実際に適用してみましょう。--check のオプションを外して実行します

```
ansible-playbook -i ./inventory ./playbook.yaml
```
最後まで実行できOKやChangedのみカウントされている（failedが０件）であれば成功です

```
PLAY [web server] ************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************
The authenticity of host '163.68.81.18 (163.68.81.18)' can't be established.
ED25519 key fingerprint is SHA256:v67X4aA0ci5ZecGX6i3T45kYFpatotKKeAI8F33oLhs.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
ok: [163.68.81.18]

TASK [Install Nginx] *********************************************************************************************************
changed: [163.68.81.18]

TASK [Create Contents] *******************************************************************************************************
changed: [163.68.81.18]

TASK [Start Nginx service] ***************************************************************************************************
changed: [163.68.81.18]

PLAY RECAP *******************************************************************************************************************
163.68.81.18               : ok=4    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

```

最後に動作確認として、指定したサーバーにブラウザでアクセスして80ポートでnginxが起動しているか確認します
Hello World!!!と表示されていればOKです

