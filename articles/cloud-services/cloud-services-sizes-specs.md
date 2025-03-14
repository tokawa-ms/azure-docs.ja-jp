---
title: Azure Cloud Services (クラシック) の仮想マシンのサイズ | Microsoft Docs
description: Azure のクラウド サービスの Web ロールと worker ロールのさまざまな仮想マシンのサイズ (および ID) の一覧を示します。
ms.topic: article
ms.service: cloud-services
ms.date: 10/14/2020
ms.author: tagore
author: tanmaygore
ms.reviewer: mimckitt
ms.custom: ''
ms.openlocfilehash: a31c9c4747ccc72f0552ebe2f8daf85ef5fba8a3
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/11/2021
ms.locfileid: "103015303"
---
# <a name="sizes-for-cloud-services-classic"></a>Cloud Services (クラシック) のサイズ

> [!IMPORTANT]
> [Azure Cloud Services (延長サポート)](../cloud-services-extended-support/overview.md) は、Azure Cloud Services 製品向けの新しい Azure Resource Manager ベースのデプロイ モデルです。 この変更により、Azure Service Manager ベースのデプロイ モデルで実行されている Azure Cloud Services は Cloud Services (クラシック) という名前に変更されました。そのため、すべての新しいデプロイでは [Cloud Services (延長サポート)](../cloud-services-extended-support/overview.md) を使用する必要があります。

