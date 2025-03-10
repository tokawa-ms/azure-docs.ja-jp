---
title: よく寄せられる質問
description: Azure Object Anchors サービスに関する FAQ。
author: craigktreasure
manager: vriveras
ms.author: crtreasu
ms.date: 04/01/2020
ms.topic: overview
ms.service: azure-object-anchors
ms.openlocfilehash: 6de027f317072512bd9893303d3a79d0f34f3e3e
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/03/2021
ms.locfileid: "101747744"
---
# <a name="frequently-asked-questions-about-azure-object-anchors"></a>Azure Object Anchors についてよく寄せられる質問

Azure Object Anchors を使用すると、アプリケーションで 3D モデルを使用して現実世界のオブジェクトを検出し、その 6-DoF 姿勢を推定することができます。

詳しくは、「[Azure Object Anchors の概要](overview.md)」を参照してください。

## <a name="product-faq"></a>製品に関する FAQ
**Q: 使用するオブジェクトに関して、どのような推奨事項がありますか?**

**A:** 以下の特性を備えたオブジェクトが推奨されます。

* 各次元が 1-10 メートルである
* 非対称で、ジオメトリに十分な変化がある
* 反射率が低く (つや消し面) 色が明るい
* 静止したオブジェクトである
* 関節がない、または少ない
* 乱雑さがない、または最小限の、クリアな背景
* スキャンされたオブジェクトは、トレーニングに使用したモデルと 1:1 で一致している必要がある

**Q: モデルの取り込みで処理できるオブジェクトの最大寸法はどれくらいですか?**

**A:** CAD モデルの各次元は 10 メートル未満である必要があります。

**Q: 取り込みで処理できる CAD モデルの最大サイズはどれくらいですか?**

**A:** モデルのファイル サイズは 150 MB 未満である必要があります。

**Q: どのような CAD 形式がサポートされますか?**

**A:** 現在、`fbx`、`ply`、`obj`、`glb`、および `gltf` のファイルの種類がサポートされています。

**Q: モデル取り込みサービスで必要となる重力方向と単位とは何ですか?それらを調べるにはどうすればよいですか?**

**A:** 重力方向は、地球に向かう下方向のベクトルです。 CAD モデルの場合、重力方向は通常、上方向の逆になります。 たとえば、多くの場合 +Z が上方向を表しますが、この場合は -Z または `Vector3(0.0, 0.0, -1.0)` が重力方向を表します。 重力を決定するときは、モデルだけでなく、実行時にモデルがどの向きで見られるかも考慮する必要があります。 平らな場所に置かれた現実世界の椅子を検出しようとしている場合、重力は `Vector3(0.0, 0.0, -1.0)` かもしれません。 しかし、椅子が 45 度の斜面に置かれている場合、重力は `Vector3(0.0, -Sqrt(2)/2, -Sqrt(2)/2)` である可能性があります。

