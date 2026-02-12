# -n8nclaw
# Architecture
                        TRIGGERS
          +-----------+-----------+-----------+
          | Telegram  | WhatsApp  |   Gmail   | Hourly
          | Trigger   | Webhook   |  Trigger  | Heartbeat
          +-----------+-----------+-----------+
                |           |           |           |
          [ Filter ]  [ Filter ]  [ Get User ] [ Get User ]
                |           |           |           |
          [ Get User Profile from Init Table ]
                |           |           |           |
          [ Edit Fields — normalize: user_message, system_prompt, last_channel ]
                \           |           |           /
                 +----------+-----------+----------+
                            |
                      +-----v------+
                      |  n8nClaw   |  (Claude Sonnet 4.5 via OpenRouter)
                      |  AI Agent  |  (Postgres Chat Memory - 15 msgs)
                      +-----+------+
                            |
               +------------+-------------+
               |            |             |
          [ Switch: last_channel ]
               |            |
          Telegram     WhatsApp
          Reply        Reply (Evolution API)

                      TOOLS & SUB-AGENTS
          +-------------------------------------------+
          | Tasks DB    | Subtasks DB  | Init/User DB  |
          | Research    | Email Mgr    | Doc Manager   |
          | Worker 1    | Worker 2     | Worker 3      |
          | Vector Store (Supabase RAG)               |
          +-------------------------------------------+

                    MEMORY PIPELINE (scheduled)
          +-------------------------------------------+
          | Postgres Chat History                      |
          |   -> Aggregate messages                    |
          |   -> Summarize (Haiku 4.5)                 |
          |   -> Embed (OpenAI)                        |
          |   -> Store in Supabase Vector DB           |
          +-------------------------------------------+
