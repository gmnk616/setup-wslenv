# wslの開発環境をansibleで構築

wslの開発環境をansibleを用いて構築します  
現時点で以下ディストリビューションが対応してます  

- ubuntu2204
- ubuntu2404

`ansible`をインストールする際は以下見出しを順番に実行します  

- ディストリビューションのインストール
- wsl.confの設定

## ディストリビューションのインストール

以下コマンドでLinuxディストリビューションをインストールします

```bash
# Ubuntu-22.04をインストール  
wsl --install Ubuntu-22.04

# Ubuntu-24.04をインストール
wsl --install Ubuntu-24.04
```

## wsl.confの設定

以下コマンドで`ansible`インストール前の準備をします。  

[user] - defaultはリストア後にrootになる問題の対策
[interop] - appendWindowsPathはWSLのコマンド保管が遅い事象の対策
[boot] - systemdはWSLでsystemdを使用するのに必要な設定

```bash
USERNAME=`whoami` && cat << EOS | sudo tee /etc/wsl.conf
[user]
default=$USERNAME
[interop]
appendWindowsPath=false
[boot]
systemd=true
EOS
```

コマンド入力後、以下コマンドでwslディストリビューションを抜けます

```bash
exit
```

以下コマンド(`powershell` or `cmd`)でwslディストリビューションを停止します  

```powershell
# Ubuntu-22.04
wsl -t Ubuntu-22.04

# Ubuntu-24.04
wsl -t Ubuntu-24.04
```

## ansibleインストール

以下コマンド(`powershell` or `cmd`)で停止したwslディストリビューションを起動(再起動)します  

```powershell
# Ubuntu-22.04
wsl -d Ubuntu-22.04 -u [ユーザー名] --cd ~/

# Ubuntu-24.04
wsl -d Ubuntu-24.04 -u [ユーザー名] --cd ~/
```

以下コマンドで`ansible`をインストールします

- pipをインストール
- venvをインストール
- venvを活性化(pipのver23.0からvenv上で実行しないとエラーとなるためです。)
- ansibleをインストール(併せてdockerとpasslibもインストール)
- ~/.bashrcにsource ~/.venv/bin/activateを追記

```bash
sudo apt update && \
sudo apt install -y python3-pip && \
sudo apt install -y python3-venv && \
source ~/.profile && \
python3 -m venv ~/.venv && \
source ~/.venv/bin/activate && \
pip install ansible && \
pip install docker && \
pip install passlib && \
echo "source ~/.venv/bin/activate " >> ~/.bashrc
```
