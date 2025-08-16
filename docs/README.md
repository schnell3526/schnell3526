職務経歴書
===

## 基本情報

| 項目     | 内容                                            |
| -------- | ----------------------------------------------- |
| 名前     | 伊藤俊太朗                                      |
| 居住地   | 東京都                                          |
| 最終学歴 | 早稲田大学大学院基幹理工学研究科 河原大輔研究室 |
| 資格     | TOEIC Listening & Reading Test 810              |
| 現所属   | Sansan株式会社                                  |

## 意欲・興味

ML技術の実応用 (MLOps) およびクラウドネイティブに興味関心があります。

## 職務経歴詳細

### Sansan 株式会社

#### GENIAC に伴うクロスクラウドデータソースのML基盤構築 (2025/07 ~ 2026/02)

【チーム編成】

- GENIACプロジェクトリード: 1人
- MLエンジニア: 3人
- MLOpsエンジニア: 1人 (自身の担当)
  - 一部期間インターン生含む

【業務内容】

新規のアカウントに VPC, IAM 等の設計/構築も含めてエンプラ水準のセキュリティ要件を満たす機械学習環境の技術選定から構築

具体的な業務は以下です
- GCP -> AWS のデータ転送料金削減のためのキャッシュシステムの構築
- Job スケジューラー(キューイングシステム) の選定
- セキュリティ要件を満たした学習環境へのアクセス方法の設計
  - 既存の整理では対応できなかったので、CTO, 情報セキュリティ部門長との折衝も含む
- GPU の利用率モニタリングシステム構築
- ParallelCluster のデフォルト設定では対応できなかった GPU の排他制御の対応

【スキルスタック】

- インフラ
  - AWS
    - EC2, ParallelCluster
  - GCP
- IaC
  - Terraform, CloudFormation
- モニタリング
  - Managed Grafana, Managed Prometheus
- セキュリティ
  - AWS: GuardDuty, SSM, IAM
  - GCP: IAM(Workload Identity 連携)
- 言語
  - Golang (キャッシュプロキシの実装に利用)
  - Python (デモの学習スクリプトの作成に利用)
  - ShellScript

【成果】

- 既存の整理の範疇では対応できない取り組みのため部長/経営層のステークホルダーとの折衝を行いセキュリティ要件を擦り合わせた
- GCS -> AWS のデータ転送料金を N% 削減した
- 期間中のGPU利用率 N% を達成した

#### LLMOps 基盤の構築 (2025/05~)

【チーム編成】

- MLOps エンジニア: 2人
- プロダクトサイドエンジニア: 2人
  - LLM 機能の開発者

【業務内容】

プロダクトサイドの需要もあり、LLMOps ツールである Langfuse(V2) を EKS + マネージドサービスを利用して構築

具体的な業務は以下
- デプロイ方法の検討
  - 最終的に EKS でアプリケーション層 (Web, Worker) をデプロイ、ClickHouse は ECS でセルフマネージド、その他の DB は RDS や ElastiCache を利用することで既存のステートレス EKS の BG デプロイの際に追加運用を発生させずに済む構成でデプロイ
- Okta SSO との連携検証
  - 機密データを管理するので会社のセキュリティ基準を踏まえて Okta Group を動的に変更して時限式でログインできる仕組みを構築

【スキルスタック】
- ミドルウェア
  - ClickHouse(on ECS), RDS, ElastiCache Valkey
- IaC
  - Terraform, k8s manifest
- モニタリング
  - EKS: fluent-bit, CloudWatch, DataDog
  - ECS: CloudWatch
- セキュリティ
  - Okta, AWS IAM Identity Center
- 言語
  - TypeScript