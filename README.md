# 概要
GUIでパッケージをインストールできるウィザードです。  
ArchLinux派生ディストリビューションを構築する際の初期セットアップとして作成しました。  
ディストリビューターの方に向いています。  

# 対応しているOS
デフォルトではArchLinux用になっていますが、各スクリプトのpacmanの部分を他のパッケージ管理システムに書き換えることで他のディストリビューションでも動作します。ArchLinux以外で使用する場合はビルド用一般ユーザーを入力する部分がスキップされます。また、AURのビルドは使用できません。

# 試しに使う
ArchLinuxの場合は、簡単に試すことができます。  

```bash
git clone https://github.com/Hayao0819/package-gui-installer.git package-gui-installer
```
ダウンロード後、設定ファイル（settings.conf）でディストリビューションごとの設定をしたあとにrun.shを実行してください。

## 注意
pacman以外のパッケージマネージャの場合はsettings.confで、installed_list関数を記述してください。dpkgの場合はpacmanのものをコメントアウトして、dpkgのものをアンコメントしてください。

# 依存関係
- bash
- polkit
- zenity

# 使い方
run.shに実行権限を付与して、起動してください。`display`変数が空の場合は起動しません。

# ソフトウェアの追加
2019年12月13日現在では、デフォルトでインストールできるソフトは`baobab`と`gparted`しかありません。  
しかし、scriptsディレクトリにファイルを追加することで簡単にソフトを追加できます。

## スクリプトについて
一つのスクリプトで一つのパッケージ（パッケージ郡）にしてください。  
スクリプトはrootで実行されます。（そのため`makepkg`などは工夫が必要です。）  
スクリプトはbashで記述してください。（zshなどはテストしていません。）

## 記述するもの

以下のもののみ記述してください。また、全て記述してください。

### name
リストで表示されるパッケージの名前を入れてください。  
空白は絶対に入れないでください。  

### package_name
pacmanで管理する際に使用されるパッケージ名を入れてください。  

### description
パッケージの説明を入れてください。  
こちらも空白は絶対に入れないでください。  

### run_preparing
パッケージのビルドを行う場合は「true」、行わない場合は「false」を入れてください。  

### preparning
パッケージのビルドやダウンロードなどを記述してください。  
この部分を実行する場合は、上記の「run_preparing」を「true」にしてください。  

### install
パッケージのインストール処理を記述してください。  
rootで実行されるため、注意して記述してください。  
また、対話はすべて無効化してください。  

### uninstall
パッケージのアンインストール処理を記述してください。  

## AURのビルドについて
前述の通り、インストールスクリプトはrootで実行されます。AURにあるパッケージは簡単にビルドできるよう、既にスクリプトが用意されています。  
yayの`name`をAURのパッケージ名へ、`description`をそのパッケージの説明へと書き換え、ファイル名を変えることで簡単にAURのパッケージをビルドできます。 
AURにないパッケージで`makepkg`する必要がある場合は、最初に入力するビルド用一般ユーザーが格納されている変数${aur_user}を使用して外部のビルドスクリプトをsuで呼び出してビルドする必要があります。  

# ディストリビューターの方へ
`run.sh`を`/usr/bin`に、名前を変更して入れ、それ以外のファイルは`/etc/package-gui-installer`に入れ、設定の値等を変更することをおすすめします。これは、Linuxでの標準的な配置に従うためです。変更する値は、`run.sh`の設定ファイルへのパスとスクリプトディレクトリへのパスです。