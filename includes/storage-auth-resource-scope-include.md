---
title: インクルード ファイル
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 02/10/2021
ms.author: tamram
ms.openlocfilehash: 483f5853c321eee4ac6d10543f0e360a0a5e54b9
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2021
ms.locfileid: "100373754"
---
セキュリティ プリンシパルに Azure RBAC ロールを割り当てる前に、セキュリティ プリンシパルに必要なアクセスのスコープを決定します。 ベスト プラクティスとしては、常にできるだけ狭いスコープのみを付与するのが最善の方法です。 より広い範囲で定義されている Azure RBAC ロールは、その下のリソースによって継承されます。

次の一覧で、Azure の BLOB とキューのリソースへのアクセスのスコープとして指定できるレベルを、最も狭いスコープから順に示します。

- **個々のコンテナー。** このスコープでは、ロールの割り当ては、コンテナー内のすべての BLOB と、コンテナーのプロパティおよびメタデータに適用されます。
- **個々のキュー。** このスコープでは、ロールの割り当ては、キュー内のメッセージと、キューのプロパティおよびメタデータに適用されます。
- **ストレージ アカウント。** このスコープでは、ロールの割り当ては、すべてのコンテナーとその BLOB、またはすべてのキューとそのメッセージに適用されます。
- **リソース グループ。** このスコープでは、ロールの割り当ては、リソース グループ内のすべてのストレージ アカウントのすべてのコンテナーまたはキューに適用されます。
- **サブスクリプション。** このスコープでは、ロールの割り当ては、サブスクリプション内のすべてのリソース グループ内の、すべてのストレージ アカウントのすべてのコンテナーまたはキューに適用されます。
- **管理グループ。** このスコープでは、ロールの割り当ては、管理グループ内のすべてのサブスクリプション内のすべてのリソース グループ内のすべてのストレージ アカウントのすべてのコンテナーまたはキューに適用されます。

Azure ロールの割り当ての詳細については、「[Azure ロールベースのアクセス制御 (Azure RBAC) とは](../articles/role-based-access-control/overview.md)」を参照してください。
