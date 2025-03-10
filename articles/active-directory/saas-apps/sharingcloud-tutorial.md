---
title: チュートリアル:Azure Active Directory シングル サインオン (SSO) と SharingCloud の統合 | Microsoft Docs
description: Azure Active Directory と Instant Suite の間でシングル サインオンを構成する方法について説明します。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/09/2021
ms.author: jeedes
ms.openlocfilehash: 7ae447a9577feba8b43b5b03a757ec4095ee2cb4
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/05/2021
ms.locfileid: "102177965"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-sharingcloud"></a>チュートリアル:Azure Active Directory シングル サインオン (SSO) と SharingCloud の統合

このチュートリアルでは、SharingCloud と Azure Active Directory (Azure AD) を統合する方法について説明します。 SharingCloud と Azure AD を統合すると、次のことができます。

* SharingCloud にアクセスする Azure AD ユーザーを制御する。
* ユーザーが自分の Azure AD アカウントを使用して SharingCloud に自動的にサインインできるように設定する。
* 1 つの中央サイト (Azure Portal) で自分のアカウントを管理します。

SaaS アプリと Azure AD の統合の詳細については、「[Azure Active Directory でのアプリケーションへのシングル サインオン](../manage-apps/what-is-single-sign-on.md)」を参照してください。

## <a name="prerequisites"></a>前提条件

開始するには、次が必要です。

