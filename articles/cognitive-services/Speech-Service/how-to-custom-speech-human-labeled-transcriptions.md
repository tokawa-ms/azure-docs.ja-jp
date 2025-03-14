---
title: ヒューマン ラベル付け文字起こしのガイドライン - Speech Service
titleSuffix: Azure Cognitive Services
description: 単語が削除されたり、誤って置き換えられたりする場合など、音声認識の精度を向上させるには、音声データと共にヒューマン ラベル付け文字起こしを使用します。 ヒューマン ラベル付け文字起こしとは、音声ファイルを逐語的に文字起こしたものです。
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/12/2021
ms.author: erhopf
ms.openlocfilehash: af6ced49071b7fbae983508e68964aa064ef38e1
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/03/2021
ms.locfileid: "101700033"
---
# <a name="how-to-create-human-labeled-transcriptions"></a>ヒューマン ラベル付け文字起こしの作成方法

認識の正確性の問題、特に単語が削除されたり誤って置き換えられたりする問題を改善したい場合は、音声データと共にヒューマン ラベル付け文字起こしを使用することがあります。 ヒューマン ラベル付け文字起こしとは これは簡単で、音声ファイルを逐語的に書き写したもののことです。

認識能力を向上させるには、大量の文字起こしデータが必要です。1 時間から 20 時間分の文字起こしデータを用意することをお勧めします。 音声サービスでは、トレーニングのために最大 20 時間分の音声が使用されます。 このページでは、質の高い文字起こしを作成するために役立つガイドラインについて説明します。 このガイドは、ロケールごとに分かれており、米国英語、標準中国語、およびドイツ語のセクションで構成されています。

