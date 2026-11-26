# 基本情報

| 項目     | 内容                                                                       |
| -------- | -------------------------------------------------------------------------- |
| 名前     | 伊藤俊太朗                                                                 |
| 居住地   | 東京都                                                                     |
| 最終学歴 | 早稲田大学大学院基幹理工学研究科 河原大輔研究室 (自然言語処理専攻; 修士卒) |
| 資格     | TOEIC Listening & Reading Test 810                                         |
| 現所属   | Sansan株式会社                                                             |
| 職種     | ML Platform Engineer                                                       |

## Summary

ML ドメインとの連携（研究者・データサイエンティストとの協業、GPU の活用、CDE / Job 基盤などの特殊な用途）に強みを持つ ML Platform Engineer です。

NLP 研究者としてのバックグラウンドがあります。学生時代に約 120 ノード・960 GPU の分散学習を経験して ML の本番運用における基盤の重要性を実感し、ML 基盤エンジニアの道に進みました。

現在は研究開発部門向けの 2 種類の EKS クラスタのオーナーとして、設計・構築・運用を担当しています。

- **推論基盤：** 本番 150〜250 ノード
- **CDE / Job 基盤：** 50 名以上の研究者が利用、マルチノード分散学習に対応

Kubernetes / AWS を中心としたクラウドネイティブ技術に精通しています。課題を OSS の導入、Controller の内製、OSS への貢献、文化の醸成のどれで解決すべきか判断できることが強みです。

また、副業として Agent Platform（Vertex AI）や BigQuery を活用した MLOps 関連の業務にも従事しています。

---

# 職務経歴詳細

## Sansan 株式会社（2024/04〜現在）

### セキュリティと利便性を両立する研究開発クラスタの構築 (2026/03〜2026/10; その後改善運用)

研究開発部を中心とした機密データを扱う複数部門向けのクラウド開発環境の発案、設計、構築、移行支援等を一気貫通で行いました。
この取り組みにより、$10000/月弱のインフラコストの削減を達成しました。また、基盤の利用者は多様な種類のインスタンスを完全セルフサービスで構築できるようになりエンジニアの依頼対応工数を削減しました。

**主な取り組み**

- ネットワーク・セキュリティ境界の設計
  - VPC / Subnet 構成および Egress 制御の見直し
  - tetragon (eBPF) を用いたランタイムセキュリティ監視の導入
- 認証・認可・Secrets 基盤
  - Pod Identity を前提とした権限委譲モデルの設計
  - 監査要件を満たす Secrets 配布フローの標準化
- マルチテナント・ガバナンス
  - Kueue を用いた GPU リソースの公平なジョブキューイング設計
  - NodePool 分離と Karpenter による動的スケーリングの両立
- 監査・可観測性・DevEx 改善
  - New Relic を中心とした監査ログ / メトリクスの統合基盤
  - Coder による研究者向けセルフサービス開発環境の提供

**技術スタック：** EKS, Karpenter, GPU Operator, Kueue, Coder, tetragon, New Relic, Terraform

---

### EKS アプリケーションプラットフォームの設計・構築・運用 (2025/10〜現在)

研究開発部門向け ML 基盤「Circuit」の Platform チームに所属。150〜250 ノード規模の EKS クラスタのオーナーとして、設計から運用まで一貫して担当しています。

**主な取り組み**

- 基盤としてのスケーラビリティを担保するための Platform の責務境界 (IF) の設計
  - Pod Identity 移行を見越した IRSA の運用方針の策定と設定責務移譲設計
  - マルチテナントクラスタの NodePool 設計
- Kubernetes 運用体制の構築
  - コントロールプレーンのバージョン更新戦略の作成
  - 各種 Controller の運用方針策定（Renovate 駆動のバージョンアップ戦略、運用上の考慮事項の周知） 
- Platform の拡張コンポーネントの実装と運用
  - ArgoCD と連携する Image 更新 worker の実装
  - Job/Workflow の実行完了通知の仕組み
  - GPU インスタンスの RI 使用量を把握するための Controller の実装

**技術スタック：** EKS, Karpenter, GPU Operator, ArgoCD, Argo Workflows, Argo Rollouts, KEDA, Istio, Terraform, Datadog, NewRelic

---

### GENIAC プロジェクト: GPU クラスタの構築 (2025/07〜2026/02、2026/07~2027/02)

経済産業省 GENIAC プロジェクトにおいて、Cycle3, Cycle4 と2期続けて GPU クラスタの構築管理のリードをしました。
Cycle3 では AWS ParallelCluster を採用し、Cycle4 では部門の技術スタックの親和性を考慮して Kubernetes ベースのマルチノード分散学習にも対応するクラスタの構築をしました。

**チーム編成：** MLOps エンジニア 1名（自分）、MLエンジニア 3名、プロジェクトリード 1名

**主な取り組み**

- AWS ParallelCluster / Amazon EKS を用いた GPU 学習環境の設計・構築
- GCP → AWS のデータ転送コスト削減のための Google Cloud SDK・AWS SDK 互換のキャッシュプロキシの設計（実装の一部はインターン生に委譲、Golang）
  - Cycle3: 各ノードに systemd でデプロイ
  - Cycle4 では Pod 単位の細かい認可要件を満たすためにK8sサイドカーとしてデプロイ
- GPU 利用率(利用率、Mem利用率、SM使用率、帯域使用率、NVLink使用率、Xid エラー)等のモニタリングシステムの構築
  - Cycle3: メトリクス送信 Job (on systemd timer) -> Amazon Cloud Watch
  - Cycle4: メトリクス収集 DaemonSet -> NewRelic
- エンタープライズ水準のセキュリティ要件を満たすためのシステム設計および CTO・情報セキュリティ部門長との折衝

