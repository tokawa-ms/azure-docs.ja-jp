---
title: V2 の Azure IoT Central アプリケーションを V3 に移行する | Microsoft Docs
description: 管理者が V2 の Azure IoT Central アプリケーションを V3 に移行する方法について学習します。
author: troyhopwood
ms.author: troyhop
ms.date: 01/18/2021
ms.topic: conceptual
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 735ad7ad9ded6baded59ab3f08e239d1c8376b74
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/03/2021
ms.locfileid: "101702727"
---
# <a name="migrate-your-v2-iot-central-application-to-v3"></a>V2 の IoT Central アプリケーションを V3 に移行する

*この記事は管理者向けです。*

現在、新しい IoT Central アプリケーションを作成すると、V3 アプリケーションになります。 アプリケーションを以前に作成した場合、作成した時期によっては V2 である場合があります。 この記事では、V2 を V3 アプリケーションに移行して、最新の IoT Central 機能を使用できるようにする方法について説明します。

IoT Central アプリケーションのバージョンを特定する方法については、「[アプリケーションに関する情報](howto-get-app-info.md)」を参照してください。

V2 から V3 にアプリケーションを移行する手順を次に示します。

1. V2 アプリケーションから新しい V3 アプリケーションを作成します。
1. V3 アプリケーションを構成します。
1. V2 アプリケーションを削除します。

## <a name="create-a-new-v3-application"></a>新しい V3 アプリケーションを作成する

移行ウィザードを使用して、新しい V3 アプリケーションを作成します。

IoT Central は、既存の V3 アプリケーションへの移行をサポートしていません。 既存のデバイスを自動的に移動するには、移行ウィザードを使用して V3 アプリケーションを作成します。

移行ウィザード:

- 新しい V3 アプリケーションを作成します。
- デバイス テンプレートに V3 との互換性があるかどうかを確認します。
- すべてのデバイス テンプレートを新しい V3 アプリケーションにコピーします。
- すべてのユーザーとロールの割り当てを新しい V3 アプリケーションにコピーします。

> [!NOTE]
> V3 とデバイスの互換性を確保するために、デバイス テンプレートのプリミティブ型はオブジェクトのプロパティになります。 デバイス コードに変更を加える必要はありません。

アプリケーションを V3 に移行するには、管理者である必要があります。

1. **[管理]** メニューで、 **[Migrate to a V3 application]\(V3 アプリケーションに移行する\)** を選択します。

    :::image type="content" source="media/howto-migrate/migrate-menu.png" alt-text="アプリケーション移行ウィザードを示すスクリーンショット":::

1. 新しい **アプリケーション名** を入力し、必要に応じて、自動生成された **URL** を変更します。 URL は、現在の V2 アプリケーションの URL と同じにすることはできません。 ただし、後で V2 アプリケーションを削除した後に、URL を変更できます。

1. **[Create a new V3 app]\(新しい V3 アプリを作成する\)** を選択します。

    :::image type="content" source="media/howto-migrate/create-app.png" alt-text="アプリケーション移行ウィザードのオプションを示すスクリーンショット":::

    デバイス テンプレートの数と複雑さによっては、このプロセスが完了するまでに数分かかる場合があります。

    > [!Warning]
    > デバイス テンプレートに数字で始まるフィールドがあるか、または次のいずれかの文字 (`+`、`*`、`?`、`^`、`$`、`(`、`)`、`[`、`]`、`{`、`}`、`|`、`\`) が含まれるフィールドがある場合、V3 アプリケーションの作成は失敗します。 V3 アプリケーションで使用される DTDL デバイス テンプレート スキーマでは、これらの文字は許可されません。 V3 に移行する前に、デバイス テンプレートを更新してこの問題を解決してください。

1. 新しい V3 アプリの準備ができたら、 **[Open your new app]\(新しいアプリを開く\)** を選択してアプリを開きます。

    :::image type="content" source="media/howto-migrate/open-app.png" alt-text="アプリケーション移行後のアプリケーション移行ウィザードを示すスクリーンショット":::

## <a name="configure-the-v3-application"></a>V3 アプリケーションを構成する

新しい V3 アプリケーションを作成した後、デバイスを V2 アプリケーションから V3 アプリケーションに移動する前に、構成を変更します。

> [!TIP]
> V3 は以前のバージョンとはいくつかの違いがあるため、少し時間をとって [V3 について理解を深めてください](overview-iot-central-tour.md#navigate-your-application)。

推奨される構成手順を次に示すので考慮してください。

- [ダッシュボードを構成する](howto-add-tiles-to-your-dashboard.md)
- [データのエクスポートを構成する](howto-export-data.md)
- [規則とアクションを構成する](quick-configure-rules.md)
- [アプリケーションの UI をカスタマイズする](howto-customize-ui.md)

V3 アプリケーションをニーズに合わせて構成したら、V2 アプリケーションから V3 アプリケーションにデバイスを移動する準備は完了です。

## <a name="move-your-devices-to-the-v3-application"></a>デバイスを V3 アプリケーションに移動する

この手順が完了すると、すべてのデバイスが新しい V3 アプリケーションとのみ通信します。

> [!IMPORTANT]
> デバイスを V3 アプリケーションに移動する前に、V3 アプリケーションで作成したすべてのデバイスを削除してください。

この手順では、既存のすべてのデバイスを新しい V3 アプリケーションに移動します。 デバイス データはコピーされません。

**[Move all devices]\(すべてのデバイスを移動する\)** を選択して、デバイスの移動を開始します。 この手順は、完了までに数分かかる場合があります。

:::image type="content" source="media/howto-migrate/move-devices.png" alt-text="デバイス移動オプションが表示されたアプリケーション移行ウィザードを示すスクリーンショット":::

移動が完了したら、すべてのデバイスを再起動して、新しい V3 アプリケーションに接続できるようにします。

> [!WARNING]
> これで V2 アプリケーションは動作しなくなるため、V3 アプリケーションは削除しないでください。

## <a name="delete-your-old-v2-application"></a>古い V2 アプリケーションを削除する

新しい V3 アプリケーションですべてが想定どおりに動作することを確認した後、古い V2 アプリケーションを削除します。 この手順により、使用しなくなったアプリケーションに対して課金されなくなります。

> [!Note]
> アプリケーションを削除するには、アプリケーションの作成時に選択した Azure サブスクリプションのリソースを削除するためのアクセス許可が必要です。 詳細は、「[ロールベースのアクセス制御を使用して Azure サブスクリプション リソースへのアクセスを管理する](../../role-based-access-control/role-assignments-portal.md)」を参照してください。

1. V2 アプリケーションで、メニューの **[管理]** タブを選択します。
2. **[削除]** を選択して、IoT Central アプリケーションを完全に削除します。 このオプションにより、そのアプリケーションに関連付けられているすべてのデータが完全に削除されます。

## <a name="next-steps"></a>次のステップ

アプリケーションを移行する方法を学習したので、次の手順として、[Azure IoT Central の UI](overview-iot-central-tour.md) を確認して、V3 での変更点を確認することをお勧めします。