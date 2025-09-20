---
title: GEARAspiWDM.sysを削除してコア分離を有効にする
date: "2025-09-20"
---
# `GEARAspiWDM.sys`？
[Windows](https://microsoft.com/ja-jp/windows/) におけるディスクドライブのドライバです。古めのソフトウェアと依存関係にあるようです。

例えば [Studio One 4 Artist OEM](https://my.presonus.com/products/detail/590) をインストールすると`GEARAspiWDM.sys`も一緒にインストールされます。

# `GEARAspiWDM.sys`の何が問題？
[Windows](https://microsoft.com/ja-jp/windows/) のセキュリティを高める上で [コア分離](https://support.microsoft.com/ja-jp/windows/windows-セキュリティ-アプリのデバイス-セキュリティ-afa11526-de57-b1c5-599f-3a4c6a61c5e2#bkmk_coreisolation) という機能があります。`GEARAspiWDM.sys`はこの機能と競合関係にあります。

要するに`GEARAspiWDM.sys`を削除しないと [コア分離](https://support.microsoft.com/ja-jp/windows/windows-セキュリティ-アプリのデバイス-セキュリティ-afa11526-de57-b1c5-599f-3a4c6a61c5e2#bkmk_coreisolation) を有効にできません。（[注記1](#注記1)）

## 注記1
`GEARAspiWDM.sys`を（例えば [Studio One 4 Artist OEM](https://my.presonus.com/products/detail/590) ごと）アンインストールした状態で [コア分離](https://support.microsoft.com/ja-jp/windows/windows-セキュリティ-アプリのデバイス-セキュリティ-afa11526-de57-b1c5-599f-3a4c6a61c5e2#bkmk_coreisolation) を有効した後に再インストールすると [コア分離](https://support.microsoft.com/ja-jp/windows/windows-セキュリティ-アプリのデバイス-セキュリティ-afa11526-de57-b1c5-599f-3a4c6a61c5e2#bkmk_coreisolation) の有効を維持できます。

その場合`GEARAspiWDM.sys`が読み込めない旨の警告が [Windows](https://microsoft.com/ja-jp/windows/) 起動時に通知されるようになります。再度通知させないようにもできます。

ただ注意点として、そのような共存を目指すと [コア分離](https://support.microsoft.com/ja-jp/windows/windows-セキュリティ-アプリのデバイス-セキュリティ-afa11526-de57-b1c5-599f-3a4c6a61c5e2#bkmk_coreisolation) が有効の場合限り [Windows](https://microsoft.com/ja-jp/windows/) が`GEARAspiWDM.sys`を使用しないように調整をしなければなりません。そうしないとディスクドライブが常に認識しなくなります。

要するに [コア分離](https://support.microsoft.com/ja-jp/windows/windows-セキュリティ-アプリのデバイス-セキュリティ-afa11526-de57-b1c5-599f-3a4c6a61c5e2#bkmk_coreisolation) の有効時に`GEARAspiWDM.sys`が読み込めないのでエラーになるということです。（`デバイス マネージャー > DVD/CD-ROM ドライブ > あなたのディスクドライブ名 > イベント > すべてのイベントの表示(V)`などで詳細なエラーが確認できます。）

本記事では共存の方針は取りませんのでご承知おきください。

# 方針を決める
前述の通り、いいとこ取りができません。方針を決めましょう。
1. [コア分離](https://support.microsoft.com/ja-jp/windows/windows-セキュリティ-アプリのデバイス-セキュリティ-afa11526-de57-b1c5-599f-3a4c6a61c5e2#bkmk_coreisolation) を進める。（`GEARAspiWDM.sys`を削除する。）
2. [コア分離](https://support.microsoft.com/ja-jp/windows/windows-セキュリティ-アプリのデバイス-セキュリティ-afa11526-de57-b1c5-599f-3a4c6a61c5e2#bkmk_coreisolation) を諦める。（`GEARAspiWDM.sys`を削除しない。）

以降は 1 の（[コア分離](https://support.microsoft.com/ja-jp/windows/windows-セキュリティ-アプリのデバイス-セキュリティ-afa11526-de57-b1c5-599f-3a4c6a61c5e2#bkmk_coreisolation) を諦めたくない）方向けの内容になります。②の方はお疲れ様でした。

# 方針を決める（補足）
`GEARAspiWDM.sys`と依存関係にあるソフトウェアを使わないのが一番楽です。丸ごとアンインストールしちゃいましょう。

そうもいかない方は「そのソフトウェアでディスクドライブが使えない制約を受ける」ことになります。

# ゴールの定義
- `デバイス マネージャー > DVD/CD-ROM ドライブ > あなたのディスクドライブ名 > デバイスの状態`が`このデバイスは正常に動作しています。`になっている。
- `デバイス マネージャー > DVD/CD-ROM ドライブ > あなたのディスクドライブ名 > ドライバー > ドライバーの詳細 > ドライバー ファイル(D)`が`C:\Windows\System32\drivers\cdrom.sys`になっている。

`cdrom.sys`は [Windows](https://microsoft.com/ja-jp/windows/) の標準ドライバで [コア分離](https://support.microsoft.com/ja-jp/windows/windows-セキュリティ-アプリのデバイス-セキュリティ-afa11526-de57-b1c5-599f-3a4c6a61c5e2#bkmk_coreisolation) と競合しません。本記事ではこの標準状態をゴールとします。

# `GEARAspiWDM.sys`を削除してコア分離を有効にする（要管理者権限）
サービスから削除します。（サービスが実行されていない又は登録されていない場合は手順をスキップしてください。）
```cmd
sc stop GEARAspiWDM
sc delete GEARAspiWDM
```

レジストリから削除します。（レジストリに存在しなければ手順をスキップしてください。）
- `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\GEARAspiWDM`
- `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Class\{4d36e965-e325-11ce-bfc1-08002be10318}\UpperFilters`  
  `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Class\{4d36e965-e325-11ce-bfc1-08002be10318}\LowerFilters`

ファイルを削除します。（削除できない場合は再起動後にリトライしてください。）
- `C:\Windows\System32\drivers\GEARAspiWDM.sys`

デバイスを削除します。その後は再起動をします。
- `デバイス マネージャー > DVD/CD-ROM ドライブ > あなたのディスクドライブ名 > 右クリック > デバイスのアンインストール(U)`

[コア分離](https://support.microsoft.com/ja-jp/windows/windows-セキュリティ-アプリのデバイス-セキュリティ-afa11526-de57-b1c5-599f-3a4c6a61c5e2#bkmk_coreisolation) を有効にします。その後は再起動をします。
- `設定 > プライバシーとセキュリティ > Windows セキュリティ > デバイス セキュリティ > コア分離 > オン`

[ゴールの定義](#ゴールの定義) の通りになっていれば成功です。お疲れ様でした。