重力の方向は、[MeshLab](http://www.meshlab.net/) などの 3D レンダリング ツールを使用して推論できます。

単位は、モデルの測定単位を表します。 サポートされている単位は、**Microsoft.Azure.ObjectAnchors.Ingestion.Unit** 列挙型を使用して確認できます。

**Q: CAD モデルの取り込みにはどのくらいの時間がかかりますか?**

**A:** `ply` モデルの場合は、通常 3-15 分です。 他の形式のモデルを送信する場合は、ファイル サイズに応じて 15-60 分の待機時間を想定してください。

**Q: Object Anchors ではどのようなデバイスがサポートされていますか?**

**A:** HoloLens 2 です 

**Q: HoloLens ではどの OS ビルドを実行する必要がありますか?**

**A:** OS ビルド 18363.720 以降 (2020 年 3 月 12 日以降にリリース) です。

  詳細については、[Windows 10 2020 年 3 月 12 日の更新](https://support.microsoft.com/help/4551762)を参照してください。

**Q: HoloLens でオブジェクトを検出するのにどれくらいの時間がかかりますか?**

**A:** オブジェクトのサイズとスキャン プロセスによって異なります。 迅速な検出を実現するには、詳細なスキャンのためのベスト プラクティスに従ってください。 各次元が 2 メートル以内の小さなオブジェクトの場合、検出は数秒以内に実行される可能性があります。 より大きなオブジェクト (車など) の場合、信頼性の高い検出を行うために、ユーザーはオブジェクトの周りを完全に一周する必要があります。つまり、検出に数十秒かかる可能性があります。

**Q: HoloLens アプリケーションで Object Anchors を使用する際のベスト プラクティスは何ですか?**

**A:**

 1. 正確なレンダリングを行うために、目のキャリブレーションを実行します。
 2. 部屋に豊富な視覚テクスチャと良好な照明を用意します。
 3. オブジェクトを動かさず、可能であれば乱雑さから遠ざけます。
 4. 必要に応じて、HoloLens デバイスの[空間マッピング](https://docs.microsoft.com/windows/mixed-reality/spatial-mapping) キャッシュをクリアします。
 5. オブジェクトの周りを歩いてオブジェクトをスキャンします。 オブジェクトの大部分を観察するようにします。
 6. オブジェクトをカバーする十分に大きい検索領域を設定します。
 7. 検出中、オブジェクトは静止したままである必要があります。
 8. オブジェクトの検出を開始し、推定された姿勢に基づいてレンダリングを視覚化します。
 9. 姿勢が正確に安定したら、バッテリ寿命を保つために、検出されたオブジェクトをロックする、または追跡を停止します。

**Q: 推定された姿勢はどの程度正確ですか?**

**A:** オブジェクトのサイズ、素材、環境などによって異なります。小さいオブジェクトの場合、誤差 2 cm 以内で姿勢を推定することが可能です。 車のような大きなオブジェクトの場合、誤差は最大 2-8 cm になることがあります。

**Q: Object Anchors で動くオブジェクトを扱うことはできますか?**

**A:** **継続的に動く**、または **動的な** オブジェクトはサポートされていません。

**Q: Object Anchors で変形や関節を扱うことはできますか?**

**A:** 変形や関節によってオブジェクトの形やジオメトリがどの程度変化するかに応じて、部分的には可能です。 オブジェクトのジオメトリが頻繁に変化する場合、ユーザーはその構成に対して別のモデルを作成し、検出に使用することができます。

**Q: Object Anchors で同時に検出できる、異なるオブジェクトの数はいくつですか?**

**A:** 現時点では、一度に 1 個のオブジェクト モデルの検出がサポートされています。 

**Q: Object Anchors では、同じオブジェクト モデルの複数のインスタンスを検出することはできますか?**

**A:** はい。同じモデルの種類のオブジェクトを最大 3 個検出できます。 アプリケーションでは、異なるクエリで `ObjectObserver.DetectAsync` を複数回呼び出し、同じモデルの複数のインスタンスを検出することができます。

**Q: Object Anchors ランタイムによってオブジェクトが検出されない場合は、どうすればよいですか?**

**A:**

* ポスターをいくつか追加して、部屋に十分なテクスチャがあるようにします。
* オブジェクトをより完全にスキャンします。
* 前述のようにモデルのパラメーターを調整します。
* 検索領域として、オブジェクトの全体または大部分を含む狭い境界ボックスを指定します。
* 空間マッピングのキャッシュをクリアし、オブジェクトを再スキャンします。
* 診断情報をキャプチャし、Microsoft にデータを送信します。

**Q: オブジェクトのクエリ パラメーターはどのように選択すればよいですか?**

**A:**

* 検出の速度と精度を向上させるために、理想的にはオブジェクト全体をカバーする、狭い検索領域を指定します。
* 通常はオブジェクト モデルの既定の `ObjectQuery.MinSurfaceCoverage` が適切です。それ以外の場合は、迅速な検出を行うためにより小さな値を使用します。
* オブジェクトが直立状態である必要がある場合は、`ObjectQuery.ExpectedMaxVerticalOrientationInDegrees` に小さい値を使用します。
* アプリでは、常に検出用に `1:1` のオブジェクト モデルを使用する必要があります。 推定されたスケールは、理想的には誤差 1% 以内で 1 に近い必要があります。 アプリでは、`ObjectQuery.MaxScaleChange` を `0` または `0.1` に設定してスケールの推定を無効または有効にし、インスタンスの姿勢を定性的に評価することができます。

**Q: HoloLens から Object Anchors の診断データを取得するにはどうすればよいですか?**

**A:** アプリケーションでは、診断情報のアーカイブの場所を指定できます。 Object Anchors のサンプル アプリでは、**TempState** フォルダーに診断情報が書き込まれます。

**Q: Object Anchors Unity SDK によって返された姿勢を使用するときに、ソース モデルが物理オブジェクトと一致しないのはなぜですか?**

**A:** Unity では、オブジェクト モデルのインポート時に座標系が変更されることがあります。 たとえば、Object Anchors Unity SDK では座標系を右手系から左手系に変換するときに Z 軸が反転されますが、Unity では X 軸、Y 軸いずれかに関して追加の回転が適用される可能性があります。 開発者は、座標系を視覚化して比較することで、この追加の回転を判断できます。

**Q: 2D はサポートされていますか?**

**A:** ジオメトリ ベースであるため、3D のみがサポートされています。

**Q: 色の異なる同じモデルを区別することは可能ですか?**

**A:** 使用されるアルゴリズムはジオメトリ ベースであるため、色違いの同じモデルで検出時の動作が変化することはありません。

**Q: インターネット接続なしで Object Anchors を使用することはできますか?**

**A:** 
* モデルの取り込みとトレーニングはクラウド内で行われるため、これらについては接続が必要です。
* ランタイム セッションは完全にデバイス上で実行され、すべての計算が HoloLens 2 で行われるため、接続は必要ありません。

## <a name="privacy-faq"></a>プライバシーに関する FAQ
**Q: Azure Object Anchors ではどのようにデータが保存されますか?**

**A:** Microsoft によって保存されるのはシステム メタデータのみです。それは Microsoft マネージド データ暗号化キーを使用して保存時に暗号化されます。
