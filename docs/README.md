# 基本情報

| 項目     | 内容                                                                       |
| -------- | -------------------------------------------------------------------------- |
| 名前     | 伊藤俊太朗                                                                 |
| 居住地   | 東京都                                                                     |
| 最終学歴 | 早稲田大学大学院基幹理工学研究科 河原大輔研究室 (自然言語処理専攻; 修士卒) |
| 資格     | TOEIC Listening & Reading Test 810 / 個人情報保護士                        |
| 現所属   | Sansan株式会社                                                             |
| 職種     | ML Platform Engineer                                                       |
| リンク   | [GitHub](https://github.com/schnell3526) / [Hugging Face](https://huggingface.co/schnell) |

## Summary

研究者・データサイエンティストと協業しながら、GPU 基盤や CDE（Cloud Development Environment）/ Job 基盤といった ML 特有の基盤を作ることに強みを持つ ML Platform Engineer です。

NLP 研究者としてのバックグラウンドがあります。学生時代に約 120 ノード・960 GPU の分散学習を経験して ML の本番運用における基盤の重要性を実感し、ML 基盤エンジニアの道に進みました。研究側の経験があるからこそ、ML エンジニアや研究者との協業では依頼にそのまま応えるのではなく、本当に求められていることを先読みして提案し、基盤を変えていくことを重視しています。

現在は研究開発部門向けの 2 種類の EKS クラスタのオーナーとして、設計・構築・運用を担当しています。

- **推論基盤（Circuit）：** 本番 150〜250 ノード
- **CDE / Job 基盤（研究開発クラスタ）：** 3 部門・70 名以上が利用、月 1,000 本以上の Job を実行、マルチノード分散学習に対応

主な成果：

- 開発環境へのアクセスをブラウザに限定し DLP をかける方式を提案し、全社のセキュリティ基準として採用された
- 研究開発クラスタの刷新で月額 $10,000 弱のコストを削減し、環境構築の依頼を月 10 件以上から 1〜2 件に削減
- 社内の機密データを扱える Langfuse 基盤を立ち上げ、約 4 プロダクトで使われる事実上の全社 LLM 評価基盤に発展

Kubernetes / AWS を中心としたクラウドネイティブ技術を軸に、構築して終わりにせず、運用しながら仕組みとチームの両方を継続的に改善することを重視しています。課題を OSS の導入、Controller の内製、OSS への貢献、文化の醸成のどれで解決すべきか判断できることが強みです。

クラスタのオーナーとして、チームの成果を最大化するために、基盤の機能開発方針の策定、タスクの分配、レビューを通じたメンバーの支援を担っています。また、採用・評価基準づくり・メンタリングを通じてチームづくりにも関わっています。

---

# 職務経歴詳細

## Sansan 株式会社（2024/04〜現在）

### セキュリティと利便性を両立する研究開発クラスタ（CDE / Job 基盤）の構築 (2026/03〜2026/10; その後改善運用)

研究開発部を中心に、機密データを扱う 3 部門・70 名以上が使うクラウド開発環境（CDE）と、そこから実験を投入する Job 基盤について、発案から設計・構築・移行支援までを一気通貫で担当しました。

#### CDE（クラウド開発環境）

**以前の課題**

- Terraform で 1 人 1 台の EC2 インスタンスを用意し、AMI も自前でビルドしていた
- アクセス経路は SSH（L4）か SSM で、SSH は L7 で制御できないためセキュリティ面に不安があり、SSM は設定が煩雑だった
- インスタンスの追加・変更には Terraform の変更が必要で、ML エンジニアが自分で書くのは難しく、エンジニアへの依頼（月 10 件以上）がトイルになっていた
- 常時稼働を前提に、Savings Plans でコストを抑える運用だった
- 機密データを扱う環境での AI 利用に制限がかかり、従来の構成のままでは AI エージェントを活用した開発が根本的に難しくなっていた

**変えたこと**

- Coder によるライフサイクル管理に移行し、インスタンスファミリー・サイズ・GPU の種類・CPU アーキテクチャ・ストレージサイズなどを利用者が完全セルフサービスで選べるようにした
- 自動停止を有効にし、「常時稼働 + Savings Plans」から「使わないときは止める」というシンプルな運用に一本化した
- アクセス経路を 443 番ポートに統一し、認証を外部 IdP に寄せて社内標準の認証フローに乗せた
- 開発環境へのアクセスをブラウザ経由に限定し、エンタープライズブラウザで DLP をかける方式を提案・導入した

**成果**

- 「開発環境はブラウザ経由に限定し、エンタープライズブラウザで DLP をかける」という方式が、自分の提案をきっかけに全社のセキュリティ基準として採用された
- 機密データを扱う環境の AI 利用制限の下でも、AI エージェントを活用した開発を続けられる環境を実現
- 月額 $10,000 弱のインフラコスト削減（主に自動停止による）
- 環境構築の依頼が月 10 件以上から 1〜2 件に減少
- 3 部門・70 名以上が移行（将来的に約 100 名規模を想定）

#### Job 基盤

**以前の課題**

- 1 人 1〜2 台の EC2 インスタンス上で実験を回しており、同時に流せる実験の数が手元のマシンの台数に縛られ、実験スループットに限界があった
- 夜間に実験が終わると、朝まで GPU が使われないまま稼働し続けて無駄になっていた
- CDE に自動停止ポリシーを設定するにあたり、長時間のプロセスを実行したい場合のオフロード先が必要になった

**変えたこと**

- 本番クラスタのような厳格な GitOps ではなく、CLI で作業環境から気軽に Job を投入できることを設計上の最優先にした
- Karpenter で必要なときに on-demand でインスタンスを確保する構成にし、利用者が Job のスケジューリングを基本的に考えなくてよい状態にした。Job が終わればノードも片付くため、実験終了後に GPU が空回りすることもなくなった
- 将来 RI を購入して共有する構成や、Capacity Blocks for ML で確保した H100 を共有する需要を見据え、Kueue を初期から導入して Job の admission を管理する層を用意した

**成果**

- 毎月 1,000 本以上の Job が実行され、研究開発の実験スループットが大幅に向上
- 長時間の処理を Job に逃がせるようになり、CDE の自動停止を前提とした運用が成り立つようになった

#### クラスタ全体の設計・構築

**主な内容**

- ネットワーク・セキュリティ境界の設計
  - VPC / Subnet 構成および Egress 制御の見直し
  - Tetragon (eBPF) を用いたランタイムセキュリティ監視の導入
- 認証・認可・Secrets 基盤
  - Pod Identity を前提とした権限委譲モデルの設計
  - 監査要件を満たす Secrets 配布フローの標準化
- マルチテナント・ガバナンス
  - Kueue による Job の admission 管理（RI / Capacity Blocks for ML の共有を見据えた設計）
  - NodePool 分離と Karpenter による動的スケーリングの両立
- 監査・可観測性
  - New Relic を中心とした監査ログ / メトリクスの統合基盤

**技術スタック：** EKS, Karpenter, GPU Operator, Kueue, Coder, Tetragon, New Relic, Terraform

---

### EKS アプリケーションプラットフォーム（推論基盤 Circuit）の設計・構築・運用 (2025/10〜現在)

研究開発部門向け ML 基盤「Circuit」の Platform チームに所属。150〜250 ノード規模の EKS クラスタのオーナーとして、設計から運用まで一貫して担当しています。

**主な取り組み**

- 基盤としてのスケーラビリティを担保するための Platform の責務境界 (IF) の設計
  - Pod Identity 移行を見越した IRSA の運用方針の策定と設定責務移譲設計
  - マルチテナントクラスタの NodePool 設計
- Kubernetes 運用体制の構築
  - コントロールプレーンのバージョン更新戦略の作成
  - 各種 Controller の運用方針策定（運用上の考慮事項の周知）
- アドオン更新を「年 1 回の一括更新」から「週 2 回程度の継続的な更新」へ移行
  - 従来は約 1 年に 1 回のクラスタ更新時に、すべてのアドオンをまとめて上げるビッグバンリリースだった
  - 約 20 種類のアドオン（Karpenter, GPU Operator, Istio, Argo CD, KEDA, cert-manager, EKS 標準アドオンなど）を Renovate でバージョン管理し、3 名のチームで Renovate の PR を起点に分担して更新する運用を確立
  - 更新の担当者には、そのアドオンがクラスタ内でどう動いているか、更新でどんな影響が起こりうるかを理解したうえで作業してもらう形にし、更新作業そのものをチームのクラスタ知識のキャッチアップの場にした
  - 成果：こまめな更新によるセキュリティの強化、Blue/Green 方式のクラスタ更新で一度に扱う差分が減ったことによる負担の軽減、属人化の解消
- AMI 更新を「クラスタのライフサイクルと同一」から「AMI のリリースごと（1〜2 週間に 1 回程度）」へ移行
  - 課題：AMI がクラスタと同じ寿命だったため、長寿命のワークロードでは Karpenter の `expireAfter` による強制的なノード入れ替えが、Node Disruption Budget を無視して日中に発生していた。また、セキュリティ対応で AMI の入れ替えを求められる機会も増えていた
  - AMI をこまめに入れ替える前提の仕組みを構築
    - Renovate による新しい AMI の検知
    - stg 環境でのクリティカルな GPU ワークロードの動作確認と、Argo Workflows で作成したテスト用 Workflow による事前のエラー検知
    - AMI の入れ替えを Budget を無視する Expiration ではなく Drift で行うようにし、Node Disruption Budget（`reasons: [Drifted]` + `schedule`）で Drift による disruption を夜間に限定して、影響の少ない時間帯にノードを入れ替え
    - 許容されるサービスレベルが低い学習用の研究開発クラスタ（CDE / Job 基盤）で先行導入してから、本番の推論基盤（Circuit）に展開
  - 成果：研究開発クラスタでの先行検証で、NVIDIA AMI の `pause.tar` 破損によりノードがクラスタに参加できない不具合を本番展開前に発見。upstream に報告し、修正がマージされた（[awslabs/amazon-eks-ami#2855](https://github.com/awslabs/amazon-eks-ami/issues/2855)）
- Platform の拡張コンポーネントの実装と運用
  - Argo CD と連携する Image 更新 worker の実装
  - Job/Workflow の実行完了通知の仕組み
  - GPU インスタンスの RI 使用量を把握するための Controller の実装

**技術スタック：** EKS, Karpenter, GPU Operator, Argo CD, Argo Workflows, Argo Rollouts, KEDA, Istio, Renovate, Terraform, Datadog, New Relic

---

### GENIAC プロジェクト: GPU クラスタの構築 (2025/07〜2026/02、2026/07〜現在; 2027/02 終了予定)

経済産業省 GENIAC プロジェクトにおいて、Cycle3 で GPU クラスタの構築管理をリードし、現在は Cycle4 でも引き続きリードしています。Cycle3 は「視覚接地した文書特化型視覚言語基盤モデルの構築」というテーマで採択されたものです。

**チーム編成：** MLOps エンジニア 1名（自分）、MLエンジニア 3名、プロジェクトリード 1名

**技術選定**

- Cycle3: Slurm（AWS ParallelCluster）を採用。ML エンジニアの学習コストが低いこと、数日〜数週間走る事前学習では Kubernetes だと Eviction 対策などの追加設計が要ること、期間限定のプロジェクトで組織の標準外でも長期的な負債にならないことから判断した
- Cycle4: Cycle3 の振り返りから、マルチテナントやセキュリティの要件を OS レベルの設定で頑張るより、部門に蓄積のある Kubernetes に乗せる方が合理的と判断し、マルチノード分散学習にも対応する EKS ベースのクラスタに切り替えた。研究開発クラスタ（CDE / Job 基盤）の spoke として使えるように設計している

**主な取り組み**

- p5.48xlarge（NVIDIA H100 × 8）による GPU 学習環境の設計・構築（Cycle3）
  - EFA / Placement Group を有効にした構成を Terraform でコード化し、インフラ担当 1 名で 3 週間でマルチユーザー運用・監視・セキュリティ対応まで含めて提供開始
  - ODCR で確保した GPU を夜間・休日も遊ばせないよう、複数の ML エンジニアが非同期に Job を投入できるスケジューラ前提の運用にした
  - 1 ノード内の GPU を複数人で共有するための排他制御を、cgroups による分離の検証を経て、Slurm の gres と GPU 枚数に比例した CPU / メモリ配分の運用ルールで実現
- GCP → AWS のデータ転送コスト削減のための Google Cloud SDK・AWS SDK 互換のキャッシュプロキシの設計（実装の一部はインターン生に委譲、Go）
  - 課題：約 20TB のデータセットを 100 エポック以上、毎エポック GCS から直接読む学習では、転送コストが試算で約 $240,000 に達する。削除依頼への即時対応などデータガバナンス上の要件があり、データセットを事前に丸ごとコピーする方法は取れなかった
  - GCS から取得したデータを S3 にキャッシュする方式にし、ML 側の変更をエンドポイントの設定だけに抑えた
  - 成果：データ転送料金を従来の 1/100 以下に削減（プロキシ経由の遅延は 10〜20% 程度）。コストの懸念で踏み出せなかった大規模な事前学習に取り組めるようになった
  - Cycle3: 各ノードに systemd でデプロイ
  - Cycle4: Pod 単位の細かい認可要件を満たすために K8s サイドカーとしてデプロイ
- GPU の利用状況のモニタリングシステムの構築
  - Cycle3: 自社のセキュリティ基準上 W&B が使えなかったため自前で構築。systemd timer で GPU 使用率・メモリ使用率を収集して Amazon CloudWatch に送信し、利用率が低い状態が続くと Slack に通知するアラートを設定
  - Cycle4: メトリクス収集 DaemonSet -> New Relic。使用率、メモリ使用率、SM 使用率、帯域使用率、NVLink 使用率、Xid エラーなど、分散学習のボトルネック分析に使えるメトリクスまで収集
- エンタープライズ水準のセキュリティ要件を満たすためのシステム設計および CTO・情報セキュリティ部門長との折衝
  - 自社の AWS Organizations 外から払い出されるアカウントで IAM Identity Center が使えない前提のもと、全ノードを Private Subnet に置き、クロスアカウント IAM ロール + SSM Session Manager でアクセス経路を設計
  - GuardDuty / CloudTrail / VPC Flow Logs のログを管理アカウント側の SIEM に連携し、メトリクスは OAM で既存のダッシュボードに統合

**技術スタック：** AWS ParallelCluster (Slurm, CloudFormation), Amazon EKS, Terraform, Amazon CloudWatch, New Relic, Okta

**関連記事**

- [GENIAC における ParallelCluster GPU クラスタの構築記録 (Sansan Tech Blog)](https://buildersbox.corp-sansan.com/entry/2026/03/27/170000)
- [GCS→S3キャッシュプロキシで大規模ML学習のデータ転送コストを削減した話 (Sansan Tech Blog)](https://buildersbox.corp-sansan.com/entry/2026/01/07/140000)（メンタリング担当のインターン生による執筆）

---

### LLMOps 基盤の構築 (2025/05〜2026/03)

プロダクト開発チームと連携し、LLMOps ツール Langfuse (V3) の社内基盤を構築しました。[[外部登壇資料](https://speakerdeck.com/sansan_randd/shibuya-ai-4)]

**チーム編成：** MLOps エンジニア 2名、プロダクトサイドエンジニア 2名

**担当範囲：** 導入の判断から初期構築までを担当し、その後の運用は後任に引き継ぎ

**背景**

- LLM をプロダクトに組み込むにあたり、Langfuse を使いたいという要望があった
- データの機密性の都合で SaaS 版は使えず、条件を満たす現実的な選択肢はセルフホストの Langfuse だけだった
- 一方でセルフホストの構成は非常に重く、プロンプト評価の専門家もいる R&D でホストしてはどうかという話が挙がった

**判断**

- LLM 活用が広がる流れを考えると、社内で最も機密性の高いデータまで入力できる状態でセルフホストすれば、今後の利用者も十分に見込め、運用の負担より提供できる価値の方が大きくなると考えて導入を決めた

**やったこと**

- EKS（アプリケーション層）+ ECS（ClickHouse）+ マネージドサービス（RDS, ElastiCache）のハイブリッド構成設計
- 既存の Blue/Green デプロイフローに追加運用を発生させない構成を実現
- Okta SSO 連携による時限式アクセス制御の実装

**成果**

- 約 4 プロダクトをまたいで利用されている
- Langfuse に蓄積したデータを使って LLM-as-a-Judge をはじめとしたプロンプト改善の仕組みが整い、事実上の全社の LLM 評価基盤に発展した

**技術スタック：** EKS, ECS, ClickHouse, RDS, ElastiCache Valkey, Okta, Terraform

---

### 採用・組織開発 (2025/04〜現在)

- MLOps エンジニアの技術面接を担当（30 名以上を面接し、テクニカルリード相当の複数名が入社）
- MLOps エンジニアに求められるスキルの言語化・評価基準の策定・JD の執筆
- インターン生のメンタリング（2名）
- コミュニティイベント登壇
  - [渋谷 Biz × AI: ビジネスにおける AI 利活用 事例勉強会 第4回](https://sansan.connpass.com/event/380668)

---

## OSS 貢献

### 主な Issue 投稿

| プロジェクト | 内容 | リンク |
| --- | --- | --- |
| awslabs/amazon-eks-ami | AMI 更新の先行検証で見つけた、NVIDIA AMI の `pause.tar` 破損でノードがクラスタに参加できない不具合を、影響範囲の調査とあわせて報告。[修正 PR](https://github.com/awslabs/amazon-eks-ami/pull/2858) がマージされ解決 | [#2855](https://github.com/awslabs/amazon-eks-ami/issues/2855) |
| hashicorp/terraform-provider-helm | 重要なバグ修正を取り込むコミュニティフォークの共有と、upstream への還元の相談 | [#1750](https://github.com/hashicorp/terraform-provider-helm/issues/1750) |
| argoproj/argo-rollouts | 1つの Rollout で複数の workload を参照する機能を提案 | [#4565](https://github.com/argoproj/argo-rollouts/issues/4565) |
| kedacore/keda | AWS SQS scaler に、メッセージの動きに基づくスケーリングを提案 | [#7327](https://github.com/kedacore/keda/issues/7327) |

### マージされた PR

| プロジェクト | 内容 | リンク |
| --- | --- | --- |
| coder/registry | vscode-web の healthcheck 先を `/version` に修正 | [#923](https://github.com/coder/registry/pull/923) |
| hashicorp/terraform-provider-kubernetes | ドキュメントとサンプルの exec 認証の API version を `v1` に更新 | [#2823](https://github.com/hashicorp/terraform-provider-kubernetes/pull/2823) |
| pytorch/tutorials | チュートリアルのモデル定義から出力層の不要な ReLU を削除 | [#1644](https://github.com/pytorch/tutorials/pull/1644) |
| EducationalTestingService/factor_analyzer | `randomized_svd()` に `random_state` を指定して警告を解消 | [#95](https://github.com/EducationalTestingService/factor_analyzer/pull/95) |

### フォークのメンテナンス

- [schnell3526/terraform-provider-helm](https://github.com/schnell3526/terraform-provider-helm)
