---
title: Azure Migrate の Azure セキュリティ ベースライン
description: Azure Migrate セキュリティ ベースラインでは、Azure セキュリティ ベンチマークで指定されているセキュリティに関する推奨事項を実装するための手順のガイダンスとリソースが提供されます。
author: msmbaldwin
ms.topic: conceptual
ms.date: 12/04/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: e10413987a12b32a5f1ca507aa272bab01b20797
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/18/2021
ms.locfileid: "101094820"
---
# <a name="azure-security-baseline-for-azure-migrate"></a>Azure Migrate の Azure セキュリティ ベースライン

このセキュリティ ベースラインにより、[Azure セキュリティ ベンチマーク バージョン 1.0](../security/benchmarks/overview-v1.md) のガイダンスが Azure Migrate に適用されます。 Azure セキュリティ ベンチマークには、Azure 上のクラウド ソリューションをセキュリティで保護する方法に関する推奨事項がまとめてあります。
内容は、**セキュリティ制御** によってグループ化されています。これは、Azure セキュリティ ベンチマークと、Azure Migrate に適用できる関連ガイダンスによって定義されています。 Azure Migrate に適用できない **制御** は、除外されています。

 
Azure Migrate を Azure セキュリティ ベンチマークに完全にマップする方法については、[完全な Azure Migrate セキュリティ ベースライン マッピング ファイル](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines)を参照してください。

## <a name="identity-management"></a>ID 管理

*詳細については、[Azure セキュリティ ベンチマークの「ID 管理](../security/benchmarks/security-controls-v2-identity-management.md).* 」を参照してください。

### <a name="im-1-standardize-azure-active-directory-as-the-central-identity-and-authentication-system"></a>IM-1:Azure Active Directory を中央 ID および認証システムとして標準化する

**ガイダンス**:Azure Migrate では、Azure Active Directory (Azure AD) を既定の ID およびアクセス管理サービスとして使用します。 Azure AD を標準化して、次のリソースでの組織の ID とアクセス管理を統制する必要があります。

- Azure portal、Azure Storage、Azure 仮想マシン (Linux と Windows)、Azure Key Vault、PaaS、SaaS アプリケーションなどの Microsoft Cloud リソース。

- Azure 上のアプリケーションや企業ネットワーク リソースなどの組織のリソース。

Azure AD を保護することは、組織のクラウド セキュリティ プラクティスの中で高い優先順位を持つ必要があります。 Azure AD では、Microsoft のベスト プラクティスの推奨事項を基準にした、お客様の ID セキュリティ体制を評価するために役立つ ID セキュリティ スコアが提供されます。 スコアを使用して、構成がベスト プラクティスの推奨事項とどの程度一致しているかを測定し、セキュリティ体制を強化します。

Azure AD では、外部 ID がサポートされます。これにより、Microsoft アカウントを持たないユーザーが、外部 ID を使用してアプリケーションやリソースにサインインできるようになります。

- [Azure Active Directory のテナント](../active-directory/develop/single-and-multi-tenant-apps.md) 

- [Azure AD インスタンスを作成して構成する方法](../active-directory/fundamentals/active-directory-access-create-new-tenant.md) 

- [アプリケーションに外部 ID プロバイダーを使用する](../active-directory/external-identities/identity-providers.md) 

- [Azure Active Directory の ID セキュリティ スコアとは](../active-directory/fundamentals/identity-secure-score.md)

**Azure Security Center の監視**: 適用なし

**責任**: Customer

### <a name="im-3-use-azure-ad-single-sign-on-sso-for-application-access"></a>IM-3:アプリケーションのアクセスに Azure AD シングル サインオン (SSO) を使用する

**ガイダンス**:Azure Migrate では、Azure Active Directory を使用して、Azure リソース、クラウド アプリケーション、オンプレミスのアプリケーションに ID とアクセス管理を提供します。 これには、従業員などの企業 ID だけでなく、パートナー、ベンダー、サプライヤーなどの外部 ID も含まれます。 これにより、オンプレミスおよびクラウド内の組織のデータとリソースへのアクセスを管理し、セキュリティで保護するためのシングルサインオン (SSO) が可能になります。 すべてのユーザー、アプリケーション、デバイスを Azure AD に接続することで、シームレスで安全なアクセスを実現し、可視性と制御性を高めることができます。

- [Azure AD を使用したアプリケーションの SSO について理解する](../active-directory/manage-apps/what-is-single-sign-on.md)

**Azure Security Center の監視**: 適用なし

**責任**: Customer

### <a name="im-4-use-strong-authentication-controls-for-all-azure-active-directory-based-access"></a>IM-4:すべての Azure Active Directory ベースのアクセスに強力な認証制御を使用する

**ガイダンス**:Azure Migrate では、多要素認証による強力な認証制御と、強力なパスワードレス手法をサポートしている Azure Active Directory を使用します。

多要素認証 - Azure AD の多要素認証を有効にして、多要素認証の設定におけるいくつかのベスト プラクティスについて、Azure Security Center の ID とアクセス管理の推奨事項に従います。 多要素認証は、選択されたすべてのユーザーに対して、またはサインインの条件とリスク要因に基づいてユーザーごとのレベルで適用できます。

パスワードレス認証 - 3 つのパスワードレス認証オプションを使用できます。Windows Hello for Business、Microsoft Authenticator アプリ、スマート カードなどのオンプレミスの認証方法。

管理者と特権ユーザーについて、高いレベルの強力な認証方法が使用されていることを確認し、その後、適切な強力な認証ポリシーを他のユーザーにロールアウトします。

- [Azure で多要素認証を有効にする方法](../active-directory/authentication/howto-mfa-getstarted.md) 

- [Azure Active Directory のパスワードレス認証オプションの概要](../active-directory/authentication/concept-authentication-passwordless.md) 

