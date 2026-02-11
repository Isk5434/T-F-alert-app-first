# 開発タスク (Weekly Sprint Plan)

6週間 (1.5ヶ月) でのMVPリリースを目指すスプリント計画。

## Sprint 1: 基盤構築 & クローラー開発
- [ ] **Infra**: GCP/AWS プロジェクトセットアップ (Cloud Functions, Firestore, Pub/Sub)
- [ ] **Crawler**: `robots.txt` チェッカーの実装
- [ ] **Crawler**: JAAFサイトのHTML差分検知プロトタイプ作成
- [ ] **Crawler**: PDFテキスト抽出検証 (PDFMiner / Azure Document Intelligence)
- [ ] **Admin**: 開発者用ログ基盤の整備

## Sprint 2: インテリジェンス & DB設計
- [ ] **Backend**: DBスキーマ設計 (Users, Alerts, Orgs)
- [ ] **AI**: OpenAI APIを用いた「差分要約」プロンプトのエンジニアリング
- [ ] **AI**: 重要度 (HIGH/MID/LOW) 判定ロジックの実装
- [ ] **API**: 基本API (GET /alerts) の実装

## Sprint 3: 管理画面 & 重大通知フロー
- [ ] **Admin**: 管理画面 (Retool 等で簡易作成) の構築
- [ ] **Admin**: HIGHアラートの「承認キュー」実装
- [ ] **Notification**: LINE Messaging API 連携
- [ ] **Legal**: 利用規約・プライバシーポリシー・免責事項のドラフト作成

## Sprint 4: モバイルフロントエンド (Core)
- [ ] **App**: Flutter/React Native プロジェクト作成
- [ ] **App**: UI実装 (Onboarding, Dashboard) - "Sports Stylish" デザイン適用
- [ ] **App**: 通知詳細画面 & 原文リンク遷移の実装
- [ ] **App**: Push通知受信設定 (FCM)

## Sprint 5: カレンダー & 詳細機能
- [ ] **App**: カレンダービューの実装 (大会日程・期限表示)
- [ ] **App**: フィルタリング設定画面 (種目・県)
- [ ] **Backend**: キャッシュ戦略の実装 (Read負荷対策)
- [ ] **Share**: シェア用OGP生成機能の実装

## Sprint 6: 統合テスト & リリース準備
- [ ] **QA**: E2Eテスト (検知〜通知〜アプリ閲覧)
- [ ] **QA**: 誤報テスト (Copyright変更などを無視できるか)
- [ ] **Ops**: リリースビルド & デプロイ
- [ ] **GTM**: 初期のテスター(友人・コーチ)への招待送付
