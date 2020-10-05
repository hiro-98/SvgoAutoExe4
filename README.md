# SvgoAutoExe
編集中のSVGファイルを監視して上書き保存時にファイル軽量化を実行します。実行される処理はSVGOMGと同じです。

# 使い方
## 起動方法
解答したフォルダで SvgoAutoExe.exe を実行してください。

## メインウィンドウ
![image](https://user-images.githubusercontent.com/36811209/95040977-5b71fc00-0710-11eb-8479-35ba9e33afba.png)
1. Inkscapeなどで編集中のSVGファイルを"対象ファイル"で選択します。
1. "監視開始"ボタンを押下します。
1. 編集中のSVGファイルを上書き保存すると自動的に軽量化を行い"出力ファイル"に出力されます。

## プレビューウィンドウ
![image](https://user-images.githubusercontent.com/36811209/95041569-0800ad80-0712-11eb-93b9-d7f2665ed10f.png)

プレビューウインドウを開くと保存したファイルが表示されます。これを見ながらズレを修正するのも良いかも知れません。最全面にしたい場合は右下の"最前面に表示"ボタンを押下してください。枠のドラッグでウィンドウサイズの変更が可能です。

## サイズ表示ウィンドウ
![image](https://user-images.githubusercontent.com/36811209/95041665-5150fd00-0712-11eb-94fe-3f31309a0cb1.png)

サイズ表示ウィンドウを開くとこのような小さなウィンドウが開きます。マウスドラッグで任意の場所に移動してください。このウィンドウは最全面固定です。

----

# 設定ファイルについて
SvgoConfig.ymlを手で書き換えないでください。プログラム中で設定値を認識できなくなる可能性があります。
（YAMLを読み込んでSVGOをAPI呼び出ししようと思っていたのですが、めんどくさそうだったのでファイルを書き換えて渡してます…）

# SVGOについて
本プログラムは、SVGOをexe呼び出しで動作させています。

## exe化手順
exe化にはnexeを使用するので予めグローバルインストールしておいてください。
以下手順でexe化を行います。
1. SVGOリポジトリをクローンする
1. 任意バージョンのタグに移動する
1. ノードモジュールをダウンロードする（`npm ci -production`）
1. `nexe .\bin\svgo`を実行するとsvgo.exeが生成される

## 配置
SvgoAutoExe.exeと同じディレクトリにsvgoディレクトリを作成し、以下を入れてください。
* svgo.exe
* package.json
* .svgo.yml
* libディレクトリ
* node_modulesディレクトリ
* pluginsディレクトリ

# 環境
* Visual Studio 2019
* .NET Framework 4.6
* WPFアプリケーション
* SVGO 1.3.2

# メモ

## 通知について
トースト通知を出したかったが問題があったため断念

### 純正
時間指定ができない。
削除や更新をするにはインストーラの作成が必要。
```
FileInfo svgFileInfo = new FileInfo(OutputFilePath);
string message = String.Format("現在の容量: {0:#,0} Byte", svgFileInfo.Length);

var type = ToastTemplateType.ToastText01;
var content = ToastNotificationManager.GetTemplateContent(type);
var text = content.GetElementsByTagName("text").First();
text.AppendChild(content.CreateTextNode(message));
var notifier = ToastNotificationManager.CreateToastNotifier("Microsoft.Windows.Computer");
notifier.Show(new ToastNotification(content));
```

### Notifications.Wpf
https://github.com/Platonenkov/Notifications.Wpf
Inkscapeの操作ができなくなる（フォーカスがずれる？）
```
FileInfo svgFileInfo = new FileInfo(outputFilePath);
string message = String.Format("    現在の容量: {0:#,0} Byte", svgFileInfo.Length);
var notificationManager = new NotificationManager();
notificationManager.Show(new NotificationContent
{
    Title = "SVGOを実行しました",
    Message = message,
    Type = NotificationType.Information
});
```
