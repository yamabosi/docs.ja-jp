---
title: WCF のイベント ログ
ms.date: 03/30/2017
helpviewer_keywords:
- event logging [WCF]
ms.assetid: aac0530d-f44c-45a1-bada-e30e0677b41f
ms.openlocfilehash: 0c74b93be026007a5593372c938cf73e08fafb15
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/07/2019
ms.locfileid: "70797799"
---
# <a name="event-logging-in-wcf"></a>WCF のイベント ログ
Windows Communication Foundation (WCF) は、Windows イベントログの内部イベントをトレースします。  
  
## <a name="viewing-event-logs"></a>イベント ログの表示  
 イベント ログは、既定で自動的に有効になります。無効にする方法はありません。 WCF によってログに記録されたイベントは、イベントビューアーを使用して表示できます。 このツールを起動するには、 **[スタート]** をクリックし、 **[コントロールパネル]** をクリックします。次に、 **[管理ツール]** をダブルクリックし、 **[イベントビューアー]** をダブルクリックします。  
  
### <a name="application-event-log"></a>アプリケーション イベント ログ  
 **アプリケーションイベントログ**には、WCF によって生成されるイベントのほとんどが含まれます。 このエントリの多くは、アプリケーションに関して特定の機能を起動できなかったことを示しています。 たとえば、次のものがあります。  
  
- メッセージログ/トレース:WCF は、トレースとメッセージログが失敗したときにイベントをイベントログに書き込みます。 ただし、トレース エラーが発生するたびにイベントがトリガーされるわけではありません。 イベントログにトレースエラーが完全に記録されないようにするために、WCF では、このようなイベントに対して10分のブラックアウト期間を実装しています。 これは、WCF がトレースエラーをイベントログに書き込む場合、少なくとも10分間は再度実行されないことを意味します。  
  
- 共有リスナー:WCF TCP ポート共有サービスは、開始に失敗したときにイベントをログに記録します。  
  
- CardSpaceサービスの開始に失敗したときにイベントをログに記録します。  
  
- 起動エラーやクラッシュなど、重大なエラー イベント。  
  
- メッセージログが有効になっている:メッセージログが有効になったときにイベントをログに記録します。 これには機密情報やアプリケーション固有の情報がメッセージのヘッダーや本文に記録されている可能性があることを管理者に知らせる目的があります。  
  
- `enableLoggingKnownPII` ファイルの `machineSettings` 要素で、`machine.config` 属性が設定されている場合、イベントはログに記録されます。 この属性は、コンピューター上で実行しているアプリケーションが、既知の個人を特定できる情報 (PII) をログに記録できるかどうかを指定します。  
  
- 特定のアプリケーションで PII ログを有効にするために、`logKnownPii` ファイルまたは `app.config` ファイルの `web.config` 属性が `true` に設定され、`enableLoggingKnownPII` ファイルの `machineSettings` 要素で、`machine.config` 属性が `false` に設定されている場合、イベントはログに記録されます。 また、`logKnownPii` と `enableLoggingKnownPII` の両方が `true` に設定されている場合、イベントはログに記録されます。 これらの構成設定の詳細については、「[メッセージログの構成](../configuring-message-logging.md)」トピックの「セキュリティ」セクションを参照してください。  
  
### <a name="security-event-log"></a>セキュリティ イベント ログ  
 **セキュリティイベントログ**には、WCF によってログに記録されるセキュリティ監査イベントが含まれます。  
  
### <a name="system-event-log"></a>システム イベント ログ  
 WCF では、**システムイベントログ**には何も記録されません。  
  
### <a name="event-log-entries"></a>イベント ログ エントリ  
 イベントの**ソース**は、ログエントリを生成するアセンブリの名前です。  
  
 イベント ログ エントリの型によって、イベントの重大度がわかります。 各イベントは単一の型でなければなりません。アプリケーションがイベントをレポートする際には、そのイベントの型が示されます。 イベント ビューアーは、この型を使用して、ログのリスト ビューに表示するアイコンを決定します。 イベント ログ エントリで使用されるイベントの型については、「<xref:System.Diagnostics.EventLogEntryType>」を参照してください。  
  
 イベントビューアーでイベントを表示するときに [詳細情報] をクリックすると、イベントビューアーはインターネット経由で情報を送信する場合があります。 詳細については、イベント ビューアーのヘルプを参照してください。  
  
## <a name="see-also"></a>関連項目

- [管理と診断](../index.md)
- [イベント一覧](events-general-reference.md)
