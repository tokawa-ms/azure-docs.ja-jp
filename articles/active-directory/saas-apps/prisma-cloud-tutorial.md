---
title: チュートリアル:Azure Active Directory シングル サインオン (SSO) と Prisma Cloud SSO の統合 | Microsoft Docs
description: Azure Active Directory と Prisma Cloud SSO の間でシングル サインオンを構成する方法について説明します。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/08/2021
ms.author: jeedes
ms.openlocfilehash: d92ca345b0800523d86ce7b42220209a1f237103
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/02/2021
ms.locfileid: "101651426"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-prisma-cloud-sso"></a>チュートリアル:Azure Active Directory シングル サインオン (SSO) と Prisma Cloud SSO の統合

このチュートリアルでは、Prisma Cloud SSO と Azure Active Directory (Azure AD) を統合する方法について説明します。 Prisma Cloud SSO と Azure AD を統合すると、次のことができます。

* Prisma Cloud SSO にアクセスできるユーザーを Azure AD で制御できます。
* ユーザーが自分の Azure AD アカウントを使用して Prisma Cloud SSO に自動的にサインインできるように設定できます。
* 1 つの中央サイト (Azure Portal) で自分のアカウントを管理します。

## <a name="prerequisites"></a>前提条件

開始するには、次が必要です。

