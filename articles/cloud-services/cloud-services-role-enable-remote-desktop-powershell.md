---
title: PowerShell を使用してロールのリモート デスクトップを有効にする
description: PowerShell で Azure クラウド サービス アプリケーションを構成してリモート デスクトップ接続を許可する方法
ms.topic: article
ms.service: cloud-services
ms.date: 10/14/2020
ms.author: tagore
author: tanmaygore
ms.reviewer: mimckitt
ms.custom: ''
ms.openlocfilehash: 5b1650edb575de8fd59ad2495dafcd628a717c02
ms.sourcegitcommit: d135e9a267fe26fbb5be98d2b5fd4327d355fe97
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2021
ms.locfileid: "102610401"
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-classic-using-powershell"></a>PowerShell を使用して Azure Cloud Services (クラシック) のロールでリモート デスクトップ接続を有効にする

> [!IMPORTANT]
> [Azure Cloud Services (延長サポート)](../cloud-services-extended-support/overview.md) は、Azure Cloud Services 製品向けの新しい Azure Resource Manager ベースのデプロイ モデルです。 この変更により、Azure Service Manager ベースのデプロイ モデルで実行されている Azure Cloud Services は Cloud Services (クラシック) という名前に変更されました。そして、すべての新しいデプロイでは [Cloud Services (延長サポート)](../cloud-services-extended-support/overview.md) を使用する必要があります。

> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](cloud-services-role-enable-remote-desktop-visual-studio.md)

リモート デスクトップを使用して、Azure で実行されているロールのデスクトップにアクセスできます。 リモート デスクトップ接続を使用すると、アプリケーションの実行中にそのアプリケーションの問題のトラブルシューティングや診断を行うことができます。

この記事では、PowerShell を使用して、クラウド サービスのロールでリモート デスクトップを有効にする方法について説明します。 この記事で求められる前提条件については、 [Azure PowerShell のインストールおよび構成方法](/powershell/azure/) に関するページを参照してください。 PowerShell では、リモート デスクトップ拡張機能を使用するため、アプリケーションのデプロイ後にリモート デスクトップを有効化できます。

## <a name="configure-remote-desktop-from-powershell"></a>PowerShell からリモート デスクトップを構成する
[Set-AzureServiceRemoteDesktopExtension](/powershell/module/servicemanagement/azure.service/set-azureserviceremotedesktopextension) コマンドレットを使用すると、クラウド サービス デプロイの指定したロールまたはすべてのロールでリモート デスクトップを有効にすることができます。 このコマンドレットでは、PSCredential オブジェクトを受け入れる *Credential* パラメーターを使用してリモート デスクトップ ユーザーのユーザー名とパスワードを指定できます。

PowerShell を対話形式で使用している場合は、 [Get-Credentials](/powershell/module/microsoft.powershell.security/get-credential) コマンドレットを呼び出すことで、PSCredential オブジェクトを簡単に設定できます。

```powershell
$remoteusercredentials = Get-Credential
```

このコマンドにより、リモート ユーザーのユーザー名とパスワードを安全な方法で入力できるダイアログ ボックスが表示されます。

PowerShell は自動化のシナリオで役立つため、ユーザー操作を必要としない方法で **PSCredential** オブジェクトを設定することもできます。 最初に、セキュリティで保護されたパスワードを設定する必要があります。 まずプレーンテキストのパスワードを指定し、 [ConvertTo-SecureString](/powershell/module/microsoft.powershell.security/convertto-securestring)を使用してそのパスワードをセキュリティで保護された文字列に変換します。 次に、 [ConvertFrom-SecureString](/powershell/module/microsoft.powershell.security/convertfrom-securestring)を使用して、セキュリティで保護されたこの文字列を、暗号化された標準文字列に変換する必要があります。 これで、この暗号化された標準文字列を、 [Set-Content](/previous-versions/windows/it-pro/windows-powershell-1.0/ee176959(v=technet.10))を使用してファイルに保存することができます。

パスワードを毎回手動で入力しなくても済むように、セキュリティで保護されたパスワード ファイルを作成することもできます。 また、セキュリティで保護されたパスワード ファイルはプレーン テキスト ファイルよりも適切です。 セキュリティで保護されたパスワード ファイルを作成するには、次の PowerShell を使用します。

```powershell
ConvertTo-SecureString -String "Password123" -AsPlainText -Force | ConvertFrom-SecureString | Set-Content "password.txt"
```

> [!IMPORTANT]
> パスワードを設定するときは、 [複雑さの要件](/previous-versions/windows/it-pro/windows-server-2003/cc786468(v=ws.10))を満たしていることを確認してください。

セキュリティで保護されたパスワード ファイルから資格情報オブジェクトを作成するには、ファイルの内容を読み取り、 [ConvertTo-SecureString](/powershell/module/microsoft.powershell.security/convertto-securestring)を使用して、セキュリティで保護された文字列にもう一度変換する必要があります。

