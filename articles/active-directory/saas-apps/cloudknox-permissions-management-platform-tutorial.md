---
title: チュートリアル:Azure Active Directory シングル サインオン (SSO) と CloudKnox Permissions Management Platform の統合 | Microsoft Docs
description: Azure Active Directory と CloudKnox Permissions Management Platform の間でシングル サインオンを構成する方法について説明します。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/27/2021
ms.author: jeedes
ms.openlocfilehash: 8c1b45d78d4fb61d239fef178cbcb9fa6efd9539
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/02/2021
ms.locfileid: "101645690"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-cloudknox-permissions-management-platform"></a>チュートリアル:Azure Active Directory シングル サインオン (SSO) と CloudKnox Permissions Management Platform の統合

このチュートリアルでは、CloudKnox Permissions Management Platform と Azure Active Directory (Azure AD) を統合する方法について説明します。 CloudKnox Permissions Management Platform を Azure AD と統合すると、次のことができるようになります。

* CloudKnox Permissions Management Platform にアクセスできるユーザーを Azure AD で制御する。
* ユーザーが自分の Azure AD アカウントを使用して CloudKnox Permissions Management Platform に自動的にサインインできるように設定する。
* 1 つの中央サイト (Azure Portal) で自分のアカウントを管理します。

## <a name="prerequisites"></a>前提条件

開始するには、次が必要です。