* Azure AD サブスクリプション。 サブスクリプションがない場合は、[無料アカウント](https://azure.microsoft.com/free/)を取得できます。
* Prisma Cloud SSO でのシングル サインオン (SSO) が有効なサブスクリプション。

## <a name="scenario-description"></a>シナリオの説明

このチュートリアルでは、テスト環境で Azure AD の SSO を構成してテストします。

* Prisma Cloud SSO では、**IDP** Initiated SSO がサポートされます。

* Prisma Cloud SSO では、**Just-In-Time** ユーザー プロビジョニングがサポーされます。

## <a name="add-prisma-cloud-sso-from-the-gallery"></a>ギャラリーからの Prisma Cloud SSO の追加

Azure AD への Prisma Cloud SSO の統合を構成するには、ギャラリーからマネージド SaaS アプリの一覧に Prisma Cloud SSO を追加する必要があります。

1. 職場または学校アカウントか、個人の Microsoft アカウントを使用して、Azure portal にサインインします。
1. 左のナビゲーション ウィンドウで **[Azure Active Directory]** サービスを選択します。
1. **[エンタープライズ アプリケーション]** に移動し、 **[すべてのアプリケーション]** を選択します。
1. 新しいアプリケーションを追加するには、 **[新しいアプリケーション]** を選択します。
1. **[ギャラリーから追加する]** セクションで、検索ボックスに、「**Prisma Cloud SSO**」と入力します。
1. 結果のパネルから **[Prisma Cloud SSO]** を選択し、アプリを追加します。 お使いのテナントにアプリが追加されるのを数秒待機します。

## <a name="configure-and-test-azure-ad-sso-for-prisma-cloud-sso"></a>Prisma Cloud SSO の Azure AD SSO の構成とテスト

**B.Simon** というテスト ユーザーを使用して、Prisma Cloud SSO に対する Azure AD SSO を構成してテストします。 SSO が機能するためには、Azure AD ユーザーと Prisma Cloud SSO の関連ユーザーとの間にリンク関係を確立する必要があります。

Prisma Cloud SSO に対して Azure AD SSO を構成してテストするには、次の手順を行います。

1. **[Azure AD SSO の構成](#configure-azure-ad-sso)** - ユーザーがこの機能を使用できるようにします。
    1. **[Azure AD のテスト ユーザーの作成](#create-an-azure-ad-test-user)** - B.Simon で Azure AD のシングル サインオンをテストします。
    1. **[Azure AD テスト ユーザーの割り当て](#assign-the-azure-ad-test-user)** - B.Simon が Azure AD シングル サインオンを使用できるようにします。
1. **[Prisma Cloud の SSO の構成](#configure-prisma-cloud-sso)** - アプリケーション側でシングル サインオン設定を構成します。
    1. **[Prisma Cloud SSO のテスト ユーザーの作成](#create-prisma-cloud-sso-test-user)** - Prisma Cloud SSO で B.Simon に対応するユーザーを作成し、Azure AD の B.Simon にリンクさせます。
1. **[SSO のテスト](#test-sso)** - 構成が機能するかどうかを確認します。

## <a name="configure-azure-ad-sso"></a>Azure AD SSO の構成

これらの手順に従って、Azure portal で Azure AD SSO を有効にします。

1. Azure portal の **Prisma Cloud SSO** アプリケーション統合ページで、 **[管理]** セクションを探して、 **[シングル サインオン]** を選択します。
1. **[シングル サインオン方式の選択]** ページで、 **[SAML]** を選択します。
1. **[SAML によるシングル サインオンのセットアップ]** ページで、 **[基本的な SAML 構成]** の鉛筆アイコンをクリックして設定を編集します。

   ![基本的な SAML 構成を編集する](common/edit-urls.png)

1. **[SAML でシングル サインオンをセットアップします]** ページで、次のフィールドの値を入力します。

    a. **[識別子]** ボックスに、`https://app2.prismacloud.io/customer/<CUSTOMERID>` の形式で URL を入力します。

    b. **[応答 URL]** の値は固定されており、Azure portal に事前設定されています。 実際の要件に応じた適切な URL を選択する必要があります。

    > [!NOTE]
    > この識別子の値は実際のものではありません。 この値を実際の識別子で更新してください。 この値を取得するには、[Prisma Cloud SSO クライアント サポート チーム](mailto:support@paloaltonetworks.com)に問い合わせてください。 Azure portal の **[基本的な SAML 構成]** セクションに示されているパターンを参照することもできます。

1. **[SAML でシングル サインオンをセットアップします]** ページの **[SAML 署名証明書]** セクションで、 **[証明書 (Base64)]** を見つけて、 **[ダウンロード]** を選択し、証明書をダウンロードして、お使いのコンピューターに保存します。

    ![証明書のダウンロードのリンク](common/certificatebase64.png)

1. **[Set up Prisma Cloud SSO]\(Prisma Cloud SSO のセットアップ\)** セクションで、要件に基づいて適切な URL をコピーします。

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

このセクションでは、B. Simon に Prisma Cloud SSO へのアクセスを許可することで、このユーザーが Azure シングル サインオンを使用できるようにします。

1. Azure portal で **[エンタープライズ アプリケーション]** を選択し、 **[すべてのアプリケーション]** を選択します。
1. アプリケーションの一覧で **[Prisma Cloud SSO]** を選択します。
1. アプリの概要ページで、 **[管理]** セクションを見つけて、 **[ユーザーとグループ]** を選択します。
1. **[ユーザーの追加]** を選択し、 **[割り当ての追加]** ダイアログで **[ユーザーとグループ]** を選択します。
1. **[ユーザーとグループ]** ダイアログの [ユーザー] の一覧から **[B.Simon]** を選択し、画面の下部にある **[選択]** ボタンをクリックします。
1. ユーザーにロールが割り当てられることが想定される場合は、 **[ロールの選択]** ドロップダウンからそれを選択できます。 このアプリに対してロールが設定されていない場合は、[既定のアクセス] ロールが選択されていることを確認します。
1. **[割り当ての追加]** ダイアログで、 **[割り当て]** をクリックします。

## <a name="configure-prisma-cloud-sso"></a>Prisma Cloud の SSO の構成

**Prisma Cloud SSO** 側でシングル サインオンを構成するには、ダウンロードした **証明書 (Base64)** と Azure portal からコピーした適切な URL を [Prisma Cloud SSO サポート チーム](mailto:support@paloaltonetworks.com)に送信する必要があります。 サポート チームはこれを設定して、SAML SSO 接続が両方の側で正しく設定されるようにします。

### <a name="create-prisma-cloud-sso-test-user"></a>Prisma Cloud SSO のテスト ユーザーの作成

このセクションでは、B. Simon というユーザーを Prisma Cloud SSO に作成します。 Prisma Cloud SSO では、Just-In-Time プロビジョニングがサポートされています。この設定は既定で有効になっています。 このセクションでは、ユーザー側で必要な操作はありません。 ユーザーがまだ Prisma Cloud SSO に存在しない場合は、Prisma Cloud SSO にアクセスしようとしたときに新しいユーザーが作成されます。

## <a name="test-sso"></a>SSO のテスト 

このセクションでは、次のオプションを使用して Azure AD のシングル サインオン構成をテストします。

* Azure portal で [このアプリケーションをテストします] をクリックすると、SSO を設定した Prisma Cloud SSO に自動的にサインインされます。

* Microsoft マイ アプリを使用することができます。 マイ アプリで [Prisma Cloud SSO] タイルをクリックすると、SSO を設定した Prisma Cloud SSO に自動的にサインインされます。 マイ アプリの詳細については、[マイ アプリの概要](../user-help/my-apps-portal-end-user-access.md)に関するページを参照してください。

## <a name="next-steps"></a>次のステップ

Prisma Cloud SSO を構成したら、組織の機密データを流出と侵入からリアルタイムで保護するセッション制御を適用することができます。 セッション制御は、条件付きアクセスを拡張したものです。 [Microsoft Cloud App Security でセッション制御を強制する方法](/cloud-app-security/proxy-deployment-any-app)をご覧ください。