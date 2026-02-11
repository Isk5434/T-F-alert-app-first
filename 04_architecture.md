# 技術アーキテクチャ図 (System Architecture)

Track & Field Alerts PoCのシステム構成図。

```mermaid
graph TD
    subgraph Data Sources [監視対象データソース]
        WEB[陸協公式サイト (HTML)]
        PDF[要項/リザルト (PDF)]
    end

    subgraph Crawler System [収集・検知システム (Cloud Functions/Batch)]
        Scheduler[Scheduler (Cron)] --> Spider[Web Spider]
        Spider -- Access (Low freq) --> WEB
        Spider -- Download --> PDF
        
        Spider --> RateLimiter[Rate Limiter & Compliance Check]
        RateLimiter --> DiffEngine[Diff Engine (Text/Hash)]
        
        PDF --> OCR[OCR / PDF Parser]
        OCR --> DiffEngine
    end

    subgraph Intelligence Layer [解析・加工]
        DiffEngine --> LLM[LLM Summarizer (GPT-4o mini)]
        LLM -- "変更の意味・重要度判定" --> Analyzer[Impact Analyzer]
        Analyzer --> DB[(Info Database)]
    end

    subgraph Admin [管理・承認]
        DB -- "HIGH Alert" --> AdminUI[管理画面 (Human Approval)]
        AdminUser((運営担当者)) -- "Approve/Reject" --> AdminUI
        AdminUI --> PubSub{Notification Pub/Sub}
    end

    subgraph Delivery [配信システム]
        DB -- "MID/LOW Alert" --> PubSub
        PubSub --> Push[FCM (App Push)]
        PubSub --> LineAPI[LINE Messaging API]
        PubSub --> Email[Email Service]
        PubSub --> Slack[Slack Webhook]
    end

    subgraph User App [モバイルアプリ (PWA/Flutter)]
        API[API Gateway]
        User((ユーザー)) -- "閲覧/設定" --> AppUI[Mobile App UI]
        AppUI -- "REST/GraphQL" --> API
        API --> DB
    end
    
    style Data Sources fill:#f9f,stroke:#333
    style Crawler System fill:#bbf,stroke:#333
    style Admin fill:#faa,stroke:#333
    style Intelligence Layer fill:#dfd,stroke:#333
```

## アーキテクチャのポイント
1.  **Crawler & Compliance**: `Scheduler` が定期実行し、`RateLimiter` を介してアクセス。`robots.txt` 準拠を徹底。
2.  **Diff & Intelligence**: 単純なHTML差分だけでなく、PDF解析とLLMを用いて「意味のある差分（日程変更、資格変更）」を抽出。
3.  **Human in the Loop**: 重要度判定 (`Analyzer`) で `HIGH` と判定されたものは、必ず `AdminUI` で人間が承認しないと配信されない安全設計。
4.  **Multi-Channel Delivery**: ユーザーの好みに合わせて Push, LINE, Email, Slack へ配信。
