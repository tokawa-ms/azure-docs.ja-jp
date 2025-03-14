---
title: Azure CLI で BLOB データへのアクセスの承認方法を選択する
titleSuffix: Azure Storage
description: Azure CLI を使用して、BLOB データに対するデータ操作を承認する方法を指定します。 データ操作を承認するには、アカウント アクセス キーまたは アクセス共有シグネチャ (SAS) トークンを使用して、Azure AD 資格情報を使用します。
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 02/10/2021
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.custom: devx-track-azurecli
ms.openlocfilehash: 06b37e8b25d932115384124a45156c801fb9708f
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2021
ms.locfileid: "100361674"
---
# <a name="choose-how-to-authorize-access-to-blob-data-with-azure-cli"></a>Azure CLI で BLOB データへのアクセスの承認方法を選択する

Azure Storage には、BLOB データに対する操作を承認する方法を指定できるようにするための Azure CLI の拡張機能が用意されています。 データ操作は、次の方法で承認できます。

- Azure Active Directory (Azure AD) サービス プリンシパルを使用します。 セキュリティと使いやすさのために、 Azure AD を使用することをお勧めします。
- アカウントアクセスキーまたはアクセス共有シグネチャ (SAS) トークンを使用します。

## <a name="specify-how-data-operations-are-authorized"></a>データ操作の承認方法を指定する

BLOB データの読み取りと書き込みのための Azure CLI コマンドには、オプションの `--auth-mode`パラメーターがあります。 データ操作を承認する方法を示すには、このパラメーターを指定します。

- Azure AD セキュリティ プリンシパルを利用してサインインするために、`--auth-mode` パラメーターを `login` に設定します（推奨）。
- `--auth-mode` パラメーターをレガシ `key` 値に設定して、承認に使用するアカウント アクセス キーを取得しようとします。 また、 `--auth-mode` パラメーターを省略すると、Azure CLI はアクセス キーの取得を試みます。

`--auth-mode` パラメーターを使用するには、 Azure CLI バージョン 2.0.46 以降がインストールされていることを確認してください。 `az --version` を実行し、インストールされたバージョンを確認します。

> [!NOTE]
> ストレージ アカウントが Azure Resource Manager **ReadOnly** ロックでロックされている場合、そのストレージ アカウントに対して[キーの一覧表示](/rest/api/storagerp/storageaccounts/listkeys)操作は許可されません。 **キーの一覧表示** は POST 操作であり、アカウントに対して **ReadOnly** ロックが構成されている場合、すべての POST 操作が禁止されます。 このため、アカウントが **ReadOnly** ロックでロックされている場合、アカウント キーをまだ所有していないユーザーは、Azure AD 資格情報を使用して BLOB データにアクセスする必要があります。