**技術スタック：** AWS ParallelCluster (Slurm, CloudFormation), Amazon EKS, Terraform, Okta

**関連記事**

- [GENIAC における ParallelCluster GPU クラスタの構築記録 (Sansan Tech Blog)](https://buildersbox.corp-sansan.com/entry/2026/03/27/170000)
- [GCS→S3キャッシュプロキシで大規模ML学習のデータ転送コストを削減した話 (Sansan Tech Blog)](https://buildersbox.corp-sansan.com/entry/2026/01/07/140000)（メンタリング担当のインターン生による執筆）

---

### LLMOps 基盤の構築 (2025/05〜2026/03)

プロダクト開発チームと連携し、LLMOps ツール Langfuse (V3) の社内基盤を構築しました。[[外部登壇資料](https://speakerdeck.com/sansan_randd/shibuya-ai-4)]

**チーム編成：** MLOps エンジニア 2名、プロダクトサイドエンジニア 2名

**主な取り組み**

- EKS（アプリケーション層）+ ECS（ClickHouse）+ マネージドサービス（RDS, ElastiCache）のハイブリッド構成設計
- 既存の Blue/Green デプロイフローに追加運用を発生させない構成を実現
- Okta SSO 連携による時限式アクセス制御の実装

**技術スタック：** EKS, ECS, ClickHouse, RDS, ElastiCache Valkey, Okta, Terraform

---

### 担当ドメインのマイクロサービス群のモノレポ化 (2024/12〜2025/10)

分散管理されていた複数サービスをモノレポに統合し、開発・運用効率を改善しました。

**主な取り組み**

- 最新の Python 開発環境（Dev Container, uv, AI ツール）を標準化
- GitHub Actions を使い、10 秒で新規プロジェクトを作成できる仕組みを実現
- 実動作するプロジェクトとしてテンプレートを管理し、Cookiecutter 方式と比べてメンテナンス工数を削減

---

### 採用・組織開発 (2025/04〜現在)

- MLOps エンジニアの技術面接を担当（30名以上、テクニカルリード相当複数名の入社実績あり）
- MLOps エンジニアに求められるスキルの言語化・評価基準の策定・JD の執筆
- インターン生のメンタリング（2名）
- コミュニティイベント登壇
  - [渋谷 Biz × AI: ビジネスにおける AI 利活用 事例勉強会 第4回](https://sansan.connpass.com/event/380668)

---

## OSS 貢献

### マージされた主なコントリビューション

| プロジェクト | 内容 | リンク |
| --- | --- | --- |
| coder/registry | vscode-web の healthcheck 先を `/version` に修正 | [#923](https://github.com/coder/registry/pull/923) |
| hashicorp/terraform-provider-kubernetes | ドキュメントとサンプルの exec 認証の API version を `v1` に更新 | [#2823](https://github.com/hashicorp/terraform-provider-kubernetes/pull/2823) |
| pytorch/tutorials | チュートリアルのモデル定義から出力層の不要な ReLU を削除 | [#1644](https://github.com/pytorch/tutorials/pull/1644) |
| EducationalTestingService/factor_analyzer | `randomized_svd()` に `random_state` を指定して警告を解消 | [#95](https://github.com/EducationalTestingService/factor_analyzer/pull/95) |

### 主な Issue 投稿

| プロジェクト | 内容 | リンク |
| --- | --- | --- |
| awslabs/amazon-eks-ami | NVIDIA AMI の `pause.tar` 破損でノードがクラスタに参加できない不具合を、影響範囲の調査とあわせて報告。[修正 PR](https://github.com/awslabs/amazon-eks-ami/pull/2858) がマージされ解決 | [#2855](https://github.com/awslabs/amazon-eks-ami/issues/2855) |
| hashicorp/terraform-provider-helm | 重要なバグ修正を取り込むコミュニティフォークの共有と、upstream への還元の相談 | [#1750](https://github.com/hashicorp/terraform-provider-helm/issues/1750) |
| argoproj/argo-rollouts | 1つの Rollout で複数の workload を参照する機能を提案 | [#4565](https://github.com/argoproj/argo-rollouts/issues/4565) |
| kedacore/keda | AWS SQS scaler に、メッセージの動きに基づくスケーリングを提案 | [#7327](https://github.com/kedacore/keda/issues/7327) |

### フォークのメンテナンス

- [schnell3526/terraform-provider-helm](https://github.com/schnell3526/terraform-provider-helm)

---

## 副業

### MLOps パイプライン構築支援

Vertex AI Pipelines + BigQuery を用いた MLOps パイプラインの設計・構築を支援しています。

### GPU 環境の可観測性設計

ECS on EC2 の GPU 環境における可観測性の設計・構築を支援。DCGM Exporter の導入、タグ設計によるコスト・リソースの可視化を実施しています。

---

# 技術的関心・今後の志向

## ML 推論最適化

量子化、プルーニング、知識蒸留などのモデル圧縮技術や、TensorRT、ONNX Runtime、vLLM などの推論エンジンを活用した高速化に関心があり、大規模言語モデルの推論コスト削減とレイテンシ改善を追求できる仕事に興味があります。

約 960 GPU での分散学習経験（データ並列、パイプライン並列、ZeRO）と Transformer アーキテクチャの研究開発経験が、この領域への足がかりになると考えています。

## Platform Engineering / SRE

大規模分散システムの可観測性向上、開発者体験を重視したセルフサービス型プラットフォームの構築、SLI/SLO ベースの信頼性管理に関心があります。

現職での EKS クラスタ運用、Prometheus/Grafana/Datadog による監視基盤構築、GitOps による宣言的インフラ管理の経験をさらに深めていきたいです。
