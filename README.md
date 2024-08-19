# SentinelIncidentNotificationTeams
このレポジトリは Microsoft Sentinel のインシデントを Microsoft Teams に通知するサンプルテンプレートです。

# 画面イメージ
> Sentinel インシデントトリガー時に、ターゲットの Teams チャネルに情報を発信します

- 通知時
  - [Teams Adaptive Card Format](https://learn.microsoft.com/ja-jp/microsoftteams/platform/task-modules-and-cards/cards/cards-reference#adaptive-card) を用いて、インシデント内容をメッセージに、インシデントに含まれるコメント/エンティティ情報を返信として通知します。
![image](https://github.com/user-attachments/assets/1f1ea47a-cd3f-47b0-aa9f-6724943ae787)
- インシデントクローズ時
  - インシデントクローズ処理時を Sentinel オートメーションより起動し、Teams メッセージの更新情報として通知します
    - Defender XDR 側でクローズ処理を行った場合、仕様により Defender XDR ログインユーザー情報は出力されないため、代わりに**インシデントの担当者アサイン**を情報を付与することを推奨します
    - Sentinel 側でクローズ処理を行った場合、Teams メッセージ投稿内容を更新して、ステータス変更 / 更新者 / クローズ時の理由を情報として通知させています
![image](https://github.com/user-attachments/assets/5a5aa4d1-c44b-4df9-8547-e33991b3be3c)

# Deploy To Azure
> 2つのロジックアプリをデプロイして下さい

- ``SentinelNotifyTeamsEnrichment`` テンプレート
  - Sentinel インシデントトリガーを用いて、Microsoft Teams に AdaptiveCard Format で通知します
  - 後段で用いる ``CloseSentinelIncidentEnrichment`` のため、投稿したメッセージID をタグに付与させています<p>
[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhisashin0728%2FSentinelIncidentNotificationTeams%2Fmain%2FSentinelNotifyTeamsEnrichment.json)

- ``CloseSentinelIncidentEnrichment`` テンプレート
  - メッセージタグを読み取って、対象のチャネルにクローズされた情報を通知します<p>
[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhisashin0728%2FSentinelIncidentNotificationTeams%2Fmain%2FCloseSentinelIncidentEnrichmentMaster.json)

# 設定作業
> テンプレートデプロイ後、以下作業を行って下さい

- ロジックアプリ (マネージドID) に対して、「Sentinel レスポンダー」ロールを付与して下さい
- API より Teams チャネルに投稿可能なユーザー権限で認証を行って下さい
- Sentinel オートメーション設定を以下行って下さい
  - クローズ用ロジックアプリ起動用のオートメーションを先に (以下例では ``ChangeStatus``)
  - Teams 発砲用ロジックアプリのオートメションを次に (以下例では ``Send-Teams-Enrichment``)
<img width="1080" alt="image" src="https://github.com/user-attachments/assets/d0337d2d-933c-4eb1-b4c0-92b510f2a6fe">

- クローズ用ロジックアプリのオートメーション設定例
  - トリガーが「**インシデントの更新時**」になることに注意
<img src="https://github.com/user-attachments/assets/e0489e5f-d9e0-4543-8bb4-5dbe3a342538" width="500">

- 起動用ロジックアプリのオートメーション設定例
  - トリガーが「**インシデントが作成されたとき**」になることに注意 
  - お客様状況に応じて、通知させる条件 (データソースなど) を設定(フィルタ) してカスタマイズして下さい
<img src="https://github.com/user-attachments/assets/c48a7d13-955f-439f-858c-766daaeffbb1" width="500">

# 免責事項

- 本レポジトリのコンテンツによって発生するコストについては、利用するユーザーが責任を負います。
- 本レポジトリのコンテンツによって作成される環境から出力される内容について、作成者は責任を負いません。
- 本レポジトリはオープンソースです。 