> [!NOTE]
> すべての基本モデルでオーディオ ファイルのカスタマイズがサポートされているわけではありません。 基本モデルでサポートされていない場合、トレーニングでは、関連するテキストが使用されるのと同じ方法で文字起こしテキストが使用されます。 オーディオ データを使用したトレーニングをサポートする基本モデルの一覧については、「[言語のサポート](language-support.md#speech-to-text)」を参照してください。

> [!NOTE]
> トレーニングに使用する基本モデルを変更し、トレーニング データセットにオーディオが含まれる場合は、選択した新しい基本モデルが [オーディオ データを使用したトレーニングをサポート](language-support.md#speech-to-text)しているかどうかを "*常に*" 確認します。 以前使用した基本モデルでオーディオ データを使用したトレーニングがサポートされておらず、トレーニング データセットにオーディオが含まれる場合は、新しい基本モデルを使用したトレーニングの時間が **大幅に** 増加し、数時間から数日以上かかる可能性が大いにあります。 これは特に、音声サービスのサブスクリプションが、トレーニング用の [専用ハードウェアがあるリージョン](custom-speech-overview.md#set-up-your-azure-account)に **存在しない** 場合に当てはまります。
>
> 上の段落で説明されている問題が発生した場合、データセット内のオーディオの量を減らすか、完全に削除してテキストのみを残すことで、トレーニング時間を簡単に短縮できます。 音声サービスのサブスクリプションが、トレーニング用の [専用ハードウェアがあるリージョン](custom-speech-overview.md#set-up-your-azure-account)に **存在しない** 場合、後者のオプションを強くお勧めします。

## <a name="us-english-en-us"></a>英語 (米国) (en-US)

英語音声用のヒューマン ラベル付け文字起こしは、ASCII 文字のみを使用するプレーンテキストとして提供する必要があります。 Latin-1 または Unicode の句読点は使用しないでください。 これらの文字は、ワード プロセッシング アプリケーションからテキストをコピーしたり、Web ページからデータを切り取ったりするときに、誤って追加されることがよくあります。 これらの文字が存在する場合は、必ず適切な ASCII 代替文字に置き換えてください。

次に例をいくつか示します。

| 使用を避ける文字 | 代入 | Notes |
| ------------------- | ------------ | ----- |
| “Hello world” | "Hello world" | 開始と終了の引用符が、適切な ASCII 文字に置き換えられています。 |
| John’s day | John's day | アポストロフィが、適切な ASCII 文字に置き換えられています。 |
| it was good—no, it was great! | it was good--no, it was great! | em ダッシュが、2 つのハイフンに置き換えられています。 |

### <a name="text-normalization-for-us-english"></a>米国英語用のテキスト正規化

テキストの正規化とは、モデルをトレーニングするときに使用する一定の形式に単語を変換することです。 一部の正規化規則はテキストに自動的に適用されますが、ヒューマン ラベル付け文字起こしデータを準備する場合は、以下のガイドラインに従うことをお勧めします。

- 略語は、略さない単語で書き表します。
- 非標準の数値文字列は、単語で書き表します (会計用語など)。
- アルファベット以外の文字や、英数字が混在する語句は、発音どおりの文字で書き表します。
- 単語として発音される略語は、編集しません ("radar"、"laser"、"RAM"、"NATO" など)。
- 個々の文字として発音される略語は、各文字をスペースで区切って書き表します。
- 音声を使用する場合、数値は音声に一致する単語として文字起こしされます (たとえば、"101" は "one oh one" または "one hundred and one" と発音される可能性があります)。
- "yeah yeah yeah yeah" など、文字、単語、または単語のグループを 3 回より多く繰り返すことは避けます。 このような繰り返しを含む行は、Speech Service によって削除される可能性があります。

次に、文字起こしに対して行う必要がある正規化の例をいくつか示します。

| 元のテキスト               | 正規化後のテキスト              |
| --------------------------- | ------------------------------------- |
| Dr.Bruce Banner            | Doctor Bruce Banner                   |
| James Bond, 007             | James Bond, double oh seven           |
| Ke$ha                       | Kesha                                 |
| How long is the 2x4         | How long is the two by four           |
| The meeting goes from 1-3pm | The meeting goes from one to three pm |
| My blood type is O+         | My blood type is O positive           |
| Water is H20                | Water is H 2 O                        |
| Play OU812 by Van Halen     | Play O U 8 1 2 by Van Halen           |
| UTF-8 with BOM              | U T F 8 with BOM                      |

次の正規化規則は、文字起こしに自動的に適用されます。

- 小文字を使用します。
- 単語内のアポストロフィを除く、すべての句読点を削除します。
- 金額などの数値を、単語および発音形式に展開します。

次に、文字起こしに対して自動的に行われる正規化の例をいくつか示します。

| 元のテキスト                          | 正規化後のテキスト          |
| -------------------------------------- | --------------------------------- |
| "Holy cow!" said Batman.               | holy cow said batman              |
| "What?" said Batman's sidekick, Robin. | what said batman's sidekick robin |
| Go get -em!                            | go get em                         |
| I'm double-jointed                     | i’m double jointed                |
| 104 Elm Street                         | one oh four Elm street            |
| Tune to 102.7                          | tune to one oh two point seven    |
| Pi is about 3.14                       | pi is about three point one four  |
| It costs \$3.14                        | it costs three fourteen           |

## <a name="mandarin-chinese-zh-cn"></a>標準中国語 (zh-CN)

標準中国語音声のヒューマン ラベル付け文字起こしは、バイト順マーカー付きの UTF-8 でエンコードする必要があります。 半角句読点文字の使用は避けます。 これらの文字は、ワード プロセッシング プログラムでデータを準備するときや、Web ページからデータを収集するときに誤って含まれる可能性があります。 これらの文字が存在する場合は、必ず適切な全角代替文字に置き換えてください。

次に例をいくつか示します。

| 使用を避ける文字 | 代入   | Notes |
| ------------------- | -------------- | ----- |
| "你好" | "你好" | 開始と終了の引用符が、適切な文字に置き換えられています。 |
| 需要什么帮助? | 需要什么帮助？| 疑問符が、適切な文字に置き換えられています。 |

### <a name="text-normalization-for-mandarin-chinese"></a>標準中国語用のテキスト正規化

テキストの正規化とは、モデルをトレーニングするときに使用する一定の形式に単語を変換することです。 一部の正規化規則はテキストに自動的に適用されますが、ヒューマン ラベル付け文字起こしデータを準備する場合は、以下のガイドラインに従うことをお勧めします。

- 略語は、略さない単語で書き表します。
- 数値文字列は読み上げ形式で書き出す

次に、文字起こしに対して行う必要がある正規化の例をいくつか示します。

| 元のテキスト | 正規化後のテキスト |
| ------------- | ------------------------ |
| 我今年 21 | 我今年二十一 |
| 3 号楼 504 | 三号 楼 五 零 四 |

次の正規化規則は、文字起こしに自動的に適用されます。

- すべての句読点を削除します
- 数字を発音形式に展開します
- 全角文字を半角文字に変換します
- すべての英単語に大文字を使用する

次に、文字起こしに対して自動的に行われる正規化の例をいくつか示します。

| 元のテキスト | 正規化後のテキスト |
| ------------- | ------------------------ |
| 3.1415 | 三 点 一 四 一 五 |
| ￥ 3.5 | 三 元 五 角 |
| w f y z | W F Y Z |
| 1992 年 8 月 8 日 | 一 九 九 二 年 八 月 八 日 |
| 你吃饭了吗? | 你 吃饭 了 吗 |
| 下午 5:00 的航班 | 下午 五点 的 航班 |
| 我今年 21 岁 | 我 今年 二十 一 岁 |

## <a name="german-de-de-and-other-languages"></a>ドイツ語 (de-DE) およびその他の言語

ドイツ語 (および、英語または標準中国語以外の言語) の音声のヒューマン ラベル付け文字起こしは、バイト順マーカー付きの UTF-8 でエンコードする必要があります。 音声ファイルごとに、ヒューマン ラベル付けトランスクリプトを 1 つ用意する必要があります。

### <a name="text-normalization-for-german"></a>ドイツ語のテキスト正規化

テキストの正規化とは、モデルをトレーニングするときに使用する一定の形式に単語を変換することです。 一部の正規化規則はテキストに自動的に適用されますが、ヒューマン ラベル付け文字起こしデータを準備する場合は、以下のガイドラインに従うことをお勧めします。

- 小数点は、"." ではなく "," にします。
- 時間と分の区切り記号は、"." ではなく ":" にします (例: 12:00 Uhr)。
- "ca." などの省略形は 置き換えられません。 完全な発音形式を使用することをお勧めします。
- 4 つの主要な算術演算子 (+、-、\*、/) は削除されます。 これらを記述形式 ("plus"、"minus"、"mal"、"geteilt") に置き換えることをお勧めします。
- 比較演算子 (=、<、および >) は削除されます。 比較演算子は "gleich"、"kleiner als"、"grösser als" で置換することをお勧めします。
- 3/4 などの分数は、記述形式で表記します (例: 3/4 ではなく、"drei viertel" を使用)。
- "€" 記号は、"Euro" という記述形式に置き換えます。

次に、文字起こしに対して行う必要がある正規化の例をいくつか示します。

| 元のテキスト    | ユーザー正規化後のテキスト | システム正規化後のテキスト       |
| ---------------- | ----------------------------- | ------------------------------------- |
| Es ist 12.23 Uhr | Es ist 12:23 Uhr              | es ist zwölf uhr drei und zwanzig uhr |
| {12.45}          | {12,45}                       | zwölf komma vier fünf                 |
| 2 + 3 - 4        | 2 plus 3 minus 4              | zwei plus drei minus vier             |

次の正規化規則は、文字起こしに自動的に適用されます。

- すべてのテキストで小文字を使用します。
- 各種引用符を含むすべての句読点を削除します ("test"、'test'、"test„、«test» は許可される)。
- いずれかの特殊文字 (¢ ¤ ¥ ¦ § © ª ¬ ® ° ± ² µ × ÿ Ø¬¬) を含む行を破棄します。
- 金額 (ドルまたはユーロ) などの数字を発音形式に展開します。
- a、o、および u に対してだけ、ウムラウトを許可します。 それ以外は "th" に置き換えられるか、破棄されます。

次に、文字起こしに対して自動的に行われる正規化の例をいくつか示します。

| 元のテキスト    | 正規化後のテキスト |
| ---------------- | ------------------------ |
| Frankfurter Ring | frankfurter ring         |
| ¡Eine Frage!     | eine frage               |
| wir, haben       | wir haben                |

### <a name="text-normalization-for-japanese"></a>日本語のテキストの正規化

日本語 (ja-JP) では、各文に対して最大 90 文字の長さがあります。 長い文を含む行は破棄されます。 これよりも長いテキストを追加するには、間にピリオドを挿入します。

## <a name="next-steps"></a>次の手順

- [データを準備してテストする](./how-to-custom-speech-test-and-train.md)
- [データを検査する](how-to-custom-speech-inspect-data.md)
- [データを評価する](how-to-custom-speech-evaluate-data.md)
- [モデルをトレーニングする](how-to-custom-speech-train-model.md)
- [モデルをデプロイする](./how-to-custom-speech-train-model.md)