---
title: Cognitive Services コンテナー イメージ タグ
titleSuffix: Azure Cognitive Services
description: すべての Cognitive Service コンテナー イメージ タグの包括的一覧
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: reference
ms.date: 11/17/2020
ms.author: aahi
ms.openlocfilehash: 2ac8633e48c98b9e1d778ac1d07835683e00e1c0
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/07/2021
ms.locfileid: "102424480"
---
# <a name="azure-cognitive-services-container-image-tags-and-release-notes"></a>Azure Cognitive Services コンテナー イメージ タグとリリース ノート

Azure Cognitive Services からはさまざまなコンテナー イメージが提供されます。 コンテナー レジストリとそれに対応するリポジトリは、コンテナー イメージによって異なります。 各コンテナー イメージ名にからは複数のタグが与えられます。 コンテナー イメージ タグは、コンテナー イメージのバージョンを管理するためのメカニズムです。 この記事は、Cognitive Services コンテナー イメージと利用できるタグをすべてリストアップする包括的リファレンスとして使用されることを意図して作成されています。

> [!TIP]
> [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) を使用する場合、コンテナー レジストリ、リポジトリ、コンテナー イメージ名、それらに対応するタグの大文字/小文字の違いに注意してください。これらでは **大文字と小文字が区別されます**。

## <a name="anomaly-detector"></a>Anomaly Detector

[Anomaly Detector][ad-containers] コンテナー イメージは `mcr.microsoft.com` コンテナー レジストリ シンジケートで見つけることができます。 `azure-cognitive-services/decision` リポジトリ内にあり、`anomaly-detector` という名前が付いています。 完全修飾コンテナー イメージ名は `mcr.microsoft.com/azure-cognitive-services/decision/anomaly-detector` です。