[Set-AzureServiceRemoteDesktopExtension](/powershell/module/servicemanagement/azure.service/set-azureserviceremotedesktopextension) コマンドレットは、ユーザー アカウントの期限が切れる *日時* を指定する **Expiration** パラメーターも受け入れます。 たとえば、現在の日時から数日後にアカウントの期限が切れるように設定することもできます。

次の PowerShell の例では、クラウド サービスでリモート デスクトップ拡張機能を設定する方法を示しています。

```powershell
$servicename = "cloudservice"
$username = "RemoteDesktopUser"
$securepassword = Get-Content -Path "password.txt" | ConvertTo-SecureString
$expiry = $(Get-Date).AddDays(1)
$credential = New-Object System.Management.Automation.PSCredential $username,$securepassword
Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry
```
また、必要に応じて、リモート デスクトップを有効にするデプロイ スロットおよびロールを指定することもできます。 これらのパラメーターが指定されていない場合、コマンドレットは **運用環境** のデプロイ スロットに含まれるすべてのロールでリモート デスクトップを有効にします。

リモート デスクトップ拡張機能は、デプロイに関連付けられています。 サービスの新しいデプロイを作成した場合は、そのデプロイでリモート デスクトップを有効にする必要があります。 常にリモート デスクトップを有効にしておく必要がある場合は、PowerShell スクリプトをデプロイのワークフローに統合することを検討してください。

## <a name="remote-desktop-into-a-role-instance"></a>ロール インスタンスへのリモート デスクトップ接続

クラウド サービスの特定のロール インスタンスにリモート デスクトップ接続するには、 [Get-AzureRemoteDesktopFile](/powershell/module/servicemanagement/azure.service/get-azureremotedesktopfile) コマンドレットを使用します。 *LocalPath* パラメーターを使用すると、RDP ファイルをローカルにダウンロードできます。 *Launch* パラメーターを使用して、クラウド サービスのロール インスタンスにアクセスするための [リモート デスクトップ接続] ダイアログを直接起動することもできます。

```powershell
Get-AzureRemoteDesktopFile -ServiceName $servicename -Name "WorkerRole1_IN_0" -Launch
```

## <a name="check-if-remote-desktop-extension-is-enabled-on-a-service"></a>サービスでリモート デスクトップ拡張機能が有効になっているかどうかを確認する

[Get-AzureServiceRemoteDesktopExtension](/powershell/module/servicemanagement/azure.service/get-azureremotedesktopfile) コマンドレットを使用すると、サービスのデプロイでリモート デスクトップが有効になっているか、無効になっているかが表示されます。 このコマンドレットによって、リモート デスクトップ ユーザーのユーザー名と、リモート デスクトップ拡張機能が有効になっているロールが返されます。 既定では、これはデプロイ スロットで行われるため、代わりにステージング スロットを使用することもできます。

```powershell
Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
```

## <a name="remove-remote-desktop-extension-from-a-service"></a>リモート デスクトップ拡張機能をサービスから削除する

デプロイで既にリモート デスクトップ拡張機能を有効にしている状態で、リモート デスクトップ設定を更新する必要がある場合は、まず拡張機能を削除する必要があります。 次に、新しい設定でもう一度有効にします。 たとえば、リモート ユーザー アカウントに新しいパスワードを設定する必要がある場合や、ユーザー アカウントの有効期限が切れた場合などです。 これは、リモート デスクトップ拡張機能が有効になっている既存のデプロイで必要な操作です。 新しいデプロイの場合は、単に拡張機能を直接適用できます。

リモート デスクトップ拡張機能をデプロイから削除するには、 [Remove-AzureServiceRemoteDesktopExtension](/powershell/module/servicemanagement/azure.service/remove-azureserviceremotedesktopextension) コマンドレットを使用します。 また、必要に応じて、リモート デスクトップ拡張機能を削除するデプロイ スロットおよびロールを指定することもできます。

```powershell
Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration
```

> [!NOTE]
> 拡張機能の構成を完全に削除するには、 *UninstallConfiguration* パラメーターを使用して **Remove** コマンドレットを呼び出す必要があります。
>
> **UninstallConfiguration** パラメーターを使用すると、サービスに適用されている拡張機能の構成がアンインストールされます。 拡張機能の構成は、いずれもサービスの構成に関連付けられています。 **UninstallConfiguration** を使用せずに *Remove* コマンドレットを呼び出すと、拡張機能の構成から <mark>デプロイ</mark>との関連付けが解除されるため、実質的には拡張機能が削除されることになります。 ただし、拡張機能の構成は、サービスに関連付けられたままになります。

## <a name="additional-resources"></a>その他のリソース

[Cloud Services の構成方法](cloud-services-how-to-configure-portal.md)