このトピックでは、クラウド サービスのロール インスタンス (Web ロールと worker ロール) で使用できるサイズとオプションについて説明します。 また、これらのリソースの使用を計画するときに注意するデプロイメントに関する考慮事項も示します。 それぞれのサイズには、[サービス定義ファイル](cloud-services-model-and-package.md#csdef)に配置する ID があります。 サイズごとの価格は「[Cloud Services の価格](https://azure.microsoft.com/pricing/details/cloud-services/)」ページで表示されています。

> [!NOTE]
> 関連する Azure の制限については、 [Azure サブスクリプションとサービスの制限、クォータ、制約](../azure-resource-manager/management/azure-subscription-service-limits.md)
>
>

## <a name="sizes-for-web-and-worker-role-instances"></a>Web ロールと worker ロールのインスタンスのサイズ
Azure では複数の標準的なサイズを選択できます。 これらのサイズに関する考慮事項は次のとおりです。

* D シリーズ VM は、より高いコンピューティング能力と一時ディスクのパフォーマンスを必要とするアプリケーションを実行するように設計されています。 D シリーズ VM は、より高速なプロセッサ、より高いメモリ対コア比、一時ディスク用ソリッド ステート ドライブ (SSD) を提供します。 詳細については、Azure ブログの投稿「 [新しい D シリーズ仮想マシンのサイズ](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/)」をご覧ください。
* オリジナルの D シリーズに続く Dv3 シリーズと Dv2 シリーズには、より強力な CPU が備わっています。 Dv2 シリーズの CPU は D シリーズの CPU よりも、およそ 35% 高速です。 これは最新世代の 2.4 GHz Intel Xeon® E5-2673 v3 (Haswell) プロセッサに基づいており、Intel Turbo Boost Technology 2.0 を使用することで、最大 3.1 GHz まで実現できます。 Dv2 シリーズのメモリ構成とディスク構成は D シリーズと同じです。
* G シリーズ VM は、最も多くのメモリを提供し、Intel Xeon E5 V3 ファミリのプロセッサが搭載されたホスト上で実行されます。
* A シリーズ VM は、多様なハードウェアの種類とプロセッサにデプロイできます。 デプロイされるハードウェアに関係なく、実行中のインスタンスに対して一貫したプロセッサ パフォーマンスを提供するため、ハードウェアに基づいてサイズが調整されます。 このサイズがデプロイされる物理ハードウェアを判断するには、仮想マシン内から仮想ハードウェアをクエリします。
* A0 サイズは、物理ハードウェアでオーバーサブスクライブされます。 この特定のサイズの場合のみ、他の顧客デプロイメントは、実行中のワークロードのパフォーマンスに影響することがあります。 下に、予想される基準として相対パフォーマンスを示していますが、約 15% の変動の可能性があります。

仮想マシンのサイズは価格に影響します。 また、サイズは仮想マシンの処理、メモリ、記憶容量にも影響します。 Storage のコストは、ストレージ アカウントで使用されるページに基づいて個別に計算されます。 詳細については、「[Cloud Services の価格の詳細](https://azure.microsoft.com/pricing/details/cloud-services/)」ページと「[Azure Storage 料金](https://azure.microsoft.com/pricing/details/storage/)」ページを参照してください。

サイズを決定する際に役立つ考慮事項は次のとおりです。

* A8 ～ A11 と H シリーズのサイズは、 *コンピューティング集中型インスタンス* とも呼ばれます。 これらのサイズを実行するハードウェアは、ハイ パフォーマンス コンピューティング (HPC) クラスター アプリケーション、モデリング、シミュレーションなど、コンピューティング集中型およびネットワーク集中型アプリケーション用に設計および最適化されています。 A8 ～ A11 シリーズは Intel Xeon E5-2670 @ 2.6 GHZ を使用し、H シリーズは Intel Xeon E5-2667 v3 @ 3.2 GHz を使用します。 これらのサイズの使用に関する詳細な情報と考慮事項については、「[ハイ パフォーマンス コンピューティング VM のサイズ](../virtual-machines/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)」をご覧ください。
* Dv3 シリーズ、Dv2 シリーズ、D シリーズ、G シリーズは、より高速の CPU やより高いローカル ディスク パフォーマンスが必要なアプリケーション、またはメモリ要求がより高いアプリケーションに最適です。 多数のエンタープライズ レベルのアプリケーションに、強力な組み合わせで対処します。
* Azure データ センターの物理ホストの一部では、A5 ～ A11 などの大きな仮想マシンのサイズをサポートしていない場合があります。 その結果、既存の仮想マシンのサイズを新しいサイズに変更した場合、2013 年 4 月 16 日よりも前に作成された仮想ネットワーク内に新しい仮想マシンを作成した場合、または既存のクラウド サービスに新しい仮想マシンを追加した場合に、 **"仮想マシン {マシン名} を構成できませんでした"** または **"仮想マシン {マシン名} を作成できませんでした"** というエラー メッセージが表示されることがあります。 各デプロイ シナリオの回避策については、サポート フォーラムで、[エラー:"仮想マシンを構成できませんでした"](https://social.msdn.microsoft.com/Forums/9693f56c-fcd3-4d42-850e-5e3b56c7d6be/error-failed-to-configure-virtual-machine-with-a5-a6-or-a7-vm-size?forum=WAVirtualMachinesforWindows) というトピックを参照してください。
* お客様のサブスクリプションによっては、特定のサイズ ファミリにデプロイできるコア数が制限されることがあります。 コア クォータを増やすには、Azure サポートにお問い合わせください。

## <a name="performance-considerations"></a>パフォーマンスに関する考慮事項
Azure SKU 間で計算 (CPU) パフォーマンスを比較する手段を提供したり、パフォーマンスの必要を満たす最適な SKU を特定したりするため、Azure Compute Unit (ACU) の概念が作成されています。  現在、ACU は小さい (Standard_A1) VM を 100 として標準化されており、他のすべての SKU についてはその SKU が標準ベンチマークをそれよりどれくらい速く実行できるかが表されます。

> [!IMPORTANT]
> ACU はガイドラインに過ぎません。 ワークロードの結果は異なる場合があります。
>
>

<br>

| SKU ファミリ | ACU/コア |
| --- | --- |
| [ExtraSmall](#a-series) |50 |
| [Small-ExtraLarge](#a-series) |100 |
| [A5-7](#a-series) |100 |
| [A8 ～ A11](#a-series) |225* |
| [A v2](#av2-series) |100 |
| [D](#d-series) |160 |
| [D v2](#dv2-series) |210 - 250* |
| [D v3](#dv3-series) |160 - 190* |
| [E v3](#ev3-series) |160 - 190* |
| [G](#g-series) |180 ～ 240* |
| [H](#h-series) |290 ～ 300* |

\* が付いている ACU は、Intel® Turbo テクノロジを使用して CPU 周波数を上げ、パフォーマンスを増強します。 増強量は、VM のサイズ、ワークロード、および同じホストで実行されている他のワークロードによって変化します。

## <a name="size-tables"></a>サイズ一覧表
次の表に、サイズとそのサイズで提供される容量を示します。

* ストレージ容量は GiB (1024^3 バイト) 単位で示されています。 GB (1000^3 バイト) 単位のディスクと GiB (1024^3 バイト) 単位のディスクを比較する場合は、GiB 単位の方が容量の数値が小さく見えることに注意してください。 たとえば、1023 GiB = 1098.4 GB です。
* ディスク スループットの測定単位は、1 秒あたりの入力/出力操作数 (IOPS) および MBps です (MBps = 10^6 バイト/秒)。
* データ ディスクは、キャッシュを有効にしたモードでも無効化したモードでも動作します。 キャッシュを有効にしたデータ ディスクの操作では、ホスト キャッシュ モードは **ReadOnly** または **ReadWrite** に設定されています。 キャッシュを無効にしたデータ ディスクの操作では、ホスト キャッシュ モードは **None** に設定されています。
* 最大ネットワーク帯域幅は、VM の種類ごとに割り当てられた最大集約帯域です。 最大帯域幅は、適切なネットワーク容量を確保するための適切な VM の種類を選択するためのガイダンスを提供します。 低、中、高、非常に高の順でスループットが増加します。 実際のネットワークのパフォーマンスは、ネットワークおよびアプリケーションの負荷、アプリケーションのネットワーク設定など、多くの要因に左右されます。

## <a name="a-series"></a>A シリーズ
| サイズ            | CPU コア数 | メモリ:GiB  | 一時ストレージ:GiB       | 最大 NIC/ネットワーク帯域幅 |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| ExtraSmall      | 1         | 0.768        | 20                   | 1/低 |
| Small           | 1         | 1.75         | 225                  | 1/中 |
| Medium          | 2         | 3.5          | 490                  | 1/中 |
| Large           | 4         | 7            | 1000                 | 2/高 |
| ExtraLarge      | 8         | 14           | 2040                 | 4/高 |
| A5              | 2         | 14           | 490                  | 1/中 |
| A6              | 4         | 28           | 1000                 | 2/高 |
| A7              | 8         | 56           | 2040                 | 4/高 |

## <a name="a-series---compute-intensive-instances"></a>A シリーズ - コンピューティング集中型インスタンス
これらのサイズの使用に関する情報と考慮事項については、「[ハイ パフォーマンス コンピューティング VM のサイズ](../virtual-machines/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)」をご覧ください。

| サイズ            | CPU コア数 | メモリ:GiB  | 一時ストレージ:GiB       | 最大 NIC/ネットワーク帯域幅 |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| A8*             |8          | 56           | 1817                 | 2/高 |
| A9*             |16         | 112          | 1817                 | 4/非常に高 |
| A10             |8          | 56           | 1817                 | 2/高 |
| A11             |16         | 112          | 1817                 | 4/非常に高 |

\*RDMA 対応

## <a name="av2-series"></a>Av2 シリーズ

| サイズ            | CPU コア数 | メモリ:GiB  | 一時ストレージ (SSD):GiB       | 最大 NIC/ネットワーク帯域幅 |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_A1_v2  | 1         | 2            | 10                   | 1/中                 |
| Standard_A2_v2  | 2         | 4            | 20                   | 2/中                 |
| Standard_A4_v2  | 4         | 8            | 40                   | 4/高                     |
| Standard_A8_v2  | 8         | 16           | 80                   | 8/高                     |
| Standard_A2m_v2 | 2         | 16           | 20                   | 2/中                 |
| Standard_A4m_v2 | 4         | 32           | 40                   | 4/高                     |
| Standard_A8m_v2 | 8         | 64           | 80                   | 8/高                     |


## <a name="d-series"></a>D シリーズ
| サイズ            | CPU コア数 | メモリ:GiB  | 一時ストレージ (SSD):GiB       | 最大 NIC/ネットワーク帯域幅 |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_D1     | 1         | 3.5          | 50                   | 1/中 |
| Standard_D2     | 2         | 7            | 100                  | 2/高 |
| Standard_D3     | 4         | 14           | 200                  | 4/高 |
| Standard_D4     | 8         | 28           | 400                  | 8/高 |
| Standard_D11    | 2         | 14           | 100                  | 2/高 |
| Standard_D12    | 4         | 28           | 200                  | 4/高 |
| Standard_D13    | 8         | 56           | 400                  | 8/高 |
| Standard_D14    | 16        | 112          | 800                  | 8/非常に高 |

## <a name="dv2-series"></a>Dv2 シリーズ
| サイズ            | CPU コア数 | メモリ:GiB  | 一時ストレージ (SSD):GiB       | 最大 NIC/ネットワーク帯域幅 |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_D1_v2  | 1         | 3.5          | 50                   | 1/中 |
| Standard_D2_v2  | 2         | 7            | 100                  | 2/高 |
| Standard_D3_v2  | 4         | 14           | 200                  | 4/高 |
| Standard_D4_v2  | 8         | 28           | 400                  | 8/高 |
| Standard_D5_v2  | 16        | 56           | 800                  | 8/極めて高 |
| Standard_D11_v2 | 2         | 14           | 100                  | 2/高 |
| Standard_D12_v2 | 4         | 28           | 200                  | 4/高 |
| Standard_D13_v2 | 8         | 56           | 400                  | 8/高 |
| Standard_D14_v2 | 16        | 112          | 800                  | 8/極めて高 |
| Standard_D15_v2 | 20        | 140          | 1,000                | 8/極めて高 |

## <a name="dv3-series"></a>Dv3 シリーズ

| サイズ            | CPU コア数 | メモリ:GiB   | 一時ストレージ (SSD):GiB       | 最大 NIC/ネットワーク帯域幅 |
|---------------- | --------- | ------------- | -------------------- | ---------------------------- |
| Standard_D2_v3  | 2         | 8             | 50                   | 2/中 |
| Standard_D4_v3  | 4         | 16            | 100                  | 2/高 |
| Standard_D8_v3  | 8         | 32            | 200                  | 4/高 |
| Standard_D16_v3 | 16        | 64            | 400                  | 8/極めて高 |
| Standard_D32_v3 | 32        | 128           | 800                  | 8/極めて高 |
| Standard_D48_v3 | 48        | 192           | 1200                 | 8/極めて高 |
| Standard_D64_v3 | 64        | 256           | 1600                 | 8/極めて高 |

## <a name="ev3-series"></a>Ev3 シリーズ

| サイズ            | CPU コア数 | メモリ:GiB   | 一時ストレージ (SSD):GiB       | 最大 NIC/ネットワーク帯域幅 |
|---------------- | --------- | ------------- | -------------------- | ---------------------------- |
| Standard_E2_v3  | 2         | 16            | 50                   | 2/中 |
| Standard_E4_v3  | 4         | 32            | 100                  | 2/高 |
| Standard_E8_v3  | 8         | 64            | 200                  | 4/高 |
| Standard_E16_v3 | 16        | 128           | 400                  | 8/極めて高 |
| Standard_E32_v3 | 32        | 256           | 800                  | 8/極めて高 |
| Standard_E48_v3 | 48        | 384           | 1200                 | 8/極めて高 |
| Standard_E64_v3 | 64        | 432           | 1600                 | 8/極めて高 |


## <a name="g-series"></a>G シリーズ
| サイズ            | CPU コア数 | メモリ:GiB  | 一時ストレージ (SSD):GiB       | 最大 NIC/ネットワーク帯域幅 |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_G1     | 2         | 28           | 384                  |1/高 |
| Standard_G2     | 4         | 56           | 768                  |2/高 |
| Standard_G3     | 8         | 112          | 1,536                |4/非常に高 |
| Standard_G4     | 16        | 224          | 3,072                |8/極めて高 |
| Standard_G5     | 32        | 448          | 6,144                |8/極めて高 |

## <a name="h-series"></a>H シリーズ
Azure H シリーズの仮想マシンは、分子モデリングや流体力学などのハイエンド コンピューティングのニーズを目的とした、次世代型のハイ パフォーマンス コンピューティング VM です。 これらの 8 および 16 コアの VM は、DDR4 メモリとローカル SSD ベースの記憶域を備えた Intel Haswell E5-2667 V3 プロセッサ テクノロジをベースに構築されています。

H シリーズのラインナップは強力な CPU パワーに加えて、FDR InfiniBand を使用した低待機時間 RDMA ネットワークのためのさまざまなオプションと、複数のメモリ構成を備えており、メモリ集中型のコンピューティング要件にも対応しています。

| サイズ            | CPU コア数 | メモリ:GiB  | 一時ストレージ (SSD):GiB       | 最大 NIC/ネットワーク帯域幅 |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_H8     | 8         | 56           | 1000                 | 8/高 |
| Standard_H16    | 16        | 112          | 2000                 | 8/非常に高 |
| Standard_H8m    | 8         | 112          | 1000                 | 8/高 |
| Standard_H16m   | 16        | 224          | 2000                 | 8/非常に高 |
| Standard_H16r*  | 16        | 112          | 2000                 | 8/非常に高 |
| Standard_H16mr* | 16        | 224          | 2000                 | 8/非常に高 |

\*RDMA 対応

## <a name="configure-sizes-for-cloud-services"></a>Cloud Services のサイズの構成
ロール インスタンスの仮想マシンのサイズを、 [サービス定義ファイル](cloud-services-model-and-package.md#csdef)で記述するサービス モデルの一部として指定できます。 ロールのサイズによって、CPU コアの数、メモリ容量、および実行中のインスタンスに割り当てられるローカル ファイル システムのサイズが決まります。 ロールのサイズは、アプリケーションのリソース要件に基づいて選択します。

Web ロール インスタンスのロール サイズを Standard_D2 に設定する例を次に示します。

```xml
<WorkerRole name="Worker1" vmsize="Standard_D2">
...
</WorkerRole>
```

## <a name="changing-the-size-of-an-existing-role"></a>既存のロールのサイズを変更します

ワークロードの性質が変化したり、新しい VM のサイズが使用可能になったりすると、ロールのサイズを変更する場合があります。 これを行うには、(上記のように) サービス定義ファイルで VM のサイズを変更し、Cloud Service を再パッケージ化してデプロイする必要があります。

>[!TIP]
> さまざまな環境でのロールに対して異なる VM サイズを使用することがあります (例: テスト対運用)。 これを行う 1 つの方法は、プロジェクトで複数のサービス定義 (.csdef) ファイルを作成し、自動化されたビルドの最中に CSPack ツールを使用して、環境ごとに異なるクラウド サービス パッケージを作成します。 クラウド サービス パッケージとその作成方法の詳しい要素については、「[クラウド サービス モデルおよびパッケージ方法について](cloud-services-model-and-package.md)」を参照してください。
>
>

## <a name="get-a-list-of-sizes"></a>サイズの一覧を取得する
PowerShell または REST API を使用して、サイズの一覧を取得できます。 REST API については、[こちら](/previous-versions/azure/reference/dn469422(v=azure.100))を参照してください。 次のコードは、Cloud Services で利用可能なすべてのサイズを一覧表示する PowerShell コマンドです。 

```powershell
Get-AzureRoleSize | where SupportedByWebWorkerRoles -eq $true | select InstanceSize, RoleSizeLabel
```

## <a name="next-steps"></a>次のステップ
* [Azure サブスクリプションとサービスの制限、クォータ、制約](../azure-resource-manager/management/azure-subscription-service-limits.md)について学習してください。
* HPC ワークロードのためのハイ パフォーマンス コンピューティング VM のサイズについては[こちら](../virtual-machines/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)をご覧ください。
