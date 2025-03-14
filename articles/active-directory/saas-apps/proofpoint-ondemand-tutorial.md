---
title: 'チュートリアル: Azure Active Directory と Proofpoint on Demand の統合 | Microsoft Docs'
description: Azure Active Directory と Proofpoint on Demand の間でシングル サインオンを構成する方法について説明します。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/29/2021
ms.author: jeedes
ms.openlocfilehash: e9beb2c9e265982a9e10f3e6abee6bc3cba4a4a3
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/02/2021
ms.locfileid: "101648460"
---
# <a name="tutorial-azure-active-directory-integration-with-proofpoint-on-demand"></a>チュートリアル: Azure Active Directory と Proofpoint on Demand の統合

このチュートリアルでは、Proofpoint on Demand と Azure Active Directory (Azure AD) を統合する方法について説明します。 Proofpoint on Demand と Azure AD を統合すると、次のことができます。

* Proofpoint on Demand にアクセスできるユーザーを Azure AD で制御できます。
* ユーザーが自分の Azure AD アカウントを使用して Proofpoint on Demand に自動的にサインインできるように設定できます。
* 1 つの中央サイト (Azure Portal) で自分のアカウントを管理します。

## <a name="prerequisites"></a>前提条件

開始するには、次が必要です。

* Azure AD サブスクリプション。 サブスクリプションがない場合は、[無料アカウント](https://azure.microsoft.com/free/)を取得できます。
* Proofpoint on Demand のシングル サインオン (SSO) が有効なサブスクリプション。

> [!NOTE]
> Azure AD で MFA またはパスワードレス認証を使用している場合は、SAML 要求で AuthnContext 値をオフにしてください。 そうしないと、Azure AD によって AuthnContext の不一致エラーがスローされ、トークンがアプリケーションに送り返されなくなります。

## <a name="scenario-description"></a>シナリオの説明

このチュートリアルでは、テスト環境で Azure AD のシングル サインオンを構成してテストします。

* Proofpoint on Demand では、**SP** によって開始される SSO がサポートされます

## <a name="add-proofpoint-on-demand-from-the-gallery"></a>ギャラリーからの Proofpoint on Demand の追加

Azure AD への Proofpoint on Demand の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に Proofpoint on Demand を追加する必要があります。

1. 職場または学校アカウントか、個人の Microsoft アカウントを使用して、Azure portal にサインインします。
1. 左のナビゲーション ウィンドウで **[Azure Active Directory]** サービスを選択します。
1. **[エンタープライズ アプリケーション]** に移動し、 **[すべてのアプリケーション]** を選択します。
1. 新しいアプリケーションを追加するには、 **[新しいアプリケーション]** を選択します。
1. **[ギャラリーから追加する]** セクションで、検索ボックスに「**Proofpoint on Demand**」と入力します。
1. 結果パネルで **[Proofpoint on Demand]** を選択し、アプリを追加します。 お使いのテナントにアプリが追加されるのを数秒待機します。

## <a name="configure-and-test-azure-ad-sso-for-proofpoint-on-demand"></a>Proofpoint on Demand の Azure AD SSO の構成とテスト

**B.Simon** というテスト ユーザーを使用して、Proofpoint on Demand に対する Azure AD SSO を構成してテストします。 SSO が機能するためには、Azure AD ユーザーと Proofpoint on Demand の関連ユーザーとの間にリンク関係を確立する必要があります。

Proofpoint on Demand に対して Azure AD SSO を構成してテストするには、次の手順を実行します。

1. **[Azure AD SSO の構成](#configure-azure-ad-sso)** - ユーザーがこの機能を使用できるようにします。
    1. **[Azure AD のテスト ユーザーの作成](#create-an-azure-ad-test-user)** - B.Simon で Azure AD のシングル サインオンをテストします。
    1. **[Azure AD テスト ユーザーの割り当て](#assign-the-azure-ad-test-user)** - B.Simon が Azure AD シングル サインオンを使用できるようにします。
1. **[Proofpoint on Demand の SSO の構成](#configure-proofpoint-on-demand-sso)** - アプリケーション側でシングル サインオン設定を構成します。
    1. **[Proofpoint on Demand のテスト ユーザーの作成](#create-proofpoint-on-demand-test-user)** - Proofpoint on Demand で B.Simon に対応するユーザーを作成し、Azure AD の B.Simon にリンクさせます。
1. **[SSO のテスト](#test-sso)** - 構成が機能するかどうかを確認します。

### <a name="configure-azure-ad-sso"></a>Azure AD SSO の構成

これらの手順に従って、Azure portal で Azure AD SSO を有効にします。

1. Azure portal の **Proofpoint on Demand** アプリケーション統合ページで、 **[管理]** セクションを見つけて、 **[シングル サインオン]** を選択します。
1. **[シングル サインオン方式の選択]** ページで、 **[SAML]** を選択します。
1. **[SAML によるシングル サインオンのセットアップ]** ページで、 **[基本的な SAML 構成]** の鉛筆アイコンをクリックして設定を編集します。

   ![基本的な SAML 構成を編集する](common/edit-urls.png)

4. **[基本的な SAML 構成]** セクションで、次の手順を実行します。

    ![[Proofpoint on Demand のドメインと URL] のシングル サインオン情報](common/sp-identifier-reply.png)

    a. **[サインオン URL]** ボックスに、`https://<hostname>.pphosted.com/ppssamlsp_hostname` という形式で URL を入力します。

    b. **[識別子]** ボックスに、`https://<hostname>.pphosted.com/ppssamlsp` という形式で URL を入力します。

    c. **[応答 URL]** ボックスに、`https://<hostname>.pphosted.com:portnumber/v1/samlauth/samlconsumer` のパターンを使用して URL を入力します

    > [!NOTE]
    > これらは実際の値ではありません。 実際のサインオン URL、識別子、および応答 URL で値を更新します。 これらの値を取得するには、[Proofpoint on Demand クライアント サポート チーム](https://www.proofpoint.com/us/support-services)に問い合わせてください。 Azure portal の **[基本的な SAML 構成]** セクションに示されているパターンを参照することもできます。

5. **[SAML でシングル サインオンをセットアップします]** ページの **[SAML 署名証明書]** セクションで、 **[ダウンロード]** をクリックして要件のとおりに指定したオプションからの **証明書 (Base64)** をダウンロードして、お使いのコンピューターに保存します。

    ![証明書のダウンロードのリンク](common/certificatebase64.png)

6. **[Proofpoint on Demand のセットアップ]** セクションで、要件どおりの適切な URL をコピーします。

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

このセクションでは、B.Simon に Proofpoint on Demand へのアクセスを許可することで、このユーザーが Azure シングル サインオンを使用できるようにします。

1. Azure portal で **[エンタープライズ アプリケーション]** を選択し、 **[すべてのアプリケーション]** を選択します。
1. アプリケーションの一覧で、**[Proofpoint on Demand]** を選択します。
1. アプリの概要ページで、 **[管理]** セクションを見つけて、 **[ユーザーとグループ]** を選択します。
1. **[ユーザーの追加]** を選択し、 **[割り当ての追加]** ダイアログで **[ユーザーとグループ]** を選択します。
1. **[ユーザーとグループ]** ダイアログの [ユーザー] の一覧から **[B.Simon]** を選択し、画面の下部にある **[選択]** ボタンをクリックします。
1. ユーザーにロールが割り当てられることが想定される場合は、 **[ロールの選択]** ドロップダウンからそれを選択できます。 このアプリに対してロールが設定されていない場合は、[既定のアクセス] ロールが選択されていることを確認します。
1. **[割り当ての追加]** ダイアログで、 **[割り当て]** をクリックします。

### <a name="configure-proofpoint-on-demand-sso"></a>Proofpoint on Demand の SSO の構成

**Proofpoint on Demand** 側でシングル サインオンを構成するには、ダウンロードした **証明書 (Base64)** と Azure portal からコピーした適切な URL を [Proofpoint on Demand サポート チーム](https://www.proofpoint.com/us/support-services)に送信する必要があります。 サポート チームはこれを設定して、SAML SSO 接続が両方の側で正しく設定されるようにします。

### <a name="create-proofpoint-on-demand-test-user"></a>Proofpoint on Demand のテスト ユーザーの作成

このセクションでは、Proofpoint on Demand で Britta Simon というユーザーを作成します。 [Proofpoint on Demand サポート チーム](https://www.proofpoint.com/us/support-services)と協力して、Proofpoint on Demand プラットフォームにユーザーを追加します。

### <a name="test-sso"></a>SSO のテスト

このセクションでは、次のオプションを使用して Azure AD のシングル サインオン構成をテストします。 

* Azure portal で **[このアプリケーションをテストします]** をクリックします。 これにより、ログイン フローを開始できる Proofpoint on Demand のサインオン URL にリダイレクトされます。 

* Proofpoint on Demand のサインオン URL に直接移動し、そこからログイン フローを開始します。

* Microsoft マイ アプリを使用することができます。 マイ アプリで [Proofpoint on Demand] タイルをクリックすると、SSO を設定した Proofpoint on Demand に自動的にサインインします。 マイ アプリの詳細については、[マイ アプリの概要](../user-help/my-apps-portal-end-user-access.md)に関するページを参照してください。

## <a name="next-steps"></a>次のステップ

Proofpoint on Demand を構成したら、組織の機密データを流出と侵入からリアルタイムで保護するセッション制御を適用できます。 セッション制御は、条件付きアクセスを拡張したものです。 [Microsoft Cloud App Security でセッション制御を強制する方法](/cloud-app-security/proxy-deployment-any-app)をご覧ください。