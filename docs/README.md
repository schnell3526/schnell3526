# 基本情報

| 項目     | 内容                                                                       |
| -------- | -------------------------------------------------------------------------- |
| 名前     | 伊藤俊太朗                                                                 |
| 居住地   | 東京都                                                                     |
| 最終学歴 | 早稲田大学大学院基幹理工学研究科 河原大輔研究室 (自然言語処理専攻; 修士卒) |
| 資格     | TOEIC Listening & Reading Test 810                                         |
| 現所属   | Sansan株式会社                                                             |
| 職種     | Platform Engineer                                                          |

## Summary

ML基盤に特化した Platform Engineer。NLP 研究のバックグラウンドを持ち、学生時代に約 120 ノード 960 GPU での分散学習を経験し ML の本番運用における基盤の重要性を実感し、ML 基盤エンジニアの道に進みました。

現在は研究開発部門向け EKS クラスタ（本番 80-200ノード・1,000+ Pod）のオーナーとして設計・構築・運用を担当し Kubernetes / AWS を中心としたクラウドネイティブ技術に精通し、課題に対して OSS 導入・Controller 内製・OSS 貢献・文化醸成のいずれで解決すべきかを判断できることを強みとして働いています。

---

# 職務経歴詳細

## Sansan 株式会社（2024/04〜現在）

### EKS アプリケーションプラットフォームの設計・構築・運用 (2025/10〜現在)

研究開発部門向け ML 基盤「Circuit」の Platform チームに所属。80-200ノード・1,000+ Pod 規模の EKS クラスタのオーナーとして、設計から運用まで一貫して担当しています。

**主な取り組み**

- 基盤としてのスケーラビリティを担保するための Platform の責務境界 (IF) の設計
  - Pod Identity 移行を見越した IRSA の運用方針の策定と設定責務移譲設計
  - マルチテナントクラスタの NodePool 設計
- Kubernetes 運用体制の構築
  - コントロールプレーンのバージョン更新戦略の作成
  - 各種 Controller の運用 (version up 戦略や運用の考慮事項の周知) 方針策定
- Platform の拡張コンポーネントの実装と運用
  - ArgoCD と連携する Image 更新 worker の実装
  - Job/Workflow の実行完了通知の仕組み
  - GPU インスタンスの RI 使用量を把握するための Controller の実装

**技術スタック：** EKS, Karpenter, ArgoCD, Argo Workflows, KEDA, GPU Operator, Istio, Terraform, Datadog

---

### GENIAC プロジェクト: GPU クラスタの構築 (2025/07〜2026/02)

経済産業省 GENIAC プロジェクトにおいて、AWS ParallelCluster による大規模 GPU 実験基盤の構築を技術選定からリードしました。

**チーム編成：** MLOps エンジニア 1名（自分）、MLエンジニア 3名、プロジェクトリード 1名

**主な取り組み**

- AWS ParallelCluster を用いた GPU 学習環境の設計・構築
- GCP → AWS のデータ転送コスト削減のための google sdk, aws sdk 互換のキャッシュプロキシ実装（Golang）
- GPU 利用率モニタリングシステムの構築
- エンタープライズ水準のセキュリティ要件を満たすためのシステム設計および CTO・情報セキュリティ部門長との折衝

**技術スタック：** AWS ParallelCluster (Slurm, CloudFormation), AWS VPC, Terraform, Okta, Golang, Workload Identity Federation (Google Cloud 連携)

---

### LLMOps 基盤の構築 (2025/05〜現在)

プロダクト開発チームと連携し、LLMOps ツール Langfuse (V3) の社内基盤を構築しました。

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

- 最新の Python 開発環境（DevContainer, UV, AI ）を標準化
- GitHub Actions 経由で10秒での新規プロジェクト作成を実現
- 実動作するプロジェクトとしてテンプレートを管理し、Cookiecutter 方式と比べてメンテナンス工数を削減

---

### 採用・組織開発 (2025/04〜現在)

- MLOps エンジニアの技術面接を担当（10名以上、複数名の入社実績あり）
- MLOps エンジニアに求められるスキルの言語化・評価基準の策定
- インターン生のメンタリング（1名）
- コミュニティイベント登壇
  - [渋谷 Biz × AI: ビジネスにおける AI 利活用 事例勉強会 第4回](https://sansan.connpass.com/event/380668)

---

## OSS 貢献

| プロジェクト                            | 内容               | リンク                                                                        |
| --------------------------------------- | ------------------ | ----------------------------------------------------------------------------- |
| hashicorp/terraform-provider-kubernetes | docs 更新          | [#2823](https://github.com/hashicorp/terraform-provider-kubernetes/pull/2823) |
| aws/karpenter-provider-aws              | 機能追加 PR        | [#8913](https://github.com/aws/karpenter-provider-aws/pull/8913)              |
| schnell3526/terraform-provider-helm     | forkのメンテナンス | https://github.com/schnell3526/terraform-provider-helm                        |

※ Issue 出しや Issue での議論はこまめにやっています

---

## 副業

### MLOps パイプライン構築支援

Vertex AI Pipelines + BigQuery を用いた MLOps パイプラインの設計・構築を支援しています。

### GPU 環境の可観測性設計

ECS on EC2 の GPU 環境における可観測性の設計・構築を支援。DCGM Exporter の導入、タグ設計によるコスト・リソースの可視化を実施しています。

---

# 技術的関心・今後の志向

## ML 推論最適化

量子化、プルーニング、知識蒸留などのモデル圧縮技術や、TensorRT、ONNX Runtime、vLLM などの推論エンジンを活用した高速化に関心があり、大規模言語モデルの推論コスト削減とレイテンシ改善を追求できるような仕事に興味があります。

約960GPU での分散学習経験（データ並列、パイプライン並列、ZeRO）と Transformer アーキテクチャの研究開発経験が、この領域への足がかりになると考えています。

## Platform Engineering / SRE

大規模分散システムの可観測性向上、開発者体験を重視したセルフサービス型プラットフォームの構築、SLI/SLO ベースの信頼性管理に関心があります。

現職での EKS クラスタ運用、Prometheus/Grafana/Datadog による監視基盤構築、GitOps による宣言的インフラ管理の経験をさらに深めていきたいです。
