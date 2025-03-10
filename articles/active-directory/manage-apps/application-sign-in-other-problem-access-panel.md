---
title: Azure AD マイ アプリからのアプリケーションへのサインインに関する問題のトラブルシューティング
description: Azure AD マイ アプリからのアプリケーションへのサインインに関する問題のトラブルシューティング
services: active-directory
author: kenwith
manager: daveba
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: troubleshooting
ms.date: 07/11/2017
ms.author: kenwith
ms.reviewer: japere
ms.custom: contperf-fy21q2
ms.openlocfilehash: a54c7d6cc4ccf1d9f42702be030598ad1edfab24
ms.sourcegitcommit: 94c3c1be6bc17403adbb2bab6bbaf4a717a66009
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2021
ms.locfileid: "103225151"
---
# <a name="troubleshoot-problems-signing-in-to-an-application-from-azure-ad-my-apps"></a>Azure AD マイ アプリからのアプリケーションへのサインインに関する問題のトラブルシューティング

Web ベースのポータルであるマイ アプリを使用すると、Azure Active Directory (Azure AD) の職場または学校アカウントを持つユーザーは、Azure AD 管理者によってアクセスを許可されたクラウドベースのアプリケーションを表示および起動することができます。 マイ アプリは、[https://myapps.microsoft.com](https://myapps.microsoft.com) で Web ブラウザーの使用によりアクセスされます。

アプリの ID プロバイダーとして Azure AD を使用する方法については、[Azure AD でのアプリケーション管理とは何か](what-is-application-management.md)に関するページを参照してください。 すばやく開始するには、[アプリケーション管理のクイックスタート シリーズ](view-applications-portal.md)に関するページをご確認ください。

これらのアプリケーションは、Azure AD ポータルでユーザーに代わって構成されます。 マイ アプリでアプリケーションを表示するには、アプリケーションが正しく構成され、ユーザーまたはユーザーがメンバーであるグループに割り当てられている必要があります。 

ユーザーに表示されるアプリの種類は、次のカテゴリに分類されます。
-   Microsoft 365 アプリケーション
-   フェデレーション ベースの SSO で構成されたマイクロソフトとサード パーティのアプリケーション
-   パスワードベースの SSO アプリケーション
-   既存の SSO ソリューションのアプリケーション

ここでは、アプリが表示されるかどうかを確認する方法について説明します。
- アプリが Azure AD に追加されていることを確認し、ユーザーが割り当てられていることを確認します。 詳細については、[アプリケーション管理のクイックスタート シリーズ](add-application-portal.md)に関するページを参照してください。
- アプリが最近追加された場合は、ユーザーにサインアウトしてから再度サインインしてもらいます。 
- Office のように、アプリにライセンスが必要な場合は、ユーザーに適切なライセンスが割り当てられていることを確認します。
- ライセンスの変更にかかる時間は、グループの大きさと複雑さによって異なります。

## <a name="general-issues-to-check-first"></a>最初にチェックすべき一般的な問題

-   Web ブラウザーが要件を満たしていることを確認してください。詳細については、[マイ アプリでサポートされているブラウザー](../user-help/my-apps-portal-end-user-access.md)に関するページを参照してください。
-   ユーザーのブラウザーが、その **信頼済みサイト** にアプリケーションの URL を追加していることを確認します。
-   アプリケーションが正しく **構成** されていることを確認します。
-   ユーザーのアカウントがサインイン用に **有効になっている** ことを確認します。
-   ユーザーのアカウントが **ロックアウトされていない** ことを確認します。
-   ユーザーの **パスワードが期限切れになったり、忘れられたりしていない** ことを確認します。
-   **多要素認証** がユーザー アクセスをブロックしていないことを確認します。
-   **条件付きアクセス ポリシー** または **ID 保護** ポリシーがユーザー アクセスをブロックしていないことを確認します。
-   ユーザーの **認証の連絡先情報** が最新のものであり、多要素認証または条件付きアクセス ポリシーを適用できることを確認します。
-   ブラウザーの Cookie を削除してから、再度サインインを試行して、サインインできることを確認します。

## <a name="problems-with-the-users-account"></a>ユーザーのアカウントに関する問題
ユーザーのアカウントに問題があるために、マイ アプリへのアクセスがブロックされる可能性があります。 ユーザーとそのアカウントの設定に関する問題をトラブルシューティングして解決するための方法を次に示します。
-   [ユーザー アカウントが Azure Active Directory に存在するかどうかを調べる](#check-if-a-user-account-exists-in-azure-active-directory)
-   [ユーザーのアカウントの状態を確認する](#check-a-users-account-status)
-   [ユーザーのパスワードをリセットする](#reset-a-users-password)
-   [セルフサービス パスワード リセットを有効にする](#enable-self-service-password-reset)
-   [ユーザーの多要素認証の状態を確認する](#check-a-users-multi-factor-authentication-status)
-   [ユーザーの認証の連絡先情報を確認する](#check-a-users-authentication-contact-info)
-   [ユーザーのグループ メンバーシップを確認する](#check-a-users-group-memberships)
-   [ユーザーに 999 個を超えるアプリ ロールの割り当てがあるかどうかを確認する](#check-if-a-user-has-more-than-999-app-role-assignments)
-   [ユーザーに割り当てられているライセンスを確認する](#check-a-users-assigned-licenses)
-   [ユーザーにライセンスを割り当てる](#assign-a-user-a-license)

### <a name="check-if-a-user-account-exists-in-azure-active-directory"></a>ユーザー アカウントが Azure Active Directory に存在するかどうかを調べる
ユーザーのアカウントが存在するかどうかを確認するには、次の手順に従います。
1.  [**Azure Portal**](https://portal.azure.com/) を開き、**グローバル管理者** としてサインインします。
2.  左側のメイン ナビゲーション メニューの上部にある **[すべてのサービス]** を選択して **[Azure Active Directory 拡張機能]** を開きます。
3.  フィルター検索ボックスに「**Azure Active Directory**」と入力し、 **[Azure Active Directory]** 項目を選択します。
4.  ナビゲーション メニューで **[ユーザーとグループ]** を選択します。
5.  **[すべてのユーザー]** を選択します。
6.  対象のユーザーを **検索** し、**行を選択** することでユーザーを選択します。
7.  ユーザー オブジェクトのプロパティを調べて、予想した通りに表示されていることと、紛失したデータがないことを確認します。

### <a name="check-a-users-account-status"></a>ユーザーのアカウントの状態を確認する
ユーザーのアカウントの状態を確認するには、次の手順に従います。
1.  [**Azure Portal**](https://portal.azure.com/) を開き、**グローバル管理者** としてサインインします。
2.  左側のメイン ナビゲーション メニューの上部にある **[すべてのサービス]** を選択して **[Azure Active Directory 拡張機能]** を開きます。
3.  フィルター検索ボックスに「**Azure Active Directory**」と入力し、 **[Azure Active Directory]** 項目を選択します。
4.  ナビゲーション メニューで **[ユーザーとグループ]** を選択します。
5.  **[すべてのユーザー]** を選択します。
6.  対象のユーザーを **検索** し、**行を選択** することでユーザーを選択します。
7.  **[プロファイル]** を選択します。
8.  **[設定]** で、**[サインインのブロック]** が **[いいえ]** に設定されていることを確認します。

### <a name="reset-a-users-password"></a>ユーザーのパスワードをリセットする
ユーザーのパスワードをリセットするには、次の手順に従います。
1.  [**Azure Portal**](https://portal.azure.com/) を開き、**グローバル管理者** としてサインインします。
2.  左側のメイン ナビゲーション メニューの上部にある **[すべてのサービス]** を選択して **[Azure Active Directory 拡張機能]** を開きます。
3.  フィルター検索ボックスに「**Azure Active Directory**」と入力し、 **[Azure Active Directory]** 項目を選択します。
4.  ナビゲーション メニューで **[ユーザーとグループ]** を選択します。
5.  **[すべてのユーザー]** を選択します。
6.  対象のユーザーを **検索** し、**行を選択** することでユーザーを選択します。
7.  ユーザー ウィンドウの上部にある **[パスワードのリセット]** ボタンを選択します。
8.  表示された **[パスワードのリセット]** ウィンドウ上の **[パスワードのリセット]** ボタンを選択します。
9.  **一時パスワード** をコピーするか、ユーザーの **新しいパスワードを入力** します。
10. この新しいパスワードをユーザーに知らせます。ユーザーは次の Azure Active Directory サインイン時にこのパスワードを変更する必要があります。

### <a name="enable-self-service-password-reset"></a>セルフサービス パスワード リセットを有効にする
セルフサービス パスワード リセットを有効にするには、次のデプロイ手順に従います。
-   [Azure Active Directory パスワードをユーザーがリセットできるようにする](../authentication/tutorial-enable-sspr.md)
-   [オンプレミス Active Directory パスワードをユーザーがリセットまたは変更できるようにする](../authentication/tutorial-enable-sspr.md)

### <a name="check-a-users-multi-factor-authentication-status"></a>ユーザーの多要素認証の状態を確認する
ユーザーの多要素認証の状態を確認するには、次の手順に従います。
1. [**Azure Portal**](https://portal.azure.com/) を開き、**グローバル管理者** としてサインインします。
2. 左側のメイン ナビゲーション メニューの上部にある **[すべてのサービス]** を選択して **[Azure Active Directory 拡張機能]** を開きます。
3. フィルター検索ボックスに「**Azure Active Directory**」と入力し、 **[Azure Active Directory]** 項目を選択します。
4. ナビゲーション メニューで **[ユーザーとグループ]** を選択します。
5. **[すべてのユーザー]** を選択します。
6. ウィンドウの上部にある **[多要素認証]** ボタンを選択します。
7. **多要素認証管理ポータル** をロードしたら、自分が **[ユーザー]** タブに存在していることを確認します。
8. ユーザーの一覧で、検索、フィルター処理、または並べ替えによってユーザーを見つけます。
9. ユーザーの一覧からユーザーを選択し、多要素認証を必要に応じて **有効**、**無効**、または **適用** します。
   >[!NOTE]
   >ユーザーが **適用** 状態の場合は、一時的に **無効** に設定してユーザーが自分のアカウントに戻れるようにできます。 ユーザーがアカウントに戻ったら、ユーザーの状態を再度 **有効** に変更して、ユーザーの連絡先情報を次のサインイン時に再登録するように要求します。 あるいは、[ユーザーの認証の連絡先情報を確認する](#check-a-users-authentication-contact-info)の手順に従って、このユーザーのデータを検証または設定することができます。

### <a name="check-a-users-authentication-contact-info"></a>ユーザーの認証の連絡先情報を確認する
多要素認証、条件付きアクセス、ID 保護、およびパスワード リセットで使用されるユーザーの認証の連絡先情報を確認するには、次の手順に従います。
1.  [**Azure Portal**](https://portal.azure.com/) を開き、**グローバル管理者** としてサインインします。
2.  左側のメイン ナビゲーション メニューの上部にある **[すべてのサービス]** を選択して **[Azure Active Directory 拡張機能]** を開きます。
3.  フィルター検索ボックスに「**Azure Active Directory**」と入力し、 **[Azure Active Directory]** 項目を選択します。
4.  ナビゲーション メニューで **[ユーザーとグループ]** を選択します。
5.  **[すべてのユーザー]** を選択します。
6.  対象のユーザーを **検索** し、**行を選択** することでユーザーを選択します。
7.  **[プロファイル]** を選択します。
8.  下の **[認証の連絡先情報]** までスクロールします。
9.  登録されたユーザーのデータを **レビュー** して、必要に応じて更新します。

### <a name="check-a-users-group-memberships"></a>ユーザーのグループ メンバーシップを確認する
ユーザーのグループ メンバーシップを確認するには、次の手順に従います。
1.  [**Azure Portal**](https://portal.azure.com/) を開き、**グローバル管理者** としてサインインします。
2.  左側のメイン ナビゲーション メニューの上部にある **[すべてのサービス]** を選択して **[Azure Active Directory 拡張機能]** を開きます。
3.  フィルター検索ボックスに「**Azure Active Directory**」と入力し、 **[Azure Active Directory]** 項目を選択します。
4.  ナビゲーション メニューで **[ユーザーとグループ]** を選択します。
5.  **[すべてのユーザー]** を選択します。
6.  対象のユーザーを **検索** し、**行を選択** することでユーザーを選択します。
7.  **[グループ]** を選択すると、ユーザーがメンバーになっているグループが表示されます。

### <a name="check-if-a-user-has-more-than-999-app-role-assignments"></a>ユーザーに 999 個を超えるアプリ ロールの割り当てがあるかどうかを確認する
ユーザーに 999 個を超えるアプリ ロールの割り当てがある場合、それらのアプリの一部がマイ アプリに表示されないことがあります。

これは、ユーザーが割り当てられているアプリを特定するために、マイ アプリでは現在、最大 999 個のアプリ ロールの割り当てが読み取られるためです。 ユーザーが 999 個を超えるアプリに割り当てられている場合、マイ アプリ ポータルに表示されるアプリを制御することはできません。

ユーザーに 999 個を超えるアプリ ロールの割り当てがあるかどうかを確認するには、以下の手順に従います。
1. [**Microsoft.Graph**](https://github.com/microsoftgraph/msgraph-sdk-powershell) PowerShell モジュールをインストールします。
2. `Connect-MgGraph -Scopes "User.ReadBasic.All Application.Read.All"` を実行します。
3. `(Get-MgUserAppRoleAssignment -UserId "<user-id>" -PageSize 999).Count` を実行して、ユーザーが現在付与されているアプリ ロールの割り当て数を確認します。
4. 結果が 999 の場合、そのユーザーには 999 個を超えるアプリ ロールの割り当てがある可能性があります。

### <a name="check-a-users-assigned-licenses"></a>ユーザーに割り当てられているライセンスを確認する
ユーザーに割り当てられているライセンスを確認するには、次の手順に従います。
1.  [**Azure Portal**](https://portal.azure.com/) を開き、**グローバル管理者** としてサインインします。
2.  左側のメイン ナビゲーション メニューの上部にある **[すべてのサービス]** を選択して **[Azure Active Directory 拡張機能]** を開きます。
3.  フィルター検索ボックスに「**Azure Active Directory**」と入力し、 **[Azure Active Directory]** 項目を選択します。
4.  ナビゲーション メニューで **[ユーザーとグループ]** を選択します。
5.  **[すべてのユーザー]** を選択します。
6.  対象のユーザーを **検索** し、**行を選択** することでユーザーを選択します。
7.  **[ライセンス]** を選択して、ユーザーが現在割り当てられているライセンスを確認します。

### <a name="assign-a-user-a-license"></a>ユーザーにライセンスを割り当てる 
ユーザーにライセンスを割り当てるには、次の手順に従います。
1.  [**Azure Portal**](https://portal.azure.com/) を開き、**グローバル管理者** としてサインインします。
2.  左側のメイン ナビゲーション メニューの上部にある **[すべてのサービス]** を選択して **[Azure Active Directory 拡張機能]** を開きます。
3.  フィルター検索ボックスに「**Azure Active Directory**」と入力し、 **[Azure Active Directory]** 項目を選択します。
4.  ナビゲーション メニューで **[ユーザーとグループ]** を選択します。
5.  **[すべてのユーザー]** を選択します。
6.  対象のユーザーを **検索** し、**行を選択** することでユーザーを選択します。
7.  **[ライセンス]** を選択して、ユーザーが現在割り当てられているライセンスを確認します。
8.  **[割り当て]** ボタンを選択します。
9.  **1 つまたは複数の製品** を利用可能な製品の一覧から選択します。
10. **省略可能** より細かい単位で製品を割り当てるには、 **[割り当てオプション]** 項目を選択します。 **[OK]** を選択します。
11. **[割り当て]** ボタンを選択して、これらのライセンスをこのユーザーに割り当てます。

## <a name="troubleshooting-deep-links"></a>ディープ リンクのトラブルシューティング
ディープ リンクまたはユーザー アクセス URL は、ユーザーがブラウザーの URL バーから直接自分のパスワード SSO アプリケーションにアクセスするために使用する可能性のあるリンクです。 このリンクに移動すると、ユーザーは最初にマイ アプリに移動しなくても、アプリケーションに自動的にサインインされます。 このリンクは、ユーザーが Microsoft 365 アプリケーション起動プログラムからこれらのアプリケーションにアクセスするために使用するものと同じです。

### <a name="checking-the-deep-link"></a>ディープ リンクの確認

正しいディープ リンクが存在するかどうかを確認するには、次の手順に従います。
1. [**Azure Portal**](https://portal.azure.com/) を開き、**グローバル管理者** または **共同管理者** としてサインインします。
2. 左側のメイン ナビゲーション メニューの上部にある **[すべてのサービス]** を選択して **[Azure Active Directory 拡張機能]** を開きます。
3. フィルター検索ボックスに「**Azure Active Directory**」と入力し、 **[Azure Active Directory]** 項目を選択します。
4. Azure Active Directory の左側のナビゲーション メニューから **[エンタープライズ アプリケーション]** を選択します。
5. **[すべてのアプリケーション]** を選択して、すべてのアプリケーションの一覧を表示します。
   * ここに表示したいアプリケーションが表示されない場合は、 **[All Applications List (すべてのアプリケーション リスト)]** の上部にある **[フィルター]** コントロールを使用して、 **[表示]** オプションを **[すべてのアプリケーション]** に設定します。
6. [**Azure Portal**](https://portal.azure.com/) を開き、**グローバル管理者** または **共同管理者** としてサインインします。
7. 左側のメイン ナビゲーション メニューの上部にある **[すべてのサービス]** を選択して **[Azure Active Directory 拡張機能]** を開きます。
8. フィルター検索ボックスに「**Azure Active Directory**」と入力し、 **[Azure Active Directory]** 項目を選択します。
9. Azure Active Directory の左側のナビゲーション メニューから **[エンタープライズ アプリケーション]** を選択します。
10. **[すべてのアプリケーション]** を選択して、すべてのアプリケーションの一覧を表示します。
    * ここに表示したいアプリケーションが表示されない場合は、 **[All Applications List (すべてのアプリケーション リスト)]** の上部にある **[フィルター]** コントロールを使用して、 **[表示]** オプションを **[すべてのアプリケーション]** に設定します。
11. ディープ リンクを確認するアプリケーションを選択します。
12. ラベル **[ユーザー アクセス URL]** を見つけます。 ディープ リンクはこの URL に一致している必要があります。

## <a name="contact-support"></a>サポートにお問い合せください
次の情報 (取得可能な場合) を含むサポート チケットを開きます。
-   関連エラー ID
-   UPN (ユーザーの電子メール アドレス)
-   TenantId
-   ブラウザーの種類
-   タイム ゾーンとエラーが発生した時刻/タイムフレーム
-   Fiddler のトレース

## <a name="next-steps"></a>次のステップ
- [アプリケーション管理のクイックスタート シリーズ](view-applications-portal.md)
