---
title: LogTransport2.exeのエラーを解決する
date: "2025-09-24"
---
# どんなエラー？
[Windows](https://www.microsoft.com/ja-jp/windows) シャットダウン時に`LogTransport2.exe`からアプリケーションエラーが発生します。

- 日本語
    ```text
    LogTransport2.exe - Application Error
    　×　アプリケーションを正しく終了できませんでした（0xc0000142）。[OK]をクリックしてアプリケーションを閉じてください。
    ```

- 英語
    ```text
    LogTransport2.exe - Application Error
    　×　The application was unable to start correctly (0xc0000142). Click OK to close the application.
    ```

普通にシャットダウンはできますが毎回出てくるのも邪魔なので解決していきます。

# `LogTransport2.exe`？
[Adobe](https://www.adobe.com/) に使用状況などのログを送信するためのプログラムのようです。[Adobe](https://www.adobe.com/) 製品を起動していなくても`LogTransport2.exe`だけは（何故か）起動されます。

[Windows](https://www.microsoft.com/ja-jp/windows) シャットダウン時に強制終了となり、アプリケーションエラーが発生しています。

# `LogTransport2.exe`のエラーを解決する
複数の方法があります。目的はどれも同じなのでお好みでどうぞ。（私は気持ちが悪いので全部やっています。）

## 設定を`オフ`にする
アプリケーション内の環境設定を探しがちですが、実は [Adobe Account](https://account.adobe.com/)（要ログイン, `ヘルプ(H) > アカウントを管理... `からも遷移可能）にあります。最近の [Adobe](https://www.adobe.com/) 製品はオンラインのアカウント情報と連携する作りになっているようです。

`プロフィールを編集 > データとプライバシー設定`の下記の設定を`オフ`にします。
- `アドビデスクトップアプリケーションの使用方法に関する情報を提供します。`
- `製品改善のために、アドビのサーバーで処理または保存されたコンテンツをアドビが分析することを許可します。`

## 実行ファイルをリネームする
`LogTransport2.exe`のファイル名を変えて直接呼び出せないようにします。

[Adobe Acrobat](https://www.adobe.com/jp/acrobat.html)（32bit版）以外の方は適宜パスを読み替えてください。
- `C:\Program Files (x86)\Adobe\Acrobat DC\Acrobat\LogTransport2.exe -> LogTransport2.exe.bak`（[補足1](#補足1)）

ただしアップデート時に問題や戻る可能性があるので注意が必要です。

### 補足1
リネームしたままが嫌な方はリネーム後に元のファイル名に戻すのも手です。エラーは再発しないはずです。
- `LogTransport2.exe.bak -> LogTransport2.exe`

## レジストリを編集する
アクセスできる範囲を制限して実行できないようにします。

下記を`右クリック > アクセス許可(P)... > SYSTEM > 削除(R)`します。（要`詳細設定(V) > 継承の無効化(I)`）
- `HKEY_CURRENT_USER\Software\Adobe\CommonFiles\UsageCC`

# ついでに
確証が得にくい話ですが、副次的な効果としてマウス操作のカクつき（途切れ）が減った気がします。他の方でも同様の報告を目にしました。（[参考文献](#参考文献)）

原因不明でマウス操作が安定しない時は`LogTransport2.exe`が悪さをしている可能性があります。どのような繋がりがあるのかは不明ですが試す価値はありそうです。

# 参考文献
- [Fix Logtransport2 Error on Shutdown in Windows 10](https://www.tecklyfe.com/fix-logtransport2-error-on-shutdown-in-windows-10/)
- [LogTransport2.exe Error on closing the computer](https://community.adobe.com/t5/acrobat-discussions/logtransport2-exe-error-on-closing-the-computer/td-p/11290093)
