---
title: エグレス トラフィックに静的 IP を使用する
titleSuffix: Azure Kubernetes Service
description: Azure Kubernetes Service (AKS) クラスターでエグレス トラフィックの静的パブリック IP アドレスを作成して使用する方法を説明します
services: container-service
ms.topic: article
ms.date: 03/04/2019
ms.openlocfilehash: 2eefeecfa550683dafcf66d936837e2a891c4c84
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/03/2021
ms.locfileid: "101726548"
---
# <a name="use-a-static-public-ip-address-for-egress-traffic-with-a-basic-sku-load-balancer-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) で *Basic* SKU ロード バランサーと共に、エグレス トラフィックに静的パブリック IP アドレスを使用する

既定では、Azure Kubernetes Service (AKS) クラスターからのエグレス アドレスはランダムに割り当てられます。 この構成は、外部サービスにアクセスするための IP アドレスを識別する必要がある場合には適していません。 代わりに、サービス アクセスの許可リストに追加される静的 IP アドレスを割り当てる必要がある場合があります。

この記事では、AKS クラスターでエグレス トラフィックに使用する静的パブリック IP アドレスを作成して使用する方法を示します。

## <a name="before-you-begin"></a>開始する前に

この記事では、Azure Basic Load Balancer を使用していることを前提としています。  [Azure Standard Load Balancer](../load-balancer/load-balancer-overview.md) の使用をお勧めします。より高度な機能を使用して、[AKS エグレス トラフィックを制御](./limit-egress-traffic.md)できます。

この記事は、AKS クラスターがすでに存在していることを前提としています。 AKS クラスターが必要な場合は、[Azure CLI を使用した場合][aks-quickstart-cli]または [Azure portal を使用した場合][aks-quickstart-portal]の AKS のクイックスタートを参照してください。

また、Azure CLI バージョン 2.0.59 以降がインストールされ、構成されている必要もあります。 バージョンを確認するには、`az --version` を実行します。 インストールまたはアップグレードする必要がある場合は、[Azure CLI のインストール][install-azure-cli]に関するページを参照してください。

> [!IMPORTANT]
> この記事では、*Basic* SKU のロード バランサーと単一のノード プールを使用します。 複数のノード プールでは *Basic* SKU のロード バランサーがサポートされないため、この構成は利用できません。 *Standard* SKU のロード バランサーの使用について詳しくは、「[Azure Kubernetes Service (AKS) でパブリック Standard Load Balancer を使用する][slb]」を参照してください。

## <a name="egress-traffic-overview"></a>エグレス トラフィックの概要

AKS クラスターからの送信トラフィックは [Azure Load Balancer の規則][outbound-connections]に従っています。 `LoadBalancer` 型の最初の Kubernetes サービスが作成される前、AKS クラスターのエージェント ノードはどれも Azure Load Balancer プールのメンバーではありません。 この構成では、どのノードもインスタンス レベルのパブリック IP アドレスが持たないことになります｡ Azure では、送信フローを構成可能でも決定論的でもないパブリック ソース IP アドレスに変換します。

`LoadBalancer` 型の Kubernetes サービスが作成されると、エージェント ノードは Azure Load Balancer プールに追加されます。 複数の (パブリック) IP フロントエンドが送信フローの候補である場合、送信フローに使用される 1 つのフロントエンドが Load Balancer Basic によって選択されます。 この選択は構成することができません。選択アルゴリズムはランダムであると考える必要があります。 このパブリック IP アドレスで、そのリソースの有効期間の間のみ有効です。 Kubernetes LoadBalancer サービスを削除すると、関連付けられているロード バランサーと IP アドレスも削除されます。 デプロイし直された Kubernetes サービスに対して特定の IP アドレスを割り当てるか､あるいは IP アドレスを保持する場合は、静的パブリック IP アドレスを作成して､使用することができます。

## <a name="create-a-static-public-ip"></a>静的パブリック IP を作成する

[az aks show][az-aks-show] コマンドを使用してリソース グループ名を取得し、`--query nodeResourceGroup` クエリ パラメーターを追加します。 次の例では、リソース グループ名 *myResourceGroup* にある AKS クラスター名のノード リソース グループ *myAKSCluster* を取得しています｡