このコンテナー イメージには次のタグを利用できます。 また、[MCR でタグ](https://mcr.microsoft.com/v2/azure-cognitive-services/decision/anomaly-detector/tags/list)の完全な一覧を確認することもできます。

# <a name="latest-version"></a>[最新バージョン](#tab/current)

| イメージ タグ                    | Notes |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.013560003-amd64-preview` |      |

# <a name="previous-versions"></a>[以前のバージョン](#tab/previous)

| イメージ タグ                    | Notes |
|-------------------------------|:------|
| `1.1.012300001-amd64-preview` |       |

---

## <a name="read-ocr-optical-character-recognition"></a>Read OCR (光学式文字認識) 

[Computer Vision][cv-containers] Read OCR コンテナー イメージは `mcr.microsoft.com` コンテナー レジストリ シンジケートにあります。 `azure-cognitive-services` リポジトリ内にあり、`read` という名前が付いています。 完全修飾コンテナー イメージ名は `mcr.microsoft.com/azure-cognitive-services/vision/read` です。

このコンテナー イメージには次のタグを利用できます。 また、[MCR でタグ](https://mcr.microsoft.com/v2/azure-cognitive-services/vision/read/tags/list)の完全な一覧を確認することもできます。

# <a name="latest-version"></a>[最新バージョン](#tab/current)

`3.2-preview.2` のリリース ノート:
* ディストリビューションレス リリース
* JSON 応答でテキスト行の順序を選択する ReadingOrder パラメーター
* 強化されたログ記録
* CJK モデルへの修正プログラム

| イメージ タグ                    | Notes |
|-------------------------------|:------|
|3.2.2.014850001-49e0eac6-amd64-preview                     |       |

# <a name="previous-versions"></a>[以前のバージョン](#tab/previous)

`v2.0.013250001-amd64-preview` のリリース ノート:

* コンテナーのメモリ使用量をさらに減らします。
* マルチポッド セットアップには外部キャッシュが必要です。 たとえば、キャッシュ用に Redis を設定します。
* Redis キャッシュが設定されており、`ResultExpirationPeriod` が 0 に設定されている場合に結果が欠落する問題を修正しました。
* 要求本文サイズの 26MB の制限を取り除きます。 コンテナーでは 26MB を超えるファイルを受け取れるようになります。
* タイム スタンプとビルド バージョンをコンソール ログに追加します。

`1.1.013050001-amd64-preview` のリリース ノート

* システムで認識結果を消去するタイミングを指定するために、`ReadEngineConfig:ResultExpirationPeriod` コンテナー初期化構成を追加しました。 
    * 設定は時間単位であり、既定値は 48 時間です。
    * この設定により、結果格納時のメモリ使用量を減らすことができます。特に、コンテナーのメモリ内ストレージが使用されるときに効果があります。
    * 例 1. ReadEngineConfig:ResultExpirationPeriod=1、プロセスから 1 時間後に認識結果が消去されます。
    * この構成が 0 に設定されている場合、結果が取得された後にシステムで認識結果が消去されます。

* 無効なイメージ形式がシステムに渡されたときの 500 内部サーバー エラーを修正しました。 400 エラーが返されるようになりました。

    ```json
    {
        "error": {
        "code": "InvalidImageSize",
        "message": "Image must be between 1024 and 209715200 bytes."
        }
    }
    ```

| イメージ タグ                    | Notes |
|-------------------------------|:------|
| `2.0.013250001-amd64-preview` |       |
| `1.1.013050001-amd64-preview` |       |
| `1.1.011580001-amd64-preview` |       |
| `1.1.009920003-amd64-preview` |       |
| `1.1.009910003-amd64-preview` |       |

---


## <a name="form-recognizer"></a>Form Recognizer

[Form Recognizer][fr-containers] コンテナー イメージは `mcr.microsoft.com` コンテナー レジストリ シンジケートにあります。 `azure-cognitive-services/custom-form` リポジトリ内にあり、`labeltool` という名前が付いています。 完全修飾コンテナー イメージ名は `mcr.microsoft.com/azure-cognitive-services/custom-form/labeltool` です。

このコンテナー イメージには次のタグを利用できます。 また、[MCR でタグ](https://mcr.microsoft.com/v2/azure-cognitive-services/custom-form/labeltool/tags/list)の完全な一覧を確認することもできます。

# <a name="latest-version"></a>[最新バージョン](#tab/current)

| イメージ タグ                    | Notes |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.009301-amd64-preview`    |       |


# <a name="previous-versions"></a>[以前のバージョン](#tab/previous)

| イメージ タグ                    | Notes |
|-------------------------------|:------|
| `1.1.008640001-amd64-preview` |       |
| `1.1.008510001-amd64-preview` |       |

---

## <a name="language-understanding-luis"></a>Language Understanding (LUIS)

[LUIS][lu-containers] コンテナー イメージは `mcr.microsoft.com` コンテナー レジストリ シンジケートにあります。 `azure-cognitive-services/language` リポジトリ内にあり、`luis` という名前が付いています。 完全修飾コンテナー イメージ名は `mcr.microsoft.com/azure-cognitive-services/language/luis` です。

このコンテナー イメージには次のタグを利用できます。 また、[MCR でタグ](https://mcr.microsoft.com/v2/azure-cognitive-services/language/luis/tags/list)の完全な一覧を確認することもできます。

# <a name="latest-version"></a>[最新バージョン](#tab/current)

| イメージ タグ                    | Notes |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.012280003-amd64-preview` |       |


# <a name="previous-version"></a>[以前のバージョン](#tab/previous)

| イメージ タグ                    | Notes |
|-------------------------------|:------|
| `1.1.012130003-amd64-preview` |       |

---

## <a name="custom-speech-to-text"></a>カスタム音声変換

[カスタム音声変換][sp-cstt]コンテナー イメージは `mcr.microsoft.com` コンテナー レジストリ シンジケートにあります。 `azure-cognitive-services/speechservices/` リポジトリ内にあり、`custom-speech-to-text` という名前が付いています。 完全修飾コンテナー イメージ名は `mcr.microsoft.com/azure-cognitive-services/speechservices/custom-speech-to-text` です。 また、[MCR でタグ](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/custom-speech-to-text/tags/list)の完全な一覧を確認することもできます。


# <a name="latest-version"></a>[最新バージョン](#tab/current)

`2.10.0-amd64` のリリース ノート:

**機能**
* 最新のモデルにアップグレードします。

フレーズ リストが含まれているため、このコンテナー イメージのサイズが大きくなっていることに注意してください。

| イメージ タグ                    | Notes | ダイジェスト                                                                   |
|-------------------------------|:------|:-------------------------------------------------------------------------|
| `latest`                      |       | `sha256:944f051e3b90aa8eb8a37a4d4a18d4e13dbb49f9fe6d5ced61a0f99b3b3e312c`|
| `2.10.0-amd64`                |       | `sha256:944f051e3b90aa8eb8a37a4d4a18d4e13dbb49f9fe6d5ced61a0f99b3b3e312c`|


# <a name="previous-version"></a>[以前のバージョン](#tab/previous)

`2.9.0-amd64` のリリース ノート:

**機能**
* ID によってカスタム モデルをフェッチする際の問題に関する詳細なエラー。
* 既定では、会話の結果で推定がサポートされます。

`2.7.0-amd64` のリリース ノート:

**機能**
* 句読点は、既定で有効に設定されています。

フレーズ リストが含まれているため、このコンテナー イメージのサイズが大きくなっていることに注意してください。

`2.6.0-amd64` のリリース ノート:

**機能**
* Phraselist v2 のサポート 
* フレーズ リストは次のロケールでサポートされています。
    * en-au
    * en-ca
    * en-gb
    * en-in
    * ja-JP
    * zh-cn
* カスタムベース モデルのダウンロードのサポート。 
    * `docker run` コマンドで `BaseModelLocale=<locale>` を使用する
* .NET 3.1 に完全に移行しました

**修正**
* 信頼スコアがダイアライゼーション モードで常に 1 になるという問題を修正しました
* TextAnalytics 3.0 API に移行しました

フレーズ リストが含まれているため、このコンテナー イメージのサイズが大きくなっていることに注意してください。

`2.5.0-amd64` のリリース ノート:

**機能**
* カスタム モデルでのカスタム発音のサポート
* Azure および Azure US Government Cloud のサポート

**修正**
* ダイアライゼーション モードで非 root ユーザーとして実行する場合の問題を修正しました

| イメージ タグ                    | Notes               |
|-------------------------------|:--------------------|
| `2.9.0-amd64`                 |                     |
| `2.7.0-amd64`                 |                     |
| `2.6.0-amd64`                 |                     |
| `2.5.0-amd64`                 |   最初の GA バージョン    |

---

## <a name="custom-text-to-speech"></a>カスタム テキスト読み上げ

[カスタム テキスト読み上げ][sp-ctts]コンテナー イメージは `mcr.microsoft.com` コンテナー レジストリ シンジケートにあります。 `azure-cognitive-services/speechservices/` リポジトリ内にあり、`custom-text-to-speech` という名前が付いています。 完全修飾コンテナー イメージ名は `mcr.microsoft.com/azure-cognitive-services/speechservices/custom-text-to-speech` です。 また、[MCR でタグ](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/custom-text-to-speech/tags/list)の完全な一覧を確認することもできます。


# <a name="latest-version"></a>[最新バージョン](#tab/current)

`1.12.0-amd64` のリリース ノート:

**機能**
* 最新のモデルにアップグレードします。

| イメージ タグ                    | Notes | ダイジェスト                                                                   |
|-------------------------------|:------|:-------------------------------------------------------------------------|
| `latest`                      |       | `sha256:82806245b8ccce808523c4276d8f447c8429db8b04ff26bdbc4d36ba6a704db4` |
| `1.12.0-amd64`                |       | `sha256:82806245b8ccce808523c4276d8f447c8429db8b04ff26bdbc4d36ba6a704db4` |


# <a name="previous-version"></a>[以前のバージョン](#tab/previous)

`1.11.0-amd64` のリリース ノート:

**機能**
* ID によってカスタム モデルをフェッチする際の問題に関する詳細なエラー。

`1.9.0-amd64` のリリース ノート:

定期的な毎月のリリース

`1.8.0-amd64` のリリース ノート:

**機能**
* .NET 3.1 に完全に移行しました

`1.7.0-amd64` のリリース ノート:

**機能**
* .NET 3.1 に部分的に移行されました

| イメージ タグ                    | Notes               |
|-------------------------------|:--------------------|
| `1.11.0-amd64`                |                     |
| `1.9.0-amd64`                 |                     |
| `1.8.0-amd64`                 |                     |
| `1.7.0-amd64`                 |   最初の GA バージョン    |

---

## <a name="speech-to-text"></a>音声テキスト変換

[音声テキスト変換][sp-stt]コンテナー イメージは `mcr.microsoft.com` コンテナー レジストリ シンジケートにあります。 `azure-cognitive-services/speechservices/` リポジトリ内にあり、`speech-to-text` という名前が付いています。 完全修飾コンテナー イメージ名は `mcr.microsoft.com/azure-cognitive-services/speechservices/speech-to-text` です。 [MCR でタグ](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/speech-to-text/tags/list)の完全な一覧を確認することができます。

音声テキスト変換 v2.5.0 以降、イメージは "*米国政府バージニア*" リージョンでサポートされています。 このリージョンを使う場合は、"*米国政府バージニア*" の課金エンドポイントと API キーを使用してください。

# <a name="latest-version"></a>[最新バージョン](#tab/current)

`2.10.0-amd64-<locale>` のリリース ノート:

**機能**
* 最新のモデルにアップグレードします。

フレーズ リストが含まれているため、このコンテナー イメージのサイズが大きくなっていることに注意してください。 

| イメージ タグ                    | Notes                                                                                                |
|-------------------------------|:-----------------------------------------------------------------------------------------------------|
| `latest`                      | `en-US` ロケールのコンテナー イメージ。                                                             |
| `2.10.0-amd64-<locale>`       | `<locale>` を、以下に一覧表示されている使用可能なロケールのいずれかに置き換えます。 たとえば、「 `2.10.0-amd64-en-us` 」のように指定します。|

このコンテナー イメージには次のロケールを使用できます。

| v2.10.0 のロケール          | Notes                                    | ダイジェスト                                                                    |
|-----------------------------|:-----------------------------------------|:--------------------------------------------------------------------------|
| `ar-ae`                     | `ar-AE` ロケールのコンテナー イメージ。 | `sha256:f81f6c53e8ca9c3ae10c335ad45054cea571eca2f4ab32e44e13445936ce3f17` |
| `ar-bh`                     | `ar-BH` ロケールのコンテナー イメージ。 | `sha256:da276dc1b481c002a9b3d2944e190af799175b5a2eabafab87153e22529bdab1` |
| `ar-eg`                     | `ar-EG` ロケールのコンテナー イメージ。 | `sha256:c2ae166526cb0c5d481b537daa3accd379c4b1bf51fce6d85ac20591e7e0b4c0` |
| `ar-iq`                     | `ar-IQ` ロケールのコンテナー イメージ。 | `sha256:7d4a6cb1d9d66f6bd62f90b82000ef811f8a3dd58b03641b6c51ad6f0f4fd4dc` |
| `ar-jo`                     | `ar-JO` ロケールのコンテナー イメージ。 | `sha256:7489a0ed06fdf1da1d25e3211f5a66abe420babee148961a2ffe8cdbd82564a7` |
| `ar-kw`                     | `ar-KW` ロケールのコンテナー イメージ。 | `sha256:f81f6c53e8ca9c3ae10c335ad45054cea571eca2f4ab32e44e13445936ce3f17` |
| `ar-lb`                     | `ar-LB` ロケールのコンテナー イメージ。 | `sha256:478e4575073660e9153811f58e74815f62395ee2ebd868d448fbc3a5e16442be` |
| `ar-om`                     | `ar-OM` ロケールのコンテナー イメージ。 | `sha256:025dcbd6a7d1912812b2556ffd7a16ad2158be6c3746e2822f2b97f460aa685b` |
| `ar-qa`                     | `ar-QA` ロケールのコンテナー イメージ。 | `sha256:f81f6c53e8ca9c3ae10c335ad45054cea571eca2f4ab32e44e13445936ce3f17` |
| `ar-sa`                     | `ar-SA` ロケールのコンテナー イメージ。 | `sha256:f81f6c53e8ca9c3ae10c335ad45054cea571eca2f4ab32e44e13445936ce3f17` |
| `ar-sy`                     | `ar-SY` ロケールのコンテナー イメージ。 | `sha256:5af93722e70e445b3a4102bf621e6d5bb5854bcc99f60d4590e23fc24e50297e` |
| `bg-bg`                     | `bg-BG` ロケールのコンテナー イメージ。 | `sha256:a9402f03b02150288d51e03ec97b8efb98ad6c444df3ab50a3b4ce1129d02d86` |
| `ca-es`                     | `ca-ES` ロケールのコンテナー イメージ。 | `sha256:122df16df46a84a14b28e4ff406a047947fdc10a65b40482438beee55579f687` |
| `cs-cz`                     | `cs-CZ` ロケールのコンテナー イメージ。 | `sha256:7b7d7ef798a0210b8c33a3a201ba149e1264cc7ac6ddaf986721d86e91e5e444` |
| `da-dk`                     | `da-DK` ロケールのコンテナー イメージ。 | `sha256:ba8dd6564939eda7b81b1a4c13ad31672927528dd146698fce10c12d21f647a9` |
| `de-de`                     | `de-DE` ロケールのコンテナー イメージ。 | `sha256:d0fa9bc409238ebdab0a15174b3169c99cbad42323087ea589bb7812a0550149` |
| `el-gr`                     | `el-GR` ロケールのコンテナー イメージ。 | `sha256:4c4a115ae8daf53e344c1c4f838ebc68c3de2dae4d1f1aceb021425807d96ac0` |
| `en-au`                     | `en-AU` ロケールのコンテナー イメージ。 | `sha256:f18c31f2bc9e655b93f71049b40dae2213c7417169f7a4e42f603d5891857b2a` |
| `en-ca`                     | `en-CA` ロケールのコンテナー イメージ。 | `sha256:67f02cdb2285c2891aff8ff8d35ee20bad11f2d1cc1d67c461185466edefa5d6` |
| `en-gb`                     | `en-GB` ロケールのコンテナー イメージ。 | `sha256:ed606155b5f9b6c6dd68c0c1f5e48a0735bc4a5ded872655c0ef7de2bf084312` |
| `en-hk`                     | `en-HK` ロケールのコンテナー イメージ。 | `sha256:2fb6a64aaea5efdb2cac8bda2c7d437638fca93aa24268a45f2a395285e022df` |
| `en-ie`                     | `en-IE` ロケールのコンテナー イメージ。 | `sha256:9ddb64e481cec6449dfc48091092247fa401fcd48ab1d955c5186565f903bd34` |
| `en-in`                     | `en-IN` ロケールのコンテナー イメージ。 | `sha256:060a87ae817a82486966a4f10d1e872d30370ea58e297ca4c2018d0e034bfbe5` |
| `en-nz`                     | `en-NZ` ロケールのコンテナー イメージ。 | `sha256:ece4299bd7f02fe4403b53320cf55bb2e3ab65da3d94bfea09124c14955a3de3` |
| `en-ph`                     | `en-PH` ロケールのコンテナー イメージ。 | `sha256:6b47286a882122de8114942d426cbb8b4f1aded318032317b03a6b68237372e0` |
| `en-sg`                     | `en-SG` ロケールのコンテナー イメージ。 | `sha256:41fa2caec6a732736f75b682e0410b89ba5e12307cd6e2652986a2676a5dd560` |
| `en-us`                     | `en-US` ロケールのコンテナー イメージ。 | `sha256:80ae57602d8e66c6ed0366327a87c0ed5717b44c596b981a2b5be09c7f5a4c8a` |
| `en-za`                     | `en-ZA` ロケールのコンテナー イメージ。 | `sha256:705c125e5105b6eed37d745e2092d55ca8b6ccff22f4eeac9c2df958f36c72e9` |
| `es-ar`                     | `es-AR` ロケールのコンテナー イメージ。 | `sha256:67f794f16fdac457f0e0a84192e588611adb43777635b14706754c19fd90b130` |
| `es-bo`                     | `es-BO` ロケールのコンテナー イメージ。 | `sha256:94f755e70043dbe011424a0f756970f1d01ec51cb95a469531e3a6b0aa84aed1` |
| `es-cl`                     | `es-CL` ロケールのコンテナー イメージ。 | `sha256:c42eb56cbb48e0957f73793f83435c705ed0f857579acb020394025abdd760e2` |
| `es-co`                     | `es-CO` ロケールのコンテナー イメージ。 | `sha256:7cfacb01fdb80bd1b5e68d16f9e2741237ae4ec1a41a9121aed1be2622fc9f3f` |
| `es-cr`                     | `es-CR` ロケールのコンテナー イメージ。 | `sha256:7dbe5becdf4f3264764eb596d61781a2b2ee54bf9552bbb8f4db5e7fcf75d8f8` |
| `es-cu`                     | `es-CU` ロケールのコンテナー イメージ。 | `sha256:a1064b4498b7c5972a8a79ea84b78c2e1e7698c039eab49fd08963d11798ac61` |
| `es-do`                     | `es-DO` ロケールのコンテナー イメージ。 | `sha256:03cd0f0bae11df645dff52b15746e31493522db5399a18878df765b6aace0a80` |
| `es-ec`                     | `es-EC` ロケールのコンテナー イメージ。 | `sha256:3dc8d3f0842089edde4703abe8df3a219fb177afd5ac370c5b04c85abae4ca15` |
| `es-es`                     | `es-ES` ロケールのコンテナー イメージ。 | `sha256:7b0927c3b60bf38e995c57a27843680d9062d88611c49378dda8f71a4602f7a4` |
| `es-gt`                     | `es-GT` ロケールのコンテナー イメージ。 | `sha256:72e51683124c76255ec9280cd0641d6e44633199bda769ddb31336362f6e641d` |
| `es-hn`                     | `es-HN` ロケールのコンテナー イメージ。 | `sha256:a948970cd11e2597ba150291b2dcc72f2d59ad4f693933ef1f72c210f19fb663` |
| `es-mx`                     | `es-MX` ロケールのコンテナー イメージ。 | `sha256:b773cd7bbb5eba548bc468c2f6d50732e2553c5f8ba4b955404140def4c3f3fd` |
| `es-ni`                     | `es-NI` ロケールのコンテナー イメージ。 | `sha256:db73492bd83597c1fa47e7c4ab5eedbc1afa7662088fb03df2aaa5b737b5f837` |
| `es-pa`                     | `es-PA` ロケールのコンテナー イメージ。 | `sha256:d3a86e840438eb2278d0bbfdf1fc98a48fd744fb8c92118f6d3d6298c45a2b96` |
| `es-pe`                     | `es-PE` ロケールのコンテナー イメージ。 | `sha256:b60dae65bf1fe20e698ce32811373473d811bc363d4db093b643238f71461d4c` |
| `es-pr`                     | `es-PR` ロケールのコンテナー イメージ。 | `sha256:2a81a9d1b32c546ec03caeeaeddb1b26e5e00747c691f5be62f9d23c5ba84377` |
| `es-py`                     | `es-PY` ロケールのコンテナー イメージ。 | `sha256:c4b91cd5e017060a82a34f83d3f62a16b856313c02fea048d300abf149aadf67` |
| `es-sv`                     | `es-SV` ロケールのコンテナー イメージ。 | `sha256:2a5bddc5355d6eb0b101423c733d6cf067bafd0e152b63bf6c4dcd943ff561f3` |
| `es-us`                     | `es-US` ロケールのコンテナー イメージ。 | `sha256:f60037ad8dd2b40f588608a5eace8b0b9f3171d05d39a02c2dd1afe98ea7e18d` |
| `es-uy`                     | `es-UY` ロケールのコンテナー イメージ。 | `sha256:e302da84ee0264221f0e663470f579348664ddef37050bc0fe57c620264bae06` |
| `es-ve`                     | `es-VE` ロケールのコンテナー イメージ。 | `sha256:b6a79de315c73ec3301aa0cfa7ed920abbf8b6f80fd3d42637b785ee97a85584` |
| `et-ee`                     | `et-EE` ロケールのコンテナー イメージ。 | `sha256:2de931f1e6f38cdc2f54a08bc1e64a13876326d57784f0ad1c50384381790b05` |
| `fi-fi`                     | `fi-FI` ロケールのコンテナー イメージ。 | `sha256:47c1b3cceb8a6f0b2ea16160ba8c503d39ac77f44c254dc880b5e17d2aba4a4c` |
| `fr-ca`                     | `fr-CA` ロケールのコンテナー イメージ。 | `sha256:bf40fbfce8241e14656df47178d7b57f19022cc6b2598de5b337c6710eba99b6` |
| `fr-fr`                     | `fr-FR` ロケールのコンテナー イメージ。 | `sha256:93e0d58ed07d637c3e394ce80ee93524697063cb693da2aed9013660b2543702` |
| `ga-ie`                     | `ga-IE` ロケールのコンテナー イメージ。 | `sha256:1d239549ecf7f6bef5f9d258f5fd34f81fb0e5fff89c66dfec769e912b1cbf7b` |
| `gu-in`                     | `gu-IN` ロケールのコンテナー イメージ。 | `sha256:596f42a366a61d1cf05dedb81a4f373cfae2dc04e8bec3479bfec121417dd4fb` |
| `hi-in`                     | `hi-IN` ロケールのコンテナー イメージ。 | `sha256:fcdad9382db8fc7ff0a7ad59fa9fd4cd319ca258edff869b66d76031bcfee640` |
| `hr-hr`                     | `hr-HR` ロケールのコンテナー イメージ。 | `sha256:533a6420a4a98d4a2c947d26511e90651fc341c96b90a02615b38ce2a799f058` |
| `hu-hu`                     | `hu-HU` ロケールのコンテナー イメージ。 | `sha256:ec6b95c03d9d5030457c4a9e1fd8e07fbae24ec50b0bb3b2a95eadcd81a1d136` |
| `it-it`                     | `it-IT` ロケールのコンテナー イメージ。 | `sha256:67cc80b8159122c530913505fed0f7bc4edfd3d77b25bc34b6c6157d57178728` |
| `ja-jp`                     | `ja-JP` ロケールのコンテナー イメージ。 | `sha256:2b1f3b4220f8a7a44c8339e4c6a4b9a55f7583b5540f045997c9cab8364facb2` |
| `ko-kr`                     | `ko-KR` ロケールのコンテナー イメージ。 | `sha256:4703fd5e1c5020d5c58b1adde30e5209b1e6f21d0636bac11013dcf8da9340d3` |
| `lt-lt`                     | `lt-LT` ロケールのコンテナー イメージ。 | `sha256:58c2bb9cf2ead05fc77b3962ee7cef0e0eec33e32697757f65ae8925d55f87b8` |
| `lv-lv`                     | `lv-LV` ロケールのコンテナー イメージ。 | `sha256:dcdeed91559fb7e1b7d2ea70215ec373a59afa6b67468d13316af109314ca384` |
| `mr-in`                     | `mr-IN` ロケールのコンテナー イメージ。 | `sha256:06745d241654571428c219c38cd43b56e92b97eeb5aa6656ac726da79460afc1` |
| `mt-mt`                     | `mt-MT` ロケールのコンテナー イメージ。 | `sha256:3c85f1057b5942c5d2094055e7b9ecc6ef995905bbdabfad48bfefb805f436cf` |
| `nb-no`                     | `nb-NO` ロケールのコンテナー イメージ。 | `sha256:313f2fb20b8c2a18bd6ce5e7877899310575d390a2c3c54cd2519d0538393201` |
| `nl-nl`                     | `nl-NL` ロケールのコンテナー イメージ。 | `sha256:7c897fdb38661eb60f576c0a1a9d69bab9e44e7a70e8136fa3d12531cde0e4d7` |
| `pl-pl`                     | `pl-PL` ロケールのコンテナー イメージ。 | `sha256:9bf17aa5d4c577a440c770b6a63b66037a201cbea0202af4856257fde0548f0e` |
| `pt-br`                     | `pt-BR` ロケールのコンテナー イメージ。 | `sha256:08f0bb7f1454c5d6c740d218013f47c54bed17701e05c239364f5b2eba07692e` |
| `pt-pt`                     | `pt-PT` ロケールのコンテナー イメージ。 | `sha256:0643e1c342cf6d526620a46b3435c130702b9320a6075ede1351810956ed6ae9` |
| `ro-ro`                     | `ro-RO` ロケールのコンテナー イメージ。 | `sha256:139f83900395a0d1af99dc90e661238ca2fa0bc06c74cbac28631ba0399345bf` |
| `ru-ru`                     | `ru-RU` ロケールのコンテナー イメージ。 | `sha256:1c38423ccd1b8042d43eabe013f5b6989556610ada803b4367848b58c4832a76` |
| `sk-sk`                     | `sk-SK` ロケールのコンテナー イメージ。 | `sha256:2b33e5d5ae0cc46bd9a4ae860fe22f088903d4978b287df4eff6ae63b91566f3` |
| `sl-si`                     | `sl-SI` ロケールのコンテナー イメージ。 | `sha256:40a667412882bfe8073abf376fe94378d7c364e7b22aee410d7b6e99d65e55be` |
| `sv-se`                     | `sv-SE` ロケールのコンテナー イメージ。 | `sha256:d55464b46585fcfd86c420a30d11b10f3b5c9c0d70390b75f40fe9dbbeeefa99` |
| `ta-in`                     | `ta-IN` ロケールのコンテナー イメージ。 | `sha256:06f3f986ff92f16e963771da485695ec9e1da482b10f35babb2d54e260da23e7` |
| `te-in`                     | `te-IN` ロケールのコンテナー イメージ。 | `sha256:0566062d116cb06c3eb365dce6e86d9c46ce37293b11ef71c4e219c3a11ca559` |
| `th-th`                     | `th-TH` ロケールのコンテナー イメージ。 | `sha256:0fa6da985d839919fedb503625383dcda04de6bd39558f2f72b64410675b8f85` |
| `tr-tr`                     | `tr-TR` ロケールのコンテナー イメージ。 | `sha256:88418775c8a8df79aa52de03091b938b7a4efc708907556dfbe3e1d686050e81` |
| `zh-cn`                     | `zh-CN` ロケールのコンテナー イメージ。 | `sha256:9087a08cc455772515f5775a788cdde35d7f5bbe3aa3ba34ae99573fd87b29a1` |
| `zh-hk`                     | `zh-HK` ロケールのコンテナー イメージ。 | `sha256:372e1c256520e9ee84c4c400eae935c1d6b1d59adb2be4c4dbc56439db069ba0` |
| `zh-tw`                     | `zh-TW` ロケールのコンテナー イメージ。 | `sha256:8406a3be34530c7d654d1dfa1c593dd51b8946b480fe80a100e599e86385dc2b` |


# <a name="previous-version"></a>[以前のバージョン](#tab/previous)

`2.9.0-amd64-<locale>` のリリース ノート:

**機能**
* ID によってカスタム モデルをフェッチする際の問題に関する詳細なエラー。
* 既定では、会話の結果で推定がサポートされます。

`2.7.0-amd64-<locale>` のリリース ノート:

**機能**
* 次の新しいロケールのサポート:
    * ar-bh、ar-iq、ar-jo、ar-lb、ar-om、ar-sy
    * bg-bg
    * el-gr
    * en-hk、en-ie、en-ph、en-sg、en-za
    * es-ar、es-bo、es-cl、es-co、es-cr、es-cu、es-do、es-ec、es-gt、es-pa、es-pe、es-pr、es-py、es-sv、es-us、es-uy、es-ve
    * et-ee
    * ga-ie
    * hr-hr
    * hu-hu
    * lt-lt
    * lv-lv
    * mt-mt
    * ro-ro
    * sk-sk
    * sl-sl
* 句読点は、既定で有効です。

フレーズ リストが含まれているため、このコンテナー イメージのサイズが大きくなっていることに注意してください。 

`2.6.0-amd64-<locale>` のリリース ノート:

**機能**
* 最新のモデルにアップグレードし、.NET 3.1 に完全に移行しました
* Phraselist v2 のサポート
* フレーズ リストは次のロケールでサポートされています。
    * en-au
    * en-ca
    * en-gb
    * en-in
    * ja-JP
    * zh-cn
* 新しいロケール `cs-CZ` のサポート 
    * 大文字と小文字の区別と句読点は現在サポートされていません。

**修正**
* 信頼スコアがダイアライゼーション モードで常に 1 になるという問題を修正しました
* TextAnalytics 3.0 API を使用するように移行しました

フレーズ リストが含まれているため、このコンテナー イメージのサイズが大きくなっていることに注意してください。 

`2.5.0-amd64-<locale>` のリリース ノート:

**機能**
* Azure US Government Cloud のサポート

**修正**
* ダイアライゼーション モードで非 root ユーザーとして実行する場合の問題を修正しました

| イメージ タグ                  | Notes                                    |
|-----------------------------|:-----------------------------------------|
| `2.9.0-amd64-<locale>`      | `<locale>` を、以下に一覧表示されている使用可能なロケールのいずれかに置き換えます。 たとえば、「 `2.9.0-amd64-en-us` 」のように指定します。 |
| `2.7.0-amd64-<locale>`      | `<locale>` を、以下に一覧表示されている使用可能なロケールのいずれかに置き換えます。 たとえば、「 `2.7.0-amd64-en-us` 」のように指定します。 |
| `2.6.0-amd64-<locale>`      | `<locale>` を、以下に一覧表示されている使用可能なロケールのいずれかに置き換えます。 たとえば、「 `2.6.0-amd64-en-us` 」のように指定します。 |
| `2.5.0-amd64-<locale>`      | `<locale>` を、以下に一覧表示されている使用可能なロケールのいずれかに置き換えます。 たとえば、「 `2.5.0-amd64-en-us` 」のように指定します。 |


このコンテナー イメージには次のロケールを使用できます。

| v2.9.0 のロケール           | Notes                                    | ダイジェスト                                                                    |
|-----------------------------|:-----------------------------------------|:--------------------------------------------------------------------------|
| `ar-ae`                     | `ar-AE` ロケールのコンテナー イメージ。 | `sha256:08885bedb2993daf0c918ecdc6ec775f7982ffa5ca561e80ab9b8a103cde8194` |
| `ar-bh`                     | `ar-BH` ロケールのコンテナー イメージ。 | `sha256:41e7942e4026beaad93e50f199a6a2d855f77c74e60bc9636bf2bf2c7d3bd482` |
| `ar-eg`                     | `ar-EG` ロケールのコンテナー イメージ。 | `sha256:d27f383435770aa01bb4117ba2d50a05ec172a1da35c4920ab43cd0fb74f44c2` |
| `ar-iq`                     | `ar-IQ` ロケールのコンテナー イメージ。 | `sha256:ca2734a6bfc562c4c07981358051d281fb5e089815b9eac14c66a0e6f92e9858` |
| `ar-jo`                     | `ar-JO` ロケールのコンテナー イメージ。 | `sha256:57429ee8e95a76ec953f1b1f94b39a20507626cd7fe5431df826912e5b959e41` |
| `ar-kw`                     | `ar-KW` ロケールのコンテナー イメージ。 | `sha256:08885bedb2993daf0c918ecdc6ec775f7982ffa5ca561e80ab9b8a103cde8194` |
| `ar-lb`                     | `ar-LB` ロケールのコンテナー イメージ。 | `sha256:4c5fb6fdc08343e8640222583373effae3d03907cf1262a4fad3303df9385797` |
| `ar-om`                     | `ar-OM` ロケールのコンテナー イメージ。 | `sha256:5ffd280908e3ee65fcb7bea0b532844f9d8510044ab4c2c612dc3c235938ad0a` |
| `ar-qa`                     | `ar-QA` ロケールのコンテナー イメージ。 | `sha256:08885bedb2993daf0c918ecdc6ec775f7982ffa5ca561e80ab9b8a103cde8194` |
| `ar-sa`                     | `ar-SA` ロケールのコンテナー イメージ。 | `sha256:08885bedb2993daf0c918ecdc6ec775f7982ffa5ca561e80ab9b8a103cde8194` |
| `ar-sy`                     | `ar-SY` ロケールのコンテナー イメージ。 | `sha256:00f3d1fd6ccb857ccef8a72322336e7a097d04027411f0dcc5499b44229fb470` |
| `bg-bg`                     | `bg-BG` ロケールのコンテナー イメージ。 | `sha256:aa6ae12f786dcaa028e5867abba198effed875b6bc4cbafd4be37349e95dceef` |
| `ca-es`                     | `ca-ES` ロケールのコンテナー イメージ。 | `sha256:515a940ccd76ef1926bab3ad259e1cc7ac2bd90bb3860d28f83d0f6324b3f0fe` |
| `cs-cz`                     | `cs-CZ` ロケールのコンテナー イメージ。 | `sha256:03f6242d73de64c3eb3347400ea6e7408a8816bd96f3d6368ea2a8193accd457` |
| `da-dk`                     | `da-DK` ロケールのコンテナー イメージ。 | `sha256:ed6714e804ff2d1bbd41512c78906ad9b8827dfdfed0076a271817e075c2ec40` |
| `de-de`                     | `de-DE` ロケールのコンテナー イメージ。 | `sha256:386f2bb4c4b6ba797919ddcb5bbc9942bf8a03e774f9b01438f9bae0928414ef` |
| `el-gr`                     | `el-GR` ロケールのコンテナー イメージ。 | `sha256:28696d10c78404fec033794e6e6ae0bfd92b0dab5cf7eb1d24cc2cdfbfcb646d` |
| `en-au`                     | `en-AU` ロケールのコンテナー イメージ。 | `sha256:dd9ce70f83767a5bdc52fd62b96e09ce6f79ecc1903ed8e116753099b06b03cd` |
| `en-ca`                     | `en-CA` ロケールのコンテナー イメージ。 | `sha256:70095cf952565256f3a0927358d0fd802d28fe1c3b89b26ead31ba1127cd0b06` |
| `en-gb`                     | `en-GB` ロケールのコンテナー イメージ。 | `sha256:836bc38328636799ec9c8717618d51ab8b50ea2f0dc9663f342c4454938c9b23` |
| `en-hk`                     | `en-HK` ロケールのコンテナー イメージ。 | `sha256:eda3702d95d4ae3b64ceb93bda42e8522776e141a18b2a3dde3bc3fcf0e9a2b8` |
| `en-ie`                     | `en-IE` ロケールのコンテナー イメージ。 | `sha256:bfc2126fffb947bf10ac379efb70db3d2c7ee2c16dd541a5b86e03e73d7d477c` |
| `en-in`                     | `en-IN` ロケールのコンテナー イメージ。 | `sha256:5660d02eabf4e1e9f58e7993ed7e5917b1990b41ed35a484a715d7265400cd0b` |
| `en-nz`                     | `en-NZ` ロケールのコンテナー イメージ。 | `sha256:891c1805fd8011865de7371ffd4bde85d879341f2100e8053bbbc722d7c792bc` |
| `en-ph`                     | `en-PH` ロケールのコンテナー イメージ。 | `sha256:21d6d46398f940a769241fdfffec5658356e54b4127b44efe5e061724f7a7681` |
| `en-sg`                     | `en-SG` ロケールのコンテナー イメージ。 | `sha256:6f473b8ba56bad098c21a0c0496cb312dafcfb83dc1a2e1aff21011f6b39321d` |
| `en-us`                     | `en-US` ロケールのコンテナー イメージ。 | `sha256:20aa22d24e35f7d92ceac96d2cbab8ce46ee0ed7bb601f18fa867f1bd0bcf5ab` |
| `en-za`                     | `en-ZA` ロケールのコンテナー イメージ。 | `sha256:5e5ad2b016a1ceac500813e0a68ff4108ddf5a4ca98cb0aed4930b6d1e8920dd` |
| `es-ar`                     | `es-AR` ロケールのコンテナー イメージ。 | `sha256:b372d9e32e7b518bb9949d8db459bd4e300304e53aed1342aba65a054d4a4c25` |
| `es-bo`                     | `es-BO` ロケールのコンテナー イメージ。 | `sha256:d3538f3834c554ebebbdfe75e261a06f104dfa27143353601c3a6a3d41025129` |
| `es-cl`                     | `es-CL` ロケールのコンテナー イメージ。 | `sha256:0bb100ef5313b182a59c08949e4baf1086bde2c1a6bca3324c4e052f465f7632` |
| `es-co`                     | `es-CO` ロケールのコンテナー イメージ。 | `sha256:cdab27080ef3ded55dcf89cf85bc2ae16de1372f84a42d836ff5f20612b68a61` |
| `es-cr`                     | `es-CR` ロケールのコンテナー イメージ。 | `sha256:e4ea51ffa38f347adc7c0642d50237cfa045683f52b5e3e726e4c28688231d35` |
| `es-cu`                     | `es-CU` ロケールのコンテナー イメージ。 | `sha256:f81c0b7f774d64e673a1311d00604f5e4837fdba4d8fb4a2ab0c8bb8b7fde87d` |
| `es-do`                     | `es-DO` ロケールのコンテナー イメージ。 | `sha256:78035c54e649e34cd8276a402f9c9845e13bc40503da6c2f631698a16a049c67` |
| `es-ec`                     | `es-EC` ロケールのコンテナー イメージ。 | `sha256:e4e4d9c123e452f8ae89bf6cc1292a406f7b482668e36b48ef2fbb29f14c4360` |
| `es-es`                     | `es-ES` ロケールのコンテナー イメージ。 | `sha256:10a4ddd279633cc8696b00be77f6e9309494a560244a325982522aaa805806e7` |
| `es-gt`                     | `es-GT` ロケールのコンテナー イメージ。 | `sha256:a603a8f9c1778808df5d14e3fa1c7e993ef9cca3e0b515a4d4586c2c3a1d14b6` |
| `es-hn`                     | `es-HN` ロケールのコンテナー イメージ。 | `sha256:4f539f8019c489623868bf02f3c61ed4b66d3a85e89250a9b484717a91e9489e` |
| `es-mx`                     | `es-MX` ロケールのコンテナー イメージ。 | `sha256:20fc3806f08ad4e6fd5fb1f71318f1f5b591e2085ee4cbba2f25ea06135e5f6a` |
| `es-ni`                     | `es-NI` ロケールのコンテナー イメージ。 | `sha256:d65520a4f628f6a416171ac58341579fdffba97ddd2941a910bda385d31c735d` |
| `es-pa`                     | `es-PA` ロケールのコンテナー イメージ。 | `sha256:d38ea88613f5db6d6d9f879ef92a204c524bb27766848b825d1e6ce2a9b13cf7` |
| `es-pe`                     | `es-PE` ロケールのコンテナー イメージ。 | `sha256:02205d1ecc29feed3ac8442dbdc1855c419749d9dcbd98028a5d1619166f0328` |
| `es-pr`                     | `es-PR` ロケールのコンテナー イメージ。 | `sha256:c9c3e1ac800120a14f472c8be62730a489e00f29df29fe770a56429ea1c09ef5` |
| `es-py`                     | `es-PY` ロケールのコンテナー イメージ。 | `sha256:859c24c40e65bc19a866218466eb7678f71205bedfcb6ee3180b6cb721194b9a` |
| `es-sv`                     | `es-SV` ロケールのコンテナー イメージ。 | `sha256:036f13d34005f5d6634387c9d13c3535724795b0d6cad832fc46363609fc2f11` |
| `es-us`                     | `es-US` ロケールのコンテナー イメージ。 | `sha256:b8eb300d0a11dc397d0bab02e1f6b26de6091595fd052ebb607f196c28d16f1c` |
| `es-uy`                     | `es-UY` ロケールのコンテナー イメージ。 | `sha256:0ffba124ecd79777ca08055689a1d853916ccd8c8f2806d0001edf5eb4aa42fa` |
| `es-ve`                     | `es-VE` ロケールのコンテナー イメージ。 | `sha256:4d7caf48264eaf18bb2d07b0258d6f64b7c26815fdbdf812718dd8e88f1a6d1e` |
| `et-ee`                     | `et-EE` ロケールのコンテナー イメージ。 | `sha256:310abdc1a8490990a99ce061f04c9d49cafb7a452fbfdc2790de6f60e1505c6c` |
| `fi-fi`                     | `fi-FI` ロケールのコンテナー イメージ。 | `sha256:8f209d30b2d148224b296c2d2c204b5970fbe7aaf5eb3289cf8b6644bfd78373` |
| `fr-ca`                     | `fr-CA` ロケールのコンテナー イメージ。 | `sha256:11b718d4b86d606b198e47deaa25f6ce164cfc53267048e3d2dbe1bc8500cc5a` |
| `fr-fr`                     | `fr-FR` ロケールのコンテナー イメージ。 | `sha256:7a4264a0e9560e6aa3fdee80c3e3f55a0e26cddce8ebbeb7a9c87693ab451a25` |
| `ga-ie`                     | `ga-IE` ロケールのコンテナー イメージ。 | `sha256:bbc764ac08b2ef10ac58a8f9534d4d375109fdf16ab75c8cdbf2d57aa692d3e2` |
| `gu-in`                     | `gu-IN` ロケールのコンテナー イメージ。 | `sha256:2d0a83b7bcf1cfc50cf013c95442519e5236a146b7968e75e129b3a5c33ad3a1` |
| `hi-in`                     | `hi-IN` ロケールのコンテナー イメージ。 | `sha256:f0ee8f259035ac5dd9ef38807495d0f8d989ddbb8eacf83893f1fea22265e6b4` |
| `hr-hr`                     | `hr-HR` ロケールのコンテナー イメージ。 | `sha256:6101ecac9f5f35c1ea1b8cd8e52fdbbc1be2582e4f3e385c16509fd95a002217` |
| `hu-hu`                     | `hu-HU` ロケールのコンテナー イメージ。 | `sha256:9e94c4d6fff73058ce4eef609b8404430a429c6961648655c915cb2fac10656f` |
| `it-it`                     | `it-IT` ロケールのコンテナー イメージ。 | `sha256:44986ad44bb53eaf350e0865e62ea5ba7f37d1f5b52e388f61f56fd7afe8ff32` |
| `ja-jp`                     | `ja-JP` ロケールのコンテナー イメージ。 | `sha256:6b7aaa828d1b2d2fce1831e540e08ba60307088b90ca32e96fd002a67aff926b` |
| `ko-kr`                     | `ko-KR` ロケールのコンテナー イメージ。 | `sha256:1abeda544a7579daac7f8b8f8d34a2cc63b4bd3631e474315d424973ae024ab0` |
| `lt-lt`                     | `lt-LT` ロケールのコンテナー イメージ。 | `sha256:455da50a7db591df7be69d7cd361a77734b9249101d8cf86b807f0350b5167ef` |
| `lv-lv`                     | `lv-LV` ロケールのコンテナー イメージ。 | `sha256:676e17b6223e35d1897b46536e6f523e1d18b78f834b62ec00bb126ad3a2e71a` |
| `mr-in`                     | `mr-IN` ロケールのコンテナー イメージ。 | `sha256:dbfb97e52dc4b4c71dec1a9e622714f004b1e59d7900260e09a85bf15912fccd` |
| `mt-mt`                     | `mt-MT` ロケールのコンテナー イメージ。 | `sha256:19f7f644ae3a0639fdcc53acc065d0e534b74c07f8c095418d4d4d444c566bf1` |
| `nb-no`                     | `nb-NO` ロケールのコンテナー イメージ。 | `sha256:d3a13ab6fa2eb5d5ca0e3281b1092452650e9ede8749f6edcab990e3bbb8d198` |
| `nl-nl`                     | `nl-NL` ロケールのコンテナー イメージ。 | `sha256:7ad5e61f9a72c600bdc79e4c04ac63c239951ac4c0d44e02fe0607a6aff356cc` |
| `pl-pl`                     | `pl-PL` ロケールのコンテナー イメージ。 | `sha256:fe6a4812534d704b145b84fd8857fb3d9052f67fcbbd5d490c5902082e295195` |
| `pt-br`                     | `pt-BR` ロケールのコンテナー イメージ。 | `sha256:adcd34941d4ace7db01bd476d61c9bbafe071419932b4cfae5231cf202af3a14` |
| `pt-pt`                     | `pt-PT` ロケールのコンテナー イメージ。 | `sha256:0534a7e4b391f1ee666b248a274879c081496ed4939b0ad33154d8a96fd67f94` |
| `ro-ro`                     | `ro-RO` ロケールのコンテナー イメージ。 | `sha256:091ea4a31652ff9dbc6259636f6c12b0ceb79a269e2cf3cdec677a1914b6a64e` |
| `ru-ru`                     | `ru-RU` ロケールのコンテナー イメージ。 | `sha256:5eef3ae8afb445e60bb913edd6eed1415abb0bfbc439978f69f4cba7b61c8e6e` |
| `sk-sk`                     | `sk-SK` ロケールのコンテナー イメージ。 | `sha256:98709e9349d889b57933317005af42770e47ce8178a7d9c737d9fbdd81148478` |
| `sl-si`                     | `sl-SI` ロケールのコンテナー イメージ。 | `sha256:3a9139334c4780dc6f6a9b0f15fba5292e16ecf1f5d45fe49a9c8ef3b0e110b3` |
| `sv-se`                     | `sv-SE` ロケールのコンテナー イメージ。 | `sha256:b29b2a65d83c20d65ba4e4fbca66f9fc07e536e161f90448c2bb360eb8de1e55` |
| `ta-in`                     | `ta-IN` ロケールのコンテナー イメージ。 | `sha256:4302e1d979b24a23595ee2b1fd074a57ee36166ce9ac400a3deb397341ae52b2` |
| `te-in`                     | `te-IN` ロケールのコンテナー イメージ。 | `sha256:69be11a63199d9a6f63ac346e689051ba9cd5214894b110da2879aaa0f4a8e88` |
| `th-th`                     | `th-TH` ロケールのコンテナー イメージ。 | `sha256:2e4167dacdcb2c9d91930356ebae311b6b33ceb3e85f908422e880edbd42da64` |
| `tr-tr`                     | `tr-TR` ロケールのコンテナー イメージ。 | `sha256:d46289ee9ba71c9c1dbbefa5da439e71310af74633c9d6d6d448d2ebee60da02` |
| `zh-cn`                     | `zh-CN` ロケールのコンテナー イメージ。 | `sha256:49eeee500e07ffd3056ba8aab314d6c8458399a8c0d6d44ce1d9aebf50ddca06` |
| `zh-hk`                     | `zh-HK` ロケールのコンテナー イメージ。 | `sha256:5a3251ad6df9565d44dd422de4fa0d83a9b50c8a80ec15213403482940d2b2fc` |
| `zh-tw`                     | `zh-TW` ロケールのコンテナー イメージ。 | `sha256:2c45dd90b0c19d7f12b1be44d3e85fe2603cea2389c2877b79d6de351839cf6a` |

| v2.7.0 のロケール           | Notes                                    | ダイジェスト                                                                   |
|-----------------------------|:-----------------------------------------|:-------------------------------------------------------------------------|
| `ar-ae`                     | `ar-AE` ロケールのコンテナー イメージ。 | `sha256:c8e99e71e6740cf671f3bf79de8b7dd890122cb674eedd2440e71e7cbc4c66b` |
| `ar-bh`                     | `ar-BH` ロケールのコンテナー イメージ。 | `sha256:5a2c140661f50d0c95587121ec1ab8895289f4dda5b3ad14074413e869e6bd4` |
| `ar-eg`                     | `ar-EG` ロケールのコンテナー イメージ。 | `sha256:783bb8321fcfb7890b0c99935099f7e84c85a698c2fe0031c661e265358d79c` |
| `ar-iq`                     | `ar-IQ` ロケールのコンテナー イメージ。 | `sha256:abd0101f73c1cf71f30da7b11b93d2a7ac8877dbfcfc2d34553d20705aca7a2` |
| `ar-jo`                     | `ar-JO` ロケールのコンテナー イメージ。 | `sha256:d4c7fd2a1637e163aa106c23b6a759e8c78366c60ece83b3aabfe93ebabae07` |
| `ar-kw`                     | `ar-KW` ロケールのコンテナー イメージ。 | `sha256:c8e99e71e6740cf671f3bf79de8b7dd890122cb674eedd2440e71e7cbc4c66b` |
| `ar-lb`                     | `ar-LB` ロケールのコンテナー イメージ。 | `sha256:20e5c9105e86625c72de54290a6eb07630d35c3760f729c4b855e3661583dfe` |
| `ar-om`                     | `ar-OM` ロケールのコンテナー イメージ。 | `sha256:97f1b44f2cbb837a2ef86441a0a52a07f706240edb6ef6618ee4db8cbbe1c19` |
| `ar-qa`                     | `ar-QA` ロケールのコンテナー イメージ。 | `sha256:c8e99e71e6740cf671f3bf79de8b7dd890122cb674eedd2440e71e7cbc4c66b` |
| `ar-sa`                     | `ar-SA` ロケールのコンテナー イメージ。 | `sha256:c8e99e71e6740cf671f3bf79de8b7dd890122cb674eedd2440e71e7cbc4c66b` |
| `ar-sy`                     | `ar-SY` ロケールのコンテナー イメージ。 | `sha256:51980a2e2c3dd3548deedcedaf5fc688db602a5eced1a4b7df7d10750393623` |
| `bg-bg`                     | `bg-BG` ロケールのコンテナー イメージ。 | `sha256:1c1acf0fbb353ebb04692f37eb4d4cdf0b4e309720dd7e709001dada0d1ea81` |
| `ca-es`                     | `ca-ES` ロケールのコンテナー イメージ。 | `sha256:c60baa0007f61c7652b97b49645215de63411125d627c974c09222e316df204` |
| `cs-cz`                     | `cs-CZ` ロケールのコンテナー イメージ。 | `sha256:3fa09fc3a6bde6b77df2444aae8fc78b5f25fb9010171d1682db116ea5801f5` |
| `da-dk`                     | `da-DK` ロケールのコンテナー イメージ。 | `sha256:4b26dbba50c2771943880b68e0e4ea0713d0e3bb8bad884454849bccc9e94a3` |
| `de-de`                     | `de-DE` ロケールのコンテナー イメージ。 | `sha256:5109ed80b1fecf4db0328adcd50528d0aa9e726b5fc84587c40aaea4e91256d` |
| `el-gr`                     | `el-GR` ロケールのコンテナー イメージ。 | `sha256:fc8b466c588bf097efac2b79454d5ac0df5c6990398f07ede9be7e1d536e4bd` |
| `en-au`                     | `en-AU` ロケールのコンテナー イメージ。 | `sha256:3461892a27fc3eb3f9610b2def00bc15f380c6b9797c90ceca19e6abb55f6a6` |
| `en-ca`                     | `en-CA` ロケールのコンテナー イメージ。 | `sha256:a0509be39785f1e869bd96ab10e7c07d3f4e61c9aa17ff5900076e7bd64ba11` |
| `en-gb`                     | `en-GB` ロケールのコンテナー イメージ。 | `sha256:1b976fc7ac109e61dcf74af3652c12535e3db92931d2d0bb2ea59bd46f9efed` |
| `en-hk`                     | `en-HK` ロケールのコンテナー イメージ。 | `sha256:0b1e1df101f978869c98f6e50632712016b8311fc89b334e7f44e968d64bf2f` |
| `en-ie`                     | `en-IE` ロケールのコンテナー イメージ。 | `sha256:c5ba0d3c7219ce39f0b918a51a7cae8a65c277f564279cad920e068725aa39f` |
| `en-in`                     | `en-IN` ロケールのコンテナー イメージ。 | `sha256:e907f07be498f024103f6fe6abffa23e242bf3585724741b29a2f3f41d0899c` |
| `en-nz`                     | `en-NZ` ロケールのコンテナー イメージ。 | `sha256:66845f6ce20ae71d609867c6eb4772366ce042499e4bcdce4c1b579daf7fad7` |
| `en-ph`                     | `en-PH` ロケールのコンテナー イメージ。 | `sha256:e7874653bf66b1a1ab344b3391eb8767be34260b7f11b62fd057cbe17b805b2` |
| `en-sg`                     | `en-SG` ロケールのコンテナー イメージ。 | `sha256:827cdb158280e6f4037f4815410c7aa78abf9c6467876c1504aecfef787bdd7` |
| `en-us`                     | `en-US` ロケールのコンテナー イメージ。 | `sha256:248d17340055e3e137219ddc234c605e6a53ceead136ea55c9697c352da6a8d` |
| `en-za`                     | `en-ZA` ロケールのコンテナー イメージ。 | `sha256:a8abc99f498db7088bb25acec47da81e90b6a5eaa1c6f78e0f9a314d839d0ae` |
| `es-ar`                     | `es-AR` ロケールのコンテナー イメージ。 | `sha256:edf78429630851b6eb01f54f8a8a1aeeda9971c6a834403a204662eda22b3b9` |
| `es-bo`                     | `es-BO` ロケールのコンテナー イメージ。 | `sha256:5832b44f1da2f6b9a097c99babfbc370d8d0eabe1ff8daabec2c3f482dc9d63` |
| `es-cl`                     | `es-CL` ロケールのコンテナー イメージ。 | `sha256:409a712b96235e154472134f96ff9272265f1e5b555e00ad03c2260b0781009` |
| `es-co`                     | `es-CO` ロケールのコンテナー イメージ。 | `sha256:99792bc083dc16e0edf15491e6a840d786c9140b747551563a8d98f66f0b415` |
| `es-cr`                     | `es-CR` ロケールのコンテナー イメージ。 | `sha256:21fe14a538e5b8b2d288b00b8f5a02d87469e285f32e725155042079f336ac9` |
| `es-cu`                     | `es-CU` ロケールのコンテナー イメージ。 | `sha256:05d40eae01cec4c42c4febd379cd61373eb43d0aacfd47b988bb95e6a6ad216` |
| `es-do`                     | `es-DO` ロケールのコンテナー イメージ。 | `sha256:73dd0e0d4f39a259563ee7cc18c2e72c9ab20c52905fe343e0413ca7c4b3f0d` |
| `es-ec`                     | `es-EC` ロケールのコンテナー イメージ。 | `sha256:c3e69139ef365fe9332b5b68b43458242c7dad9d9f2b557431272306e81cb9e` |
| `es-es`                     | `es-ES` ロケールのコンテナー イメージ。 | `sha256:bd83fcfc116ba645a0e12a7a93b6ada74a8f701172f826a91c5f223a1dbaa61` |
| `es-gt`                     | `es-GT` ロケールのコンテナー イメージ。 | `sha256:5bb9b18b91b74e123e3720893d88bfcb0a87dac31a1f7171d23c7cb1fa09fee` |
| `es-hn`                     | `es-HN` ロケールのコンテナー イメージ。 | `sha256:941d108a4b76eb554e8f13cf5090665a702de3ebf35b75e4350f0916dfccd72` |
| `es-mx`                     | `es-MX` ロケールのコンテナー イメージ。 | `sha256:cebea03732781b4425500d162ae6580bbd7ce9b5f4ede988c4570fe311d8567` |
| `es-ni`                     | `es-NI` ロケールのコンテナー イメージ。 | `sha256:8ba165f94ad840936ebd0af17a0a63aa08a6292e7ad9029f5b93eef41165eb9` |
| `es-pa`                     | `es-PA` ロケールのコンテナー イメージ。 | `sha256:c61b7f1b6801a03c3eab0dd1aede87017a86bc7368ded2f8bad8d9e5f60d0d3` |
| `es-pe`                     | `es-PE` ロケールのコンテナー イメージ。 | `sha256:447a3ab3f302aba24d201d9f5b2877ffcd64dfd5e9d6b88d9924847160b2de2` |
| `es-pr`                     | `es-PR` ロケールのコンテナー イメージ。 | `sha256:a53b3295c986e91ee8cf93ebe1057b997c76ef7f99913508b859311a194fdd4` |
| `es-py`                     | `es-PY` ロケールのコンテナー イメージ。 | `sha256:85b3f75e75e63e29521daf772ee68a59ac2428579512501aa81dc51a2315652` |
| `es-sv`                     | `es-SV` ロケールのコンテナー イメージ。 | `sha256:db5ece7ba536e38d5de59cd37807630ab76589dcf1c97e253f98d7f44d9424e` |
| `es-us`                     | `es-US` ロケールのコンテナー イメージ。 | `sha256:99f2743725bb71e25543484f49bcfde14584ccbbaaa912678938d69d965075a` |
| `es-uy`                     | `es-UY` ロケールのコンテナー イメージ。 | `sha256:a3e11c16a97a1ae76408d812b2fee1e4b3ba07160bbcb62a22814523568ee5d` |
| `es-ve`                     | `es-VE` ロケールのコンテナー イメージ。 | `sha256:8cb431aafd84263ead8de946377c1d3f2ddfa7e172b8a4c5aa7ba477c5b41f0` |
| `et-ee`                     | `et-EE` ロケールのコンテナー イメージ。 | `sha256:943e7cf894e9d75341a58993104824c1c8cd8da1322cc5a732e9d53882c6523` |
| `fi-fi`                     | `fi-FI` ロケールのコンテナー イメージ。 | `sha256:35658e9dce796cb96a1371f250398e86351ea1b5ada080da7ce8471b30c7cae` |
| `fr-ca`                     | `fr-CA` ロケールのコンテナー イメージ。 | `sha256:62256cad671e8baa03fdd4c5f4eca7d5c5effedd64cafd9020ba72c9c4210e0` |
| `fr-fr`                     | `fr-FR` ロケールのコンテナー イメージ。 | `sha256:b385993232d9daa327d1a7b067268927b17f36eed3e8d423748794544c62746` |
| `ga-ie`                     | `ga-IE` ロケールのコンテナー イメージ。 | `sha256:ab9abdb993b0f7487edda8200f1393ac44ba4888c0f444a02afb6c85ca3e393` |
| `gu-in`                     | `gu-IN` ロケールのコンテナー イメージ。 | `sha256:328e69488f2948722d7ccc97e266071f61a8c9f65cd671688490955806526de` |
| `hi-in`                     | `hi-IN` ロケールのコンテナー イメージ。 | `sha256:b9b0bfec80aa53d06ea2cbd9097f753ec5caaf00ac2f00321ae7ad916fd7fa6` |
| `hr-hr`                     | `hr-HR` ロケールのコンテナー イメージ。 | `sha256:ab849cd2eeea682f8958bba8986fe90f0f7bb3b447512a10cf464e8e1ce4ea5` |
| `hu-hu`                     | `hu-HU` ロケールのコンテナー イメージ。 | `sha256:30f239b155d91523442cf74a1f2732304fa2b50ae7b786833bb6a020b982621` |
| `it-it`                     | `it-IT` ロケールのコンテナー イメージ。 | `sha256:288f95413870eb9d33bf1dabfa6fbd6b55b0faa52e4d5face3171d1dd4ddbdd` |
| `ja-jp`                     | `ja-JP` ロケールのコンテナー イメージ。 | `sha256:e3ab37a80c215dec565eca212f57eb81887fc2894452868dff92e3bd42c4bb9` |
| `ko-kr`                     | `ko-KR` ロケールのコンテナー イメージ。 | `sha256:c1208b8459333b606af516cd7806e9d4d5e002247bb1225e1f246563b356890` |
| `lt-lt`                     | `lt-LT` ロケールのコンテナー イメージ。 | `sha256:8dec331161d3c29fc65ba6651fcc6cfe69fa314519f408b5f9f8eb27da09830` |
| `lv-lv`                     | `lv-LV` ロケールのコンテナー イメージ。 | `sha256:7cf31282910b339666bb2b0a555caa7fc6ae414eea4423a41f35c3527f83235` |
| `mr-in`                     | `mr-IN` ロケールのコンテナー イメージ。 | `sha256:9cb012bd58ef7723d4905d6fa3c1fde96e33c354b3d96d4e3ff69cf6e1bfe3a` |
| `mt-mt`                     | `mt-MT` ロケールのコンテナー イメージ。 | `sha256:a0094c032ea555b168ec5751ab3257337d902d526e9ae335671fb751a352378` |
| `nb-no`                     | `nb-NO` ロケールのコンテナー イメージ。 | `sha256:6bbc326e20a6a785b1ca33143b42a060858efb67b863a267d6efb7aebb48f87` |
| `nl-nl`                     | `nl-NL` ロケールのコンテナー イメージ。 | `sha256:94b4ddf4cc80fa666e422f8416aea3f98ebe4842dfe9b1f4bfea7c47eb61127` |
| `pl-pl`                     | `pl-PL` ロケールのコンテナー イメージ。 | `sha256:58e5f78bf772c3c8cbd5f0c5d6e67f5348e04e3f893d84738a2a3e964bab256` |
| `pt-br`                     | `pt-BR` ロケールのコンテナー イメージ。 | `sha256:f500ef956bd28807f40df1f9f0520e437c5084f61a3be6d1379e746887d5b7c` |
| `pt-pt`                     | `pt-PT` ロケールのコンテナー イメージ。 | `sha256:c841d2dbe5f40adf6039242c106985febb1a44212feb55d9769fe31134ec116` |
| `ro-ro`                     | `ro-RO` ロケールのコンテナー イメージ。 | `sha256:93271c39c0a134e987a069c2a65289acff9869ae0d90fdcb39928c9ef0fd86b` |
| `ru-ru`                     | `ru-RU` ロケールのコンテナー イメージ。 | `sha256:8d6b3c600e56cc96813b8c14b7916c5539a20ba561dc1c6d5bbef6285d6eef6` |
| `sk-sk`                     | `sk-SK` ロケールのコンテナー イメージ。 | `sha256:6d604092cc6c964663a1c97d91c8f1c8cf4b46d07427d03f7041c0cc55eb521` |
| `sl-si`                     | `sl-SI` ロケールのコンテナー イメージ。 | `sha256:f237ed58fedefcc749e74be1258cc70e5a690ee6c5a6b6388bd24075faa61da` |
| `sv-se`                     | `sv-SE` ロケールのコンテナー イメージ。 | `sha256:da4233e6658b00eefdadb9d4acd889c6550a5e2a4a7af7a9f915c878abd4c9c` |
| `ta-in`                     | `ta-IN` ロケールのコンテナー イメージ。 | `sha256:22b77606d25e9c2f52bf3cad6218782b4719f6a9dcfadc770468d266758a56c` |
| `te-in`                     | `te-IN` ロケールのコンテナー イメージ。 | `sha256:7f4d11372862ca1d65fc9b868e2d775701b8e6eabd786c90c4e9ab82ba86e88` |
| `th-th`                     | `th-TH` ロケールのコンテナー イメージ。 | `sha256:69033bcd7c0f59d31bafec6c2b7a9ff343928cdd58c16105415c291d555d37b` |
| `tr-tr`                     | `tr-TR` ロケールのコンテナー イメージ。 | `sha256:4b7d339846a0d371dfe25aa2e626f131003c01329c9a1da468eb3703ef176ea` |
| `zh-cn`                     | `zh-CN` ロケールのコンテナー イメージ。 | `sha256:a428459830fb766083212f71c5638a65ce30d8dd84f6c624ae22768e8a76976` |
| `zh-hk`                     | `zh-HK` ロケールのコンテナー イメージ。 | `sha256:7a2903462b67336a6ce4c8e2faac42052f0a4392d1d5eb3839758cc8d0429f1` |
| `zh-tw`                     | `zh-TW` ロケールのコンテナー イメージ。 | `sha256:30fd2b3660e047d24a46fbba14ba282f15bc0339ec93f49afd0d02ff4069146` |

| v2.6.0 のロケール           | Notes                                    |
|-----------------------------|:-----------------------------------------|
| `ar-ae`                     | `ar-AE` ロケールのコンテナー イメージ。 |
| `ar-eg`                     | `ar-EG` ロケールのコンテナー イメージ。 |
| `ar-kw`                     | `ar-KW` ロケールのコンテナー イメージ。 |
| `ar-qa`                     | `ar-QA` ロケールのコンテナー イメージ。 |
| `ar-sa`                     | `ar-SA` ロケールのコンテナー イメージ。 |
| `ca-es`                     | `ca-ES` ロケールのコンテナー イメージ。 |
| `cs-cz`                     | `cs-CZ` ロケールのコンテナー イメージ。 |
| `da-dk`                     | `da-DK` ロケールのコンテナー イメージ。 |
| `de-de`                     | `de-DE` ロケールのコンテナー イメージ。 |
| `en-au`                     | `en-AU` ロケールのコンテナー イメージ。 |
| `en-ca`                     | `en-CA` ロケールのコンテナー イメージ。 |
| `en-gb`                     | `en-GB` ロケールのコンテナー イメージ。 |
| `en-in`                     | `en-IN` ロケールのコンテナー イメージ。 |
| `en-nz`                     | `en-NZ` ロケールのコンテナー イメージ。 |
| `en-us`                     | `en-US` ロケールのコンテナー イメージ。 |
| `es-es`                     | `es-ES` ロケールのコンテナー イメージ。 |
| `es-mx`                     | `es-MX` ロケールのコンテナー イメージ。 |
| `fi-fi`                     | `fi-FI` ロケールのコンテナー イメージ。 |
| `fr-ca`                     | `fr-CA` ロケールのコンテナー イメージ。 |
| `fr-fr`                     | `fr-FR` ロケールのコンテナー イメージ。 |
| `gu-in`                     | `gu-IN` ロケールのコンテナー イメージ。 |
| `hi-in`                     | `hi-IN` ロケールのコンテナー イメージ。 |
| `it-it`                     | `it-IT` ロケールのコンテナー イメージ。 |
| `ja-jp`                     | `ja-JP` ロケールのコンテナー イメージ。 |
| `ko-kr`                     | `ko-KR` ロケールのコンテナー イメージ。 |
| `mr-in`                     | `mr-IN` ロケールのコンテナー イメージ。 |
| `nb-no`                     | `nb-NO` ロケールのコンテナー イメージ。 |
| `nl-nl`                     | `nl-NL` ロケールのコンテナー イメージ。 |
| `pl-pl`                     | `pl-PL` ロケールのコンテナー イメージ。 |
| `pt-br`                     | `pt-BR` ロケールのコンテナー イメージ。 |
| `pt-pt`                     | `pt-PT` ロケールのコンテナー イメージ。 |
| `ru-ru`                     | `ru-RU` ロケールのコンテナー イメージ。 |
| `sv-se`                     | `sv-SE` ロケールのコンテナー イメージ。 |
| `ta-in`                     | `ta-IN` ロケールのコンテナー イメージ。 |
| `te-in`                     | `te-IN` ロケールのコンテナー イメージ。 |
| `th-th`                     | `th-TH` ロケールのコンテナー イメージ。 |
| `tr-tr`                     | `tr-TR` ロケールのコンテナー イメージ。 |
| `zh-cn`                     | `zh-CN` ロケールのコンテナー イメージ。 |
| `zh-hk`                     | `zh-HK` ロケールのコンテナー イメージ。 |
| `zh-tw`                     | `zh-TW` ロケールのコンテナー イメージ。 |

| v2.5.0 のロケール           | Notes                                    |
|-----------------------------|:-----------------------------------------|
| `ar-ae`                     | `ar-AE` ロケールのコンテナー イメージ。 |
| `ar-eg`                     | `ar-EG` ロケールのコンテナー イメージ。 |
| `ar-kw`                     | `ar-KW` ロケールのコンテナー イメージ。 |
| `ar-qa`                     | `ar-QA` ロケールのコンテナー イメージ。 |
| `ar-sa`                     | `ar-SA` ロケールのコンテナー イメージ。 |
| `ca-es`                     | `ca-ES` ロケールのコンテナー イメージ。 |
| `da-dk`                     | `da-DK` ロケールのコンテナー イメージ。 |
| `de-de`                     | `de-DE` ロケールのコンテナー イメージ。 |
| `en-au`                     | `en-AU` ロケールのコンテナー イメージ。 |
| `en-ca`                     | `en-CA` ロケールのコンテナー イメージ。 |
| `en-gb`                     | `en-GB` ロケールのコンテナー イメージ。 |
| `en-in`                     | `en-IN` ロケールのコンテナー イメージ。 |
| `en-nz`                     | `en-NZ` ロケールのコンテナー イメージ。 |
| `en-us`                     | `en-US` ロケールのコンテナー イメージ。 |
| `es-es`                     | `es-ES` ロケールのコンテナー イメージ。 |
| `es-mx`                     | `es-MX` ロケールのコンテナー イメージ。 |
| `fi-fi`                     | `fi-FI` ロケールのコンテナー イメージ。 |
| `fr-ca`                     | `fr-CA` ロケールのコンテナー イメージ。 |
| `fr-fr`                     | `fr-FR` ロケールのコンテナー イメージ。 |
| `gu-in`                     | `gu-IN` ロケールのコンテナー イメージ。 |
| `hi-in`                     | `hi-IN` ロケールのコンテナー イメージ。 |
| `it-it`                     | `it-IT` ロケールのコンテナー イメージ。 |
| `ja-jp`                     | `ja-JP` ロケールのコンテナー イメージ。 |
| `ko-kr`                     | `ko-KR` ロケールのコンテナー イメージ。 |
| `mr-in`                     | `mr-IN` ロケールのコンテナー イメージ。 |
| `nb-no`                     | `nb-NO` ロケールのコンテナー イメージ。 |
| `nl-nl`                     | `nl-NL` ロケールのコンテナー イメージ。 |
| `pl-pl`                     | `pl-PL` ロケールのコンテナー イメージ。 |
| `pt-br`                     | `pt-BR` ロケールのコンテナー イメージ。 |
| `pt-pt`                     | `pt-PT` ロケールのコンテナー イメージ。 |
| `ru-ru`                     | `ru-RU` ロケールのコンテナー イメージ。 |
| `sv-se`                     | `sv-SE` ロケールのコンテナー イメージ。 |
| `ta-in`                     | `ta-IN` ロケールのコンテナー イメージ。 |
| `te-in`                     | `te-IN` ロケールのコンテナー イメージ。 |
| `th-th`                     | `th-TH` ロケールのコンテナー イメージ。 |
| `tr-tr`                     | `tr-TR` ロケールのコンテナー イメージ。 |
| `zh-cn`                     | `zh-CN` ロケールのコンテナー イメージ。 |
| `zh-hk`                     | `zh-HK` ロケールのコンテナー イメージ。 |
| `zh-tw`                     | `zh-TW` ロケールのコンテナー イメージ。 |

---

## <a name="text-to-speech"></a>テキスト読み上げ

[テキスト読み上げ][sp-tts]コンテナー イメージは `mcr.microsoft.com` コンテナー レジストリ シンジケートにあります。 `azure-cognitive-services/speechservices/` リポジトリ内にあり、`text-to-speech` という名前が付いています。 完全修飾コンテナー イメージ名は `mcr.microsoft.com/azure-cognitive-services/speechservices/text-to-speech` です。

このコンテナー イメージには次のタグを利用できます。 また、[MCR でタグ](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/text-to-speech/tags/list)の完全な一覧を確認することもできます。


# <a name="latest-version"></a>[最新バージョン](#tab/current)

`1.12.0-amd64-<locale-and-voice>` のリリース ノート:

**機能**
* 最新のモデルにアップグレードします。

| イメージ タグ                                  | Notes                                                                                                         |
|---------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| `latest`                                    | `en-US` ロケールと `en-US-AriaRUS` 音声のコンテナー イメージ。                                            | 
| `1.12.0-amd64-<locale-and-voice>`           | `<locale>` を、以下に一覧表示されている使用可能なロケールのいずれかに置き換えます。 たとえば、「 `1.12.0-amd64-en-us-ariarus` 」のように指定します。 |

| v1.12.0 のロケール                         | Notes                                                                      | ダイジェスト                         |
|---------------------------------------------|:---------------------------------------------------------------------------|:-------------------------------|
| `ar-eg-hoda`                                | `ar-EG` ロケールと `ar-EG-Hoda` 音声のコンテナー イメージ。            | `sha256:987e6b3e9e13570eb29117e87829a4905b35c712a0f36429dd6404793af31627` |
| `ar-sa-naayf`                               | `ar-SA` ロケールと `ar-SA-Naayf` 音声のコンテナー イメージ。           | `sha256:7d1d3c337b7e3bdc6ae2b3e074828ffc3c64ffc0ab94abcb89896e623148d963` |
| `bg-bg-ivan`                                | `bg-BG` ロケールと `bg-BG-Ivan` 音声のコンテナー イメージ。            | `sha256:cf01bea4f1f6b7112871da84fd82fb7e6de106c11cc933f21131385173f1da09` |
| `ca-es-herenarus`                           | `ca-ES` ロケールと `ca-ES-HerenaRUS` 音声のコンテナー イメージ。       | `sha256:d6060a1e16cbe40990677b3c46f14dc619ee6887d39a4af1cac51fba2baca9a9` |
| `cs-cz-jakub`                               | `cs-CZ` ロケールと `cs-CZ-Jakub` 音声のコンテナー イメージ。           | `sha256:5033185bd60257033989fc4ff124c69b1dd02d5b99b79ff5c52ae84572095693` |
| `da-dk-hellerus`                            | `da-DK` ロケールと `da-DK-HelleRUS` 音声のコンテナー イメージ。        | `sha256:ac9655166f8181db2d0e6684cc3a5b6e089da788f17c78067af2cf061c8db660` |
| `de-at-michael`                             | `de-AT` ロケールと `de-AT-Michael` 音声のコンテナー イメージ。         | `sha256:90d222aa43c3efac04b9bc3e746b9ebea446cc16c3bdbb471b81065edfbc3023` |
| `de-ch-karsten`                             | `de-CH` ロケールと `de-CH-Karsten` 音声のコンテナー イメージ。         | `sha256:0c08c10f559c97eda9a0a3f8527f8b05810a53e8a3fd2b8e9f2ab35f587d6c46` |
| `de-de-heddarus`                            | `de-DE` ロケールと `de-DE-Hedda` 音声のコンテナー イメージ。           | `sha256:bf54713a1691f2378cf701a1f68ed0f4d32adeab25b2cbd9493f753d56d13e39` |
| `de-de-hedda`                               | `de-DE` ロケールと `de-DE-Hedda` 音声のコンテナー イメージ。           | `sha256:bf54713a1691f2378cf701a1f68ed0f4d32adeab25b2cbd9493f753d56d13e39` |
| `de-de-stefan-apollo`                       | `de-DE` ロケールと `de-DE-Stefan-Apollo` 音声のコンテナー イメージ。   | `sha256:b94c79ace4b33bad944f88259da4dab5f52da7e78af85a8b6eee0e99ed05a387` |
| `el-gr-stefanos`                            | `el-GR` ロケールと `el-GR-Stefanos` 音声のコンテナー イメージ。        | `sha256:3b331be0a6eb32b12d5c6244691bd51ee1d6b218bd3dc066c0f9cb5b78864e14` |
| `en-au-catherine`                           | `en-AU` ロケールと `en-AU-Catherine` 音声のコンテナー イメージ。       | `sha256:1bbbd1214119d2e02539f7bef8eeba48e86f17b968f2532a7d96e96ef40ecbe3` |
| `en-au-hayleyrus`                           | `en-AU` ロケールと `en-AU-HayleyRUS` 音声のコンテナー イメージ。       | `sha256:aa0a38fd20cabcf33baa97b3a88f354d01055f57ed9376bf98b7ea0993333ffa` |
| `en-ca-heatherrus`                          | `en-CA` ロケールと `en-CA-HeatherRUS` 音声のコンテナー イメージ。      | `sha256:57966c65522862572e07ba474fba7e2c6038091cc1b8a35861645dffc2fc5f5b` |
| `en-ca-linda`                               | `en-CA` ロケールと `en-CA-Linda` 音声のコンテナー イメージ。           | `sha256:57c6ff08057f199a8eb75668f8ddce26b92c87a7e01e9003b74339b98ea438c4` |
| `en-gb-george-apollo`                       | `en-GB` ロケールと `en-GB-George-Apollo` 音声のコンテナー イメージ。   | `sha256:89a8b8b8e900e6dbda665d245fd8a911d6e3286ee16a92e46f1993dc3667b631` |
| `en-gb-hazelrus`                            | `en-GB` ロケールと `en-GB-HazelRUS` 音声のコンテナー イメージ。        | `sha256:18347ce1c4e4e21180f64c27bb4bcbebbf52597e25db7e24dbeb57edcea56109` |
| `en-gb-susan-apollo`                        | `en-GB` ロケールと `en-GB-Susan-Apollo` 音声のコンテナー イメージ。    | `sha256:015905bd42f8fb4ec575d971ff2d710ac5f904da2b84909270d3a7e51f5e3029` |
| `en-ie-sean`                                | `en-IE` ロケールと `en-IE-Sean` 音声のコンテナー イメージ。            | `sha256:4a490dcc6be935178761f14edbdd0c6e4036626046dbfeda002336d871c36fdc` |
| `en-in-heera-apollo`                        | `en-IN` ロケールと `en-IN-Heera-Apollo` 音声のコンテナー イメージ。    | `sha256:f26fb9b32ca82aa00c40f8824ed5d3d95ba1be5a10343e8649946d9468f9f74f` |
| `en-in-priyarus`                            | `en-IN` ロケールと `en-IN-PriyaRUS` 音声のコンテナー イメージ。        | `sha256:43f5fffad77d3446bc08922df36e244115ecf6090e7c48c42281c2fa62d23b90` |
| `en-in-ravi-apollo`                         | `en-IN` ロケールと `en-IN-Ravi-Apollo` 音声のコンテナー イメージ。     | `sha256:0ca4a07585a61a6e15c7fd951b77bab6b5cf8934ecff65fe4ca6cfe8e47f351b` |
| `en-us-aria24krus`                          | `en-US` ロケールと `en-US-Aria24kRUS` 音声のコンテナー イメージ。      | `sha256:00857cb570528dee93f7c9c7f96bb2e11763ff6aa9cc7405a05bcbad3d85b08d` |
| `en-us-ariarus`                             | `en-US` ロケールと `en-US-AriaRUS` 音声のコンテナー イメージ。         | `sha256:00857cb570528dee93f7c9c7f96bb2e11763ff6aa9cc7405a05bcbad3d85b08d` |
| `en-us-benjaminrus`                         | `en-US` ロケールと `en-US-BenjaminRUS` 音声のコンテナー イメージ。     | `sha256:3d7c911788bda58225a7100ba1a9afbb61e0a9f8b7633b383fe6e9faa48471d0` |
| `en-us-guy24krus`                           | `en-US` ロケールと `en-US-Guy24kRUS` 音声のコンテナー イメージ。       | `sha256:251841a8399bd168644460e3ebf6d92f093dc8ea60f9defdc663a7e1f60515fa` |
| `en-us-zirarus`                             | `en-US` ロケールと `en-US-ZiraRUS` 音声のコンテナー イメージ。         | `sha256:dbc6bb44b283902755907d9cee5694f880c95c6cf939f328059d826fefe53dfa` |
| `es-es-helenarus`                           | `es-ES` ロケールと `es-ES-HelenaRUS` 音声のコンテナー イメージ。       | `sha256:9f11111e24b554d907d36516d130324d64a477b512cbd7faffa0b7d3895aa538` |
| `es-es-laura-apollo`                        | `es-ES` ロケールと `es-ES-Laura-Apollo` 音声のコンテナー イメージ。    | `sha256:04add8f669539cb2522237a1b01d263b30ed609332cd2ff6dcf2c88fcd24764a` |
| `es-es-pablo-apollo`                        | `es-ES` ロケールと `es-ES-Pablo-Apollo` 音声のコンテナー イメージ。    | `sha256:d375f7eea3592e041943a56ba18bec9ebc4bba1c99dea4d583f2012aee31cff7` |
| `es-mx-hildarus`                            | `es-MX` ロケールと `es-MX-HildaRUS` 音声のコンテナー イメージ。        | `sha256:437e38d9cb97d2cee27890529eccc1d0b96622749c83844b89c50dc119176b61` |
| `es-mx-raul-apollo`                         | `es-MX` ロケールと `es-MX-Raul-Apollo` 音声のコンテナー イメージ。     | `sha256:b6c0937fddd2e4d39a7cd96628a3d7d6004936f356cb553942e4f7dd48824b52` |
| `fi-fi-heidirus`                            | `fi-FI` ロケールと `fi-FI-HeidiRUS` 音声のコンテナー イメージ。        | `sha256:5a359ab047d811996cccb9f3f95a59a7e023ee5be72ff0f509e7ebfeb0d3a07a` |
| `fr-ca-caroline`                            | `fr-CA` ロケールと `fr-CA-Caroline` 音声のコンテナー イメージ。        | `sha256:439bab9f2933c73e52e78f1683a027e81a251c32fb8aa49b6cd8e7c9b2451f15` |
| `fr-ca-harmonierus`                         | `fr-CA` ロケールと `fr-CA-HarmonieRUS` 音声のコンテナー イメージ。     | `sha256:ca798c5d25454b60cafca44f7f7e32896146966a8de94d00cced06235e38bf00` |
| `fr-ch-guillaume`                           | `fr-CH` ロケールと `fr-CH-Guillaume` 音声のコンテナー イメージ。       | `sha256:e696a65a7c40209a8dd8d9ff59ca5334811e993f5b454f6d741ce0fc59258e07` |
| `fr-fr-hortenserus`                         | `fr-FR` ロケールと `fr-FR-HortenseRUS` 音声のコンテナー イメージ。     | `sha256:ab6e7c023ee6cef95f8dc4eeb3c804ea1b8af937cadb17efcc12e5b18adcfc69` |
| `fr-fr-julie-apollo`                        | `fr-FR` ロケールと `fr-FR-Julie-Apollo` 音声のコンテナー イメージ。    | `sha256:cb8f51f75a0b93baf6efb1624d7d01cd736926769922d61a63773eb3a1097399` |
| `fr-fr-paul-apollo`                         | `fr-FR` ロケールと `fr-FR-Paul-Apollo` 音声のコンテナー イメージ。     | `sha256:482aa2eb44f41294780cf299e6105a1a3105a2d8065233b34ef1837879f95b7f` |
| `he-il-asaf`                                | `he-IL` ロケールと `he-IL-Asaf` 音声のコンテナー イメージ。            | `sha256:2f24ef0e620eeec3ea14262302d22cbb539a8afa85d356ffa446ca9cfd723b31` |
| `hi-in-hemant`                              | `hi-IN` ロケールと `hi-IN-Hemant` 音声のコンテナー イメージ。          | `sha256:0338f8e24eddb819c45909ec3a92c430b1d5ec1567a71495cc19c9a74382b224` |
| `hi-in-kalpana-apollo`                      | `hi-IN` ロケールと `hi-IN-Kalpana-Apollo` 音声のコンテナー イメージ。  | `sha256:5d7e10ab0fd18d1d163c31341765b6f65bb198048424aa622b854172e845726d` |
| `hi-in-kalpana`                             | `hi-IN` ロケールと `hi-IN-Kalpana` 音声のコンテナー イメージ。         | `sha256:5d7e10ab0fd18d1d163c31341765b6f65bb198048424aa622b854172e845726d` |
| `hr-hr-matej`                               | `hr-HR` ロケールと `hr-HR-Matej` 音声のコンテナー イメージ。           | `sha256:08d606969abd0165a798a8e0061e6439d4a33ad6af71aa58a1228e98018e7da9` |
| `hu-hu-szabolcs`                            | `hu-HU` ロケールと `hu-HU-Szabolcs` 音声のコンテナー イメージ。        | `sha256:9613dbf91878054e2ab79d5d9c8f3686d5fe80b16c40d38db9aec3a2c3816555` |
| `id-id-andika`                              | `id-ID` ロケールと `id-ID-Andika` 音声のコンテナー イメージ。          | `sha256:037ca355d8dcf9bff5fda9b9a4a9c2a54a03f3a48c378693c11437a36a245836` |
| `it-it-cosimo-apollo`                       | `it-IT` ロケールと `it-IT-Cosimo-Apollo` 音声のコンテナー イメージ。   | `sha256:647b92d1591501ed032d67cf2cfd719e95c24ffb624143d301c2b6dc5eed7397` |
| `it-it-luciarus`                            | `it-IT` ロケールと `it-IT-LuciaRUS` 音声のコンテナー イメージ。        | `sha256:c35e40ffe1352870b9f177dcf70c1cd9eec9f22f92d35080fb5baa1fa65eac8d` |
| `ja-jp-ayumi-apollo`                        | `ja-JP` ロケールと `ja-JP-Ayumi-Apollo` 音声のコンテナー イメージ。    | `sha256:4fa1436d83439cc9672fe82e35f57a366d2c1a6eb1df1f9f9175d3a588b09610` |
| `ja-jp-harukarus`                           | `ja-JP` ロケールと `ja-JP-HarukaRUS` 音声のコンテナー イメージ。       | `sha256:82f13a16e7812857143d311b5443cecfd7c199a88235728f437ba03e7cd92342` |
| `ja-jp-ichiro-apollo`                       | `ja-JP` ロケールと `ja-JP-Ichiro-Apollo` 音声のコンテナー イメージ。   | `sha256:565bfa8bab3a11608fd5fecae1a0cd655b4508404c354d5574af0e88ff1aec76` |
| `ko-kr-heamirus`                            | `ko-KR` ロケールと `ko-KR-HeamiRUS` 音声のコンテナー イメージ。        | `sha256:2b9ab2e9d946e152b46a634ae291fedd220c76a7ba133346e80b4b19bcaa1422` |
| `ms-my-rizwan`                              | `ms-MY` ロケールと `ms-MY-Rizwan` 音声のコンテナー イメージ。          | `sha256:3a05e09241b43c149132b42079f486f0a076d493d4e4c7e4a56b8a030c5b55c7` |
| `nb-no-huldarus`                            | `nb-NO` ロケールと `nb-NO-HuldaRUS` 音声のコンテナー イメージ。        | `sha256:bb018c3c7d65c825c1755c510aca7f73f058ac4dce236dc114131c5699a1cb61` |
| `nl-nl-hannarus`                            | `nl-NL` ロケールと `nl-NL-HannaRUS` 音声のコンテナー イメージ。        | `sha256:eb2f7dc4db0981717b5fdd16c290ecb8135bd5ae409e0b569e3de34a9fb9f071` |
| `pl-pl-paulinarus`                          | `pl-PL` ロケールと `pl-PL-PaulinaRUS` 音声のコンテナー イメージ。      | `sha256:098fabd9284caabafd4af526d52d5fa70ccbd0dc0e0c658753d7c644ab3bf813` |
| `pt-br-daniel-apollo`                       | `pt-BR` ロケールと `pt-BR-Daniel-Apollo` 音声のコンテナー イメージ。   | `sha256:c7c033ef39c3da6c82ed1870e6796f501654403605268bcc8136cedd37c5ad1f` |
| `pt-br-heloisarus`                          | `pt-BR` ロケールと `pt-BR-HeloisaRUS` 音声のコンテナー イメージ。      | `sha256:2da1e4c972b47efd82a28b4a8324637d878b100bc730f90e9c9d16a6ccec75e9` |
| `pt-pt-heliarus`                            | `pt-PT` ロケールと `pt-PT-HeliaRUS` 音声のコンテナー イメージ。        | `sha256:2f0ba437a2f7fbce9923a4da986aec53ec0ad3d52858e6aa12a7464cfa190240` |
| `ro-ro-andrei`                              | `ro-RO` ロケールと `ro-RO-Andrei` 音声のコンテナー イメージ。          | `sha256:847e60ea915697dd038319a071757e095229ca0001bf05f1d922d4c52ff4b22a` |
| `ru-ru-ekaterinarus`                        | `ru-RU` ロケールと `ru-RU-EkaterinaRUS` 音声のコンテナー イメージ。    | `sha256:37914d4ed1a12d3999385592d5dc0c0ed11148f71f09e11a1bb4c9394691e3b7` |
| `ru-ru-irina-apollo`                        | `ru-RU` ロケールと `ru-RU-Irina-Apollo` 音声のコンテナー イメージ。    | `sha256:3a800cee6d1520a1c0502d9b682a7e0f98ef01de58bb39ea31573a9711ef1271` |
| `ru-ru-pavel-apollo`                        | `ru-RU` ロケールと `ru-RU-Pavel-Apollo` 音声のコンテナー イメージ。    | `sha256:70ad01c5cf6da459e0938c1da17348624e38d94b3ce4f22e181b9516262e961c` |
| `sk-sk-filip`                               | `sk-SK` ロケールと `sk-SK-Filip` 音声のコンテナー イメージ。           | `sha256:8920e7acd70d6d550b66eb3c23878d070dc98219bd59fa8fce1abaf622da4c2f` |
| `sl-si-lado`                                | `sl-SI` ロケールと `sl-SI-Lado` 音声のコンテナー イメージ。            | `sha256:c17313ddab7e7d9c2777d4a19df65b34da4e30e52b4a21f81e5c59bacdfce979` |
| `sv-se-hedvigrus`                           | `sv-SE` ロケールと `sv-SE-HedvigRUS` 音声のコンテナー イメージ。       | `sha256:91315b4a62bbf69e117cdb4ef88facb02d3ee3d436a1e313af94ba6cb0b8608d` |
| `ta-in-valluvar`                            | `ta-IN` ロケールと `ta-IN-Valluvar` 音声のコンテナー イメージ。        | `sha256:d04761de48003062617397de4c4c5f448cd9b4bf57262587d245277d4e408431` |
| `te-in-chitra`                              | `te-IN` ロケールと `te-IN-Chitra` 音声のコンテナー イメージ。          | `sha256:e41002cf7f56d948d2737adc23c0750b430d553d78abb2ac53c42427de971299` |
| `th-th-pattara`                             | `th-TH` ロケールと `th-TH-Pattara` 音声のコンテナー イメージ。         | `sha256:5f556a0c113750d8780c09be8af7db28bc29784056d22389aec61c256ab9cbcb` |
| `tr-tr-sedarus`                             | `tr-TR` ロケールと `tr-TR-SedaRUS` 音声のコンテナー イメージ。         | `sha256:c893b27edd98c0760b7e510c365018e333aa0976ef742f7714ad59c92950a8e2` |
| `vi-vn-an`                                  | `vi-VN` ロケールと `vi-VN-An` 音声のコンテナー イメージ。              | `sha256:bc34adc094183bbbc461e0350d7aa8e5140ece5e89cd9e77c60f2c96276037b2` |
| `zh-cn-huihuirus`                           | `zh-CN` ロケールと `zh-CN-HuihuiRUS` 音声のコンテナー イメージ。       | `sha256:20b23d368f83d4b2926b6d8529d23c4dd84727bb063593d549fb959ce3ace8d2` |
| `zh-cn-kangkang-apollo`                     | `zh-CN` ロケールと `zh-CN-Kangkang-Apollo` 音声のコンテナー イメージ。 | `sha256:cb638e72c8966204ab9142810b94cf4c2da54f3fd5917ae0e12a11d28a4253bb` |
| `zh-cn-yaoyao-apollo`                       | `zh-CN` ロケールと `zh-CN-Yaoyao-Apollo` 音声のコンテナー イメージ。   | `sha256:041a22b054b0acf494ff3085cdb2cd2eb4faeb7b692027f1723d27c341a8ee33` |
| `zh-hk-danny-apollo`                        | `zh-HK` ロケールと `zh-HK-Danny-Apollo` 音声のコンテナー イメージ。    | `sha256:7d9d2766713507b04c0bf3332367e867524ff392b693f4eb8a8c003a4dfc3bac` |
| `zh-hk-tracy-apollo`                        | `zh-HK` ロケールと `zh-HK-Tracy-Apollo` 音声のコンテナー イメージ。    | `sha256:b6dfbdbc5ef0d91812d96c88393c0ae4835eea42dbba4c3d36ab9c5e806bb772` |
| `zh-hk-tracyrus`                            | `zh-HK` ロケールと `zh-HK-TracyRUS` 音声のコンテナー イメージ。        | `sha256:b6dfbdbc5ef0d91812d96c88393c0ae4835eea42dbba4c3d36ab9c5e806bb772` |
| `zh-tw-hanhanrus`                           | `zh-TW` ロケールと `zh-TW-HanHanRUS` 音声のコンテナー イメージ。       | `sha256:9802fc4a9656063cb9f215ca757db5289960d323244272ce280db0395ddd46ac` |
| `zh-tw-yating-apollo`                       | `zh-TW` ロケールと `zh-TW-Yating-Apollo` 音声のコンテナー イメージ。   | `sha256:05f50dffbeb17e4215a5a53cc0791d825b63bc1e2b007b00797e5d0e1b1d6d1e` |
| `zh-tw-zhiwei-apollo`                       | `zh-TW` ロケールと `zh-TW-Zhiwei-Apollo` 音声のコンテナー イメージ。   | `sha256:e96f4aecba6e3c0741218f3e1aec35e53147b12543be9fdcd76ff98d4c34cf84` |


# <a name="previous-version"></a>[以前のバージョン](#tab/previous)

`1.11.0-amd64-<locale-and-voice>` のリリース ノート:

**機能**
* ID によってカスタム モデルをフェッチする際の問題に関する詳細なエラー。

`1.9.0-amd64-<locale-and-voice>` のリリース ノート:

* 定期的な毎月のリリース

`1.8.0-amd64-<locale-and-voice>` のリリース ノート:

**機能**

* .NET 3.1 に完全に移行しました

`1.7.0-amd64-<locale-and-voice>` のリリース ノート:

**機能**

* コンポーネントを .NET 3.1 にアップグレードしました

| イメージ タグ                                  | Notes                                                                                                         |
|---------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| `1.11.0-amd64-<locale-and-voice>`           | `<locale>` を、以下に一覧表示されている使用可能なロケールのいずれかに置き換えます。 たとえば、「 `1.11.0-amd64-en-us-ariarus` 」のように指定します。 |
| `1.9.0-amd64-<locale-and-voice>`            | `<locale>` を、以下に一覧表示されている使用可能なロケールのいずれかに置き換えます。 たとえば、「 `1.9.0-amd64-en-us-ariarus` 」のように指定します。  |
| `1.8.0-amd64-<locale-and-voice>`            | `<locale>` を、以下に一覧表示されている使用可能なロケールのいずれかに置き換えます。 たとえば、「 `1.8.0-amd64-en-us-ariarus` 」のように指定します。  |
| `1.7.0-amd64-<locale-and-voice>`            | 最初の GA バージョン。 `<locale>` を、以下に一覧表示されている使用可能なロケールのいずれかに置き換えます。 たとえば、「 `1.7.0-amd64-en-us-ariarus` 」のように指定します。  |

| v1.11.0 のロケール                         | Notes                                                                      | ダイジェスト                         |
|---------------------------------------------|:---------------------------------------------------------------------------|:-------------------------------|
| `ar-eg-hoda`                                | `ar-EG` ロケールと `ar-EG-Hoda` 音声のコンテナー イメージ。            | `sha256:7ba558f444ea482eca87b3e850e9b416c71391282b26a590d1ee3d9a81350188` |
| `ar-sa-naayf`                               | `ar-SA` ロケールと `ar-SA-Naayf` 音声のコンテナー イメージ。           | `sha256:7f0afcc205340dea7ffd959812dcba6a11448f6c5c1ab55c1422a360bd876137` |
| `bg-bg-ivan`                                | `bg-BG` ロケールと `bg-BG-Ivan` 音声のコンテナー イメージ。            | `sha256:fde80af0e2e8e49b49ddec5f1502a246cf308328738d6f572f0043e625673782` |
| `ca-es-herenarus`                           | `ca-ES` ロケールと `ca-ES-HerenaRUS` 音声のコンテナー イメージ。       | `sha256:fb2b50b128aa84ad0cd05db2462337d316ff2d2d78f393c5a9dece588a80654e` |
| `cs-cz-jakub`                               | `cs-CZ` ロケールと `cs-CZ-Jakub` 音声のコンテナー イメージ。           | `sha256:9dde22e5e2164bee77aaf9fe4e8fc141d9dfbe3c92c4b07da969d34aa14f7fd0` |
| `da-dk-hellerus`                            | `da-DK` ロケールと `da-DK-HelleRUS` 音声のコンテナー イメージ。        | `sha256:4a756cd10ad21dcc2b1c7006ec961f7e267f6d2204d9ad4efd6d4730d67a4ccc` |
| `de-at-michael`                             | `de-AT` ロケールと `de-AT-Michael` 音声のコンテナー イメージ。         | `sha256:9d531c162c4279830f99ef0d44a506a023a0137723aab3adff7a663043a1c576` |
| `de-ch-karsten`                             | `de-CH` ロケールと `de-CH-Karsten` 音声のコンテナー イメージ。         | `sha256:353d07168b4a44fcc12a0239f5bf20e2d29365b9abe26b9b844fb6194e7c9bcc` |
| `de-de-heddarus`                            | `de-DE` ロケールと `de-DE-Hedda` 音声のコンテナー イメージ。           | `sha256:d76ff817fc154ba0f5ce1abb93c5a0269fe5bf7b4feb3b3fe9fe8ffe6fd4fee4` |
| `de-de-hedda`                               | `de-DE` ロケールと `de-DE-Hedda` 音声のコンテナー イメージ。           | `sha256:d76ff817fc154ba0f5ce1abb93c5a0269fe5bf7b4feb3b3fe9fe8ffe6fd4fee4` |
| `de-de-stefan-apollo`                       | `de-DE` ロケールと `de-DE-Stefan-Apollo` 音声のコンテナー イメージ。   | `sha256:8e22964dc4b77c05f602f72b0e706a534a89a271c4d17b5117af122c34df9a18` |
| `el-gr-stefanos`                            | `el-GR` ロケールと `el-GR-Stefanos` 音声のコンテナー イメージ。        | `sha256:fcd6288d5fd4ddfe3d3e65e860895f6f7a7e81216c7113f71e7b1b01eb501150` |
| `en-au-catherine`                           | `en-AU` ロケールと `en-AU-Catherine` 音声のコンテナー イメージ。       | `sha256:e49a5ec17b696a3a73d10383d369a2ff88ccddb812898a2eedefe6e6a009ce5a` |
| `en-au-hayleyrus`                           | `en-AU` ロケールと `en-AU-HayleyRUS` 音声のコンテナー イメージ。       | `sha256:b7fb06bd992982c7e2e71da217898da45b742aab08e901bfcef9c43acf546bc0` |
| `en-ca-heatherrus`                          | `en-CA` ロケールと `en-CA-HeatherRUS` 音声のコンテナー イメージ。      | `sha256:efd7d85845ca597937b8cbea7724cf31797855e0de5f30d66984ab9bac688152` |
| `en-ca-linda`                               | `en-CA` ロケールと `en-CA-Linda` 音声のコンテナー イメージ。           | `sha256:8211077d55b440dbb26e42db6322b35ef6ec88e8c2ec6647831e0046668ed8a4` |
| `en-gb-george-apollo`                       | `en-GB` ロケールと `en-GB-George-Apollo` 音声のコンテナー イメージ。   | `sha256:f6e924720b71d8f9a1edd4f5f2280e9054263eb79ce5364e03c9b802ad92f2dd` |
| `en-gb-hazelrus`                            | `en-GB` ロケールと `en-GB-HazelRUS` 音声のコンテナー イメージ。        | `sha256:de702f70c53e4c1647e5fdd3432d37dc8972e069fcc103a1fc2b0be70f0d6d71` |
| `en-gb-susan-apollo`                        | `en-GB` ロケールと `en-GB-Susan-Apollo` 音声のコンテナー イメージ。    | `sha256:5077cb575ffeb64e3d70184a68259438821891f6c9865350d2f887ea43ee99c1` |
| `en-ie-sean`                                | `en-IE` ロケールと `en-IE-Sean` 音声のコンテナー イメージ。            | `sha256:c6f734cc12f04697a4d9b2003c46c5a4efd8c68da90838debb5628d9f8e70104` |
| `en-in-heera-apollo`                        | `en-IN` ロケールと `en-IN-Heera-Apollo` 音声のコンテナー イメージ。    | `sha256:f5a78e857bc1563cbcd74f7b856bc2e4bd981675b397aeccfa134137f1cd3392` |
| `en-in-priyarus`                            | `en-IN` ロケールと `en-IN-PriyaRUS` 音声のコンテナー イメージ。        | `sha256:667729cafd6bf5afe071a0a2989f836943e3bb6d3d1ebe35b7fab9bb311bfebc` |
| `en-in-ravi-apollo`                         | `en-IN` ロケールと `en-IN-Ravi-Apollo` 音声のコンテナー イメージ。     | `sha256:e46533f972235f297dd31fd338638f5117e3f04fa4a434d678d1cecc76db023b` |
| `en-us-aria24krus`                          | `en-US` ロケールと `en-US-Aria24kRUS` 音声のコンテナー イメージ。      | `sha256:a8f881b60021468dbd96d9733606bd00f7f889ccb523d1773492a8301128e596` |
| `en-us-ariarus`                             | `en-US` ロケールと `en-US-AriaRUS` 音声のコンテナー イメージ。         | `sha256:a8f881b60021468dbd96d9733606bd00f7f889ccb523d1773492a8301128e596` |
| `en-us-benjaminrus`                         | `en-US` ロケールと `en-US-BenjaminRUS` 音声のコンテナー イメージ。     | `sha256:53ee105977b6440f1a7fe5088255a9c6e437c39b7c66e5cd4aba984a1667b25c` |
| `en-us-guy24krus`                           | `en-US` ロケールと `en-US-Guy24kRUS` 音声のコンテナー イメージ。       | `sha256:537d2018f414b825aa9995d2e15e0bdb0119e45f2c6fc10d326e3df6f49ef713` |
| `en-us-zirarus`                             | `en-US` ロケールと `en-US-ZiraRUS` 音声のコンテナー イメージ。         | `sha256:05da3347d457ca040cbe9b3e3d586d298a844f906b34ef7b6d768c247274ff1f` |
| `es-es-helenarus`                           | `es-ES` ロケールと `es-ES-HelenaRUS` 音声のコンテナー イメージ。       | `sha256:481cc43ba896a0d3291903af84120fa618130e2a2c8dce9b0ef23172b66858a8` |
| `es-es-laura-apollo`                        | `es-ES` ロケールと `es-ES-Laura-Apollo` 音声のコンテナー イメージ。    | `sha256:8cb9d071a1e01dc3e63d5f1b1c040aa6fee94488a5bbd60f2c91704abfd921cc` |
| `es-es-pablo-apollo`                        | `es-ES` ロケールと `es-ES-Pablo-Apollo` 音声のコンテナー イメージ。    | `sha256:da293ff5c49435c020044614962382040f41b6339ec83677301921a6dabbafb7` |
| `es-mx-hildarus`                            | `es-MX` ロケールと `es-MX-HildaRUS` 音声のコンテナー イメージ。        | `sha256:9677d5bbbbe0c73df93948d4ecf3f367830ef9e7cfb3b42557cf94ec514b6c68` |
| `es-mx-raul-apollo`                         | `es-MX` ロケールと `es-MX-Raul-Apollo` 音声のコンテナー イメージ。     | `sha256:a5109a6a659aa321892d4c6844e102ac72990fc2d58f32e45a072b291849fee8` |
| `fi-fi-heidirus`                            | `fi-FI` ロケールと `fi-FI-HeidiRUS` 音声のコンテナー イメージ。        | `sha256:f8f1aa8168660ee1c21dfa4a92530bcba6f1aeb765cee9087a6cc29d7c332a8a` |
| `fr-ca-caroline`                            | `fr-CA` ロケールと `fr-CA-Caroline` 音声のコンテナー イメージ。        | `sha256:450f0f75f26299a89a80efc3ce93b42d6447a32022aaf4f88edc935e56100191` |
| `fr-ca-harmonierus`                         | `fr-CA` ロケールと `fr-CA-HarmonieRUS` 音声のコンテナー イメージ。     | `sha256:7b18adf90e6db8f8e2c5955f38aa0adfbdbd10a9a95e2cf13035b9c5416000e8` |
| `fr-ch-guillaume`                           | `fr-CH` ロケールと `fr-CH-Guillaume` 音声のコンテナー イメージ。       | `sha256:ec3c238d0bfc3d26f20349ade1c4e19805b796f4bb3d5bf1fe4a9801b1ea1471` |
| `fr-fr-hortenserus`                         | `fr-FR` ロケールと `fr-FR-HortenseRUS` 音声のコンテナー イメージ。     | `sha256:7b13613a9c5260e03ed831c79e5538633b4201867068ca0e1624b2c39fa8cf39` |
| `fr-fr-julie-apollo`                        | `fr-FR` ロケールと `fr-FR-Julie-Apollo` 音声のコンテナー イメージ。    | `sha256:162c777447e3077438865332ac34df956be43c0429ce9962bcf5df9b210dbf01` |
| `fr-fr-paul-apollo`                         | `fr-FR` ロケールと `fr-FR-Paul-Apollo` 音声のコンテナー イメージ。     | `sha256:8cdf28dc31d40a69eb6720fd42b8c19792f973c4e58760abbb6573c6129c81c1` |
| `he-il-asaf`                                | `he-IL` ロケールと `he-IL-Asaf` 音声のコンテナー イメージ。            | `sha256:3f9ec9201deca21f5e3e561d6dd673ee6fb2a7f13b4cae2985ffb69622994b99` |
| `hi-in-hemant`                              | `hi-IN` ロケールと `hi-IN-Hemant` 音声のコンテナー イメージ。          | `sha256:c6de645816587116384ada93c02257f257a13a4b696e1bd8aeecebb9a9668f15` |
| `hi-in-kalpana-apollo`                      | `hi-IN` ロケールと `hi-IN-Kalpana-Apollo` 音声のコンテナー イメージ。  | `sha256:455ab4c9bc7c2457e2e48265065789a54513e07a1dc9e4bc108651f118f1570d` |
| `hi-in-kalpana`                             | `hi-IN` ロケールと `hi-IN-Kalpana` 音声のコンテナー イメージ。         | `sha256:455ab4c9bc7c2457e2e48265065789a54513e07a1dc9e4bc108651f118f1570d` |
| `hr-hr-matej`                               | `hr-HR` ロケールと `hr-HR-Matej` 音声のコンテナー イメージ。           | `sha256:6ac24252194f91cd815736bd8be03fb95e0b965fabed5de4c631e99cd917da97` |
| `hu-hu-szabolcs`                            | `hu-HU` ロケールと `hu-HU-Szabolcs` 音声のコンテナー イメージ。        | `sha256:bf20ea91d922beb682e321a31cabb11ebec474f47edcf4e3787882e2a204b3b5` |
| `id-id-andika`                              | `id-ID` ロケールと `id-ID-Andika` 音声のコンテナー イメージ。          | `sha256:859bef31e5d882b508154ec00632e5e1e95bc8ea2dde6198f157703d759746c7` |
| `it-it-cosimo-apollo`                       | `it-IT` ロケールと `it-IT-Cosimo-Apollo` 音声のコンテナー イメージ。   | `sha256:b6c81ab4bd0aba217977b0bd83a8a65f7c09b5954cda0870dea15aec0dbbe1ed` |
| `it-it-luciarus`                            | `it-IT` ロケールと `it-IT-LuciaRUS` 音声のコンテナー イメージ。        | `sha256:e216a1390a0d4d9f111c56c1d655f36614947eea18d6ec91a9f6d050048b1ad4` |
| `ja-jp-ayumi-apollo`                        | `ja-JP` ロケールと `ja-JP-Ayumi-Apollo` 音声のコンテナー イメージ。    | `sha256:ba2042523ea1fff9d2c8b805ac36075169c3aecce0c965d09e326c06eab5a36f` |
| `ja-jp-harukarus`                           | `ja-JP` ロケールと `ja-JP-HarukaRUS` 音声のコンテナー イメージ。       | `sha256:fdbc8f59fc1c4b52c11d248ee9a5d7fe4e58343f036e558fbb33282e24d5b71f` |
| `ja-jp-ichiro-apollo`                       | `ja-JP` ロケールと `ja-JP-Ichiro-Apollo` 音声のコンテナー イメージ。   | `sha256:08ea0ed61ac152dc5caea2d4cacc81175c272cb4a835eecaa7f8e7c5485740b7` |
| `ko-kr-heamirus`                            | `ko-KR` ロケールと `ko-KR-HeamiRUS` 音声のコンテナー イメージ。        | `sha256:40ff95e5fb92278e369b4f37d7dbb109431ecb115b1b9516aa887e6bb4fd030b` |
| `ms-my-rizwan`                              | `ms-MY` ロケールと `ms-MY-Rizwan` 音声のコンテナー イメージ。          | `sha256:70cfe68a81ee860136cfaed35909f522c28c20ef5514c2d9d96c283892f8b7f5` |
| `nb-no-huldarus`                            | `nb-NO` ロケールと `nb-NO-HuldaRUS` 音声のコンテナー イメージ。        | `sha256:9941cda0e65884900532e6a0ba68e475f373277105594bf09e67225450192d3c` |
| `nl-nl-hannarus`                            | `nl-NL` ロケールと `nl-NL-HannaRUS` 音声のコンテナー イメージ。        | `sha256:c71d980dfc70575421d1589c74e8b3e7cc036551412d0ad0f89dbc543252a405` |
| `pl-pl-paulinarus`                          | `pl-PL` ロケールと `pl-PL-PaulinaRUS` 音声のコンテナー イメージ。      | `sha256:e5fbd98a70eb1dcf80c446b48b8f17e47ac12853bb255f0aed174c78196de257` |
| `pt-br-daniel-apollo`                       | `pt-BR` ロケールと `pt-BR-Daniel-Apollo` 音声のコンテナー イメージ。   | `sha256:9f57f9847f2372fa341cf037410ac68ada1c3075ab9b77cffbcf01d199f7c1f5` |
| `pt-br-heloisarus`                          | `pt-BR` ロケールと `pt-BR-HeloisaRUS` 音声のコンテナー イメージ。      | `sha256:ef546532c582392e6ed47df55c0fbfa6dca6d3e523547089263b57354a4efb1a` |
| `pt-pt-heliarus`                            | `pt-PT` ロケールと `pt-PT-HeliaRUS` 音声のコンテナー イメージ。        | `sha256:116aefb76ddf39bed379c023c8260d2607314ad1b31ddef83ec2818ad9805a0b` |
| `ro-ro-andrei`                              | `ro-RO` ロケールと `ro-RO-Andrei` 音声のコンテナー イメージ。          | `sha256:6968fdefdd798adab48faeb40857c8cdca55712dbf4806703e11ccdfab874051` |
| `ru-ru-ekaterinarus`                        | `ru-RU` ロケールと `ru-RU-EkaterinaRUS` 音声のコンテナー イメージ。    | `sha256:48add20e3c147fb4be26c948841a12736c8b10d053aa7d25984df8e4016e939f` |
| `ru-ru-irina-apollo`                        | `ru-RU` ロケールと `ru-RU-Irina-Apollo` 音声のコンテナー イメージ。    | `sha256:ce5c055aedb3f9323f41a9de8d8f3dd23fb2ad0621d499f914f5cb3856e995f3` |
| `ru-ru-pavel-apollo`                        | `ru-RU` ロケールと `ru-RU-Pavel-Apollo` 音声のコンテナー イメージ。    | `sha256:badc02f9ccdee13ab7dbd4e178bd5c57d332cc3acd2d4a9a3f889d317e0517be` |
| `sk-sk-filip`                               | `sk-SK` ロケールと `sk-SK-Filip` 音声のコンテナー イメージ。           | `sha256:763d4fe74b6f04a976482880eed76175854f659bb5bfcb315dce8ef69acead2e` |
| `sl-si-lado`                                | `sl-SI` ロケールと `sl-SI-Lado` 音声のコンテナー イメージ。            | `sha256:73374363f9b69e03b8b9de34b319d7797876a3dae40bdce0830a67cf4bb4d4f2` |
| `sv-se-hedvigrus`                           | `sv-SE` ロケールと `sv-SE-HedvigRUS` 音声のコンテナー イメージ。       | `sha256:317d6b5d69f56c9087cd1e8004e60a48841b997937dcdccc97e7c0b2e2ffb631` |
| `ta-in-valluvar`                            | `ta-IN` ロケールと `ta-IN-Valluvar` 音声のコンテナー イメージ。        | `sha256:d1aaad1d5f32a910e245e6c117178c0703d39035e4053fe2dd2bb646fc02f7b8` |
| `te-in-chitra`                              | `te-IN` ロケールと `te-IN-Chitra` 音声のコンテナー イメージ。          | `sha256:0224ac3b2de11c4f6ef65ce0bdcd1b9c4112ea472b3bd5626fdff47a5185f54c` |
| `th-th-pattara`                             | `th-TH` ロケールと `th-TH-Pattara` 音声のコンテナー イメージ。         | `sha256:16c7384bfe210f30e09eae3542a58ff9bdbfa9253fdf4d380a53b37809f82c7d` |
| `tr-tr-sedarus`                             | `tr-TR` ロケールと `tr-TR-SedaRUS` 音声のコンテナー イメージ。         | `sha256:5c7786c00a66346438ee4065e3eaa03ef9f8323ba839068344492b8a3b6d997a` |
| `vi-vn-an`                                  | `vi-VN` ロケールと `vi-VN-An` 音声のコンテナー イメージ。              | `sha256:6925744597c45eed8761a9597f3525f435dd420b67ff775a73211fdef9cd9cb2` |
| `zh-cn-huihuirus`                           | `zh-CN` ロケールと `zh-CN-HuihuiRUS` 音声のコンテナー イメージ。       | `sha256:b38a3f465062853b171d2bce6c6d8afa14d223e24bfd5ea0827e34c26a09a2c8` |
| `zh-cn-kangkang-apollo`                     | `zh-CN` ロケールと `zh-CN-Kangkang-Apollo` 音声のコンテナー イメージ。 | `sha256:fa9555e2f520340457d5cebe469af40516237fb9398a5f90046565655b2862f8` |
| `zh-cn-yaoyao-apollo`                       | `zh-CN` ロケールと `zh-CN-Yaoyao-Apollo` 音声のコンテナー イメージ。   | `sha256:d7eeca43e45d09a1c22611f865fb1f8b42673688a11a2acffd37a4e08a7fd8c4` |
| `zh-hk-danny-apollo`                        | `zh-HK` ロケールと `zh-HK-Danny-Apollo` 音声のコンテナー イメージ。    | `sha256:ee7257c0179fbe015324b4d29f16fe93964e5f1901906240477fb1d820a500f2` |
| `zh-hk-tracy-apollo`                        | `zh-HK` ロケールと `zh-HK-Tracy-Apollo` 音声のコンテナー イメージ。    | `sha256:dfa4effbf7d0ec6c9130c142241b3e247e226e13dc218fd44f986ca1c7fff2ed` |
| `zh-hk-tracyrus`                            | `zh-HK` ロケールと `zh-HK-TracyRUS` 音声のコンテナー イメージ。        | `sha256:dfa4effbf7d0ec6c9130c142241b3e247e226e13dc218fd44f986ca1c7fff2ed` |
| `zh-tw-hanhanrus`                           | `zh-TW` ロケールと `zh-TW-HanHanRUS` 音声のコンテナー イメージ。       | `sha256:263153fd6e05970e04af9a9bd95fb13591f0138ac030a632a6a78d95936afa4b` |
| `zh-tw-yating-apollo`                       | `zh-TW` ロケールと `zh-TW-Yating-Apollo` 音声のコンテナー イメージ。   | `sha256:b8289bb550b9328d83d6a7ec93bdf9524087222f537a55db0b2eb5402c2bf663` |
| `zh-tw-zhiwei-apollo`                       | `zh-TW` ロケールと `zh-TW-Zhiwei-Apollo` 音声のコンテナー イメージ。   | `sha256:af4bc0ef2211f69a92541bb14596341375e1003aef541aefcea7843192046b4c` |

| v1.9.0 のロケール                          | Notes                                                                      | ダイジェスト                         |
|---------------------------------------------|:---------------------------------------------------------------------------|:-------------------------------|
| `ar-eg-hoda`                                | `ar-EG` ロケールと `ar-EG-Hoda` 音声のコンテナー イメージ。            | `sha256:2b19cfd2212d6517b286aa18617d2f9d1dd1520078b559cbbf9240599270d10` | 
| `ar-sa-naayf`                               | `ar-SA` ロケールと `ar-SA-Naayf` 音声のコンテナー イメージ。           | `sha256:6063aae5fb15c62b234cf945220916516a06ca81354c5311dee02af4d8cb0d3` |
| `bg-bg-ivan`                                | `bg-BG` ロケールと `bg-BG-Ivan` 音声のコンテナー イメージ。            | `sha256:c6786916464755e64ffa64e69e8f3e7ef16115bac00bb6ea1e45368c42c58d1` |
| `ca-es-herenarus`                           | `ca-ES` ロケールと `ca-ES-HerenaRUS` 音声のコンテナー イメージ。       | `sha256:2a8a1accbf99e2746c9345b77e2f261e0111227312c402cc2e1cd8760cdc82a` |
| `cs-cz-jakub`                               | `cs-CZ` ロケールと `cs-CZ-Jakub` 音声のコンテナー イメージ。           | `sha256:3e464356bb08c9c966af2b28a88ccafd591aecd2e37a0fedb356bd443720e8d` |
| `da-dk-hellerus`                            | `da-DK` ロケールと `da-DK-HelleRUS` 音声のコンテナー イメージ。        | `sha256:b85c43080804103673ff99dddea644a516c4103e8b1f11fa3dd34857492cd40` |
| `de-at-michael`                             | `de-AT` ロケールと `de-AT-Michael` 音声のコンテナー イメージ。         | `sha256:87b57ee61f964e4d72e75d860c499fa3b3d8dbda6a96c97d696beb20aa8b2a9` |
| `de-ch-karsten`                             | `de-CH` ロケールと `de-CH-Karsten` 音声のコンテナー イメージ。         | `sha256:ab1385b9746f4f054204302b9d564a433ae03748021b8ed71b4a3a224af1e9b` |
| `de-de-heddarus`                            | `de-DE` ロケールと `de-DE-Hedda` 音声のコンテナー イメージ。           | `sha256:82185a710c87f9dde678d88036867559ab3bf5f08f234d60d1548d3e106db57` |
| `de-de-hedda`                               | `de-DE` ロケールと `de-DE-Hedda` 音声のコンテナー イメージ。           | `sha256:82185a710c87f9dde678d88036867559ab3bf5f08f234d60d1548d3e106db57` |
| `de-de-stefan-apollo`                       | `de-DE` ロケールと `de-DE-Stefan-Apollo` 音声のコンテナー イメージ。   | `sha256:56a1c63e7e6a0f5623ddc1f6a44ac6e51471d073e02e14e8c8b1e577930d816` |
| `el-gr-stefanos`                            | `el-GR` ロケールと `el-GR-Stefanos` 音声のコンテナー イメージ。        | `sha256:ccbbb09f29ff8f276e246037183c7a3e9a3eb5bf33a942b22205cce3c6857f2` |
| `en-au-catherine`                           | `en-AU` ロケールと `en-AU-Catherine` 音声のコンテナー イメージ。       | `sha256:0c7374890f963e1ae9507e89dc9965a94723bd57802826c0677cd5262189783` |
| `en-au-hayleyrus`                           | `en-AU` ロケールと `en-AU-HayleyRUS` 音声のコンテナー イメージ。       | `sha256:7430bf8eace8294ca085f36ea56399261b2b4f69027e86649e8f3868fc3d811` |
| `en-ca-heatherrus`                          | `en-CA` ロケールと `en-CA-HeatherRUS` 音声のコンテナー イメージ。      | `sha256:0166ce1de3d669ea4ad80738c63369b7032125a54ecabade07241d740a94cfe` |
| `en-ca-linda`                               | `en-CA` ロケールと `en-CA-Linda` 音声のコンテナー イメージ。           | `sha256:50bed6a7bde9b793d307bcc3ace4c0f28d4a33c7a4dad9b3a394dc39a3e1c28` |
| `en-gb-george-apollo`                       | `en-GB` ロケールと `en-GB-George-Apollo` 音声のコンテナー イメージ。   | `sha256:50b800c0018a39609ddb1cee1b10062bf38a907644c393d20786db7c3ade748` |
| `en-gb-hazelrus`                            | `en-GB` ロケールと `en-GB-HazelRUS` 音声のコンテナー イメージ。        | `sha256:2aa79394dfeac8cec0cc1704a5199949cfccf347fe61161d02c7000c4ffcfa6` |
| `en-gb-susan-apollo`                        | `en-GB` ロケールと `en-GB-Susan-Apollo` 音声のコンテナー イメージ。    | `sha256:7a3174b3aae5f10241e731d392b56f124808cdd506f881ced919ced73d836c0` |
| `en-ie-sean`                                | `en-IE` ロケールと `en-IE-Sean` 音声のコンテナー イメージ。            | `sha256:2457202fadb2354fc8d3666432096bd87c07760a4e3f4dbcc49853fff658577` |
| `en-in-heera-apollo`                        | `en-IN` ロケールと `en-IN-Heera-Apollo` 音声のコンテナー イメージ。    | `sha256:e4068cd7ca4272ea94819e2ba8743d2a76c8710b162db5e9ecbde6c92c12877` |
| `en-in-priyarus`                            | `en-IN` ロケールと `en-IN-PriyaRUS` 音声のコンテナー イメージ。        | `sha256:9d63a0ed53ac06178ab84588551421c0e1d04b8bad3321410ebb99c3ca2a9e8` |
| `en-in-ravi-apollo`                         | `en-IN` ロケールと `en-IN-Ravi-Apollo` 音声のコンテナー イメージ。     | `sha256:67049c9ce591336655943f5030afcfdaa150a8aace7b372425a69cc33a6b7b9` |
| `en-us-aria24krus`                          | `en-US` ロケールと `en-US-Aria24kRUS` 音声のコンテナー イメージ。      | `sha256:a95acf6874bf3df7ae8e96be779f80cb5405d21250227b0c4b3ddbcb3014082` |
| `en-us-ariarus`                             | `en-US` ロケールと `en-US-AriaRUS` 音声のコンテナー イメージ。         | `sha256:a95acf6874bf3df7ae8e96be779f80cb5405d21250227b0c4b3ddbcb3014082` |
| `en-us-benjaminrus`                         | `en-US` ロケールと `en-US-BenjaminRUS` 音声のコンテナー イメージ。     | `sha256:93cd49adaaa2a1bdfb06ab655be164ae66f206cb7c03a2cbd59e5fba70610ab` |
| `en-us-guy24krus`                           | `en-US` ロケールと `en-US-Guy24kRUS` 音声のコンテナー イメージ。       | `sha256:7b788bfcaae4c63c274ca15924bfd861cfcafd5fec13f685d80babc25b2949d` |
| `en-us-zirarus`                             | `en-US` ロケールと `en-US-ZiraRUS` 音声のコンテナー イメージ。         | `sha256:bfc87a77df5695ad43481348500fba8f6a7b495708fba200706049469b5ba97` |
| `es-es-helenarus`                           | `es-ES` ロケールと `es-ES-HelenaRUS` 音声のコンテナー イメージ。       | `sha256:0b6c17aca75efb64aa9bfc0d83303038fe58d4b2fb1fc94c9380a4335b80796` |
| `es-es-laura-apollo`                        | `es-ES` ロケールと `es-ES-Laura-Apollo` 音声のコンテナー イメージ。    | `sha256:d6fcffc944c37a2dd0de29c39b82f3f8cce3a95ad925d2814ed7538335d5d4f` |
| `es-es-pablo-apollo`                        | `es-ES` ロケールと `es-ES-Pablo-Apollo` 音声のコンテナー イメージ。    | `sha256:a460bc53d9083d3c3770129995cf96cc1069ae4e8101f1739d304fe210f0af0` |
| `es-mx-hildarus`                            | `es-MX` ロケールと `es-MX-HildaRUS` 音声のコンテナー イメージ。        | `sha256:5b7578fc5b00158dfa674d95a3f1d57f22eb285e8333b4006d1fe1808bda7ba` |
| `es-mx-raul-apollo`                         | `es-MX` ロケールと `es-MX-Raul-Apollo` 音声のコンテナー イメージ。     | `sha256:03922fb017783c86d788c72e01c7ede440f8f3c913c86cab19bad4dfc2e4a2b` |
| `fi-fi-heidirus`                            | `fi-FI` ロケールと `fi-FI-HeidiRUS` 音声のコンテナー イメージ。        | `sha256:146c1f98d6fa061016eba41db6e7b654eef222d37f35406d4b43477bb2ff897` |
| `fr-ca-caroline`                            | `fr-CA` ロケールと `fr-CA-Caroline` 音声のコンテナー イメージ。        | `sha256:1ee2e53f12ad1c72665d2aef64e9d4a7f9ea05670cad84dcae5e75409494f32` |
| `fr-ca-harmonierus`                         | `fr-CA` ロケールと `fr-CA-HarmonieRUS` 音声のコンテナー イメージ。     | `sha256:a21d25d3ac699af4e9ba9194aadd9b45f35fd9205224f3429a4c7da41fc38fe` |
| `fr-ch-guillaume`                           | `fr-CH` ロケールと `fr-CH-Guillaume` 音声のコンテナー イメージ。       | `sha256:216125a9bd89a95d3c4dc2d7e031398659427b3aa7d4663d23a65737972e42b` |
| `fr-fr-hortenserus`                         | `fr-FR` ロケールと `fr-FR-HortenseRUS` 音声のコンテナー イメージ。     | `sha256:795a698120eecbd80c48e738f73300739c1698ca859130ddb4236317bcdf70f` |
| `fr-fr-julie-apollo`                        | `fr-FR` ロケールと `fr-FR-Julie-Apollo` 音声のコンテナー イメージ。    | `sha256:f6eb70d523c435c2e3a713b32a8af4a781df7ec043caad2fc7f458ee341eb2f` |
| `fr-fr-paul-apollo`                         | `fr-FR` ロケールと `fr-FR-Paul-Apollo` 音声のコンテナー イメージ。     | `sha256:28864c662a20f459b3051b1da2967a605e06267e6408285f7c2552748cf4eed` |
| `he-il-asaf`                                | `he-IL` ロケールと `he-IL-Asaf` 音声のコンテナー イメージ。            | `sha256:eaa834bac6b69abef096b36a8baead741db78fe438af3d30f60abde3631d639` |
| `hi-in-hemant`                              | `hi-IN` ロケールと `hi-IN-Hemant` 音声のコンテナー イメージ。          | `sha256:cfea0fa7cce9cc512f2fbb8b76f1c00fe5c32fad853c90b15934cf4ee6262fa` |
| `hi-in-kalpana-apollo`                      | `hi-IN` ロケールと `hi-IN-Kalpana-Apollo` 音声のコンテナー イメージ。  | `sha256:afbd6cc0413f3a3c9f6df044b6df6d9dac9e8e888c2cb619fefbdc3e105c644` |
| `hi-in-kalpana`                             | `hi-IN` ロケールと `hi-IN-Kalpana` 音声のコンテナー イメージ。         | `sha256:afbd6cc0413f3a3c9f6df044b6df6d9dac9e8e888c2cb619fefbdc3e105c644` |
| `hr-hr-matej`                               | `hr-HR` ロケールと `hr-HR-Matej` 音声のコンテナー イメージ。           | `sha256:86683597c62752b4d769b69e5294979fafd4c277aaef1536e1cb19f9f06c0bf` |
| `hu-hu-szabolcs`                            | `hu-HU` ロケールと `hu-HU-Szabolcs` 音声のコンテナー イメージ。        | `sha256:aa64eed28ca2ad060e2e02188e0401bf34e4caf7e2182b70a30ce33b3c11c9c` |
| `id-id-andika`                              | `id-ID` ロケールと `id-ID-Andika` 音声のコンテナー イメージ。          | `sha256:0e1394d231a57a1df8163ccb634dc2ef2f8103b10608a40ab3efc5c0fbe9ded` |
| `it-it-cosimo-apollo`                       | `it-IT` ロケールと `it-IT-Cosimo-Apollo` 音声のコンテナー イメージ。   | `sha256:eef97f2817fc24405823a5fe4e825244db32279b44c0e6631e8ad9a5c1acf40` |
| `it-it-luciarus`                            | `it-IT` ロケールと `it-IT-LuciaRUS` 音声のコンテナー イメージ。        | `sha256:ebc331b0685f482d2f55619fa81fd451fd7c8f107f9cd7ad159bc6213ae4e33` |
| `ja-jp-ayumi-apollo`                        | `ja-JP` ロケールと `ja-JP-Ayumi-Apollo` 音声のコンテナー イメージ。    | `sha256:e9cb7dfd2eec154c8f3d530c16b66e8558c5955a2edaede69740067f00e43cf` |
| `ja-jp-harukarus`                           | `ja-JP` ロケールと `ja-JP-HarukaRUS` 音声のコンテナー イメージ。       | `sha256:93ce2ef6177c0d8ac70b61df8b11fcbcdfd3c0be0cc51cd8644f26679a741c2` |
| `ja-jp-ichiro-apollo`                       | `ja-JP` ロケールと `ja-JP-Ichiro-Apollo` 音声のコンテナー イメージ。   | `sha256:6a18bae69ac63b42ba992b8b74d8d31d91ca984d61b5f62f38be988cf38645e` |
| `ko-kr-heamirus`                            | `ko-KR` ロケールと `ko-KR-HeamiRUS` 音声のコンテナー イメージ。        | `sha256:7a48252d4ada2af43f9266a70113426d330bac192348cbdc929022295a0e727` |
| `ms-my-rizwan`                              | `ms-MY` ロケールと `ms-MY-Rizwan` 音声のコンテナー イメージ。          | `sha256:90e2ecac14f8e960934fd013d208fc2a0afe1bfff037d5648d422bda8d8a76e` |
| `nb-no-huldarus`                            | `nb-NO` ロケールと `nb-NO-HuldaRUS` 音声のコンテナー イメージ。        | `sha256:217b61bd6244b5effda8f12a2c563ce1b4572e9c5b8a08df143665f9ff754e4` |
| `nl-nl-hannarus`                            | `nl-NL` ロケールと `nl-NL-HannaRUS` 音声のコンテナー イメージ。        | `sha256:fbff48dfc9dfadadf377867b28f6e3a3bd605e59da20f77a531efcc7d85d16e` |
| `pl-pl-paulinarus`                          | `pl-PL` ロケールと `pl-PL-PaulinaRUS` 音声のコンテナー イメージ。      | `sha256:856a033a09925773fa4b4531e199ab7c03c537f366acecbda60f8d21735725e` |
| `pt-br-daniel-apollo`                       | `pt-BR` ロケールと `pt-BR-Daniel-Apollo` 音声のコンテナー イメージ。   | `sha256:2d1ec975f1aee56a6fc6039d154fb3f2fbeb4636f7078c5dfe99aeddb6a3634` |
| `pt-br-heloisarus`                          | `pt-BR` ロケールと `pt-BR-HeloisaRUS` 音声のコンテナー イメージ。      | `sha256:b7d629f37ab3305274764264dc08fab5236e60ef18d40e987618115db67ce44` |
| `pt-pt-heliarus`                            | `pt-PT` ロケールと `pt-PT-HeliaRUS` 音声のコンテナー イメージ。        | `sha256:8b380ae7e4aac9d4ada4d15fa9e667387bc9ca038796d9b6999953bfbc97259` |
| `ro-ro-andrei`                              | `ro-RO` ロケールと `ro-RO-Andrei` 音声のコンテナー イメージ。          | `sha256:b00ca7f1411169a5baf7263a8d7e5eed1a72084d9489eaf458429dfc338564a` |
| `ru-ru-ekaterinarus`                        | `ru-RU` ロケールと `ru-RU-EkaterinaRUS` 音声のコンテナー イメージ。    | `sha256:31c588c31e3ac67305af66091e7756dfc4ca454317d0228116ea0b2fedf5d71` |
| `ru-ru-irina-apollo`                        | `ru-RU` ロケールと `ru-RU-Irina-Apollo` 音声のコンテナー イメージ。    | `sha256:e76437f8da7c279b38d2643defc997a13b4a364e9a212895cdb33a9a3f6457f` |
| `ru-ru-pavel-apollo`                        | `ru-RU` ロケールと `ru-RU-Pavel-Apollo` 音声のコンテナー イメージ。    | `sha256:461c1efa6cce0b10a87f338bc637aca76aef8458061a688870fb3343d682da0` |
| `sk-sk-filip`                               | `sk-SK` ロケールと `sk-SK-Filip` 音声のコンテナー イメージ。           | `sha256:7fb0cfab4c0fe2913eb20f28a25c6663015d62f82e7e7864d9f7fac2d27697b` |
| `sl-si-lado`                                | `sl-SI` ロケールと `sl-SI-Lado` 音声のコンテナー イメージ。            | `sha256:5336173d410e10ffeb5dc211a583887e33754319c757914955057d398dfbb0a` |
| `sv-se-hedvigrus`                           | `sv-SE` ロケールと `sv-SE-HedvigRUS` 音声のコンテナー イメージ。       | `sha256:5dc8cdcc3054386bf69596707d9d261d4db5bfd09f1882ceb4e29238a34b24e` |
| `ta-in-valluvar`                            | `ta-IN` ロケールと `ta-IN-Valluvar` 音声のコンテナー イメージ。        | `sha256:74ea485f23e4c1fe0029e06894860aa0188c36c0e14ea3584a06d4216ccef56` |
| `te-in-chitra`                              | `te-IN` ロケールと `te-IN-Chitra` 音声のコンテナー イメージ。          | `sha256:ff2977a98ef691da543db08be9cfe04d7fc3bf8f78b29310c163e47303b2ddd` |
| `th-th-pattara`                             | `th-TH` ロケールと `th-TH-Pattara` 音声のコンテナー イメージ。         | `sha256:ba7e2c0e5e75d9f2b52aa50c97728616c43e81f48c15e24665e4c2ea5770a8f` |
| `tr-tr-sedarus`                             | `tr-TR` ロケールと `tr-TR-SedaRUS` 音声のコンテナー イメージ。         | `sha256:375a8ceae89ea1f0dda551feff30ae3679231189b527992edbc49988d042d66` |
| `vi-vn-an`                                  | `vi-VN` ロケールと `vi-VN-An` 音声のコンテナー イメージ。              | `sha256:b6f82148295b38b4039c45c48695ec50b4e97cd02b18d49c39bf9fca3bec958` |
| `zh-cn-huihuirus`                           | `zh-CN` ロケールと `zh-CN-HuihuiRUS` 音声のコンテナー イメージ。       | `sha256:3e773931f3adaac92cba43773a241692a2b471ebe73ec51c475df8ff63b7ee1` |
| `zh-cn-kangkang-apollo`                     | `zh-CN` ロケールと `zh-CN-Kangkang-Apollo` 音声のコンテナー イメージ。 | `sha256:05fc0d5075a1094caf70d98b4a9469952be52cb6eb4d9f7b9ff4ae961100c7b` |
| `zh-cn-yaoyao-apollo`                       | `zh-CN` ロケールと `zh-CN-Yaoyao-Apollo` 音声のコンテナー イメージ。   | `sha256:d7613bcefc48e85b9d6f07c8cd223c16d4958bcf7f24087575250e97c593ac1` |
| `zh-hk-danny-apollo`                        | `zh-HK` ロケールと `zh-HK-Danny-Apollo` 音声のコンテナー イメージ。    | `sha256:efe22bc123dac9312dcaeb859a377d81f61fbb25ef46e4678d36ec6bebc5d32` |
| `zh-hk-tracy-apollo`                        | `zh-HK` ロケールと `zh-HK-Tracy-Apollo` 音声のコンテナー イメージ。    | `sha256:802c60bc65012c03ffe96268dca79b8c6dcd0c5cc6180ec271c50ef5c9ba132` |
| `zh-hk-tracyrus`                            | `zh-HK` ロケールと `zh-HK-TracyRUS` 音声のコンテナー イメージ。        | `sha256:802c60bc65012c03ffe96268dca79b8c6dcd0c5cc6180ec271c50ef5c9ba132` |
| `zh-tw-hanhanrus`                           | `zh-TW` ロケールと `zh-TW-HanHanRUS` 音声のコンテナー イメージ。       | `sha256:95d58922463d577d4c4722ab722a5768af35fb62236d47f6709717dea758909` |
| `zh-tw-yating-apollo`                       | `zh-TW` ロケールと `zh-TW-Yating-Apollo` 音声のコンテナー イメージ。   | `sha256:33eec6e3aaaedafaf3969746eeaf97a1760e763505decfe2abaa03f5054bfd2` |
| `zh-tw-zhiwei-apollo`                       | `zh-TW` ロケールと `zh-TW-Zhiwei-Apollo` 音声のコンテナー イメージ。   | `sha256:456db2898b2e5a9c30b7071ce6ea3f141438cbf1aa4899c7ffccfc2f0dde5bd` |

| v1.8.0 のロケール                          | Notes                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `ar-eg-hoda`                                | `ar-EG` ロケールと `ar-EG-Hoda` 音声のコンテナー イメージ。            |
| `ar-sa-naayf`                               | `ar-SA` ロケールと `ar-SA-Naayf` 音声のコンテナー イメージ。           |
| `bg-bg-ivan`                                | `bg-BG` ロケールと `bg-BG-Ivan` 音声のコンテナー イメージ。            |
| `ca-es-herenarus`                           | `ca-ES` ロケールと `ca-ES-HerenaRUS` 音声のコンテナー イメージ。       |
| `cs-cz-jakub`                               | `cs-CZ` ロケールと `cs-CZ-Jakub` 音声のコンテナー イメージ。           |
| `da-dk-hellerus`                            | `da-DK` ロケールと `da-DK-HelleRUS` 音声のコンテナー イメージ。        |
| `de-at-michael`                             | `de-AT` ロケールと `de-AT-Michael` 音声のコンテナー イメージ。         |
| `de-ch-karsten`                             | `de-CH` ロケールと `de-CH-Karsten` 音声のコンテナー イメージ。         |
| `de-de-hedda`                               | `de-DE` ロケールと `de-DE-Hedda` 音声のコンテナー イメージ。           |
| `de-de-heddarus`                            | `de-DE` ロケールと `de-DE-Hedda` 音声のコンテナー イメージ。           |
| `de-de-stefan-apollo`                       | `de-DE` ロケールと `de-DE-Stefan-Apollo` 音声のコンテナー イメージ。   |
| `el-gr-stefanos`                            | `el-GR` ロケールと `el-GR-Stefanos` 音声のコンテナー イメージ。        |
| `en-au-catherine`                           | `en-AU` ロケールと `en-AU-Catherine` 音声のコンテナー イメージ。       |
| `en-au-hayleyrus`                           | `en-AU` ロケールと `en-AU-HayleyRUS` 音声のコンテナー イメージ。       |
| `en-ca-heatherrus`                          | `en-CA` ロケールと `en-CA-HeatherRUS` 音声のコンテナー イメージ。      |
| `en-ca-linda`                               | `en-CA` ロケールと `en-CA-Linda` 音声のコンテナー イメージ。           |
| `en-gb-george-apollo`                       | `en-GB` ロケールと `en-GB-George-Apollo` 音声のコンテナー イメージ。   |
| `en-gb-hazelrus`                            | `en-GB` ロケールと `en-GB-HazelRUS` 音声のコンテナー イメージ。        |
| `en-gb-susan-apollo`                        | `en-GB` ロケールと `en-GB-Susan-Apollo` 音声のコンテナー イメージ。    |
| `en-ie-sean`                                | `en-IE` ロケールと `en-IE-Sean` 音声のコンテナー イメージ。            |
| `en-in-heera-apollo`                        | `en-IN` ロケールと `en-IN-Heera-Apollo` 音声のコンテナー イメージ。    |
| `en-in-priyarus`                            | `en-IN` ロケールと `en-IN-PriyaRUS` 音声のコンテナー イメージ。        |
| `en-in-ravi-apollo`                         | `en-IN` ロケールと `en-IN-Ravi-Apollo` 音声のコンテナー イメージ。     |
| `en-us-benjaminrus`                         | `en-US` ロケールと `en-US-BenjaminRUS` 音声のコンテナー イメージ。     |
| `en-us-guy24krus`                           | `en-US` ロケールと `en-US-Guy24kRUS` 音声のコンテナー イメージ。       |
| `en-us-aria24krus`                          | `en-US` ロケールと `en-US-Aria24kRUS` 音声のコンテナー イメージ。      |
| `en-us-ariarus`                             | `en-US` ロケールと `en-US-AriaRUS` 音声のコンテナー イメージ。         |
| `en-us-zirarus`                             | `en-US` ロケールと `en-US-ZiraRUS` 音声のコンテナー イメージ。         |
| `es-es-helenarus`                           | `es-ES` ロケールと `es-ES-HelenaRUS` 音声のコンテナー イメージ。       |
| `es-es-laura-apollo`                        | `es-ES` ロケールと `es-ES-Laura-Apollo` 音声のコンテナー イメージ。    |
| `es-es-pablo-apollo`                        | `es-ES` ロケールと `es-ES-Pablo-Apollo` 音声のコンテナー イメージ。    |
| `es-mx-hildarus`                            | `es-MX` ロケールと `es-MX-HildaRUS` 音声のコンテナー イメージ。        |
| `es-mx-raul-apollo`                         | `es-MX` ロケールと `es-MX-Raul-Apollo` 音声のコンテナー イメージ。     |
| `fi-fi-heidirus`                            | `fi-FI` ロケールと `fi-FI-HeidiRUS` 音声のコンテナー イメージ。        |
| `fr-ca-caroline`                            | `fr-CA` ロケールと `fr-CA-Caroline` 音声のコンテナー イメージ。        |
| `fr-ca-harmonierus`                         | `fr-CA` ロケールと `fr-CA-HarmonieRUS` 音声のコンテナー イメージ。     |
| `fr-ch-guillaume`                           | `fr-CH` ロケールと `fr-CH-Guillaume` 音声のコンテナー イメージ。       |
| `fr-fr-hortenserus`                         | `fr-FR` ロケールと `fr-FR-HortenseRUS` 音声のコンテナー イメージ。     |
| `fr-fr-julie-apollo`                        | `fr-FR` ロケールと `fr-FR-Julie-Apollo` 音声のコンテナー イメージ。    |
| `fr-fr-paul-apollo`                         | `fr-FR` ロケールと `fr-FR-Paul-Apollo` 音声のコンテナー イメージ。     |
| `he-il-asaf`                                | `he-IL` ロケールと `he-IL-Asaf` 音声のコンテナー イメージ。            |
| `hi-in-hemant`                              | `hi-IN` ロケールと `hi-IN-Hemant` 音声のコンテナー イメージ。          |
| `hi-in-kalpana-apollo`                      | `hi-IN` ロケールと `hi-IN-Kalpana-Apollo` 音声のコンテナー イメージ。  |
| `hi-in-kalpana`                             | `hi-IN` ロケールと `hi-IN-Kalpana` 音声のコンテナー イメージ。         |
| `hr-hr-matej`                               | `hr-HR` ロケールと `hr-HR-Matej` 音声のコンテナー イメージ。           |
| `hu-hu-szabolcs`                            | `hu-HU` ロケールと `hu-HU-Szabolcs` 音声のコンテナー イメージ。        |
| `id-id-andika`                              | `id-ID` ロケールと `id-ID-Andika` 音声のコンテナー イメージ。          |
| `it-it-cosimo-apollo`                       | `it-IT` ロケールと `it-IT-Cosimo-Apollo` 音声のコンテナー イメージ。   |
| `it-it-luciarus`                            | `it-IT` ロケールと `it-IT-LuciaRUS` 音声のコンテナー イメージ。        |
| `ja-jp-ayumi-apollo`                        | `ja-JP` ロケールと `ja-JP-Ayumi-Apollo` 音声のコンテナー イメージ。    |
| `ja-jp-harukarus`                           | `ja-JP` ロケールと `ja-JP-HarukaRUS` 音声のコンテナー イメージ。       |
| `ja-jp-ichiro-apollo`                       | `ja-JP` ロケールと `ja-JP-Ichiro-Apollo` 音声のコンテナー イメージ。   |
| `ko-kr-heamirus`                            | `ko-KR` ロケールと `ko-KR-HeamiRUS` 音声のコンテナー イメージ。        |
| `ms-my-rizwan`                              | `ms-MY` ロケールと `ms-MY-Rizwan` 音声のコンテナー イメージ。          |
| `nb-no-huldarus`                            | `nb-NO` ロケールと `nb-NO-HuldaRUS` 音声のコンテナー イメージ。        |
| `nl-nl-hannarus`                            | `nl-NL` ロケールと `nl-NL-HannaRUS` 音声のコンテナー イメージ。        |
| `pl-pl-paulinarus`                          | `pl-PL` ロケールと `pl-PL-PaulinaRUS` 音声のコンテナー イメージ。      |
| `pt-br-daniel-apollo`                       | `pt-BR` ロケールと `pt-BR-Daniel-Apollo` 音声のコンテナー イメージ。   |
| `pt-br-heloisarus`                          | `pt-BR` ロケールと `pt-BR-HeloisaRUS` 音声のコンテナー イメージ。      |
| `pt-pt-heliarus`                            | `pt-PT` ロケールと `pt-PT-HeliaRUS` 音声のコンテナー イメージ。        |
| `ro-ro-andrei`                              | `ro-RO` ロケールと `ro-RO-Andrei` 音声のコンテナー イメージ。          |
| `ru-ru-ekaterinarus`                        | `ru-RU` ロケールと `ru-RU-EkaterinaRUS` 音声のコンテナー イメージ。    |
| `ru-ru-irina-apollo`                        | `ru-RU` ロケールと `ru-RU-Irina-Apollo` 音声のコンテナー イメージ。    |
| `ru-ru-pavel-apollo`                        | `ru-RU` ロケールと `ru-RU-Pavel-Apollo` 音声のコンテナー イメージ。    |
| `sk-sk-filip`                               | `sk-SK` ロケールと `sk-SK-Filip` 音声のコンテナー イメージ。           |
| `sl-si-lado`                                | `sl-SI` ロケールと `sl-SI-Lado` 音声のコンテナー イメージ。            |
| `sv-se-hedvigrus`                           | `sv-SE` ロケールと `sv-SE-HedvigRUS` 音声のコンテナー イメージ。       |
| `ta-in-valluvar`                            | `ta-IN` ロケールと `ta-IN-Valluvar` 音声のコンテナー イメージ。        |
| `te-in-chitra`                              | `te-IN` ロケールと `te-IN-Chitra` 音声のコンテナー イメージ。          |
| `th-th-pattara`                             | `th-TH` ロケールと `th-TH-Pattara` 音声のコンテナー イメージ。         |
| `tr-tr-sedarus`                             | `tr-TR` ロケールと `tr-TR-SedaRUS` 音声のコンテナー イメージ。         |
| `vi-vn-an`                                  | `vi-VN` ロケールと `vi-VN-An` 音声のコンテナー イメージ。              |
| `zh-cn-huihuirus`                           | `zh-CN` ロケールと `zh-CN-HuihuiRUS` 音声のコンテナー イメージ。       |
| `zh-cn-kangkang-apollo`                     | `zh-CN` ロケールと `zh-CN-Kangkang-Apollo` 音声のコンテナー イメージ。 |
| `zh-cn-yaoyao-apollo`                       | `zh-CN` ロケールと `zh-CN-Yaoyao-Apollo` 音声のコンテナー イメージ。   |
| `zh-hk-danny-apollo`                        | `zh-HK` ロケールと `zh-HK-Danny-Apollo` 音声のコンテナー イメージ。    |
| `zh-hk-tracy-apollo`                        | `zh-HK` ロケールと `zh-HK-Tracy-Apollo` 音声のコンテナー イメージ。    |
| `zh-hk-tracyrus`                            | `zh-HK` ロケールと `zh-HK-TracyRUS` 音声のコンテナー イメージ。        |
| `zh-tw-hanhanrus`                           | `zh-TW` ロケールと `zh-TW-HanHanRUS` 音声のコンテナー イメージ。       |
| `zh-tw-yating-apollo`                       | `zh-TW` ロケールと `zh-TW-Yating-Apollo` 音声のコンテナー イメージ。   |
| `zh-tw-zhiwei-apollo`                       | `zh-TW` ロケールと `zh-TW-Zhiwei-Apollo` 音声のコンテナー イメージ。   |

| V 1.7.0 のロケール                          | Notes                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `ar-eg-hoda`                                | `ar-EG` ロケールと `ar-EG-Hoda` 音声のコンテナー イメージ。            |
| `ar-sa-naayf`                               | `ar-SA` ロケールと `ar-SA-Naayf` 音声のコンテナー イメージ。           |
| `bg-bg-ivan`                                | `bg-BG` ロケールと `bg-BG-Ivan` 音声のコンテナー イメージ。            |
| `ca-es-herenarus`                           | `ca-ES` ロケールと `ca-ES-HerenaRUS` 音声のコンテナー イメージ。       |
| `cs-cz-jakub`                               | `cs-CZ` ロケールと `cs-CZ-Jakub` 音声のコンテナー イメージ。           |
| `da-dk-hellerus`                            | `da-DK` ロケールと `da-DK-HelleRUS` 音声のコンテナー イメージ。        |
| `de-at-michael`                             | `de-AT` ロケールと `de-AT-Michael` 音声のコンテナー イメージ。         |
| `de-ch-karsten`                             | `de-CH` ロケールと `de-CH-Karsten` 音声のコンテナー イメージ。         |
| `de-de-hedda`                               | `de-DE` ロケールと `de-DE-Hedda` 音声のコンテナー イメージ。           |
| `de-de-heddarus`                            | `de-DE` ロケールと `de-DE-Hedda` 音声のコンテナー イメージ。           |
| `de-de-stefan-apollo`                       | `de-DE` ロケールと `de-DE-Stefan-Apollo` 音声のコンテナー イメージ。   |
| `el-gr-stefanos`                            | `el-GR` ロケールと `el-GR-Stefanos` 音声のコンテナー イメージ。        |
| `en-au-catherine`                           | `en-AU` ロケールと `en-AU-Catherine` 音声のコンテナー イメージ。       |
| `en-au-hayleyrus`                           | `en-AU` ロケールと `en-AU-HayleyRUS` 音声のコンテナー イメージ。       |
| `en-ca-heatherrus`                          | `en-CA` ロケールと `en-CA-HeatherRUS` 音声のコンテナー イメージ。      |
| `en-ca-linda`                               | `en-CA` ロケールと `en-CA-Linda` 音声のコンテナー イメージ。           |
| `en-gb-george-apollo`                       | `en-GB` ロケールと `en-GB-George-Apollo` 音声のコンテナー イメージ。   |
| `en-gb-hazelrus`                            | `en-GB` ロケールと `en-GB-HazelRUS` 音声のコンテナー イメージ。        |
| `en-gb-susan-apollo`                        | `en-GB` ロケールと `en-GB-Susan-Apollo` 音声のコンテナー イメージ。    |
| `en-ie-sean`                                | `en-IE` ロケールと `en-IE-Sean` 音声のコンテナー イメージ。            |
| `en-in-heera-apollo`                        | `en-IN` ロケールと `en-IN-Heera-Apollo` 音声のコンテナー イメージ。    |
| `en-in-priyarus`                            | `en-IN` ロケールと `en-IN-PriyaRUS` 音声のコンテナー イメージ。        |
| `en-in-ravi-apollo`                         | `en-IN` ロケールと `en-IN-Ravi-Apollo` 音声のコンテナー イメージ。     |
| `en-us-benjaminrus`                         | `en-US` ロケールと `en-US-BenjaminRUS` 音声のコンテナー イメージ。     |
| `en-us-guy24krus`                           | `en-US` ロケールと `en-US-Guy24kRUS` 音声のコンテナー イメージ。       |
| `en-us-aria24krus`                          | `en-US` ロケールと `en-US-Aria24kRUS` 音声のコンテナー イメージ。      |
| `en-us-ariarus`                             | `en-US` ロケールと `en-US-AriaRUS` 音声のコンテナー イメージ。         |
| `en-us-zirarus`                             | `en-US` ロケールと `en-US-ZiraRUS` 音声のコンテナー イメージ。         |
| `es-es-helenarus`                           | `es-ES` ロケールと `es-ES-HelenaRUS` 音声のコンテナー イメージ。       |
| `es-es-laura-apollo`                        | `es-ES` ロケールと `es-ES-Laura-Apollo` 音声のコンテナー イメージ。    |
| `es-es-pablo-apollo`                        | `es-ES` ロケールと `es-ES-Pablo-Apollo` 音声のコンテナー イメージ。    |
| `es-mx-hildarus`                            | `es-MX` ロケールと `es-MX-HildaRUS` 音声のコンテナー イメージ。        |
| `es-mx-raul-apollo`                         | `es-MX` ロケールと `es-MX-Raul-Apollo` 音声のコンテナー イメージ。     |
| `fi-fi-heidirus`                            | `fi-FI` ロケールと `fi-FI-HeidiRUS` 音声のコンテナー イメージ。        |
| `fr-ca-caroline`                            | `fr-CA` ロケールと `fr-CA-Caroline` 音声のコンテナー イメージ。        |
| `fr-ca-harmonierus`                         | `fr-CA` ロケールと `fr-CA-HarmonieRUS` 音声のコンテナー イメージ。     |
| `fr-ch-guillaume`                           | `fr-CH` ロケールと `fr-CH-Guillaume` 音声のコンテナー イメージ。       |
| `fr-fr-hortenserus`                         | `fr-FR` ロケールと `fr-FR-HortenseRUS` 音声のコンテナー イメージ。     |
| `fr-fr-julie-apollo`                        | `fr-FR` ロケールと `fr-FR-Julie-Apollo` 音声のコンテナー イメージ。    |
| `fr-fr-paul-apollo`                         | `fr-FR` ロケールと `fr-FR-Paul-Apollo` 音声のコンテナー イメージ。     |
| `he-il-asaf`                                | `he-IL` ロケールと `he-IL-Asaf` 音声のコンテナー イメージ。            |
| `hi-in-hemant`                              | `hi-IN` ロケールと `hi-IN-Hemant` 音声のコンテナー イメージ。          |
| `hi-in-kalpana-apollo`                      | `hi-IN` ロケールと `hi-IN-Kalpana-Apollo` 音声のコンテナー イメージ。  |
| `hi-in-kalpana`                             | `hi-IN` ロケールと `hi-IN-Kalpana` 音声のコンテナー イメージ。         |
| `hr-hr-matej`                               | `hr-HR` ロケールと `hr-HR-Matej` 音声のコンテナー イメージ。           |
| `hu-hu-szabolcs`                            | `hu-HU` ロケールと `hu-HU-Szabolcs` 音声のコンテナー イメージ。        |
| `id-id-andika`                              | `id-ID` ロケールと `id-ID-Andika` 音声のコンテナー イメージ。          |
| `it-it-cosimo-apollo`                       | `it-IT` ロケールと `it-IT-Cosimo-Apollo` 音声のコンテナー イメージ。   |
| `it-it-luciarus`                            | `it-IT` ロケールと `it-IT-LuciaRUS` 音声のコンテナー イメージ。        |
| `ja-jp-ayumi-apollo`                        | `ja-JP` ロケールと `ja-JP-Ayumi-Apollo` 音声のコンテナー イメージ。    |
| `ja-jp-harukarus`                           | `ja-JP` ロケールと `ja-JP-HarukaRUS` 音声のコンテナー イメージ。       |
| `ja-jp-ichiro-apollo`                       | `ja-JP` ロケールと `ja-JP-Ichiro-Apollo` 音声のコンテナー イメージ。   |
| `ko-kr-heamirus`                            | `ko-KR` ロケールと `ko-KR-HeamiRUS` 音声のコンテナー イメージ。        |
| `ms-my-rizwan`                              | `ms-MY` ロケールと `ms-MY-Rizwan` 音声のコンテナー イメージ。          |
| `nb-no-huldarus`                            | `nb-NO` ロケールと `nb-NO-HuldaRUS` 音声のコンテナー イメージ。        |
| `nl-nl-hannarus`                            | `nl-NL` ロケールと `nl-NL-HannaRUS` 音声のコンテナー イメージ。        |
| `pl-pl-paulinarus`                          | `pl-PL` ロケールと `pl-PL-PaulinaRUS` 音声のコンテナー イメージ。      |
| `pt-br-daniel-apollo`                       | `pt-BR` ロケールと `pt-BR-Daniel-Apollo` 音声のコンテナー イメージ。   |
| `pt-br-heloisarus`                          | `pt-BR` ロケールと `pt-BR-HeloisaRUS` 音声のコンテナー イメージ。      |
| `pt-pt-heliarus`                            | `pt-PT` ロケールと `pt-PT-HeliaRUS` 音声のコンテナー イメージ。        |
| `ro-ro-andrei`                              | `ro-RO` ロケールと `ro-RO-Andrei` 音声のコンテナー イメージ。          |
| `ru-ru-ekaterinarus`                        | `ru-RU` ロケールと `ru-RU-EkaterinaRUS` 音声のコンテナー イメージ。    |
| `ru-ru-irina-apollo`                        | `ru-RU` ロケールと `ru-RU-Irina-Apollo` 音声のコンテナー イメージ。    |
| `ru-ru-pavel-apollo`                        | `ru-RU` ロケールと `ru-RU-Pavel-Apollo` 音声のコンテナー イメージ。    |
| `sk-sk-filip`                               | `sk-SK` ロケールと `sk-SK-Filip` 音声のコンテナー イメージ。           |
| `sl-si-lado`                                | `sl-SI` ロケールと `sl-SI-Lado` 音声のコンテナー イメージ。            |
| `sv-se-hedvigrus`                           | `sv-SE` ロケールと `sv-SE-HedvigRUS` 音声のコンテナー イメージ。       |
| `ta-in-valluvar`                            | `ta-IN` ロケールと `ta-IN-Valluvar` 音声のコンテナー イメージ。        |
| `te-in-chitra`                              | `te-IN` ロケールと `te-IN-Chitra` 音声のコンテナー イメージ。          |
| `th-th-pattara`                             | `th-TH` ロケールと `th-TH-Pattara` 音声のコンテナー イメージ。         |
| `tr-tr-sedarus`                             | `tr-TR` ロケールと `tr-TR-SedaRUS` 音声のコンテナー イメージ。         |
| `vi-vn-an`                                  | `vi-VN` ロケールと `vi-VN-An` 音声のコンテナー イメージ。              |
| `zh-cn-huihuirus`                           | `zh-CN` ロケールと `zh-CN-HuihuiRUS` 音声のコンテナー イメージ。       |
| `zh-cn-kangkang-apollo`                     | `zh-CN` ロケールと `zh-CN-Kangkang-Apollo` 音声のコンテナー イメージ。 |
| `zh-cn-yaoyao-apollo`                       | `zh-CN` ロケールと `zh-CN-Yaoyao-Apollo` 音声のコンテナー イメージ。   |
| `zh-hk-danny-apollo`                        | `zh-HK` ロケールと `zh-HK-Danny-Apollo` 音声のコンテナー イメージ。    |
| `zh-hk-tracy-apollo`                        | `zh-HK` ロケールと `zh-HK-Tracy-Apollo` 音声のコンテナー イメージ。    |
| `zh-hk-tracyrus`                            | `zh-HK` ロケールと `zh-HK-TracyRUS` 音声のコンテナー イメージ。        |
| `zh-tw-hanhanrus`                           | `zh-TW` ロケールと `zh-TW-HanHanRUS` 音声のコンテナー イメージ。       |
| `zh-tw-yating-apollo`                       | `zh-TW` ロケールと `zh-TW-Yating-Apollo` 音声のコンテナー イメージ。   |
| `zh-tw-zhiwei-apollo`                       | `zh-TW` ロケールと `zh-TW-Zhiwei-Apollo` 音声のコンテナー イメージ。   |

---

## <a name="neural-text-to-speech"></a>ニューラル テキスト読み上げ

[ニューラル テキスト読み上げ][sp-ntts]コンテナー イメージは、`mcr.microsoft.com` コンテナー レジストリ シンジケートにあります。 `azure-cognitive-services/speechservices/` リポジトリ内にあり、`neural-text-to-speech` という名前が付いています。 完全修飾コンテナー イメージ名は `mcr.microsoft.com/azure-cognitive-services/speechservices/neural-text-to-speech` です。

このコンテナー イメージには次のタグを利用できます。 また、[MCR でタグ](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/neural-text-to-speech/tags/list)の完全な一覧を確認することもできます。


# <a name="latest-version"></a>[最新バージョン](#tab/current)

`v1.4.0` のリリース ノート:
* 最新のモデルにアップグレードします。 
* CPU コストと待機時間が削減されました。
* SSML タグを使用した韻律チューニングのサポートの向上 (ピッチ曲線など)。

| イメージ タグ                                  | Notes                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | `en-US` ロケールと `en-US-AriaNeural` 音声のコンテナー イメージ。      |
| `1.4.0-amd64-<locale-and-voice>`    | `<locale>` を、以下に一覧表示されている使用可能なロケールのいずれかに置き換えます。 たとえば、「 `1.4.0-amd64-en-us-arianeural` 」のように指定します。 |


| v1.4.0 のロケールと音声           | Notes                                                                      |
|-------------------------------------|:---------------------------------------------------------------------------|
| `de-de-katjaneural`                 | `de-DE` ロケールと `de-DE-KatjaNeural` 音声のコンテナー イメージ。     |
| `en-au-natashaneural`               | `en-AU` ロケールと `en-AU-NatashaNeural` 音声のコンテナー イメージ。   |
| `en-ca-claraneural`                 | `en-CA` ロケールと `en-CA-ClaraNeural` 音声のコンテナー イメージ。     |
| `en-gb-libbyneural`                 | `en-GB` ロケールと `en-GB-LibbyNeural` 音声のコンテナー イメージ。     |
| `en-gb-mianeural`                   | `en-GB` ロケールと `en-GB-MiaNeural` 音声のコンテナー イメージ。       |
| `en-us-arianeural`                  | `en-US` ロケールと `en-US-AriaNeural` 音声のコンテナー イメージ。      |
| `en-us-guyneural`                   | `en-US` ロケールと `en-US-GuyNeural` 音声のコンテナー イメージ。       |
| `en-us-jennyneural`                 | `en-US` ロケールと `en-US-JennyNeural` 音声のコンテナー イメージ。     |
| `es-es-elviraneural`                | `es-ES` ロケールと `es-ES-ElviraNeural` 音声のコンテナー イメージ。    |
| `es-mx-dalianeural`                 | `es-MX` ロケールと `es-MX-DaliaNeural` 音声のコンテナー イメージ。     |
| `fr-ca-sylvieneural`                | `fr-CA` ロケールと `fr-CA-SylvieNeural` 音声のコンテナー イメージ。    |
| `fr-fr-deniseneural`                | `fr-FR` ロケールと `fr-FR-DeniseNeural` 音声のコンテナー イメージ。    |
| `hi-in-swaracpuneural`              | `hi-IN` ロケールと `hi-IN-Swaraneural` 音声のコンテナー イメージ。     |
| `it-it-elsaneural`                  | `it-IT` ロケールと `it-IT-ElsaNeural` 音声のコンテナー イメージ。      |
| `ja-jp-nanamineural`                | `ja-JP` ロケールと `ja-JP-NanamiNeural` 音声のコンテナー イメージ。    |
| `ko-kr-sunhineural`                 | `ko-KR` ロケールと `ko-KR-SunHiNeural` 音声のコンテナー イメージ。     |
| `pt-br-franciscaneural`             | `pt-BR` ロケールと `pt-BR-FranciscaNeural` 音声のコンテナー イメージ。 |
| `zh-cn-xiaoxiaoneural`              | `zh-CN` ロケールと `zh-CN-XiaoxiaoNeural` 音声のコンテナー イメージ。  |
| `zh-cn-xiaoyouneural`               | `zh-CN` ロケールと `zh-CN-XiaoYouNeural` 音声のコンテナー イメージ。   |
| `zh-cn-yunyangneural`               | `zh-CN` ロケールと `zh-CN-YunYangNeural` 音声のコンテナー イメージ。   |
| `zh-cn-yunyeneural`                 | `zh-CN` ロケールと `zh-CN-YunYeNeural` 音声のコンテナー イメージ。     |

# <a name="previous-version"></a>[以前のバージョン](#tab/previous)

`v1.3.0` のリリース ノート:
* ニューラル テキスト読み上げコンテナーの一般提供を開始しました。 

| イメージ タグ                                  | Notes                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | `en-US` ロケールと `en-US-AriaNeural` 音声のコンテナー イメージ。      |
| `1.3.0-amd64-<locale-and-voice>-preview`    | `<locale>` を、以下に一覧表示されている使用可能なロケールのいずれかに置き換えます。 たとえば、「 `1.3.0-amd64-en-us-arianeural-preview` 」のように指定します。 |
| `1.2.0-amd64-<locale-and-voice>-preview`    | `<locale>` を、以下に一覧表示されている使用可能なロケールのいずれかに置き換えます。 たとえば、「 `1.2.0-amd64-en-us-arianeural-preview` 」のように指定します。 |

| v1.3.0 のロケールと音声           | Notes                                                                      |
|-------------------------------------|:---------------------------------------------------------------------------|
| `de-de-katjaneural`                 | `de-DE` ロケールと `de-DE-KatjaNeural` 音声のコンテナー イメージ。     |
| `en-au-natashaneural`               | `en-AU` ロケールと `en-AU-NatashaNeural` 音声のコンテナー イメージ。   |
| `en-ca-claraneural`                 | `en-CA` ロケールと `en-CA-ClaraNeural` 音声のコンテナー イメージ。     |
| `en-gb-libbyneural`                 | `en-GB` ロケールと `en-GB-LibbyNeural` 音声のコンテナー イメージ。     |
| `en-gb-mianeural`                   | `en-GB` ロケールと `en-GB-MiaNeural` 音声のコンテナー イメージ。       |
| `en-us-arianeural`                  | `en-US` ロケールと `en-US-AriaNeural` 音声のコンテナー イメージ。      |
| `en-us-guyneural`                   | `en-US` ロケールと `en-US-GuyNeural` 音声のコンテナー イメージ。       |
| `en-us-jennyneural`                 | `en-US` ロケールと `en-US-JennyNeural` 音声のコンテナー イメージ。     |
| `es-es-elviraneural`                | `es-ES` ロケールと `es-ES-ElviraNeural` 音声のコンテナー イメージ。    |
| `es-mx-dalianeural`                 | `es-MX` ロケールと `es-MX-DaliaNeural` 音声のコンテナー イメージ。     |
| `fr-ca-sylvieneural`                | `fr-CA` ロケールと `fr-CA-SylvieNeural` 音声のコンテナー イメージ。    |
| `fr-fr-deniseneural`                | `fr-FR` ロケールと `fr-FR-DeniseNeural` 音声のコンテナー イメージ。    |
| `hi-in/swaracpuneural`              | `hi-IN` ロケールと `hi-IN-Swaraneural` 音声のコンテナー イメージ。     |
| `it-it-elsaneural`                  | `it-IT` ロケールと `it-IT-ElsaNeural` 音声のコンテナー イメージ。      |
| `ja-jp-nanamineural`                | `ja-JP` ロケールと `ja-JP-NanamiNeural` 音声のコンテナー イメージ。    |
| `ko-kr-sunhineural`                 | `ko-KR` ロケールと `ko-KR-SunHiNeural` 音声のコンテナー イメージ。     |
| `pt-br-franciscaneural`             | `pt-BR` ロケールと `pt-BR-FranciscaNeural` 音声のコンテナー イメージ。 |
| `zh-cn-xiaoxiaoneural`              | `zh-CN` ロケールと `zh-CN-XiaoxiaoNeural` 音声のコンテナー イメージ。  |

| v 1.2.0 Preview のロケールと音声           | Notes                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | `en-US` ロケールと `en-US-AriaNeural` 音声のコンテナー イメージ。      |
| `de-de-katjaneural-preview`                 | `de-DE` ロケールと `de-DE-KatjaNeural` 音声のコンテナー イメージ。     |
| `en-au-natashaneural-preview`               | `en-AU` ロケールと `en-AU-NatashaNeural` 音声のコンテナー イメージ。   |
| `en-ca-claraneural-preview`                 | `en-CA` ロケールと `en-CA-ClaraNeural` 音声のコンテナー イメージ。     |
| `en-gb-libbyneural-preview`                 | `en-GB` ロケールと `en-GB-LibbyNeural` 音声のコンテナー イメージ。     |
| `en-gb-mianeural-preview`                   | `en-GB` ロケールと `en-GB-MiaNeural` 音声のコンテナー イメージ。       |
| `en-us-arianeural-preview`                  | `en-US` ロケールと `en-US-AriaNeural` 音声のコンテナー イメージ。      |
| `en-us-guyneural-preview`                   | `en-US` ロケールと `en-US-GuyNeural` 音声のコンテナー イメージ。       |
| `es-es-elviraneural-preview`                | `es-ES` ロケールと `es-ES-ElviraNeural` 音声のコンテナー イメージ。    |
| `es-mx-dalianeural-preview`                 | `es-MX` ロケールと `es-MX-DaliaNeural` 音声のコンテナー イメージ。     |
| `fr-ca-sylvieneural-preview`                | `fr-CA` ロケールと `fr-CA-SylvieNeural` 音声のコンテナー イメージ。    |
| `fr-fr-deniseneural-preview`                | `fr-FR` ロケールと `fr-FR-DeniseNeural` 音声のコンテナー イメージ。    |
| `it-it-elsaneural-preview`                  | `it-IT` ロケールと `it-IT-ElsaNeural` 音声のコンテナー イメージ。      |
| `ja-jp-nanamineural-preview`                | `ja-JP` ロケールと `ja-JP-NanamiNeural` 音声のコンテナー イメージ。    |
| `ko-kr-sunhineural-preview`                 | `ko-KR` ロケールと `ko-KR-SunHiNeural` 音声のコンテナー イメージ。     |
| `pt-br-franciscaneural-preview`             | `pt-BR` ロケールと `pt-BR-FranciscaNeural` 音声のコンテナー イメージ。 |
| `zh-cn-xiaoxiaoneural-preview`              | `zh-CN` ロケールと `zh-CN-XiaoxiaoNeural` 音声のコンテナー イメージ。  |

---

## <a name="speech-language-detection"></a>音声言語検出

[音声言語検出][sp-lid]コンテナー イメージは `mcr.microsoft.com` コンテナー レジストリ シンジケートにあります。 `azure-cognitive-services/speechservices/` リポジトリ内にあり、`language-detection` という名前が付いています。 完全修飾コンテナー イメージ名は `mcr.microsoft.com/azure-cognitive-services/speechservices/language-detection` です。

このコンテナー イメージには次のタグを利用できます。 また、[MCR でタグ](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/language-detection/tags/list)の完全な一覧を確認することもできます。

| イメージ タグ                                  | Notes                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                       |      |
| `1.1.0-amd64-preview`                       |      |

## <a name="key-phrase-extraction"></a>キー フレーズ抽出

コンテナー イメージは `mcr.microsoft.com` コンテナー レジストリ シンジケートにあります。 `azure-cognitive-services/textanalytics/` リポジトリ内にあり、`keyphrase` という名前が付いています。 完全修飾コンテナー イメージ名は `mcr.microsoft.com/azure-cognitive-services/textanalytics/keyphrase` です。

このコンテナー イメージには次のタグを利用できます。 また、[MCR でタグ](https://mcr.microsoft.com/v2/azure-cognitive-services/textanalytics/keyphrase/tags/list)の完全な一覧を確認することもできます。

# <a name="latest-version"></a>[最新バージョン](#tab/current)


| イメージ タグ                    | Notes |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.013570001-amd64` |       |

# <a name="previous-versions"></a>[以前のバージョン](#tab/previous)

| イメージ タグ                    | Notes |
|-------------------------------|:------|
| `1.1.012840001-amd64` |       |
| `1.1.012830001-amd64`    |       |

---

## <a name="text-language-detection"></a>テキスト言語の検出

[言語検出][ta-la]コンテナー イメージは `mcr.microsoft.com` コンテナー レジストリ シンジケートにあります。 `azure-cognitive-services/textanalytics/` リポジトリ内にあり、`language` という名前が付いています。 完全修飾コンテナー イメージ名は `mcr.microsoft.com/azure-cognitive-services/textanalytics/language` です


このコンテナー イメージには次のタグを利用できます。 また、[MCR でタグ](https://mcr.microsoft.com/v2/azure-cognitive-services/textanalytics/language/tags/list)の完全な一覧を確認することもできます。

# <a name="latest-versions"></a>[最新バージョン](#tab/current)

| イメージ タグ                    | Notes |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.013570001-amd64` | |
   

# <a name="previous-versions"></a>[以前のバージョン](#tab/previous)


| イメージ タグ                    | Notes |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.012840001-amd64` |   |
| `1.1.012830001-amd64` |   |

---

## <a name="sentiment-analysis"></a>センチメント分析

[感情分析][ta-se]コンテナー イメージは `mcr.microsoft.com` コンテナー レジストリ シンジケートにあります。 `azure-cognitive-services/textanalytics/` リポジトリ内にあり、`sentiment` という名前が付いています。 完全修飾コンテナー イメージ名は `mcr.microsoft.com/azure-cognitive-services/textanalytics/sentiment` です

このコンテナー イメージには次のタグを利用できます。 また、[MCR でタグ](https://mcr.microsoft.com/v2/azure-cognitive-services/textanalytics/sentiment/tags/list)の完全な一覧を確認することもできます。

| イメージ タグ | Notes                                         |
|------------|:----------------------------------------------|
| `latest`   |                                               |
| `3.0-en`   | 感情分析 v3 (英語)               |
| `3.0-es`   | 感情分析 v3 (スペイン語)               |
| `3.0-fr`   | 感情分析 v3 (フランス語)                |
| `3.0-it`   | 感情分析 v3 (イタリア語)               |
| `3.0-de`   | 感情分析 v3 (ドイツ語)                |
| `3.0-zh`   | 感情分析 v3 (簡体字中国語)  |
| `3.0-zht`  | 感情分析 v3 (繁体字中国語) |
| `3.0-ja`   | 感情分析 v3 (日本語)              |
| `3.0-pt`   | 感情分析 v3 (ポルトガル語)            |
| `3.0-nl`   | 感情分析 v3 (オランダ語)                 |
| `2.1`    | 感情分析 v2      |

[ad-containers]: ../anomaly-Detector/anomaly-detector-container-howto.md
[cv-containers]: ../computer-vision/computer-vision-how-to-install-containers.md
[fa-containers]: ../face/face-how-to-install-containers.md
[fr-containers]: ../form-recognizer/form-recognizer-container-howto.md
[lu-containers]: ../luis/luis-container-howto.md
[sp-stt]: ../speech-service/speech-container-howto.md?tabs=stt
[sp-cstt]: ../speech-service/speech-container-howto.md?tabs=cstt
[sp-tts]: ../speech-service/speech-container-howto.md?tabs=tts
[sp-ctts]: ../speech-service/speech-container-howto.md?tabs=ctts
[sp-ntts]: ../speech-service/speech-container-howto.md?tabs=ntts
[sp-lid]: ../speech-service/speech-container-howto.md?tabs=lid
[ta-kp]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=keyphrase
[ta-la]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=language
[ta-se]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=sentiment
