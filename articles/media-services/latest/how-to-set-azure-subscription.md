---
title: Azure サブスクリプションを検索する方法
description: Azure サブスクリプションを検索して、環境を設定できるようにします。
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: cli,portal, devx-track-azurecli
ms.openlocfilehash: f16cc83257dbf6419fc794f34e4cd6df033d3f8b
ms.sourcegitcommit: f7eda3db606407f94c6dc6c3316e0651ee5ca37c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/05/2021
ms.locfileid: "102213965"
---
# <a name="find-your-azure-subscription"></a>Azure サブスクリプションを検索する

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="portal"></a>[ポータル](#tab/portal/)

## <a name="use-the-azure-portal"></a>Azure ポータルの使用

1. [Azure portal](https://portal.azure.com) にサインインします。
1. [Azure サービス] の見出しの下にある [サブスクリプション] を選択します。 (サブスクリプションの一覧が表示されない場合、Azure AD テナントの切り替えが必要なことがあります。)サブスクリプション ID は 2 列目に表示されます。
1. サブスクリプション ID をコピーして、後で使用するために任意のテキスト ドキュメントに貼り付けます。

## <a name="cli"></a>[CLI](#tab/cli/)

## <a name="use-the-azure-cli"></a>Azure CLI の使用

<!-- NOTE: The following are in the includes file and are reused in other How To articles. All task based content should be in the includes folder with the "task-" prepended to the file name. -->

### <a name="list-your-azure-subscriptions-with-cli"></a>CLI を使用した Azure サブスクリプションの一覧表示

[!INCLUDE [List your Azure subscriptions with CLI](./includes/task-list-set-subscription-cli.md)]

### <a name="see-also"></a>関連項目

* [Azure CLI](/cli/azure/ams)

---

## <a name="next-steps"></a>次のステップ

[ファイルのストリーミング](stream-files-dotnet-quickstart.md)
