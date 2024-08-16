# SentinelIncidentNotificationTeams
このレポジトリは Microsoft Sentinel のインシデントを Microsoft Teams に通知するサンプルテンプレートです。

# 画面イメージ
> Sentinel インシデントトリガー時に、ターゲットの Teams チャネルに情報を発信します

- 通知時
  - Teams Adaptive Card Format を用いて、インシデント内容をメッセージに、インシデントに含まれるコメント/エンティティ情報を返信として通知します。
![image](https://github.com/user-attachments/assets/1f1ea47a-cd3f-47b0-aa9f-6724943ae787)
- インシデントクローズ時
  - インシデントクローズ処理時を Sentinel オートメーションより起動し、Teams メッセージの更新情報として通知します
    - Defender XDR 側でクローズ処理を行った場合、仕様によりユーザー情報が出ませんので、インシデントの担当者アサイン情報を付与することを推奨します
    - Sentinel 側でクローズ処理を行った場合、更新者、クローズ時の理由を付加情報として通知させています
![image](https://github.com/user-attachments/assets/d866a74c-61b8-44e9-8e3c-d4a341d295e6)

# Deploy To Azure
> 2つのロジックアプリをデプロイして下さい

# 設定作業
> テンプレートデプロイ後、以下作業を行って下さい

- ロジックアプリ (マネージドID) に対して、「Sentinel レスポンダー」ロールを付与して下さい
- API より Teams チャネルに投稿可能なユーザー権限で認証を行って下さい 
