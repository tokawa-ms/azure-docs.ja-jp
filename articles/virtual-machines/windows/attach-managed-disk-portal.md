---
title: マネージド データ ディスクを Windows VM に接続する - Azure
description: Azure portal を使用して Windows VM にマネージド データ ディスクを接続する方法について説明します。
author: roygara
ms.service: virtual-machines
ms.collection: windows
ms.topic: how-to
ms.date: 02/06/2020
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 8c64b0ff5b7a9abfa58ec17d0ebcabe05b0ed6e9
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2021
ms.locfileid: "102550809"
---
# <a name="attach-a-managed-data-disk-to-a-windows-vm-by-using-the-azure-portal"></a>Azure portal を使用して Windows VM にマネージド データ ディスクを接続する

この記事では、Azure portal で新規のマネージド データ ディスクを Windows 仮想マシン (VM) に接続する方法について説明します。 VM のサイズによって、接続できるデータ ディスクの数が決まります。 詳細については、 [仮想マシンのサイズ](../sizes.md)に関するページをご覧ください。


## <a name="add-a-data-disk"></a>データ ディスクの追加

1. [Azure portal](https://portal.azure.com) に移動して、データ ディスクを追加します。 **[仮想マシン]** を検索して選択します。
2. 一覧から仮想マシンを選択します。
3. **[仮想マシン]** ページで、**[ディスク]** を選択します。
4. **[ディスク]** ページで、**[データ ディスクの追加]** を選択します。
5. 新しいディスクのドロップダウン リストで **[ディスクの作成]** を選択します。
6. **[マネージド ディスクの作成]** ページで、ディスクの名前を入力し、必要に応じてその他の設定を調整します。 完了したら **[作成]** を選択します。
7. **[ディスク]** ページで **[保存]** を選択して、VM の新しいディスク構成を保存します。
8. Azure でディスクが作成され、仮想マシンに接続されると、仮想マシンのディスク設定の **[データ ディスク]** に新しいディスクが表示されます。


## <a name="initialize-a-new-data-disk"></a>新しいデータ ディスクの初期化

1. VM に接続します
1. 実行中の VM 内で Windows の **スタート** メニューを選択し、検索ボックスに「**diskmgmt.msc**」と入力します。 **[ディスクの管理]** コンソールが開きます。
2. 新しい未初期化ディスクがディスクの管理によって認識され、**[ディスクの初期化]** ウィンドウが表示されます。
3. 新しいディスクが選択されていることを確認し、**[OK]** をクリックしてディスクを初期化します。
4. 新しいディスクが、**未割り当て** として表示されます。 ディスクの任意の場所を右クリックし、**[新しいシンプル ボリューム]** を選択します。 **新しいシンプル ボリューム ウィザード** が開きます。
5. すべての設定を既定値のままウィザードを進め、完了したら **[完了]** を選択します。
6. **[ディスクの管理]** を閉じます。
7. ポップアップが表示され、新しいディスクは使用前にフォーマットする必要があることが通知されます。 **[ディスクのフォーマット]** を選択します。
8. **[新しいディスクのフォーマット]** ウィンドウで設定を確認し、**[開始]** を選択します。
9. ディスクをフォーマットするとデータがすべて削除されることを警告するメッセージが表示されます。 **[OK]** を選択します。
10. フォーマットが完了したら、**[OK]** を選択します。

## <a name="next-steps"></a>次のステップ

- [PowerShell を使用してデータ ディスクを接続する](attach-disk-ps.md)こともできます。
- アプリケーションで *D:* ドライブを使用してデータを保存する必要がある場合は、[Windows 一時ディスクのドライブ文字を変更](change-drive-letter.md)できます。