* Azure AD サブスクリプション。 サブスクリプションをお持ちでない場合は、[ここ](https://azure.microsoft.com/pricing/free-trial/)から 1 か月間の無料試用版を入手できます。
* SharingCloud でのシングル サインオン (SSO) が有効なサブスクリプション。

## <a name="scenario-description"></a>シナリオの説明

このチュートリアルでは、テスト環境で Azure AD の SSO を構成してテストします。

* SharingCloud では、**SP Initiated SSO と IDP Initiated SSO** がサポートされます
* SharingCloud では、**Just-In-Time** ユーザー プロビジョニングがサポートされます

## <a name="adding-sharingcloud-from-the-gallery"></a>ギャラリーからの SharingCloud の追加

Azure AD への SharingCloud の統合を構成するには、ギャラリーからマネージド SaaS アプリの一覧に SharingCloud を追加する必要があります。

1. 職場または学校アカウントか、個人の Microsoft アカウントを使用して、[Azure portal](https://portal.azure.com) にサインインします。
1. 左のナビゲーション ウィンドウで **[Azure Active Directory]** サービスを選択します。

    ![Azure Active Directory のボタン](common/select-azuread.png)
    
1. **[エンタープライズ アプリケーション]** に移動し、 **[すべてのアプリケーション]** を選択します。

    ![[エンタープライズ アプリケーション] ブレード](common/enterprise-applications.png)
    
1. 新しいアプリケーションを追加するには、 **[新しいアプリケーション]** を選択します。

    ![[新しいアプリケーション] ボタン](common/add-new-app.png)
    
1. **[ギャラリーから追加する]** セクションで、検索ボックスに、「**SharingCloud**」と入力します。

    ![結果一覧の SharingCloud](common/search-new-app.png)
    
1. 結果のパネルから **[SharingCloud]** を選択し、アプリを追加します。 お使いのテナントにアプリが追加されるのを数秒待機します。

## <a name="configure-and-test-azure-ad-single-sign-on-for-sharingcloud"></a>SharingCloud の Azure AD シングル サインオンの構成とテスト

**B.Simon** というテスト ユーザーを使用して、SharingCloud に対する Azure AD SSO を構成してテストします。 SSO が機能するためには、Azure AD ユーザーと SharingCloud の関連ユーザーとの間にリンク関係を確立する必要があります。

SharingCloud で Azure AD SSO を構成してテストするには、次の構成要素を完了する必要があります。

1. **[Azure AD SSO の構成](#configure-azure-ad-sso)** - ユーザーがこの機能を使用できるようにします。
    1. **[Azure AD のテスト ユーザーの作成](#create-an-azure-ad-test-user)** - B.Simon で Azure AD のシングル サインオンをテストします。
    1. **[Azure AD テスト ユーザーの割り当て](#assign-the-azure-ad-test-user)** - B.Simon が Azure AD シングル サインオンを使用できるようにします。
1. **[SharingCloud の SSO の構成](#configure-sharingcloud-sso)** - アプリケーション側でシングル サインオン設定を構成します。
    1. **[SharingCloud のテスト ユーザーの作成](#create-sharingcloud-test-user)** - SharingCloud で B.Simon に対応するユーザーを作成し、Azure AD の B.Simon にリンクさせます。
1. **[SSO のテスト](#test-sso)** - 構成が機能するかどうかを確認します。

## <a name="configure-azure-ad-sso"></a>Azure AD SSO の構成

これらの手順に従って、Azure portal で Azure AD SSO を有効にします。

1. [Azure portal](https://portal.azure.com/) の **SharingCloud** アプリケーション統合ページで、 **[管理]** セクションを探して、 **[シングル サインオン]** を選択します。
    
    ![シングル サインオン構成のリンク](common/select-sso.png)
    
1. **[シングル サインオン方式の選択]** ページで、 **[SAML]** を選択します。

    ![シングル サインオン選択モード](common/select-saml-option.png)

1. **[SAML によるシングル サインオンのセットアップ]** ページで、 **[基本的な SAML 構成]** の **[編集]** アイコンをクリックして設定を編集します。

   ![基本的な SAML 構成を編集する](common/edit-urls.png)

1. **[基本的な SAML 構成]** セクションで、次の手順を実行します。

    SharingCloud から提供された XML ファイルを含むメタデータ ファイルをアップロードします。 このファイルを取得するには、[SharingCloud クライアント サポート チーム](mailto:support@sharingcloud.com)に連絡してください。

    ![image](common/upload-metadata.png)
    
    提供されたメタデータ ファイルを選択し、 **[アップロード]** をクリックします。

    ![image](common/browse-upload-metadata.png)

1. SharingCloud アプリケーションでは、特定の形式の SAML アサーションを受け取るため、SAML トークン属性の構成にカスタム属性マッピングを追加する必要があります。 次のスクリーンショットには、既定の属性一覧が示されています。

    ![image](common/edit_attribute.png)

1. その他に、SharingCloud アプリケーションでは、いくつかの属性が SAML 応答で返されることが想定されています。それらの属性を次に示します。 これらの属性も値が事前に設定されますが、要件に従ってそれらの値を確認することができます。

    | 名前 | ソース属性|
    | ---------------| --------- |
    | urn:sharingcloud:sso:firstname | User.givenname |
    | urn:sharingcloud:sso:lastname | User.surname |
    | urn:sharingcloud:sso:email | User.mail |

1. **[SAML でシングル サインオンをセットアップします]** ページの **[SAML 署名証明書]** セクションに表示されたオプションの **[コピー]** アイコンをクリックし、要件に従って **フェデレーション メタデータ URL** をコピーします。

    ![コピーするメタデータ URL](common/copy_metadataurl.png)

## <a name="configure-sharingcloud-sso"></a>SharingCloud の SSO の構成

**SharingCloud** 側でシングル サインオンを構成するには、Azure portal からコピーした **フェデレーション メタデータ URL** を [SharingCloud サポート チーム](mailto:support@sharingcloud.com)に送信する必要があります。 サポート チームはこれを設定して、SAML SSO 接続が両方の側で正しく設定されるようにします。

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

このセクションでは、B.Simon に SharingCloud へのアクセスを許可することで、このユーザーが Azure シングル サインオンを使用できるようにします。

1. Azure portal で **[エンタープライズ アプリケーション]** を選択し、 **[すべてのアプリケーション]** を選択します。
1. アプリケーションの一覧で **[SharingCloud]** を選択します。
1. アプリの概要ページで、 **[管理]** セクションを見つけて、 **[ユーザーとグループ]** を選択します。

   ![[ユーザーとグループ] リンク](common/users-groups-blade.png)

1. **[ユーザーの追加]** を選択し、 **[割り当ての追加]** ダイアログで **[ユーザーとグループ]** を選択します。

   ![[ユーザーの追加] リンク](common/add-assign-user.png)

1. **[ユーザーとグループ]** ダイアログの [ユーザー] の一覧から **[B.Simon]** を選択し、画面の下部にある **[選択]** ボタンをクリックします。
1. SAML アサーション内に任意のロール値が必要な場合、 **[ロールの選択]** ダイアログでユーザーに適したロールを一覧から選択し、画面の下部にある **[選択]** をクリックします。
1. **[割り当ての追加]** ダイアログで、 **[割り当て]** をクリックします。

### <a name="create-sharingcloud-test-user"></a>SharingCloud テスト ユーザーの作成

このセクションでは、Britta Simon というユーザーを SharingCloud に作成します。 SharingCloud では、Just-In-Time ユーザー プロビジョニングがサポートされています。この設定は既定で有効になっています。 このセクションでは、ユーザー側で必要な操作はありません。 SharingCloud にユーザーがまだ存在していない場合は、認証後に新規に作成されます。

## <a name="test-sso"></a>SSO のテスト 

* SharingCloud の URL に直接移動し、そこからログイン フローを開始します。

## <a name="next-steps"></a>次のステップ

SharingCloud を構成したら、組織の機密データを流出と侵入からリアルタイムで保護するセッション制御を適用することができます。 セッション制御は、条件付きアクセスを拡張したものです。 [Microsoft Cloud App Security でセッション制御を強制する方法](/cloud-app-security/proxy-deployment-any-app)をご覧ください。

