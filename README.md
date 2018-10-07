# electron_app_test

* electronでApp開発をするためのテストコード及び環境構築メモ.

## Installation

* Node.jsをインストールする.

## Documentation

* [最新版で学ぶElectron入門](https://ics.media/entry/7298)
* [Electronの導入手順(Windows＆macOS共通の手順)](https://www.youtube.com/watch?v=fl1zQ82m0kc&feature=youtu.be)


![dir](https://github.com/flow-dev/electron_app_test/blob/master/electron_dir.png)

## Usage

```
#ディレクトリを生成し移動(ここに必要なものをinstallしていく)
mkdir electron_app_test
cd electron_app_test

#package.jsonを生成
npm init -y

#electronをinstall
npm install -D electron

#Codeをおくディレクトリを生成する
mkdir src
cd src

#package.jsonを生成して,
#エントリーポイントとなるJavaScriptのファイル名を指定する
New-Item package.json

#################->ここから
{
    "main": "main.js"
}
#################->ここまで

#"main.jsを生成して,
#エントリーポイント（アプリケーションの実行開始場所）となるjsファイルを作る.
New-Item main.js

#################->ここから
// アプリケーション作成用のモジュールを読み込み
const electron = require('electron');
const app = electron.app;
const BrowserWindow = electron.BrowserWindow;
 
// メインウィンドウ
let mainWindow;
 
function createWindow() {
  // メインウィンドウを作成します
  mainWindow = new BrowserWindow({width: 800, height: 600});
 
  // メインウィンドウに表示するURLを指定します
  // （今回はmain.jsと同じディレクトリのindex.html）
  mainWindow.loadFile('index.html');
 
  // デベロッパーツールの起動
  mainWindow.webContents.openDevTools();
 
  // メインウィンドウが閉じられたときの処理
  mainWindow.on('closed', () => {
    mainWindow = null;
  });
}
 
//  初期化が完了した時の処理
app.on('ready', createWindow);
 
// 全てのウィンドウが閉じたときの処理
app.on('window-all-closed', () => {
  // macOSのとき以外はアプリケーションを終了させます
  if (process.platform !== 'darwin') {
    app.quit();
  }
});
// アプリケーションがアクティブになった時の処理(Macだと、Dockがクリックされた時）
app.on('activate', () => {
  // メインウィンドウが消えている場合は再度メインウィンドウを作成する
  if (mainWindow === null) {
    createWindow();
  }
});
#################->ここまで

#メインウィンドウとして開くHTMLファイルを作る
New-Item index.html

#################->ここから
<html>
<head>
  <meta charset="UTF-8">
  <title>Hello World!</title>
</head>
 
<body>
  <h1>初めてのElectron</h1>
  We are using node <script>document.write(process.versions.node)</script>,
  Chrome <script>document.write(process.versions.chrome)</script>,
  and Electron <script>document.write(process.versions.electron)</script>.
</body>
</html>
#################->ここまで

#electoronをtest実行
cd .. (electron_app_testに戻る)
npx electron ./src

#electronを実行ファイル化するため,electron-packagerをinstall
cd .. (electron_app_testに戻る)
npm i -D electron-packager

#WindowsOS用の実行ファイルを生成する
npx electron-packager src FirstApp --platform=win32 --arch=x64 --overwrite

electron_app_test-win32-x64というディレクトリが出来て,.exeができる.

```

* 上記で,electronでApp開発する最低限の環境構築ができる.
* **node_modules**は,electronやelectron-packagerなど実行ファイルが全部入ってるので大きい.
* ./srcとpackage.json,package-lock.jsonにnpmパッケージの依存関係が書いてあるので, **npm install**で取り込める.
* だからこのリポジトリを**git clone**して,**npm install**すれば,同じ環境が作れる.
* 1から環境構築する方法と,package.jsonから環境構築する方法,2つの方法があることを知っておくこと.

## Dependencies

* electoron
* electron-packager