- [Azure AD の規定のパスワード ポリシー](../active-directory/authentication/concept-sspr-policy.md#password-policies-that-only-apply-to-cloud-user-accounts) 

- [Azure AD のパスワード保護を使用して不適切なパスワードを排除する](../active-directory/authentication/concept-password-ban-bad.md)

**Azure Security Center の監視**: はい

**責任**: Customer

### <a name="im-5-monitor-and-alert-on-account-anomalies"></a>IM-5:アカウントの異常を監視してアラートを出す

**ガイダンス**:Azure Migrate は Azure Active Directory と統合されており、次のデータ ソースが利用できます。

- サインイン - サインイン レポートでは、マネージド アプリケーションの使用状況とユーザー サインイン アクティビティに関する情報が得られます。

- 監査ログ - Azure AD 内のさまざまな機能によって行われたすべての変更についてログによる追跡可能性を提供します。 監査ログの例として、ユーザー、アプリ、グループ、ロール、ポリシーの追加や削除など、Azure AD 内のあらゆるリソースに加えられた変更があります。

- リスクの高いサインイン - リスクの高いサインインは、ユーザー アカウントの正当な所有者ではない人物によって行われた可能性があるサインイン試行の指標です。

- リスクのフラグ付きユーザー - リスクの高いユーザーは、侵害された可能性があるユーザー アカウントの指標です。

これらのデータ ソースは、Azure Monitor、Azure Sentinel、またはサードパーティの SIEM システムと統合できます。

Azure Security Center で、認証試行の失敗回数が多すぎるなど、サブスクリプションにおける特定の不審なアクティビティや、非推奨のアカウントに対してアラートを生成することもできます。

セキュリティ ソリューションである Azure Advanced Threat Protection (ATP) では、Active Directory シグナルを使用することで、高度な脅威、セキュリティ侵害を受けた ID、および悪意のある内部関係者のアクションを特定、検出、および調査できます。

- [Azure Active Directory の監査アクティビティ レポート](../active-directory/reports-monitoring/concept-audit-logs.md) 

- [Azure AD の危険なサインインを表示する方法](../active-directory/identity-protection/overview-identity-protection.md) 

- [危険なアクティビティのフラグが設定された Azure AD ユーザーを識別する方法](../active-directory/identity-protection/overview-identity-protection.md) 

- [Azure Security Center でユーザーの ID およびアクセス アクティビティを監視する方法](../security-center/security-center-identity-access.md) 

- [Azure Security Center の脅威インテリジェンス保護モジュールでのアラート](../security-center/alerts-reference.md) 

- [Azure アクティビティ ログを Azure Monitor に統合する方法](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

**Azure Security Center の監視**: はい

**責任**: Customer

## <a name="privileged-access"></a>特権アクセス

*詳細については、[Azure セキュリティ ベンチマークの「特権アクセス](../security/benchmarks/security-controls-v2-privileged-access.md)」を参照してください。*

### <a name="pa-2-restrict-administrative-access-to-business-critical-systems"></a>PA-2:ビジネス クリティカルなシステムへの管理アクセスを制限する

**ガイダンス**:Azure Migrate は、アカウントが属するサブスクリプションと管理グループへの特権アクセスを許可するアカウントを制限することによって、Azure RBAC を使用して、ビジネス クリティカルなシステムへのアクセスを分離します。

また、お使いのビジネス クリティカルなアクセス対象への管理アクセス権を持つ、管理システム、ID システム、セキュリティ システムへのアクセスも制限してください (Active Directory Domain Controller (DC)、セキュリティ ツール、システム管理ツール、ビジネス クリティカル システムにインストールされたエージェントなど)。 これらの管理システムおよびセキュリティ システムを侵害した攻撃者は、それらをすぐに悪用してビジネス クリティカルな資産を侵害することができます。

一貫したアクセス制御を確保するため、自分の企業のセグメント化戦略にすべての種類のアクセス制御を合わせる必要があります。

- [Azure のコンポーネントと参照モデル](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151) 

- [管理グループ アクセス](../governance/management-groups/overview.md#management-group-access) 

- [Azure サブスクリプション管理者](../cost-management-billing/manage/add-change-subscription-administrator.md)

**Azure Security Center の監視**: 適用なし

**責任**: Customer

### <a name="pa-3-review-and-reconcile-user-access-regularly"></a>PA-3:ユーザー アクセスを定期的に確認して調整する

**ガイダンス**:Azure Migrate では、Azure Active Directory アカウントを使用してリソースを管理し、ユーザー アカウントとアクセス割り当てを定期的にレビューして、アカウントとそのアクセスが有効であることを確認します。 Azure AD アクセス レビューを使用して、グループ メンバーシップ、エンタープライズ アプリケーションへのアクセス、およびロールの割り当てをレビューすることができます。 Azure AD レポートによって、古いアカウントの検出に役立つログが提供されます。 また、Azure AD Privileged Identity Management を使用してアクセス レビュー レポート ワークフローを作成し、レビュー プロセスを容易にすることもできます。

さらに、過剰な数の管理者アカウントが作成された場合にアラートを発行したり、古い管理者アカウントや不適切に構成されている管理者アカウントを特定するように Azure Privileged Identity Management を構成することもできます。

一部の Azure サービスでは、Azure AD で管理されていないローカル ユーザーとロールがサポートされています。 これらのユーザーは個別に管理する必要があります。

- [Privileged Identity Management (PIM) で Azure リソース ロールのアクセス レビューを作成する](../active-directory/privileged-identity-management/pim-resource-roles-start-access-review.md) 

- [Azure AD の ID およびアクセス レビューの使用方法](../active-directory/governance/access-reviews-overview.md)

**Azure Security Center の監視**: はい

**責任**: Customer

### <a name="pa-4-set-up-emergency-access-in-azure-ad"></a>PA-4:Azure AD で緊急アクセスを設定する

**ガイダンス**:Azure Migrate は、Azure Active Directory を使用してリソースを管理します。 Azure AD 組織から誤ってロックアウトされるのを防ぐために、通常の管理者アカウントを使用できない場合にアクセスするための緊急アクセス用アカウントを設定します。 緊急アクセス用アカウントは高い特権を持っており、特定の個人に割り当てることはできません。 緊急アクセス用アカウントは、通常の管理者アカウントを使うことができない "緊急事態" に制限されます。

緊急アクセス用アカウントの資格情報 (パスワード、証明書、スマート カードなど) は安全に保管し、緊急時にのみそれらを使うことを許可された個人のみに知らせる必要があります。

- [Azure AD で緊急アクセス用アカウントを管理する](../active-directory/roles/security-emergency-access.md)

**Azure Security Center の監視**: 適用なし

**責任**: Customer

### <a name="pa-5-automate-entitlement-management"></a>PA-5:エンタイトルメント管理を自動化する 

**ガイダンス**:Azure Migrate は、Azure Active Directory との統合により、そのリソースを管理します。 Azure AD のエンタイトルメント管理機能を使用して、アクセスの割り当て、レビュー、有効期限など、アクセス要求のワークフローを自動化します。 2 段階または複数段階の承認もサポートされています。

- [Azure AD アクセス レビューとは](../active-directory/governance/access-reviews-overview.md) 
- [Azure AD エンタイトルメント管理とは](../active-directory/governance/entitlement-management-overview.md)

**Azure Security Center の監視**: 適用なし

**責任**: Customer

### <a name="pa-6-use-privileged-access-workstations"></a>PA-6:特権アクセス ワークステーションを使用する

**ガイダンス**:セキュリティで保護された分離したワークステーションは、管理者、開発者、重要なサービス オペレーターのような機密性の高い役割のセキュリティには非常に重要です。 管理タスクに高度にセキュリティ保護されたユーザー ワークステーションや Azure Bastion を使用します。 Azure Active Directory、Microsoft Defender Advanced Threat Protection (ATP)、または Microsoft Intune を使用して、管理タスクのためにセキュリティで保護されたマネージド ユーザー ワークステーションを展開します。 セキュリティで保護されたワークステーションを一元管理して、強力な認証、ソフトウェアとハードウェアのベースライン、制限された論理アクセスとネットワーク アクセスなどのセキュリティで保護された構成を実施できます。

- [特権アクセス ワークステーションを理解する](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/) 

- [特権アクセス ワークステーションを展開する](/security/compass/privileged-access-deployment)

**Azure Security Center の監視**: 適用なし

**責任**: Customer

### <a name="pa-7-follow-just-enough-administration-least-privilege-principle"></a>PA-7:Just Enough Administration (最小限の特権の原則) に従う 

**ガイダンス**:Azure Migrate は、Azure のロールベースのアクセス制御 (RBAC) と統合により、そのリソースを管理します。 Azure RBAC を使用すると、ロールの割り当てによって Azure リソースへのアクセスを管理できます。 これらのロールを、ユーザー、グループ サービス プリンシパル、およびマネージド ID に割り当てることができます。 特定のリソースに対して定義済みの組み込みロールがあります。これらのロールは、Azure CLI、Azure PowerShell、Azure portal などのツールを使用してインベントリまたは照会できます。 Azure RBAC を使用してリソースに割り当てる特権は、常に、ロールで必要なものに制限する必要があります。 これは、Azure Active Directory Privileged Identity Management (PIM) のジャスト イン タイム (JIT) アプローチを補完するものであり、定期的に見直す必要があります。

組み込みのロールを使用してアクセス許可を割り当て、カスタム ロールは必要な場合にのみ作成します。

- [Azure ロールベースのアクセス制御 (Azure RBAC) とは](../role-based-access-control/overview.md)

- [Azure RBAC を構成する方法](../role-based-access-control/role-assignments-portal.md) 

- [Azure AD の ID およびアクセス レビューの使用方法](../active-directory/governance/access-reviews-overview.md)

**Azure Security Center の監視**: 適用なし

**責任**: Customer

## <a name="data-protection"></a>データ保護

*詳細については、[Azure セキュリティ ベンチマークの「データ保護](../security/benchmarks/security-controls-v2-data-protection.md)」を参照してください。*

### <a name="dp-2-protect-sensitive-data"></a>DP-2:機密データを保護する

**ガイダンス**:Azure Migrate の Server Migration ツールは、Azure サブスクリプション内の ストレージ アカウントとマネージド ディスクに移行される、サーバーのディスクからデータをレプリケートします。 データ処理は、サブスクリプション内のストレージ アカウントまたはマネージド ディスクに書き込まれるまでは一時的なものであり、Azure Migrate には保存されません。

Azure ロール ベースのアクセス制御 (Azure RBAC)、ネットワークベースのアクセス制御、(Managed Disks 用の Storage Service Encryption など) Azure サービスの特定の制御を使用してアクセスを制限して、ストレージ アカウントと管理ディスクに格納されている機密データを保護してください。 

Microsoft によって管理される基になるプラットフォームの場合、Microsoft では顧客のすべてのコンテンツを機密として扱い、顧客データを損失や漏洩から保護します。 Microsoft では、Azure 内の顧客データが確実にセキュリティで保護されるように、データ保護コントロールおよび機能をいくつか実装しています。

- [Azure のロールベースのアクセス制御 (RBAC)](../role-based-access-control/overview.md) 

- [Azure での顧客データの保護について](../security/fundamentals/protection-customer-data.md)

**Azure Security Center の監視**: 適用なし

**責任**: Customer

### <a name="dp-5-encrypt-sensitive-data-at-rest"></a>DP-5:保存データを暗号化する

**ガイダンス**:Azure Migrate は、機密データを保持しません。 Azure Migrate に保存されているすべてのデータは、Microsoft によって管理されるキーを使用して保存時に暗号化されます。

Azure Migrate の Server Migration ツールは、Azure サブスクリプション内の ストレージ アカウントとマネージド ディスクに移行される、サーバーのディスクからデータをレプリケートします。 ディスクには、機密データが含まれる場合と含まれない場合があります。 データ処理は、サブスクリプション内のストレージ アカウントまたはマネージド ディスクに書き込まれるまでは一時的なものであり、Azure Migrate には保存されません。 ストレージ アカウントとマネージド ディスク上のレプリケート済みデータは、Microsoft が管理するキーを使用して保存時に暗号化されます。 機密性の高いデータについては、ユーザーが管理するキーを使用して、ストレージ アカウントとマネージド ディスクで保存時に追加の暗号化を実施することもできます。

- [サーバー側暗号化 (SSE) とカスタマー マネージド キー (CMK) により VMware 仮想マシンを Azure に移行する](how-to-migrate-vmware-vms-with-cmk-disks.md)

- [ストレージ アカウントの暗号化をカスタマー マネージド キーにより構成する](../storage/common/customer-managed-keys-configure-key-vault.md)

**Azure Security Center の監視**: 適用なし

**責任**: 共有

## <a name="asset-management"></a>アセット管理

*詳細については、[Azure セキュリティ ベンチマークの「アセット管理](../security/benchmarks/security-controls-v2-asset-management.md)」を参照してください。*

### <a name="am-1-ensure-security-team-has-visibility-into-risks-for-assets"></a>AM-1:セキュリティ チームが資産のリスクを確実に可視化できるようにする

**ガイダンス**:セキュリティ チームに Azure テナントとサブスクリプションのセキュリティ閲覧者アクセス許可を付与して、セキュリティ チームが Azure Security Center を使用してセキュリティ上のリスクを監視できるようにします。 

セキュリティ チームの責任がどのように構造化されているかによって、セキュリティ リスクの監視は中央のセキュリティ チームまたはローカル チームの責任になります。 ただし、セキュリティ分析情報とリスクは、常に組織内で一元的に集計する必要があります。 

セキュリティ閲覧者のアクセス許可は、テナント全体 (ルート管理グループ) に幅広く適用することも、管理グループまたは特定のサブスクリプションにスコープ指定することもできます。 

ワークロードとサービスを可視化するには、追加のアクセス許可が必要になることがあります。 

- [セキュリティ閲覧者ロールの概要](../role-based-access-control/built-in-roles.md#security-reader)

- [Azure 管理グループの概要](../governance/management-groups/overview.md)

**Azure Security Center の監視**: 現在は使用できません

**責任**: Customer

### <a name="am-2-ensure-security-team-has-access-to-asset-inventory-and-metadata"></a>AM-2:セキュリティ チームが資産インベントリとメタデータに確実にアクセスできるようにする

**ガイダンス**:Azure リソース、リソース グループ、サブスクリプションにタグを適用して、それらを論理的に分類できます。 各タグは、名前と値のペアで構成されます。 たとえば、運用環境のすべてのリソースには名前 "環境" と値 "運用" を適用できます。

Azure Migrate のリソースでは、アプリケーションを実行することも、ソフトウェアをインストールすることもできません。 

- [Azure Resource Graph Explorer を使用してクエリを作成する方法](../governance/resource-graph/first-query-portal.md) 

- [Azure Security Center の資産インベントリ管理](../security-center/asset-inventory.md) 

- [リソースの名前付けとタグ付けの意思決定ガイド](/azure/cloud-adoption-framework/decision-guides/resource-tagging/?toc=%2fazure%2fazure-resource-manager%2fmanagement%2ftoc.json)

**Azure Security Center の監視**: 適用なし

**責任**: Customer

### <a name="am-3-use-only-approved-azure-services"></a>AM-3:承認された Azure サービスのみを使用する

**ガイダンス**:Azure Policy を使用して、環境内でユーザーがプロビジョニングできるサービスを監査および制限します。 Azure Resource Graph を使用して、サブスクリプション内のリソースのクエリまたは検出を行います。 また、Azure Monitor を使用して、承認されていないサービスが検出されたときにアラートをトリガーするルールを作成することもできます。

- [Azure Policy を構成して管理する方法](../governance/policy/tutorials/create-and-manage.md) 

- [Azure Policy を使用して特定のリソースの種類を拒否する方法](../governance/policy/samples/built-in-policies.md#general) 

- [Azure Resource Graph Explorer を使用してクエリを作成する方法](../governance/resource-graph/first-query-portal.md)

**Azure Security Center の監視**: 適用なし

**責任**: Customer

### <a name="am-5-limit-users-ability-to-interact-with-azure-resource-manager"></a>AM-5:Azure Resource Manager を操作するユーザーの機能を制限する

**ガイダンス**:"Microsoft Azure Management" アプリに対して [アクセスのブロック] を構成することによって、Azure Resource Manager を操作するユーザーの機能を制限するには、Azure 条件付きアクセスを使用します。

構成方法
- [Azure Resource Manager へのアクセスをブロックするための条件付きアクセス](../role-based-access-control/conditional-access-azure-management.md)

**Azure Security Center の監視**: 適用なし

**責任**: Customer

## <a name="logging-and-threat-detection"></a>ログと脅威検出

*詳細については、[Azure セキュリティ ベンチマークの「ログと脅威検出](../security/benchmarks/security-controls-v2-logging-threat-detection.md)」を参照してください。*

### <a name="lt-2-enable-threat-detection-for-azure-identity-and-access-management"></a>LT-2:Azure ID とアクセスの管理のために脅威検出を有効にする

**ガイダンス**:Azure AD では、次のユーザー ログが記録され、これは Azure AD レポートで確認できます。また、より高度な監視と分析のユース ケースの場合は、Azure Monitor、Azure Sentinel、またはその他の SIEM/監視ツールと統合できます。

- サインイン - サインイン レポートは、マネージド アプリケーションの使用状況とユーザー サインイン アクティビティに関する情報を提供します。

監査ログ - Azure AD 内のさまざまな機能によって行われたすべての変更についてログによる追跡可能性を提供します。 監査ログの例として、ユーザー、アプリ、グループ、ロール、ポリシーの追加や削除など、Azure AD 内のあらゆるリソースに加えられた変更があります。

- リスクの高いサインイン - リスクの高いサインインは、ユーザー アカウントの正当な所有者ではない人によって行われた可能性があるサインイン試行の指標です。

- リスクのフラグ付きユーザー - リスクの高いユーザーは、侵害された可能性があるユーザー アカウントの指標です。

Azure Security Center で、認証試行の失敗回数が多すぎるなど、サブスクリプションにおける特定の不審なアクティビティや、非推奨のアカウントに対してアラートを生成することもできます。 Azure Security Center の脅威保護モジュールでは、基本的なセキュリティ検疫監視に加えて、個々の Azure コンピューティング リソース (仮想マシン、コンテナー、アプリ サービス)、データ リソース (SQL DB、ストレージ)、Azure サービス レイヤーから、さらに詳細なセキュリティ アラートを収集することもできます。 この機能を使用することにより、個々のリソース内でアカウントの異常を確認できます。

- [Azure Active Directory の監査アクティビティ レポート](../active-directory/reports-monitoring/concept-audit-logs.md) 

- [Azure Identity Protection を有効にする](../active-directory/identity-protection/overview-identity-protection.md) 

- [Azure Security Center での脅威の防止](../security-center/azure-defender.md)

**Azure Security Center の監視**: はい

**責任**: Customer

### <a name="lt-4-enable-logging-for-azure-resources"></a>LT-4:Azure リソースのログ記録を有効にする

**ガイダンス**:現在、Azure Migrate では Azure リソース ログは生成されません。

自動的に利用できるアクティビティ ログには、読み取り操作 (GET) を除く、Azure Migrate リソースに対するすべての書き込み操作 (PUT、POST、DELETE) が含まれています。 アクティビティ ログを使用すると、トラブルシューティング時にエラーを見つけたり、組織内のユーザーがリソースをどのように変更したかを監視したりできます。

- [Azure Monitor でプラットフォーム ログとメトリックを収集する方法](../azure-monitor/essentials/diagnostic-settings.md) 

- [Azure でのログ記録とログのさまざまな種類について](../azure-monitor/essentials/platform-logs-overview.md)

**Azure Security Center の監視**: 適用なし

**責任**: Customer

### <a name="lt-5-centralize-security-log-management-and-analysis"></a>LT-5:セキュリティ ログの管理と分析を一元化する

**ガイダンス**:ログ記録のストレージと分析を一元化して、相関関係を有効にします。 ログ ソースごとに、データ所有者、アクセス ガイダンス、ストレージの場所、データの処理とアクセスに使用するツール、データ保持の要件を割り当てたことを確認します。

Azure アクティビティ ログを一元的なログ記録に統合していることを確認します。 Azure Monitor を介してログを取り込み、エンドポイント デバイス、ネットワーク リソース、およびその他のセキュリティ システムによって生成されたセキュリティ データを集計します。 Azure Monitor で Log Analytics ワークスペースを使用してクエリを発行し、分析を実行して、長期のアーカイブ ストレージには Azure Storage アカウントを使用します。

さらに、Azure Sentinel またはサード パーティの SIEM に対してデータを有効にしてオンボードします。

多くの組織では、頻繁に使用される "ホット" データに対しては Azure Sentinel を、使用頻度の低い "コールド" データに対しては Azure Storage を使用することを選択しています。

- [Azure Monitor でプラットフォーム ログとメトリックを収集する方法](../azure-monitor/essentials/diagnostic-settings.md) 

- [Azure Sentinel をオンボードする方法](../sentinel/quickstart-onboard.md)

**Azure Security Center の監視**: はい

**責任**: Customer

## <a name="incident-response"></a>インシデント対応

*詳細については、[Azure セキュリティ ベンチマークの「インシデント対応](../security/benchmarks/security-controls-v2-incident-response.md)」を参照してください。*

### <a name="ir-1-preparation--update-incident-response-process-for-azure"></a>IR-1: 準備 – インシデント対応プロセスを Azure 用に更新する

**ガイダンス**:組織において、セキュリティ インシデントに対応するためのプロセスが用意されていること、Azure のこれらのプロセスが更新されていること、それらのプロセスを定期的に使用して準備されていることを確認します。

- [企業環境全体にセキュリティを実装する](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

- [インシデント対応のリファレンス ガイド](/microsoft-365/downloads/IR-Reference-Guide.pdf)

**Azure Security Center の監視**: 適用なし

**責任**: Customer

### <a name="ir-2-preparation--setup-incident-notification"></a>IR-2: 準備 – インシデント通知をセットアップする

**ガイダンス**:Azure Security Center でセキュリティ インシデントの連絡先情報を設定します。 この連絡先情報は、Microsoft Security Response Center (MSRC) でユーザーのデータが違法または権限のないユーザーによってアクセスされたことが検出された場合に、Microsoft からの連絡先として使用されます。 また、インシデント対応のニーズに応じて、異なる Azure サービスでインシデント アラートと通知をカスタマイズするオプションもあります。 

- [Azure Security Center のセキュリティ連絡先を設定する方法](../security-center/security-center-provide-security-contact-details.md)

**Azure Security Center の監視**: はい

**責任**: Customer

### <a name="ir-3-detection-and-analysis--create-incidents-based-on-high-quality-alerts"></a>IR-3: 検出と分析 – 高品質のアラートに基づいてインシデントを作成する

**ガイダンス**:高品質のアラートを作成し、アラートの品質を測定するプロセスがあることを確認します。 これにより、過去のインシデントから教訓を学び、アナリストに対するアラートに優先順位を付けることができるので、擬陽性に時間を無駄にすることがありません。 

高品質のアラートは、過去のインシデントからの経験、検証されたコミュニティ ソース、およびさまざまなシグナル ソースの融合と関連付けによってアラートを生成してクリーンアップするように設計されたツールに基づいて構築できます。 

Azure Security Center では、多数の Azure 資産について高品質のアラートが提供されます。 ASC データ コネクタを使用してアラートを Azure Sentinel にストリーミングできます。 Azure Sentinel を使用すると、高度なアラート ルールを作成し、調査のためにインシデントを自動的に生成できます。 

エクスポート機能を使用して Azure Security Center のアラートと推奨事項をエクスポートし、Azure リソースへのリスクを特定します。 アラートと推奨事項を手動で、または継続した連続的な方法でエクスポートします。

- [エクスポートを構成する方法](../security-center/continuous-export.md)

- [Azure Sentinel にアラートをストリーミングする方法](../sentinel/connect-azure-security-center.md)

**Azure Security Center の監視**: 現在は使用できません

**責任**: Customer

### <a name="ir-4-detection-and-analysis--investigate-an-incident"></a>IR-4: 検出と分析 – インシデントを調査する

**ガイダンス**:アナリストが潜在的なインシデントを調査するときに、さまざまなデータ ソースを照会して使用し、発生した内容の完全なビューを構築できることを確認します。 キル チェーン全体で潜在的な攻撃者のアクティビティを追跡して死角を回避するには、さまざまなログを収集する必要があります。  また、他のアナリストのため、および将来において履歴が参照される場合のために、確実に分析情報と学習がキャプチャされているようにします。  

調査のためのデータ ソースには、スコープ内のサービスおよび実行中のシステムから既に収集されている一元化されたログ ソースが含まれますが、次のものも含まれます。

- ネットワーク データ - ネットワーク セキュリティ グループのフロー ログ、Azure Network Watcher、Azure Monitor を使用して、ネットワーク フロー ログやその他の分析情報をキャプチャします。 

- 実行中のシステムのスナップショット: 

    - Azure 仮想マシンのスナップショット機能を使用して、実行中のシステムのディスクのスナップショットを作成します。 

    - オペレーティング システムのネイティブ メモリ ダンプ機能を使用して、実行中のシステムのメモリのスナップショットを作成します。

    - Azure サービスのスナップショット機能またはソフトウェア独自の機能を使用して、実行中のシステムのスナップショットを作成します。

Azure Sentinel により、事実上すべてのログソースに対して広範な Data Analytics と、インシデントのライフサイクル全体を管理するためのケース管理ポータルが提供されます。 調査中のインテリジェンス情報を、追跡とレポートのためにインシデントに関連付けることができます。 

- [Windows マシンのディスクのスナップショットを作成する](../virtual-machines/windows/snapshot-copy-managed-disk.md)

- [Linux マシンのディスクのスナップショットを作成する](../virtual-machines/linux/snapshot-copy-managed-disk.md)

- [Microsoft Azure サポートの診断情報とメモリ ダンプ コレクション](https://azure.microsoft.com/support/legal/support-diagnostic-information-collection/) 

- [Azure Sentinel でインシデントを調査します](../sentinel/tutorial-investigate-cases.md)

**Azure Security Center の監視**: 適用なし

**責任**: Customer

### <a name="ir-5-detection-and-analysis--prioritize-incidents"></a>IR-5: 検出と分析 – インシデントの優先順位を付ける

**ガイダンス**:アラートの重要度と資産の機密性に基づいて、最初に注目するインシデントについてのコンテキストをアナリストに提供します。 

Azure Security Center によって各アラートに重大度が割り当てられるため、最初に調査する必要があるアラートの優先順位付けに役立ちます。 重要度は、アラートの発行に使用された Security Center の信頼度と、アラートの原因となったアクティビティの背後に悪意のある意図があったかどうかの信頼レベルに基づいて決まります。

さらに、タグを使用してリソースをマークし、Azure リソース (特に、機密データを処理するもの) を識別して分類するための命名システムを作成します。  インシデントが発生した Azure リソースと環境の重要度に基づいて、アラートの修復に優先順位を付けることは、お客様の責任です。

- [Security alerts in Azure Security Center](../security-center/security-center-alerts-overview.md)

- [タグを使用した Azure リソースの整理](../azure-resource-manager/management/tag-resources.md)

**Azure Security Center の監視**: 現在は使用できません

**責任**: Customer

### <a name="ir-6-containment-eradication-and-recovery--automate-the-incident-handling"></a>IR-6: 包含、根絶、復旧 – インシデントの処理を自動化する

**ガイダンス**:手動による反復タスクを自動化して、応答時間を短縮し、アナリストの負担を軽減します。 手動タスクの実行には時間がかかり、各インシデントの速度が低下し、アナリストが処理できるインシデントの数が減少します。 手動タスクではアナリストの疲労も増加します。これにより、遅延が発生する人的エラーのリスクが増加し、複雑なタスクに効果的に焦点を当てるアナリストの能力が低下します。 Azure Security Center と Azure Sentinel のワークフロー自動化機能を使用して、自動的にアクションをトリガーしたり、プレイブックを実行して受信したセキュリティ アラートに応答したりします。 プレイブックにより、通知の送信、アカウントの無効化、問題のあるネットワークの特定などのアクションが実行されます。 

- [Security Center でワークフロー自動化を構成する](../security-center/workflow-automation.md)

- [Azure Security Center で脅威への自動対応を設定する](../security-center/tutorial-security-incident.md#triage-security-alerts)

- [Azure Sentinel で脅威への自動対応を設定します](../sentinel/tutorial-respond-threats-playbook.md)

**Azure Security Center の監視**: 現在は使用できません

**責任**: Customer

## <a name="posture-and-vulnerability-management"></a>体制と脆弱性の管理

*詳細については、[Azure セキュリティ ベンチマークの「体制と脆弱性の管理](../security/benchmarks/security-controls-v2-posture-vulnerability-management.md)」を参照してください。*

### <a name="pv-8-conduct-regular-attack-simulation"></a>PV-8: 定期的に攻撃シミュレーションを実施する

**ガイダンス**:必要に応じて、Azure リソースの侵入テストまたはレッド チーム アクティビティを実施し、セキュリティに関するすべての重大な調査結果が確実に修復されるようにします。
お客様の侵入テストが Microsoft のポリシーに違反しないように、Microsoft クラウド侵入テストの実施ルールに従ってください。 Microsoft が管理しているクラウド インフラストラクチャ、サービス、アプリケーションに対する Red Teaming およびライブ サイト侵入テストに関する Microsoft の戦略と実施を活用してください。

- [Azure での侵入テスト](../security/fundamentals/pen-testing.md)

- [侵入テストの実施ルール](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Microsoft Cloud Red Teaming](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Security Center の監視**: 適用なし

**責任**: 共有

## <a name="governance-and-strategy"></a>ガバナンスと戦略

*詳細については、[Azure セキュリティ ベンチマークの「ガバナンスと戦略](../security/benchmarks/security-controls-v2-governance-strategy.md)」を参照してください。*

### <a name="gs-1-define-asset-management-and-data-protection-strategy"></a>GS-1: 資産の管理とデータ保護の戦略を定義する 

**ガイダンス**:システムとデータを継続的に監視および保護するための明確な戦略が文書化および伝達されるようにします。 ビジネスに不可欠なデータとシステムの検出、評価、保護、監視の優先順位を決定します。 

この戦略には、次の要素に関する文書化されたガイダンス、ポリシー、標準が含まれている必要があります。 

-   ビジネス リスクに応じたデータ分類標準

-   リスクと資産のインベントリに対するセキュリティ組織の可視性 

-   Azure サービスを使用するためのセキュリティ組織の承認 

-   ライフサイクルを通じた資産のセキュリティ

-   組織のデータ分類に従った必要なアクセス制御戦略

-   Azure ネイティブおよびサードパーティのデータ保護機能の使用

-   転送中および保存中のユース ケースのデータ暗号化要件

-   適切な暗号化標準

詳細については、次のリファレンスを参照してください。
- [Azure セキュリティ アーキテクチャに関する推奨事項 - ストレージ、データ、暗号化](/azure/architecture/framework/security/storage-data-encryption?bc=%2fsecurity%2fcompass%2fbreadcrumb%2ftoc.json&toc=%2fsecurity%2fcompass%2ftoc.json)

- [Azure のセキュリティの基礎 - Azure のデータ セキュリティ、暗号化、ストレージ](../security/fundamentals/encryption-overview.md)

- [クラウド導入フレームワーク - Azure のデータ セキュリティと暗号化のベスト プラクティス](../security/fundamentals/data-encryption-best-practices.md?bc=%2fazure%2fcloud-adoption-framework%2f_bread%2ftoc.json&toc=%2fazure%2fcloud-adoption-framework%2ftoc.json)

- [Azure セキュリティ ベンチマーク - アセット管理](../security/benchmarks/security-controls-v2-asset-management.md)

- [Azure セキュリティ ベンチマーク - データ保護](../security/benchmarks/security-controls-v2-data-protection.md)

**Azure Security Center の監視**: 適用なし

**責任**: Customer

### <a name="gs-2-define-enterprise-segmentation-strategy"></a>GS-2: 企業のセグメント化戦略を定義する 

**ガイダンス**:ID、ネットワーク、アプリケーション、サブスクリプション、管理グループ、その他のコントロールを利用し、アセットへのアクセスをセグメント化する企業規模の戦略を確立します。

セキュリティ分離の必要性と、互いと通信し、データにアクセスする必要があるシステムの日常的操作を有効にするという必要性のバランスを慎重に取ります。

セグメント化戦略は、ネットワーク セキュリティ、ID とアクセス モデル、アプリケーション アクセス許可とアクセス モデル、人的プロセスの制御など、あらゆるコントロールの種類を対象として確実に一貫性をもって実装します。

- [Azure のセグメント化戦略に関するガイド (動画)](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)

- [Azure のセグメント化戦略に関するガイド (ドキュメント)](/security/compass/governance#enterprise-segmentation-strategy)

- [ネットワークのセグメント化を企業のセグメント化戦略に合わせる](/security/compass/network-security-containment#align-network-segmentation-with-enterprise-segmentation-strategy)

**Azure Security Center の監視**: 適用なし

**責任**: Customer

### <a name="gs-3-define-security-posture-management-strategy"></a>GS-3: セキュリティ態勢管理の戦略を定義する

**ガイダンス**:個々の資産とそれらがホストされている環境に対するリスクを継続的に測定し、軽減します。 高い価値を持つ資産と、攻撃に晒される可能性の高い部分 (公開されたアプリケーション、ネットワークのイングレス ポイントとエグレス ポイント、ユーザーと管理者のエンドポイントなど) を優先します。

- [Azure セキュリティ ベンチマーク - 体制と脆弱性の管理](../security/benchmarks/security-controls-v2-posture-vulnerability-management.md)

**Azure Security Center の監視**: 適用なし

**責任**: Customer

### <a name="gs-4-align-organization-roles-responsibilities-and-accountabilities"></a>GS-4: 組織の役割、責任、責務を整合させる

**ガイダンス**:セキュリティ組織における役割と責任に関する明確な戦略が文書化されて伝えられるようにします。 セキュリティに関する決定についてわかりやすく説明すること、共有される責任モデルについて全員に教育すること、クラウドをセキュリティで保護するテクノロジについて技術チームに教育することを優先とします。

- [Azure のセキュリティのベスト プラクティス 1 – 人: クラウド セキュリティに関する取り組みについてチームを教育する](/azure/cloud-adoption-framework/security/security-top-10#1-people-educate-teams-about-the-cloud-security-journey)

- [Azure のセキュリティのベスト プラクティス 2 - 人: クラウド セキュリティ テクノロジについてチームを教育する](/azure/cloud-adoption-framework/security/security-top-10#2-people-educate-teams-on-cloud-security-technology)

- [Azure のセキュリティのベスト プラクティス 3 - プロセス: クラウドのセキュリティに関する意思決定のアカウンタビリティを割り当てる](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

**Azure Security Center の監視**: 適用なし

**責任**: Customer

### <a name="gs-5-define-network-security-strategy"></a>GS-5: ネットワーク セキュリティ戦略を定義する

**ガイダンス**:組織全体のセキュリティ アクセス制御戦略の一環として、Azure ネットワーク セキュリティ アプローチを確立します。  

この戦略には、次の要素に関する文書化されたガイダンス、ポリシー、標準が含まれている必要があります。 

-   一元的なネットワーク管理とセキュリティ責任

-   エンタープライズ セグメント化戦略と一致する仮想ネットワーク セグメント化モデル

-   さまざまな脅威と攻撃のシナリオにおける修復戦略

-   インターネット エッジとイングレスおよびエグレス戦略

-   クラウドとオンプレミスのハイブリッド相互依存戦略

-   最新のネットワーク セキュリティ成果物 (例: ネットワーク図、参照ネットワーク アーキテクチャ)

詳細については、次のリファレンスを参照してください。
- [Azure のセキュリティのベスト プラクティス 11 - アーキテクチャ。単一の統合セキュリティ戦略](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)

- [Azure セキュリティ ベンチマーク - ネットワーク セキュリティ](../security/benchmarks/security-controls-v2-network-security.md)

- [Azure のネットワーク セキュリティの概要](../security/fundamentals/network-overview.md)

- [エンタープライズ ネットワーク アーキテクチャ戦略](/azure/cloud-adoption-framework/ready/enterprise-scale/architecture)

**Azure Security Center の監視**: 適用なし

**責任**: Customer

### <a name="gs-6-define-identity-and-privileged-access-strategy"></a>GS-6: ID と特権アクセス戦略を定義する

**ガイダンス**:組織全体のセキュリティ アクセス制御戦略の一環として、Azure の ID と特権アクセス アプローチを確立します。  

この戦略には、次の要素に関する文書化されたガイダンス、ポリシー、標準が含まれている必要があります。 

-   一元化された ID と認証システム、および他の内部および外部 ID システムとのその相互接続

-   さまざまなユース ケースおよび条件における強力な認証方法

-   高い特権を持つユーザーの保護

-   異常なユーザー アクティビティの監視と処理  

-   ユーザー ID とアクセスの確認と調整のプロセス

詳細については、次のリファレンスを参照してください。

- [Azure セキュリティ ベンチマーク - ID 管理](../security/benchmarks/security-controls-v2-identity-management.md)

- [Azure セキュリティ ベンチマーク - 特権アクセス](../security/benchmarks/security-controls-v2-privileged-access.md)

- [Azure のセキュリティのベスト プラクティス 11 - アーキテクチャ。単一の統合セキュリティ戦略](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)

- [Azure ID 管理のセキュリティの概要](../security/fundamentals/identity-management-overview.md)

**Azure Security Center の監視**: 適用なし

**責任**: Customer

### <a name="gs-7-define-logging-and-threat-response-strategy"></a>GS-7: ログ記録と脅威対応戦略を定義する

**ガイダンス**:コンプライアンス要件を満たしながら脅威を迅速に検出して修復するための、ログ記録と脅威対応戦略を確立します。 アナリストが、統合や手動による手順ではなく、脅威の対応に集中できるように、質の高いアラートとシームレスなエクスペリエンスを提供することを優先してください。 

この戦略には、次の要素に関する文書化されたガイダンス、ポリシー、標準が含まれている必要があります。 

-   セキュリティ運用 (SecOps) 組織の役割と責任 

-   NIST または他の業界フレームワークと一致する、明確に定義されたインシデント対応プロセス 

-   脅威の検出、インシデント対応、コンプライアンスのニーズに対応するためのログのキャプチャと保持

-   SIEM、Azure のネイティブ機能、その他のソースを使用した、脅威に関する情報の一元的な可視化と関連付け 

-   顧客、サプライヤー、および関心を持つパブリック パーティに関するコミュニケーションと通知の計画

-   ログ記録と脅威の検出、フォレンジクス、攻撃の修復と根絶など、インシデント処理に対する Azure ネイティブおよびサードパーティのプラットフォームの使用

-   インシデントとインシデント発生後のアクティビティを処理するためのプロセス (教訓や証拠の保持など)

詳細については、次のリファレンスを参照してください。

- [Azure セキュリティ ベンチマーク - ログと脅威検出](../security/benchmarks/security-controls-v2-logging-threat-detection.md)

- [Azure セキュリティ ベンチマーク - インシデント対応](../security/benchmarks/security-controls-v2-incident-response.md)

- [Azure のセキュリティのベスト プラクティス 4 - プロセス: クラウドのインシデント対応プロセスを更新する](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

- [Azure 導入フレームワーク、ログ、およびレポートの決定ガイド](/azure/cloud-adoption-framework/decision-guides/logging-and-reporting/)

- [Azure のエンタープライズ スケーリング、管理、監視](/azure/cloud-adoption-framework/ready/enterprise-scale/management-and-monitoring)

**Azure Security Center の監視**: 適用なし

**責任**: Customer

## <a name="next-steps"></a>次のステップ

- 「[Azure セキュリティ ベンチマーク V2 の概要](../security/benchmarks/overview.md)」を参照してください。
- [Azure セキュリティ ベースライン](../security/benchmarks/security-baselines-overview.md)の詳細について学習する