* Azure AD サブスクリプション。 サブスクリプションがない場合は、[無料アカウント](https://azure.microsoft.com/free/)を取得できます。
* CloudKnox Permissions Management Platform でのシングル サインオン (SSO) が有効なサブスクリプション。

## <a name="scenario-description"></a>シナリオの説明

このチュートリアルでは、テスト環境で Azure AD の SSO を構成してテストします。

* CloudKnox Permissions Management Platform では、**IDP** Initiated SSO がサポートされます

> [!NOTE]
> このアプリケーションの識別子は固定文字列値であるため、1 つのテナントで構成できるインスタンスは 1 つだけです。

## <a name="adding-cloudknox-permissions-management-platform-from-the-gallery"></a>ギャラリーからの CloudKnox Permissions Management Platform の追加

Azure AD への CloudKnox Permissions Management Platform の統合を構成するには、ギャラリーからマネージド SaaS アプリの一覧に CloudKnox Permissions Management Platform を追加する必要があります。

1. 職場または学校アカウントか、個人の Microsoft アカウントを使用して、Azure portal にサインインします。
1. 左のナビゲーション ウィンドウで **[Azure Active Directory]** サービスを選択します。
1. **[エンタープライズ アプリケーション]** に移動し、 **[すべてのアプリケーション]** を選択します。
1. 新しいアプリケーションを追加するには、 **[新しいアプリケーション]** を選択します。
1. **[ギャラリーから追加する]** セクションで、検索ボックスに「**CloudKnox Permissions Management Platform**」と入力します。
1. 結果のパネルから **[CloudKnox Permissions Management Platform]** を選択し、アプリを追加します。 お使いのテナントにアプリが追加されるのを数秒待機します。


## <a name="configure-and-test-azure-ad-sso-for-cloudknox-permissions-management-platform"></a>CloudKnox Permissions Management Platform の Azure AD SSO の構成とテスト

**B.Simon** というテスト ユーザーを使用して、CloudKnox Permissions Management Platform に対する Azure AD SSO を構成してテストします。 SSO を機能させるためには、Azure AD ユーザーと CloudKnox Permissions Management Platform の関連ユーザーとの間にリンク関係を確立する必要があります。

CloudKnox Permissions Management Platform に対して Azure AD SSO を構成してテストするには、次の手順を実行します。

1. **[Azure AD SSO の構成](#configure-azure-ad-sso)** - ユーザーがこの機能を使用できるようにします。
    1. **[Azure AD のテスト ユーザーの作成](#create-an-azure-ad-test-user)** - B.Simon で Azure AD のシングル サインオンをテストします。
    1. **[Azure AD テスト ユーザーの割り当て](#assign-the-azure-ad-test-user)** - B.Simon が Azure AD シングル サインオンを使用できるようにします。
1. **[CloudKnox Permissions Management Platform の SSO の構成](#configure-cloudknox-permissions-management-platform-sso)** - アプリケーション側でシングル サインオン設定を構成します。
    1. **[CloudKnox Permissions Management Platform のテスト ユーザーの作成](#create-cloudknox-permissions-management-platform-test-user)** - CloudKnox Permissions Management Platform で B.Simon に対応するユーザーを作成し、Azure AD のこのユーザーにリンクさせます。
1. **[SSO のテスト](#test-sso)** - 構成が機能するかどうかを確認します。

## <a name="configure-azure-ad-sso"></a>Azure AD SSO の構成

これらの手順に従って、Azure portal で Azure AD SSO を有効にします。

1. Azure portal の **CloudKnox Permissions Management Platform** アプリケーション統合ページで、 **[管理]** セクションを探して、 **[シングル サインオン]** を選択します。
1. **[シングル サインオン方式の選択]** ページで、 **[SAML]** を選択します。
1. **[SAML によるシングル サインオンのセットアップ]** ページで、 **[基本的な SAML 構成]** の鉛筆アイコンをクリックして設定を編集します。

   ![基本的な SAML 構成を編集する](common/edit-urls.png)

1. **[SAML でシングル サインオンをセットアップします]** ページで、次のフィールドの値を入力します。

    **[応答 URL]** ボックスに、`https://app.cloudknox.io/saml/<ID>` のパターンを使用して URL を入力します

    > [!NOTE]
    > 応答 URL 値は、実際の値ではありません。 実際の応答 URL でこの値を更新します。 この値を取得するには、[CloudKnox Permissions Management Platform クライアント サポート チーム](mailto:support@cloudknox.io)に問い合わせてください。 Azure portal の **[基本的な SAML 構成]** セクションに示されているパターンを参照することもできます。

1. CloudKnox Permissions Management Platform アプリケーションでは、特定の形式の SAML アサーションが求められます。そのため、カスタム属性マッピングを SAML トークン属性構成に追加する必要があります。 次のスクリーンショットには、既定の属性一覧が示されています。

    ![image](common/default-attributes.png)

1. その他に、CloudKnox Permissions Management Platform アプリケーションでは、いくつかの属性が SAML 応答で返されることが想定されています。それらの属性を次に示します。 これらの属性も値が事前に設定されますが、要件に従ってそれらの値を確認することができます。
    
    | 名前 |  ソース属性|
    | --------------- | --------- |
    | First_Name | User.givenname |
    | グループ | user.groups |
    | Last_Name | User.surname |
    | Email_Address | User.mail |

1. **[SAML でシングル サインオンをセットアップします]** ページの **[SAML 署名証明書]** セクションで、 **[フェデレーション メタデータ XML]** を探して **[ダウンロード]** を選択し、証明書をダウンロードして、お使いのコンピューターに保存します。

    ![証明書のダウンロードのリンク](common/metadataxml.png)

1. **[CloudKnox Permissions Management Platform のセットアップ]** セクションで、要件に基づいて適切な URL をコピーします。

    ![構成 URL のコピー](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD のテスト ユーザーの作成

このセクションでは、Azure portal 内で B.Simon というテスト ユーザーを作成します。

1. Azure portal の左側のウィンドウから、 **[Azure Active Directory]** 、 **[ユーザー]** 、 **[すべてのユーザー]** の順に選択します。
1. 画面の上部にある **[新しいユーザー]** を選択します。
1. **[ユーザー]** プロパティで、以下の手順を実行します。
   1. **[名前]** フィールドに「`B.Simon`」と入力します。  
   1. **[ユーザー名]** フィールドに「username@companydomain.extension」と入力します。 たとえば、「 `B.Simon@contoso.com` 」のように入力します。
   1. **[パスワードを表示]** チェック ボックスをオンにし、 **[パスワード]** ボックスに表示された値を書き留めます。
   1. **Create** をクリックしてください。

### <a name="assign-the-azure-ad-test-user"></a>Azure AD テスト ユーザーの割り当て

このセクションでは、B.Simon に CloudKnox Permissions Management Platform へのアクセスを許可することで、このユーザーが Azure シングル サインオンを使用できるようにします。

1. Azure portal で **[エンタープライズ アプリケーション]** を選択し、 **[すべてのアプリケーション]** を選択します。
1. アプリケーションの一覧で **[CloudKnox Permissions Management Platform]** を選択します。
1. アプリの概要ページで、 **[管理]** セクションを見つけて、 **[ユーザーとグループ]** を選択します。
1. **[ユーザーの追加]** を選択し、 **[割り当ての追加]** ダイアログで **[ユーザーとグループ]** を選択します。
1. **[ユーザーとグループ]** ダイアログの [ユーザー] の一覧から **[B.Simon]** を選択し、画面の下部にある **[選択]** ボタンをクリックします。
1. ユーザーにロールが割り当てられることが想定される場合は、 **[ロールの選択]** ドロップダウンからそれを選択できます。 このアプリに対してロールが設定されていない場合は、[既定のアクセス] ロールが選択されていることを確認します。
1. **[割り当ての追加]** ダイアログで、 **[割り当て]** をクリックします。

## <a name="configure-cloudknox-permissions-management-platform-sso"></a>CloudKnox Permissions Management Platform の SSO の構成

**CloudKnox Permissions Management Platform** 側でシングル サインオンを構成するには、ダウンロードした **フェデレーション メタデータ XML** と Azure portal からコピーした適切な URL を [CloudKnox Permissions Management Platform サポート チーム](mailto:support@cloudknox.io)に送信する必要があります。 サポート チームはこれを設定して、SAML SSO 接続が両方の側で正しく設定されるようにします。

### <a name="create-cloudknox-permissions-management-platform-test-user"></a>CloudKnox Permissions Management Platform のテスト ユーザーの作成

このセクションでは、CloudKnox Permissions Management Platform で Britta Simon というテスト ユーザーを作成します。 [CloudKnox Permissions Management Platform サポート チーム](mailto:support@cloudknox.io)と連携して、CloudKnox Permissions Management Platform プラットフォームにユーザーを追加してください。 シングル サインオンを使用する前に、ユーザーを作成し、有効化する必要があります。

## <a name="test-sso"></a>SSO のテスト 

このセクションでは、次のオプションを使用して Azure AD のシングル サインオン構成をテストします。

* Azure portal で [このアプリケーションをテストします] をクリックすると、SSO を設定した CloudKnox Permissions Management Platform に自動的にサインインされます

* Microsoft マイ アプリを使用することができます。 マイ アプリで [CloudKnox Permissions Management Platform] タイルをクリックすると、SSO を設定した CloudKnox Permissions Management Platform に自動的にサインインされます。 マイ アプリの詳細については、[マイ アプリの概要](../user-help/my-apps-portal-end-user-access.md)に関するページを参照してください。


## <a name="next-steps"></a>次のステップ

CloudKnox Permissions Management Platform を構成したら、組織の機密データを流出と侵入からリアルタイムで保護するセッション制御を適用できます。 セッション制御は、条件付きアクセスを拡張したものです。 [Microsoft Cloud App Security でセッション制御を強制する方法](/cloud-app-security/proxy-deployment-any-app)をご覧ください。