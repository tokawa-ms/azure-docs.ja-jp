---
title: フォルダー構造を Azure File Sync トポロジにマップする
description: Azure File Sync で使用するために既存のファイルおよびフォルダー構造を Azure ファイル共有にマップします。移行ドキュメント間で共有される一般的なテキスト ブロック。
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: cde85e245c8cc6ae8c55b24270f125bacc111737
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2021
ms.locfileid: "102547554"
---
```console
Robocopy /MT:16 /NP /NFL /NDL /B /MIR /IT /COPY:DATSO /COPY:DATSO /DCOPY:DAT /UNILOG:<FilePathAndName> <SourcePath> <Dest.Path> 
```

| Switch              | 説明 |
|---------------------|---------|
| /MT                 | RoboCopy でマルチスレッドを実行できるようにします。 既定値は 8、最大値は 128 です。 この値をプロセッサ コア数とコアあたりのスレッド数に一致させます。 実稼働サーバーに存在する可能性のある他のタスク用にコアを予約する必要があるかどうかを検討してください。 |
| /NP                 | 各ファイルとフォルダーのコピーの進行状況は表示されません。 進行状況を表示すると、コピーのパフォーマンスが大幅に低下します。 |
| /NFL                | ファイル名をログに記録しないことを指定します。 コピーのパフォーマンスを向上させます。 |
| /NDL                | ディレクトリ名をログに記録しないことを指定します。 コピーのパフォーマンスを向上させます。 |
| /B                  | バックアップ アプリケーションが使用するのと同じモードで RoboCopy を実行します。 現在のユーザーがアクセス許可を持っていないファイルを、RoboCopy によって移動できます。 |
| /MIR                | "*ソースをターゲットにミラーリング*" すると、RoboCopy では、ソースとターゲット間の差分をコピーするだけですみます。 空のサブディレクトリがコピーされます。 変更された、またはターゲットに存在しない項目 (ファイルまたはフォルダー) がコピーされます。 ターゲットに存在する一方でソースには存在しない項目は、ターゲットから消去 (削除) されます。 このスイッチを使用する場合は、ソースとターゲットのフォルダー構造を正確に一致させる必要があります。 "一致" とは、正しいソースおよびフォルダー レベルから、ターゲットの一致するフォルダー レベルにコピーすることを意味します。 その場合にのみ、"キャッチ アップ" コピーを正常に実行することができます。 ソースとターゲットが一致しない場合、`/MIR` を使用すると、大規模な削除と再コピーが行われます。 |
| /IT                 | 特定のミラー シナリオで、忠実性が維持されることを保証します。 </br>"*例*" - たとえば、Robocopy を 2 回実行する間に、ファイルで ACL の変更と属性の更新があった場合、"*非表示*" とマークされます。 /IT を使用しない場合、ACL の変更は Robocopy で見逃され、ターゲットの場所に転送されない可能性があります。 |
|/COPY:`[copyflags]`  | ファイル コピーの忠実性 (指定されていない場合の既定値は `/COPY:DAT`)、コピー フラグ: `D` = データ、`A` = 属性、`T` = タイムスタンプ、`S` = セキュリティ = NTFS ACL、`O` = 所有者情報、`U` = 監査情報。<u></u> 監査情報を Azure ファイル共有に格納することはできません。 |
| /DCOPY:`[copyflags]`| ディレクトリのコピーの忠実性 (指定されていない場合の既定値は `/DCOPY:DA`)、コピー フラグ: `D` = データ、`A` = 属性、`T` = タイムスタンプ。 |
| /UNILOG:<file name> | 状態を UNICODE 形式でログ ファイルに出力します (既存のログを上書きします)。 |
| /LFSM               | **階層型ストレージを持つターゲットの場合のみ** </br>/LFSM を使用すると、"低空き領域モード" で動作するよう RoboCopy に要求します。 このスイッチは、RoboCopy が完了する前にローカル容量が不足する可能性がある、階層型ストレージを持つターゲットにのみ有効です。 このスイッチは、Azure File Sync のクラウドの階層化が有効なターゲットで使用するために特別に追加されました。 これは、Azure File Sync とは別に使用できます。このモードでは、ファイルのコピーによって宛先ボリュームの空き領域が "床" 値よりも小さくなるたびに、RoboCopy が一時停止します。 この値は `/LFSM:n` のフラグ形式で指定できます。 パラメーター `n` は、ベース 2: `nKB`、`nMB`、または`nGB` で指定します。 明示的な床値を示さずに `/LFSM` を指定した場合、床は宛先ボリュームのサイズの 10% に設定されます。 低空き領域モードは、/MT、/EFSRAW、/B、および/ZB では利用できません。 |
| /Z                  | **慎重に使用** </br>再起動モードでファイルをコピーします。ネットワーク環境が不安定な場合にのみ、使用することをお勧めします。 このオプションでは、追加のログ記録が原因で、コピーのパフォーマンスが大幅に低下します。 |
| /ZB                 | **慎重に使用** </br>再起動モードが使用されます。 アクセスが拒否された場合、このオプションではバックアップ モードが使用されます。 このオプションでは、チェックポイント処理が原因で、コピーのパフォーマンスが大幅に低下します。 |
   