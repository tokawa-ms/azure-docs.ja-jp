---
title: Defender for IoT マイクロ エージェントのトラブルシューティング
titleSuffix: Azure Defender for IoT
description: 予期しないまたは原因不明のエラーを処理する方法について説明します。
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 1/24/2021
ms.topic: reference
ms.service: azure
ms.openlocfilehash: dade0d0d5dc4d690ea94f20deaf956b1e079bad7
ms.sourcegitcommit: dac05f662ac353c1c7c5294399fca2a99b4f89c8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/04/2021
ms.locfileid: "102124178"
---
# <a name="defender-iot-micro-agent-troubleshooting"></a>Defender for IoT マイクロ エージェントのトラブルシューティング 

予期しないまたは原因不明のエラーが発生した場合は、次のトラブルシューティング方法を使用して、問題の解決を試みてください。 必要に応じて、Azure Defender for IoT 製品チームにもお問い合わせいただけます。   

## <a name="service-status"></a>サービスの状態 

サービスの状態を表示するには、次のようにします。 

1. 次のコマンドを実行します。

    ```azurecli
    systemctl status defender-iot-micro-agent.service 
    ```

1. サービスが安定していることを確認するには、`active`であること、およびプロセスの稼働時間が適切であることを確認します。

    :::image type="content" source="media/troubleshooting/active-running.png" alt-text="アクティブであることと、稼働時間が適切であることを確認して、サービスが安定していることを確認します。":::

サービスが `inactive` として表示されている場合は、次のコマンドを使用してサービスを開始します。

```azurecli
systemctl start defender-iot-micro-agent.service 
```

プロセスの稼働時間が短すぎる場合、サービスがクラッシュしていることがわかります。 この問題を解決するには、ログを確認する必要があります。

## <a name="review-logs"></a>ログを確認する 

次のコマンドを使用して、Defender IoT マイクロ エージェント サービスが root 権限で実行されていることを確認します。

```azurecli
ps -aux | grep " defender-iot-micro-agent"
```

:::image type="content" source="media/troubleshooting/root-privileges.png" alt-text="Defender IoT マイクロ エージェント サービスが root 権限で実行されていることを確認します。":::

ログを表示するには、次のコマンドを使用します。  

```azurecli
sudo journalctl -u defender-iot-micro-agent | tail -n 200 
```

サービスを再起動するには、次のコマンドを使用します。 

```azurecli
sudo systemctl restart defender-iot-micro-agent  
```

## <a name="next-steps"></a>次のステップ

「[機能のサポートと廃止](edge-security-module-deprecation.md)」を確認してください。
