---
title: Azure Marketplace での仮想マシン (VM) 認定のトラブルシューティング
description: Azure Marketplace での仮想マシン (VM) イメージのテストと認定に関連する一般的な問題のトラブルシューティングを行います。
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: troubleshooting
author: mathapli
ms.author: mathapli
ms.date: 01/18/2021
ms.openlocfilehash: adcd91d58b3bb5fde3ffa81c828c58d4b6db48d4
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/03/2021
ms.locfileid: "101721159"
---
# <a name="troubleshoot-virtual-machine-certification"></a>仮想マシンの認定に関するトラブルシューティング

Azure Marketplace に仮想マシン (VM) イメージを公開する際に、Azure チームはそれを検証して、それが起動可能であること、セキュリティで保護されていること、および Azure との互換性があることを確認します。 VM イメージは、高品質のテストのいずれかに失敗した場合には公開されません。 問題を説明するエラー メッセージが表示されます。

この記事では、VM イメージの発行中に発生する一般的なエラー メッセージと、関連する解決策について説明します。

> [!NOTE]
> この記事に関する質問または改善のための提案がある場合は、[パートナー センターのサポート](https://aka.ms/marketplacepublishersupport)までご連絡ください。

## <a name="vm-extension-failure"></a>VM 拡張機能のエラー

イメージによって VM 拡張機能がサポートされているかどうかを確認します。

VM 拡張機能を有効にするには:

1. Linux VM を選択します。
1. **[診断設定]** に移動します。
1. **ストレージ アカウント** を更新して、基本マトリックスを有効にします。
1. **[保存]** を選択します。

   ![ゲストレベルの監視を有効にする方法を示すスクリーンショット。](./media/create-vm/vm-certification-issues-solutions-1.png)

VM 拡張機能が正しくアクティブ化されていることを確認するには:

1. VM 内で **[VM 拡張機能]** タブを選択し、 **[Linux Diagnostics 拡張機能]** の状態を確認します。
1. プロビジョニング状態を確認します。

   - 状態が *[プロビジョニング成功]* であれば、拡張機能テスト ケースは合格しています。  
   - 状態が *[プロビジョニング失敗]* の場合、拡張機能テスト ケースは失敗しており、書き込まれたフラグを設定する必要があります。

   ![プロビジョニングが成功したことを示すスクリーンショット。](./media/create-vm/vm-certification-issues-solutions-2.png)

   VM 拡張機能が失敗した場合は、[[Linux Diagnostic Extension を使用して、メトリックとログを監視する]](../virtual-machines/extensions/diagnostics-linux.md) を参照して有効にします。 VM 拡張機能を有効にしない場合は、サポート チームに連絡して、それを無効にするように依頼してください。

## <a name="vm-provisioning-issue"></a>VM のプロビジョニングの問題

オファーを送信する前に、VM のプロビジョニング プロセスに従っていることを確認してください。 VM をプロビジョニングするための JSON 形式を確認するには、「[仮想マシンのイメージをテストする](azure-vm-image-test.md)」を参照してください。

プロビジョニングの問題としては、次のような失敗シナリオが考えられます。

|シナリオ|エラー|理由|解決策|
|---|---|---|---|
|1|無効な仮想ハード ディスク (VHD)|VHD フッターにある指定された Cookie の値が正しくない場合、VHD は無効と見なされます。|イメージを再作成して、要求を送信します。|
|2|無効な BLOB の種類|使用されたブロックがページの種類ではなく、BLOB の種類であるため、VM のプロビジョニングが失敗しました。|イメージを再作成して、要求を送信します。|
|3|プロビジョニングのタイムアウト、または正しく一般化されていない|VM の一般化に問題があります。|一般化を使用してイメージを再作成し、要求を送信します。|
|

> [!NOTE]
> VM の一般化の詳細については、以下を参照してください。
> - [Linux ドキュメント](azure-vm-create-using-approved-base.md#generalize-the-image)
> - [Windows ドキュメント](../virtual-machines/windows/capture-image-resource.md#generalize-the-windows-vm-using-sysprep)

## <a name="vhd-specifications"></a>VHD の仕様

### <a name="conectix-cookie-and-other-vhd-specifications"></a>Conectix Cookie とその他の VHD の仕様

'conectix' 文字列は VHD 仕様の一部です。 これは、ファイル作成者を識別する 8 バイトの Cookie として VHD フッターで定義されています。 Microsoft によって作成されたすべての VHD ファイルにこの Cookie があります。

VHD でフォーマットされた BLOB には、この形式の 512 バイトのフッターが必要です。

|ハード ディスク フッターのフィールド|サイズ (バイト)|
|---|---|
クッキー|8
特徴|4
File Format Version\(ファイル形式のバージョン\)|4
Data Offset\(データ オフセット\)|8
タイム スタンプ|4
Creator Application\(作成者のアプリケーション\)|4
Creator Version\(作成者のバージョン\)|4
Creator Host OS\(作成者のホスト OS\)|4
[元のサイズ]|8
現在のサイズ|8
Disk Geometry\(ディスク ジオメトリ\)|4
ディスクの種類|4
Checksum|4
一意の ID|16
Saved State\(保存された状態\)|1
予約されています。|427
|

### <a name="vhd-specifications"></a>VHD の仕様

スムーズな公開エクスペリエンスを実現するために、VHD が次の条件を満たしていることを確認してください。

- Cookie に 'conectix' という文字列が含まれている。
- ディスクの種類が固定である。
- VHD の仮想サイズが 20 MB 以上である。
- VHD がアラインされている。 仮想サイズは 1 MB の倍数である必要があります。
- VHD BLOB の長さは、仮想サイズに VHD フッターの長さ (512) を加えた値と等しくなります。

VHD の仕様は[こちら](https://www.microsoft.com/download/details.aspx?id=23850)からダウンロードしてください。

## <a name="software-compliance-for-windows"></a>Windows のソフトウェア コンプライアンス

ソフトウェア コンプライアンスの問題が原因で Windows イメージの要求が拒否された場合は、SQL Server インスタンスがインストールされた Windows イメージを作成した可能性があります。 そうではなく、Azure Marketplace から関連する SQL Server バージョンの基本イメージを取得する必要があります。

SQL Server がインストールされている独自の Windows イメージを作成しないでください。 Azure Marketplace からの承認された SQL Server 基本イメージ (Enterprise、Standard、Web) を使用してください。

Visual Studio、または Office のライセンスされた製品をインストールしようとしている場合は、事前承認についてサポート チームにお問い合わせください。

承認済みのベースの選択の詳細については、[承認済みのベースからの仮想マシンの作成](azure-vm-create-using-approved-base.md)に関する記事を参照してください。

## <a name="toolkit-test-case-execution-failed"></a>ツールキットのテスト ケースの実行に失敗した

Microsoft 認定ツールキットを使用すると、テスト ケースを実行し、VHD またはイメージに Azure 環境との互換性があることを確認できます。

[Microsoft 認定ツールキット](azure-vm-image-test.md)をダウンロードします。

### <a name="linux-test-cases"></a>Linux のテスト ケース

ツールキットで実行される Linux のテスト ケースの一覧を次の表に示します。 テストの検証は、説明に記載されています。

|シナリオ|テスト ケース|説明|
|---|---|---|
|1|Bash 履歴|VM イメージを作成する前に、Bash 履歴ファイルをクリアする必要があります。|
|2|Linux エージェントのバージョン|Azure Linux エージェント 2.2.41 以降がインストールされている必要があります。|
|3|必須のカーネル パラメーター|次のカーネル パラメーターが設定されていることを確認します。 <br>console=ttyS0<br>earlyprintk=ttyS0<br>rootdelay=300 |
|4|OS ディスク上のスワップ パーティション|OS ディスクにスワップ パーティションが作成されていないことを確認します。|
|5|OS ディスク上のルート パーティション|OS ディスクの単一のルート パーティションを作成します。|
|6|OpenSSL のバージョン|OpenSSL のバージョンは、0.9.8 以降である必要があります。|
|7|Python バージョン|Python バージョン 2.6 以降を強くお勧めします。|
|8|クライアント Alive 間隔|ClientAliveInterval を 180 に設定します。 アプリケーションで必要な場合は、30 から 235 の範囲で設定できます。 エンド ユーザーに対して SSH を有効にする場合は、説明に従ってこの値を設定する必要があります。|
|9|OS アーキテクチャ|64 ビットのオペレーティング システムのみがサポートされています。|
|10|自動更新|Linux エージェントの自動更新が有効になっているかどうかを特定します。|
|

### <a name="common-test-case-errors"></a>一般的なテストケース エラー

テスト ケースの実行時に発生する可能性がある一般的なエラーについては、次の表を参照してください。

| シナリオ | テスト ケース | エラー | 解決策 |
| --- | --- | --- | --- |
| 1 | Linux エージェント バージョンのテスト ケース | Linux エージェントの最小バージョンは 2.2.41 以降です。 この要件は、2020 年 5 月 1 日以降必須になりました。 | Linux エージェントのバージョンを更新してください。 2\.241 またはそれ以降である必要があります。 詳細については、[Linux エージェントのバージョン更新ページ](https://support.microsoft.com/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support)を参照してください。 |
| 2 | Bash 履歴テスト ケース | 送信したイメージ内の Bash 履歴のサイズが 1 キロバイト (KB) を超えていると、エラーが発生します。 Bash 履歴ファイルに潜在的な機密情報が含まれることのないようにするため、このサイズは 1 KB に制限されています。 | 別の作業用 VM に VHD をマウントし、サイズが 1 KB 以下に減少するように変更を加えて解決してください。 たとえば、`.bash` 履歴ファイルを削除します。 |
| 3 | 必要なカーネル パラメーターのテスト ケース | このエラーは、`console` の値が `ttyS0` に設定されていない場合に発生します。 次のコマンドを実行して確認します。 <br /> `cat /proc/cmdline` | `console` の値を `ttyS0` に設定し、要求を再送信します。 |
| 4 | ClientAlive 間隔のテスト ケース | ツールキットで、このテスト ケースに対して失敗の結果が返された場合は、`ClientAliveInterval` の値が不適切です。 | `ClientAliveInterval` の値を 235 以下に設定してから、要求を再送信してください。 |
|

### <a name="windows-test-cases"></a>Windows のテスト ケース

ツールキットで実行される Windows テスト ケースの一覧と、テストの検証の説明を次の表に示します。

|シナリオ |テスト ケース|説明|
|---|---|---|
|1|OS アーキテクチャ|Azure では 64 ビットのオペレーティング システムのみがサポートされています。|
|2|ユーザー アカウントの依存関係|アプリケーションの実行は、管理者アカウントに依存することはできません。|
|3|フェールオーバー クラスター|Windows Server のフェールオーバー クラスタリング機能はまだサポートされていません。 アプリケーションがこの機能に依存しないようにする必要があります。|
|4|IPV6|IPv6 は、Azure 環境ではまだサポートされていません。 アプリケーションがこの機能に依存しないようにする必要があります。|
|5|[DHCP]|動的ホスト構成プロトコルのサーバー ロールは、まだサポートされていません。 アプリケーションがこの機能に依存しないようにする必要があります。|
|6|Hyper-V|Hyper-V のサーバー ロールはまだサポートされていません。 アプリケーションがこの機能に依存しないようにする必要があります。|
|7|リモート アクセス|リモート アクセス (直接アクセス) のサーバー ロールはまだサポートされていません。 アプリケーションがこの機能に依存しないようにする必要があります。|
|8|Rights Management サービス|Rights Management サービス。 このサーバー ロールはまだサポートされていません。 アプリケーションがこの機能に依存しないようにする必要があります。|
|9|Windows 展開サービス|Windows 展開サービス。 このサーバー ロールはまだサポートされていません。 アプリケーションがこの機能に依存しないようにする必要があります。|
|10|BitLocker ドライブ暗号化|BitLocker ドライブ暗号化は、オペレーティング システムのハード ディスクではサポートされていませんが、データ ディスクでは使用できます。|
|11|インターネット記憶域ネーム サーバー|インターネット記憶域ネーム サーバー機能はまだサポートされていません。 アプリケーションがこの機能に依存しないようにする必要があります。|
|12|マルチパス I/O|マルチパス I/O。 このサーバー機能はまだサポートされていません。 アプリケーションがこの機能に依存しないようにする必要があります。|
|13|ネットワーク負荷分散|ネットワーク負荷分散。 このサーバー機能はまだサポートされていません。 アプリケーションがこの機能に依存しないようにする必要があります。|
|14|ピア名解決プロトコル|ピア名解決プロトコル。 このサーバー機能はまだサポートされていません。 アプリケーションがこの機能に依存しないようにする必要があります。|
|15|SNMP サービス|簡易ネットワーク管理プロトコル (SNMP) サービス機能は、まだサポートされていません。 アプリケーションがこの機能に依存しないようにする必要があります。|
|16|Windows インターネット ネーム サービス|Windows インターネット ネーム サービス。 このサーバー機能はまだサポートされていません。 アプリケーションがこの機能に依存しないようにする必要があります。|
|17|ワイヤレス LAN サービス|ワイヤレス LAN サービス。 このサーバー機能はまだサポートされていません。 アプリケーションがこの機能に依存しないようにする必要があります。|
|

前述のテスト ケースでエラーが発生した場合、解決策については、表の **[説明]** 列を参照してください。 詳細については、サポート チームにお問い合わせください。

## <a name="data-disk-size-verification"></a>データ ディスク サイズの確認

1023 ギガバイト (GB) を超えるサイズのデータ ディスク要求は承認されません。 このルールは、Linux と Windows の両方に適用されます。

1023 GB 以下のサイズで要求を再送信してください。

## <a name="os-disk-size-validation"></a>OS ディスク サイズの検証

OS ディスク サイズの制限については、次のルールを参照してください。 要求を送信するときに、OS ディスク サイズが Linux または Windows の制限内であることを確認します。

|OS|推奨される VHD サイズ|
|---|---|
|Linux|1 GB から 1023 GB|
|Windows|30 GB から 250 GB|
|

VM を使用すると基になるオペレーティング システムにアクセスできるため、VHD のサイズが VHD に対して十分な大きさであることを確認します。 ディスクはダウンタイムなしでは展開できません。 30 GB から 50 GB のディスク サイズを使用します。

|VHD サイズ|実際の占有サイズ|解決策|
|---|---|---|
|> 500 テビバイト (TiB)|該当なし|例外の承認については、サポート チームにお問い合わせください。|
|250-500 TiB|BLOB サイズとの差異が > 200 ギビバイト (GiB)|例外の承認については、サポート チームにお問い合わせください。|
|

> [!NOTE]
> ディスク サイズを大きくすると、コストが高くなり、セットアップとレプリケーションのプロセスで遅延が発生します。 この遅延とコストのために、サポート チームでは、例外の承認に対する正当な理由を求める場合があります。

## <a name="wannacry-patch-verification-test-for-windows"></a>Windows での WannaCry 修正プログラムの検証テスト

WannaCry ウイルスに関連する潜在的な攻撃を防ぐには、すべての Windows イメージ要求が最新の修正プログラムで更新されているようにします。

イメージ ファイルのバージョンは、`C:\windows\system32\drivers\srv.sys` または `srv2.sys` で確認できます。

次の表は、修正プログラムが適用された Windows Server の最小バージョンを示しています。

|OS|Version|
|---|---|
|Windows Serve 2008 R2|6.1.7601.23689|
|Windows Server 2012|6.2.9200.22099|
|Windows Server 2012 R2|6.3.9600.18604|
|Windows Server 2016|10.0.14393.953|
|Windows Server 2019|NA|
|

> [!NOTE]
> WindowsServer2019 には、必須のバージョン要件はありません。

## <a name="sack-vulnerability-patch-verification"></a>SACK 脆弱性修正プログラムの検証

Linux イメージを送信するときに、カーネル バージョンの問題のために要求が拒否されることがあります。

承認されたバージョンでカーネルを更新し、要求を再送信します。 次の表で、承認されたカーネルのバージョンを確認できます。 バージョン番号は、ここに示されている番号以上である必要があります。

次のいずれかのカーネル バージョンでイメージがインストールされていない場合は、適切な修正プログラムを使用してそれを更新してください。 次の必要な修正プログラムを使用してイメージを更新した後で、サポート チームに必要な承認を要求します。

- CVE-2019-11477
- CVE-2019-11478
- CVE-2019-11479

|OS ファミリ|Version|カーネル|
|---|---|---|
|Ubuntu|14.04 LTS|4.4.0-151| 
||14.04 LTS|4.15.0-1049-\*-azure|
||16.04 LTS|4.15.0-1049|
||18.04 LTS|4.18.0-1023|
||18.04 LTS|5.0.0-1025|
||18.10|4.18.0-1023|
||19.04|5.0.0-1010|
||19.04|5.3.0-1004|
|RHEL と Cent OS|6.10|2.6.32-754.15.3|
||7.2|3.10.0-327.79.2|
||7.3|3.10.0-514.66.2|
||7.4|3.10.0-693.50.3|
||7.5|3.10.0-862.34.2|
||7.6|3.10.0-957.21.3|
||7.7|3.10.0-1062.1.1|
||8.0|4.18.0-80.4.2|
||8.1|4.18.0-147|
||"7-RAW" (7.6)||
||"7-LVM" (7.6)|3.10.0-957.21.3|
||RHEL-SAP 7.4|TBD|
||RHEL-SAP 7.5|TBD|
|SLES|SLES11SP4 (SAP を含む)|3.0.101-108.95.2|
||SLES12SP1 for SAP|3.12.74-60.64.115.1|
||SLES12SP2 for SAP|4.4.121-92.114.1|
||SLES12SP3|4.4180-4.31.1 (kernel-azure)|
||SLES12SP3 for SAP|4.4.180-94.97.1|
||SLES12SP4|4.12.14-6.15.2 (kernel-azure)|
||SLES12SP4 for SAP|4.12.14-95.19.1|
||SLES15|4.12.14-5.30.1 (kernel-azure)|
||SLES15 for SAP|4.12.14-5.30.1 (kernel-azure)|
||SLES15SP1|4.12.14-5.30.1 (kernel-azure)|
|Oracle|6.10|UEK2 2.6.39-400.312.2<br>UEK3 3.8.13-118.35.2<br>RHCK 2.6.32-754.15.3 
||7.0-7.5|UEK3 3.8.13-118.35.2<br>UEK4 4.1.12-124.28.3<br>RHCK は上記の RHEL に従う|
||7.6|RHCK 3.10.0-957.21.3<br>UEK5 4.14.35-1902.2.0|
|CoreOS Stable 2079.6.0|4.19.43\*|
||Beta 2135.3.1|4.19.50\*|
||Alpha 2163.2.1|4.19.50\*|
|Debian|jessie (security)|3.16.68-2|
||jessie backports|4.9.168-1+deb9u3|
||stretch (security)|4.9.168-1+deb9u3|
||Debian GNU/Linux 10 (buster)|Debian 6.3.0-18+deb9u1|
||buster、sid (stretch backports)|4.19.37-5|
|

## <a name="image-size-should-be-in-multiples-of-megabytes"></a>イメージのサイズはメガバイトの倍数である必要がある

Azure 上のすべての VHD の仮想サイズは、1 メガバイト (MB) の倍数に調整されている必要があります。 VHD が推奨される仮想サイズに従っていない場合、要求が拒否される可能性があります。

Raw ディスクから VHD に変換するときのガイドラインに従ってください。 Raw ディスクのサイズが 1 MB の倍数であることを確認します。 詳細については、「[動作保証外のディストリビューションに関する情報](../virtual-machines/linux/create-upload-generic.md)」を参照してください。

## <a name="vm-access-denied"></a>VM アクセスが拒否された

VM でのテスト ケース実行に関する "_アクセス拒否_" の問題の原因として、十分な権限がないことが考えられます。

セルフテスト ケースが実行されているアカウントに対して適切なアクセスを有効にしていることを確認します。 テスト ケースを実行するためのアクセスを有効にします (有効になっていない場合)。 アクセスを有効にしない場合は、セルフテスト ケースの結果をサポート チームと共有することができます。

SSH が無効になっているイメージを使用して認定プロセスの要求を送信するには:

1. イメージに対して [Azure VM を対象にした最新の認定テスト ツール](https://aka.ms/AzureCertificationTestTool)を実行します。

2. [サポート チケット](https://aka.ms/marketplacepublishersupport)を作成します。 必ずツールキット レポートを添付し、オファーの詳細を提供してください。
   - プラン名
   - 発行元の名前
   - プラン ID、SKU、バージョン

3. 認定要求を再送信します。

## <a name="download-failure"></a>ダウンロードの失敗

共有アクセス署名 (SAS) URL を使用して VM イメージをダウンロードするときに発生する問題については、次の表を参照してください。

|エラー|理由|解決策|
|---|---|---|
|見つからない BLOB|VHD が削除されているか、指定された場所から移動されている可能性があります。|| 
|BLOB が使用中|VHD は別の内部プロセスによって使用されています。|SAS URL を使用してダウンロードする場合、VHD は使用済みの状態である必要があります。|
|無効な SAS URL|その VHD に関連付けられている SAS URL は正しくありません。|正しい SAS URL を取得してください。|
|無効な署名|その VHD に関連付けられている SAS URL は正しくありません。|正しい SAS URL を取得してください。|
|HTTP 条件ヘッダー|SASL URL が無効です。|正しい SAS URL を取得してください。|
|無効な VHD 名|VHD 名にパーセント記号 (`%`) や引用符 (`"`) などの特殊文字が含まれているかどうかを確認します。|特殊文字を削除して VHD ファイルの名前を変更します。|
|

## <a name="first-partition-starts-at-1-mb-2048-sectors"></a>最初のパーティションは 1 MB (2048 セクター) で開始する

[独自のイメージを構築](azure-vm-create-using-own-image.md)する場合は、OS ディスクの最初の 2048 セクター (1 MB) が空であることを確認します。 これを行わないと、発行は失敗します。 この要件は、(データ ディスクではなく) OS ディスクにのみ適用されます。 [承認済みのベースから](azure-vm-create-using-approved-base.md)イメージを構築する場合は、この要件をスキップしてかまいません。

### <a name="create-a-1-mb-2048-sectors-each-sector-of-512-bytes-partition-on-an-empty-vhd"></a>空の VHD に 1 MB (2048 セクターで、各セクター 512 バイト) のパーティションを作成する

これらの手順は Linux のみに適用されます。

1. Ubuntu、Cent OS など、任意の種類の Linux VM を作成します。 必須フィールドに入力し、 **[Next: Disks >]\(次へ: ディスク >\)** をクリックします。

   ![仮想マシンの作成ページを示すスクリーンショット。[Next: Disks]\(次へ: ディスク\) コマンド ボタンが強調表示されています。](./media/create-vm/vm-certification-issues-solutions-15.png)

1. VM 用にアンマネージド ディスクを作成します。

   既定値を使用することも、NIC、NSG、パブリック IP などのフィールドに任意の値を指定することもできます。

   ![仮想マシン作成フローの [データ ディスク] ページのスクリーンショット画像。](./media/create-vm/vm-certification-issues-solutions-16.png)

1. VM 作成後、左側のペインで **[ディスク]** を選択します。

   ![VM 用にディスクを選択する方法を示すスクリーンショット。](./media/create-vm/vm-certification-issues-solutions-17.png)

1. パーティション テーブルを作成するために、VHD をデータ ディスクとして VM にアタッチします。

   1. **[Add DataDisk]\(DataDisk の追加\)**  >  **[Existing Blob]\(既存の BLOB\)** を選択します。

      ![VHD にデータ ディスクを追加する方法を示すスクリーンショット。](./media/create-vm/vm-certification-issues-solutions-18.png)

   1. VHD ストレージ アカウントを検索します。
   1. **[コンテナー]** を選択し、VHD を選択します。
   1. **[OK]** を選択します。

      ![アンマネージド ディスクのアタッチ ページのスクリーンショット。](./media/create-vm/vm-certification-issues-solutions-19.png)

      VHD がデータ ディスク LUN 0 として追加されます。

   1. VM を再起動します。

1. VM を再起動した後、Putty または別のクライアントを使用して VM にログインし、`sudo  -i` コマンドを実行してルート アクセスを取得します。

   ![sudo -i コマンドを示す Putty クライアント コマンド ラインのスクリーンショット。](./media/create-vm/vm-certification-issues-solutions-20.png)

1. VHD にパーティションを作成します。

   1. `fdisk /dev/sdb` コマンドを入力します。
   1. VHD の既存パーティション一覧を表示するには、「`p`」と入力します。
   1. 「`d`」を入力して、VHD 内の使用可能なすべての既存パーティションを削除します。 この手順は必要なければスキップできます。

      ![Putty クライアント コマンド ラインのスクリーンショット。既存のパーティションを削除するためのコマンドを示しています。](./media/create-vm/vm-certification-issues-solutions-21.png)

   1. 新しいパーティションを作成するために「`n`」と入力し、`p` (プライマリ パーティションを表す) を選択します。

   1. _first sector_ の値として「2048」を入力します。 _last sector_ は既定値のままにすることができます。

      >[!IMPORTANT]
      >既存のデータは 2048 セクター (各セクター 512 バイト) まで消去されます。 新しいパーティションを作成する前に、VHD をバックアップしてください。

      ![Putty クライアント コマンド ラインのスクリーンショット。消去されたデータのコマンドと出力を示しています。](./media/create-vm/vm-certification-issues-solutions-22.png)

   1. 「`w`」を入力して、パーティションの作成を確認します。

      ![Putty クライアント コマンド ラインのスクリーンショット。パーティションを作成するためのコマンドを示しています。](./media/create-vm/vm-certification-issues-solutions-23.png)

   1. コマンド `n fdisk /dev/sdb` を実行して「`p`」を入力することで、パーティション テーブルを検証できます。 パーティションがオフセット値 2048 で作成されていることがわかります。

      ![Putty クライアント コマンド ラインのスクリーンショット。2048 のオフセットを作成するためのコマンドを示しています。](./media/create-vm/vm-certification-issues-solutions-24.png)

1. VM から VHD をデタッチし、VM を削除します。

### <a name="create-a-1-mb-2048-sectors-each-sector-of-512-bytes-partition-by-moving-existing-data-on-vhd"></a>VHD 上の既存のデータを移動して、1 MB (2048 セクターで、各セクター 512 バイト) のパーティションを作成する

これらの手順は Linux のみに適用されます。

1. Ubuntu、Cent OS など、任意の種類の Linux VM を作成します。 必須フィールドに入力し、 **[Next: Disks >]\(次へ: ディスク >\)** をクリックします。

   ![仮想マシンの作成ページを示すスクリーンショット。[Next: Disks]\(次へ: ディスク\) コマンド ボタンが強調表示されています。](./media/create-vm/vm-certification-issues-solutions-15.png)

1. VM 用にアンマネージド ディスクを作成します。

   ![仮想マシン作成フローの [データ ディスク] ページのスクリーンショット画像。](./media/create-vm/vm-certification-issues-solutions-16.png)

   既定値を使用することも、NIC、NSG、パブリック IP などのフィールドに任意の値を指定することもできます。

1. VM 作成後、左側のペインで **[ディスク]** を選択します。

   ![VM 用にディスクを選択する方法を示すスクリーンショット。](./media/create-vm/vm-certification-issues-solutions-17.png)

1. パーティション テーブルを作成するために、VHD をデータ ディスクとして VM にアタッチします。

   1. パーティション テーブルを作成するために、VHD をデータ ディスクとして VM にアタッチします。

   1. **[Add DataDisk]\(DataDisk の追加\)**  >  **[Existing Blob]\(既存の BLOB\)** を選択します。

      ![VHD にデータ ディスクを追加する方法を示すスクリーンショット。](./media/create-vm/vm-certification-issues-solutions-18.png)

   1. VHD ストレージ アカウントを検索します。
   1. **[コンテナー]** を選択し、VHD を選択します。
   1. **[OK]** を選択します。

      ![アンマネージド ディスクのアタッチ ページのスクリーンショット。](./media/create-vm/vm-certification-issues-solutions-19.png)

      VHD がデータ ディスク LUN 0 として追加されます。

   1. VM を再起動します。

1. Putty または別のクライアントを使用して VM にログインし、`sudo  -i` コマンドを実行してルート アクセスを取得します。

   ![Putty クライアント コマンド ラインのスクリーンショット。ログインと sudo -i コマンドを示しています。](./media/create-vm/vm-certification-issues-solutions-20.png)

1. コマンド `echo '+1M,' | sfdisk --move-data /dev/sdc -N 1` を実行します。

   ![Putty クライアント コマンド ラインのスクリーンショット。コマンドの実行を示しています。](./media/create-vm/vm-certification-issues-solutions-25.png)

   >[!NOTE]
   >ディスクのサイズによっては、このコマンドが完了するまでに時間がかかる場合があります。

1. VM から VHD をデタッチし、VM を削除します。


## <a name="default-credentials"></a>既定の資格情報

送信される VHD と共に既定の資格情報を送信しないでください。 既定の資格情報を追加すると、VHD がセキュリティ上の脅威に対してより脆弱になります。 代わりに、VHD を送信するときには独自の資格情報を作成します。
  
## <a name="datadisk-mapped-incorrectly"></a>DataDisk が正しくマップされていない

順序どおりになっていない複数のデータ ディスクを使用して要求が送信されると、マッピングの問題が発生する可能性があります。 たとえば、3 つのデータ ディスクの番号付け順序は *0、1、2* である必要があります。 その他の順序は、マッピングの問題として扱われます。

データ ディスクの適切なシーケンス処理を使用して要求を再送信してください。

## <a name="incorrect-os-mapping"></a>正しくない OS マッピング

イメージが作成されるときに、間違った OS ラベルにマップされたり割り当てられたりすることがあります。 たとえば、イメージの作成中に OS 名の一部として **Windows** を選択した場合、その OS ディスクのインストールは、Windows を使用してのみ行う必要があります。 Linux にも同じ要件が適用されます。

## <a name="vm-not-generalized"></a>VM が一般化されていない

Azure Marketplace から取得したすべてのイメージを再利用する場合は、オペレーティング システムの VHD を一般化する必要があります。

- **Linux** の場合、次のプロセスでは、Linux VM を一般化して別の VM として再デプロイします。

  SSH ウィンドウで、コマンド `sudo waagent -deprovision+user` を入力します。

- **Windows** の場合、`sysreptool` を使用して Windows イメージを一般化します。

  `sysreptool` ツールの詳細については、「[Sysprep (システム準備) の概要](/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview)」を参照してください。

## <a name="datadisk-errors"></a>DataDisk エラー

データ ディスクに関連するエラーの解決策については、次の表を参照してください。

|エラー|理由|解決策|
|---|---|---|
|`DataDisk- InvalidUrl:`|このエラーは、オファー送信時に論理ユニット番号 (LUN) が無効なことが原因で発生する可能性があります。|データ ディスクの LUN 番号シーケンスがパートナー センターにあることを確認します。|
|`DataDisk- NotFound:`|このエラーは、指定された SAS URL にデータ ディスクがないことが原因で発生する可能性があります。|データ ディスクが、指定されている SAS URL にあることを確認します。|

## <a name="remote-access-issue"></a>リモート アクセスの問題

このエラーが発生するのは、リモート デスクトップ プロトコル (RDP) オプションが Windows イメージに対して有効になっていない場合です。

送信する前に、Windows イメージに対して RDP アクセスを有効にします。

## <a name="bash-history-failed"></a>Bash 履歴が失敗した

このエラーは、送信したイメージ内の Bash 履歴のサイズが 1 キロバイト (KB) を超えた場合に表示されます。 潜在的な機密情報がファイルに含まれないように制限するために、サイズは 1 KB に制限されています。

Bash 履歴を削除するには:

1. VM をデプロイし、Azure portal の **[実行コマンド]** オプションを選択します。

   ![左側のペインに [実行コマンド] オプションがある Azure portal のスクリーンショット。](./media/create-vm/vm-certification-issues-solutions-3.png)

1. 最初のオプション **[RunShellScript]** を選択し、`cat /dev/null > ~/.bash_history && history -c` コマンドを実行します。

   ![Azure portal の [スクリプト コマンドの実行] ページのスクリーンショット。](./media/create-vm/vm-certification-issues-solutions-4.png)

1. コマンドが正常に実行されたら、VM を再起動します。

1. VM を汎用化し、イメージ VHD を取得して VM を停止します。

1. 汎用化されたイメージを再送信します。

## <a name="request-an-exception-on-vm-images-for-select-tests"></a>一部のテストで VM イメージに例外を要求する

公開元は、VM の認定時に実行されるいくつかのテストに例外を求めることができます。 例外は、その要求をサポートする証拠を公開元が提出したときに、まれなケースで提供されます。 認定チームは、常に、例外を拒否または承認する権利を留保します。

このセクションでは、公開元が例外を要求する一般的なシナリオと、その要求方法について説明します。

### <a name="scenarios-for-exception"></a>例外のシナリオ

公開元は通常、次の場合に例外を要求します。

- **1 つ以上のテスト ケースの例外**。 テスト ケースの例外を要求するには、[パートナー センターのサポート](https://aka.ms/marketplacepublishersupport)に連絡してください。

- **ロックダウンされた VM、ルート アクセスなし**。 一部の公開元には、VM にインストールされたファイアウォールなどのソフトウェアが含まれているために、その VM をロックすることが必要なシナリオがあります。 この場合は、[認定テスト ツール](https://aka.ms/AzureCertificationTestTool)をダウンロードし、[パートナー センターのサポート](https://aka.ms/marketplacepublishersupport)でレポートを送信してください。

- **カスタム テンプレート**。 一部の公開元が公開する VM イメージには、VM をデプロイするためのカスタム Azure Resource Manager (ARM) テンプレートが必要です。 この場合は、認定チームが検証で使用できるように、[パートナー センターのサポート](https://aka.ms/marketplacepublishersupport)でカスタム テンプレートを送信してください。

### <a name="information-to-provide-for-exception-scenarios"></a>例外のシナリオのために提供する情報

いずれかのシナリオで例外を要求するには、[パートナー センターのサポート](https://aka.ms/marketplacepublishersupport)に連絡し、次の情報を含めてください。

- **公開元 ID**。 パートナー センター ポータルの公開元 ID を入力します。
- **オファー ID または名前**。 オファーの ID または名前を入力します。
- **SKU またはプラン ID**。 VM オファーのプラン ID または SKU を入力します。
- **バージョン**。 例外を必要とする VM オファーのバージョンを入力します。
- **例外の種類**。 テスト、ロックダウンされた VM、カスタム テンプレートから選択します。
- **要求の理由**。 例外要求の理由と、テストの除外に関する情報を含めます。
- **タイムライン**。 例外の終了日を入力します。
- **添付ファイル**。 添付された重要な証拠ドキュメント:

  - ロックダウンされた VM の場合は、テスト レポートを添付します。
  - カスタム テンプレートの場合は、カスタム ARM テンプレートを添付ファイルとして提供します。

  これらの添付ファイルを含めなかった場合、要求は拒否されます。

## <a name="address-a-vulnerability-or-an-exploit-in-a-vm-offer"></a>VM オファーの脆弱性または悪用に対処する

このセクションでは、自分の VM イメージの 1 つで脆弱性または悪用が検出された場合に、新しい VM イメージを提供する方法を説明します。 Azure Marketplace に公開される Azure VM オファーのみに適用されます。

> [!NOTE]
> プランからは最後の VM イメージを削除したり、オファーの最後のプランを販売停止にしたりすることはできません。

次のいずれかのアクションを実行します。

- 脆弱性がある VM イメージの代わりとなる新しい VM イメージがある場合は、「[修正した VM イメージを提供する](#provide-a-fixed-vm-image)」を参照してください。
- プラン唯一の VM イメージの代わりとなる新しい VM イメージがない場合、またはそのプランを終了させてもよい場合は、[そのプランの販売を停止](partner-center-portal/update-existing-offer.md#stop-selling-an-offer-or-plan)してください。
- オファー唯一の VM イメージを置き換える予定がない場合は、[そのオファーの販売を停止](partner-center-portal/update-existing-offer.md#stop-selling-an-offer-or-plan)することをお勧めします。

### <a name="provide-a-fixed-vm-image"></a>修正した VM イメージを提供する

脆弱性がある、または悪用された VM イメージの代わりとなる修正した VM イメージを提供するには:

1. セキュリティの脆弱性または悪用に対処した新しい VM イメージを提供します。
1. セキュリティの脆弱性があるまたは悪用された VM イメージを削除します。
1. オファーを再公開します。

#### <a name="provide-a-new-vm-image-to-address-the-security-vulnerability-or-exploit"></a>セキュリティの脆弱性または悪用に対処した新しい VM イメージを提供する

これらの手順を完了するには、追加する VM イメージの技術資産を準備します。 詳細については、[承認済みベースを使用した仮想マシンの作成](azure-vm-create-using-approved-base.md)または[独自イメージを使用した仮想マシンの作成](azure-vm-create-using-own-image.md)に関するページ、および [VM イメージの SAS URI の生成](azure-vm-get-sas-uri.md)に関するページを参照してください。

1. [パートナー センター](https://partner.microsoft.com/dashboard/home)にサインインします。
1. 左側のペインで、 **[コマーシャル マーケットプレース]**  >  **[概要]** を選択します。
1. **[オファーのエイリアス]** 列で、オファーを選択します。
1. **[プランの概要]** タブの **[名前]** 列で、適切なプランを選択します。
1. **[技術的な構成]** タブの **[VM イメージ]** で、 **[+ VM イメージの追加]** を選択します。

   > [!NOTE]
   > プランには一度に 1 つの VM イメージのみ追加できます。 複数の VM イメージを追加するには、最初のものを公開してから、次の VM イメージを追加します。

1. 表示されるボックスに、新しいディスクのバージョンと仮想マシン イメージを指定します。
1. **[下書きの保存]** を選択します。

次に、セキュリティの脆弱性がある VM イメージを削除します。

#### <a name="remove-the-vm-image-with-the-security-vulnerability-or-exploit"></a>セキュリティの脆弱性があるまたは悪用された VM イメージを削除する

1. [パートナー センター](https://partner.microsoft.com/dashboard/home)にサインインします。
2. 左側のペインで、 **[コマーシャル マーケットプレース]**  >  **[概要]** を選択します。
3. **[オファーのエイリアス]** 列で、オファーを選択します。
4. **[プランの概要]** タブの **[名前]** 列で、適切なプランを選択します。
5. **[技術的な構成]** タブの **[VM イメージ]** の下の削除する VM イメージの横で **[VM イメージの削除]** を選択します。
6. ダイアログ ボックスで **[続行]** を選択します。
7. **[下書きの保存]** を選択します。

次に、オファーを再公開します。

#### <a name="republish-the-offer"></a>オファーを再公開する

1. **[レビューと公開]** を選択します。
2. 認定チームに何らかの情報を提供する必要がある場合は、それを **[認定の注意書き]** ボックスに追加します。
3. **[発行]** を選択します。

公開のプロセスを完了するには、[オファーのレビューと公開](review-publish-offer.md)に関するページを参照してください。

### <a name="vm-images-with-limited-access-or-requiring-custom-templates"></a>アクセスが制限されているかカスタム テンプレートが必要な VM イメージ

#### <a name="locked-down-or-ssh-disabled-offer"></a>ロック ダウン (または) SSH 無効のオファー

  SSH 無効 (Linux 用) または RDP 無効 (Windows 用) で公開されているイメージは、ロック ダウンされた VM として扱われます。 公開元がアクセスを許可しない、または少数のユーザーにのみ制限付きアクセスを許可する、特殊なビジネス シナリオが存在します。 検証チェック中に、ロック ダウンされた VM では、特定の認証コマンドの実行を許可しない場合があります。


#### <a name="custom-templates"></a>カスタム テンプレート

   一般に、単一の VM オファーの下で公開されているすべてのイメージのデプロイは、標準の ARM テンプレートに従います。 ただし、公開元が VM をデプロイする際にカスタマイズが必要になる場合があります (複数の NIC を構成する必要があるなど)。
    
   以下のシナリオによっては、公開元は VM のデプロイにカスタム テンプレートを使用します (ただし、これらに限定されません)。

   * VM に追加のネットワーク サブネットが必要である。
   * ARM テンプレートに追加のメタデータが挿入される。
   * ARM テンプレートの実行の前提条件となるコマンド。

### <a name="vm-extensions"></a>VM 拡張機能   

   Azure 仮想マシン (VM) 拡張機能は、Azure VM でのデプロイ後の構成と自動タスクを提供する複数の小さなアプリケーションです。 たとえば、仮想マシンでソフトウェアのインストールやウイルス対策保護が必要な場合、あるいは、仮想マシン内でスクリプトを実行するために、VM 拡張機能を使用できます。 

   Linux VM 拡張機能の検証には、次のものがイメージの一部である必要があります。
* 2\.2.41 以上の Azure Linux エージェント
* バージョン 2.8 以上の Python 


詳細については、[VM 拡張機能](https://docs.microsoft.com/azure/virtual-machines/extensions/diagnostics-linux)に関するページをご覧ください。
     
## <a name="next-steps"></a>次の手順

- [VM オファーのプロパティを構成する](azure-vm-create-properties.md)
- [マーケットプレースのアクティブな報奨](partner-center-portal/marketplace-rewards.md)
- 改善に関するご質問またはご意見につきましては、[パートナー センターのサポート](https://aka.ms/marketplacepublishersupport)までご連絡ください。
 