> [!IMPORTANT]
> `--auth-mode` パラメーターを省略した場合、または `key` に設定した場合、Azure CLI はアカウント アクセス キーを使用して承認を試みます。 この場合、コマンドまたは **AZURE_STORAGE_KEY** 環境変数のいずれかでアクセス キーを指定することをお勧めします。 環境変数の詳細については、「 [承認パラメーターの環境変数を設定する](#set-environment-variables-for-authorization-parameters)」セクションを参照してください。
>
> アクセス キーを指定しない場合、Azure CLI は Azure Storage リソース プロバイダーを呼び出して各操作のために取得を試みます。 リソース プロバイダーの呼び出しを必要とする多くのデータ操作を実行すると、スロットリングが発生する可能性があります。 リソース プロバイダーの制限の詳細については、「 [Azure Storage リソース プロバイダーのスケーラビリティとパフォーマンスのターゲット](../common/scalability-targets-resource-provider.md)」を参照してください。

## <a name="authorize-with-azure-ad-credentials"></a>Azure AD の資格情報で承認する

Azure AD 資格情報で Azure CLI にサインインすると、OAuth 2.0 アクセス トークンが返されます。 そのトークンが Azure CLI によって自動的に使用され、BLOB または Queue storage に対するその後のデータ操作が承認されます。 サポートされている操作については、コマンドでアカウント キーや SAS トークンを渡す必要がなくなりました。

BLOB データへのアクセス許可を Azure ロールベースのアクセス制御 (Azure RBAC) を介して Azure AD セキュリティ プリンシパルに割り当てることができます。 Azure Storage の Azure ロールの詳細については、[Azure RBAC を使用した Azure Storage データへのアクセス権の管理](../common/storage-auth-aad-rbac-portal.md)に関する記事を参照してください。

### <a name="permissions-for-calling-data-operations"></a>データ操作呼び出しのアクセス許可

Azure Storage 拡張機能は、BLOB データの操作でサポートされています。 呼び出す操作は、Azure CLI にサインインする Azure AD セキュリティ プリンシパルに与えられているアクセス許可に依存します。 Azure Storage のコンテナーへのアクセス許可は、Azure RBAC を介して割り当てられます。 たとえば、**Storage BLOB データ閲覧者** ロールが割り当てられている場合、コンテナーからデータを読み取るスクリプト コマンドを実行できます。 **Storage BLOB データ共同作成者** ロールが割り当てられている場合、コンテナー、またはそれらに含まれているデータの読み取り、書き込み、削除を行うスクリプト コマンドを実行できます。

コンテナーでの各 Azure Storage 操作に必要なアクセス許可の詳細については、「[OAuth トークンを使用したストレージ操作の呼び出し](/rest/api/storageservices/authorize-with-azure-active-directory#call-storage-operations-with-oauth-tokens)」を参照してください。  

### <a name="example-authorize-an-operation-to-create-a-container-with-azure-ad-credentials"></a>例:Azure AD 資格情報を使用してコンテナーを作成する操作を承認する

次の例では、Azure AD の資格情報を使用して Azure CLI からコンテナーを作成する方法を示しています。 コンテナーを作成するには、Azure CLI にログインする必要があります。また、リソース グループとストレージ アカウントが必要になります。 これらのリソースの作成方法については、次をご覧ください。 [クイック スタート:Azure CLI を使用して BLOB を作成、ダウンロード、リストする ](../blobs/storage-quickstart-blobs-cli.md)。

1. コンテナーを作成する前に、[ストレージ BLOB データ共同作成者](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor)ロールを自分に割り当てます。 自分がアカウント オーナーである場合でも、ストレージ アカウントに対してデータ操作を実行するための明示的なアクセス許可が必要となります。 Azure ロールの割り当ての詳細については、「[Azure portal を使用して BLOB とキュー データへのアクセスのための Azure ロールを割り当てる](../common/storage-auth-aad-rbac-portal.md)」を参照してください。

    > [!IMPORTANT]
    > Azure ロールの割り当ての反映には数分かかることがあります。

1. [az storage container create](/cli/azure/storage/container#az-storage-container-create) コマンドを、`--auth-mode` パラメーターに `login` を設定して呼び出し、自分の Azure AD サインイン情報を使用してコンテナーを作成します。 山かっこ内のプレースホルダーをお客様独自の値に置き換えてください。

    ```azurecli
    az storage container create \
        --account-name <storage-account> \
        --name sample-container \
        --auth-mode login
    ```

## <a name="authorize-with-the-account-access-key"></a>アカウント アクセス キーを使用して承認する

アカウント キーを所有している場合、任意の Azure Storage データ操作の呼び出しができます。 一般に、アカウント キーの使用は安全性が低くなります。 アカウント キーが侵害された場合、アカウント内のすべてのデータが侵害される可能性があります。

次の例は、アカウント アクセス キーを使用してコンテナーを作成する方法を示しています。 アカウント キーを指定し、`--auth-mode` パラメーターに `key` の値を指定します。

```azurecli
az storage container create \
    --account-name <storage-account> \
    --name sample-container \
    --account-key <key>
    --auth-mode key
```

> [!IMPORTANT]
> ストレージ アカウントが Azure Resource Manager **ReadOnly** ロックでロックされている場合、そのストレージ アカウントに対して[キーの一覧表示](/rest/api/storagerp/storageaccounts/listkeys)操作は許可されません。 **キーの一覧表示** は POST 操作であり、アカウントに対して **ReadOnly** ロックが構成されている場合、すべての POST 操作が禁止されます。 このため、アカウントが **ReadOnly** ロックでロックされている場合、ユーザーは Azure AD 資格情報を使用してデータにアクセスする必要があります。

## <a name="authorize-with-a-sas-token"></a>SAS トークンを使用して承認する

SAS トークンを所有している場合は、SAS で許可されているデータ操作の呼び出しができます。 次の例は、SAS トークンを使用してコンテナーを作成する方法を示しています。

```azurecli
az storage container create \
    --account-name <storage-account> \
    --name sample-container \
    --sas-token <token>
```

## <a name="set-environment-variables-for-authorization-parameters"></a>認証パラメーターの環境変数を設定する

この環境変数に適切な値を指定すれば、Azure Storage データ操作の呼び出しのたびにパラメーターに値を設定する必要はなくなります。 次の表に、使用可能な環境変数を示します。

| 環境変数                  | 説明                                                                                                                                                                                                                                                                                                                                                                     |
|---------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    AZURE_STORAGE_ACCOUNT              |    ストレージ アカウント名。 この変数は、ストレージ アカウント キーまたは SAS トークンと組み合わせて使用する必要があります。 どちらも存在しない場合、Azure CLI は、認証された Azure AD アカウントを使用して、ストレージ アカウントのアクセス キーを取得しようとします。 一度に多数のコマンドが実行された場合、Azure Storage リソース プロバイダーの制限に達している可能性があります。 リソース プロバイダーの制限の詳細については、「 [Azure Storage リソース プロバイダーのスケーラビリティとパフォーマンスのターゲット](../common/scalability-targets-resource-provider.md)」を参照してください。             |
|    AZURE_STORAGE_KEY                  |    ストレージ アカウント キー。 この変数は、ストレージ アカウント名と組み合わせて使用する必要があります。                                                                                                                                                                                                                                                                          |
|    AZURE_STORAGE_CONNECTION_STRING    |    ストレージ アカウント キーまたは SAS トークンを含む接続文字列。 この変数は、ストレージ アカウント名と組み合わせて使用する必要があります。                                                                                                                                                                                                                       |
|    AZURE_STORAGE_SAS_TOKEN            |    アクセス共有シグネチャ (SAS) トークン。 この変数は、ストレージ アカウント名と組み合わせて使用する必要があります。                                                                                                                                                                                                                                                            |
|    AZURE_STORAGE_AUTH_MODE            |    コマンドの実行に使用する承認モード。 使用できる値は `login` (推奨) または `key`です。 `login` を指定した場合、Azure CLI は Azure AD の資格情報を使用してデータ操作を承認します。 レガシ `key` モードを指定した場合、Azure CLI はアカウント アクセス キーを照会し、そのキーを使用してコマンドを承認しようとします。    |

## <a name="next-steps"></a>次のステップ

- [Azure CLI を使用して、BLOB およびキュー データにアクセスするための Azure ロールを割り当てる](../common/storage-auth-aad-rbac-cli.md)
- [Azure リソースに対するマネージド ID を使用して BLOB およびキュー データへのアクセスを認証する](../common/storage-auth-aad-msi.md)