```azurecli-interactive
$ az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

次に、[az network public-ip create][az-network-public-ip-create] コマンドを使用して、静的パブリック IP アドレスを作成します。 上記コマンドで取得したノードのリソース グループ名を指定して､その IP アドレス リソースに対して､*myAKSPublicIP* などの名前を指定します｡

```azurecli-interactive
az network public-ip create \
    --resource-group MC_myResourceGroup_myAKSCluster_eastus \
    --name myAKSPublicIP \
    --allocation-method static
```

次の出力例 (一部) に見られるように IP アドレスが表示されます｡

```json
{
  "publicIp": {
    "dnsSettings": null,
    "etag": "W/\"6b6fb15c-5281-4f64-b332-8f68f46e1358\"",
    "id": "/subscriptions/<SubscriptionID>/resourceGroups/MC_myResourceGroup_myAKSCluster_eastus/providers/Microsoft.Network/publicIPAddresses/myAKSPublicIP",
    "idleTimeoutInMinutes": 4,
    "ipAddress": "40.121.183.52",
    [..]
  }
```

このパブリック IP アドレスは､後で [az network public ip list][az-network-public-ip-list] コマンドを使用して取得することができます。 次の例に示すように､ノードのリソース グループ名を指定して、*ipAddress* に対するクエリを指定します｡

```azurecli-interactive
$ az network public-ip list --resource-group MC_myResourceGroup_myAKSCluster_eastus --query [0].ipAddress --output tsv

40.121.183.52
```

## <a name="create-a-service-with-the-static-ip"></a>静的 IP を使用してサービスを作成する

静的パブリック IP アドレスを使用してサービスに作成するには､YAML マニフェストに `loadBalancerIP` プロパティと静的パブリック IP の値を追加します｡ `egress-service.yaml` という名前のファイルを作成し、そこに以下の YAML をコピーします。 以前の手順で作成した独自のパブリック IP アドレスを指定します。

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-egress
spec:
  loadBalancerIP: 40.121.183.52
  type: LoadBalancer
  ports:
  - port: 80
```

`kubectl apply` コマンドを使用して、サービスとデプロイを作成します。

```console
kubectl apply -f egress-service.yaml
```

このサービスを作成すると、Azure Load Balancer に新しいフロントエンド IP が構成されます。 他の IP が構成されていない場合は、**すべての** エグレス トラフィックがこのアドレスを使用するようになります。 複数のアドレスが Azure Load Balancer 上で構成されている場合、それらのパブリック IP アドレスはどれもアウトバウンド フローの候補になり、その 1 つがランダムに選択されます。

## <a name="verify-egress-address"></a>エグレス アドレスを確認する

静的パブリック IP アドレスが使用されていることの確認には、`checkip.dyndns.org` などの DNS 検索サービスを利用できます｡

基本的な *Debian* pod を起動してアタッチします｡

```console
kubectl run -it --rm aks-ip --image=debian
```

コンテナー内から web サイトにアクセスするには、`apt-get` を使用して､コンテナーに `curl` をインストールします｡

```console
apt-get update && apt-get install curl -y
```

これで､Curl を使用して *checkip.dyndns.org* サイトにアクセスできます｡ 次の出力例に示すように､エグレス IP アドレスが表示されます。 この IP アドレスは、loadBalancer サービスに対して作成､定義される静的パブリック IP アドレスと同じです｡

```console
$ curl -s checkip.dyndns.org

<html><head><title>Current IP Check</title></head><body>Current IP Address: 40.121.183.52</body></html>
```

## <a name="next-steps"></a>次のステップ

Azure Load Balancer に複数のパブリック IP アドレスを保持しないようしたい場合は、イングレス コントローラーの使用を検討してください。 イングレス コントローラーを使用すると、負荷分散、SSL/TLS の終了、URI 書き換えサポート、アップストリーム SSL/TLS 暗号化などの利点があります。 詳細は、[AKS での基本的なイングレス コント ローラーの作成][ingress-aks-cluster]を参照してください。

<!-- LINKS - internal -->
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[az-network-public-ip-list]: /cli/azure/network/public-ip#az-network-public-ip-list
[az-aks-show]: /cli/azure/aks#az-aks-show
[azure-cli-install]: /cli/azure/install-azure-cli
[ingress-aks-cluster]: ./ingress-basic.md
[outbound-connections]: ../load-balancer/load-balancer-outbound-connections.md#scenarios
[public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[slb]: load-balancer-standard.md
