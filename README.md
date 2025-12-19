# BaiYaoDuel 插件

一個用於 Minecraft Paper 1.21.8 的決鬥插件。

## 功能特色

- 支持多種決鬥模式配置
- 決鬥邀請系統（需要對方接受才能開始）
- 自動記錄玩家統計（擊殺、死亡、斷線）
- 對戰歷史記錄
- 自動廣播決鬥結果
- 支持多人隊伍對戰（支持逗號分隔玩家ID）
- 時間限制功能（時間到自動平局）
- 決鬥開始時自動治療所有玩家
- 決鬥結束後自動傳送到重生點

## 安裝

1. 將編譯後的 `BaiYaoDuel-1.0.0.jar` 放入伺服器的 `plugins` 資料夾
2. 確保已安裝 `Multiverse-Core` 插件（用於世界傳送）
3. 重啟伺服器

## 指令說明

### 一般玩家指令

#### `/duel` 或 `/BaiYaoduel`
發起決鬥邀請

**用法：**
```
/duel <模式> <玩家id> [己方其他玩家id...]
/BaiYaoduel <模式> <玩家id> [己方其他玩家id...]
```

**注意：**
- 玩家ID可以用逗號分隔多個玩家，例如：`player1,player2`
- 發起邀請後，對方需要使用 `/duelaccept` 接受才會開始決鬥
- 邀請會在 2 分鐘後自動過期

**範例：**
```
/duel pvp Steve
/duel pvp player1,player2
/duel team player1,player2,player3 player4,player5,player6
```

#### `/duelaccept`
接受決鬥邀請

**用法：**
```
/duelaccept <發起者id>
```

**範例：**
```
/duelaccept Steve
```

#### `/duelno`
拒絕決鬥邀請

**用法：**
```
/duelno
```

#### `/BaiYaoduelstats`
查看決鬥統計

**用法：**
```
/BaiYaoduelstats KD
/BaiYaoduelstats player <對方玩家id>
/BaiYaoduelstats matchhistory [頁碼]
```

**範例：**
```
/BaiYaoduelstats KD
/BaiYaoduelstats player Steve
/BaiYaoduelstats matchhistory 1
```

**統計內容：**
- `KD` - 顯示擊殺數、死亡數、斷線次數和 KD 比率
- `player` - 顯示與指定玩家的對戰記錄和獲勝次數
- `matchhistory` - 顯示對戰歷史（每頁 10 條記錄）

### 管理員指令

#### `/BaiyaoduelBroadcast`
設定決鬥廣播格式

**用法：**
```
/BaiyaoduelBroadcast <模式> <格式>
```

**格式變數：**
- `{winner}` - 獲勝者名稱
- `{loser}` - 失敗者名稱

**範例：**
```
/BaiyaoduelBroadcast pvp &a[決鬥] &e{winner} &7擊敗了 &c{loser}
```

#### `/BaiYaoduelmode`
管理決鬥模式

**創建模式：**
```
/BaiYaoduelmode create <模式名稱>
```

**設定模式：**
```
/BaiYaoduelmode set <模式名稱> players <數量>
/BaiYaoduelmode set <模式名稱> map <地圖名稱>
/BaiYaoduelmode set <模式名稱> mapspawn <隊伍(1|2)> <X> <Y> <Z>
/BaiYaoduelmode set <模式名稱> maptime <時間(秒)>
```

**注意：**
- `players` 設定的是**每隊**的玩家數量，不是總人數
- 例如：設定 `players 2` 表示每隊需要 2 名玩家（總共 4 人）

**範例：**
```
/BaiYaoduelmode create pvp
/BaiYaoduelmode set pvp players 1
/BaiYaoduelmode set pvp map pvp_arena
/BaiYaoduelmode set pvp mapspawn 1 0 64 0
/BaiYaoduelmode set pvp mapspawn 2 100 64 0
/BaiYaoduelmode set pvp maptime 300
```

## 權限

- `baiyaoduel.use` - 使用決鬥功能（預設：所有玩家）
- `baiyaoduel.admin` - 管理員權限（預設：OP）
- `baiyaoduel.*` - 所有權限（預設：OP）

## 配置說明

### 模式配置

模式配置保存在 `plugins/BaiYaoDuel/modes.yml` 中。

每個模式包含以下設定：
- `players` - 每隊玩家數量（不是總人數）
- `mapName` - 地圖名稱（Multiverse 世界名稱）
- `team1Spawn` - 隊伍 1 重生點
- `team2Spawn` - 隊伍 2 重生點
- `timeLimit` - 時間限制（秒）
- `broadcastFormat` - 廣播格式

### 數據存儲

- 玩家統計：`plugins/BaiYaoDuel/data/<UUID>.yml`
- 對戰歷史：`plugins/BaiYaoDuel/match_history.yml`

## 使用流程

1. **創建模式**（管理員）
   ```
   /BaiYaoduelmode create pvp
   ```

2. **配置模式**（管理員）
   ```
   /BaiYaoduelmode set pvp players 1
   /BaiYaoduelmode set pvp map pvp_arena
   /BaiYaoduelmode set pvp mapspawn 1 0 64 0
   /BaiYaoduelmode set pvp mapspawn 2 100 64 0
   /BaiYaoduelmode set pvp maptime 300
   ```

3. **發起決鬥**（玩家）
   ```
   /duel pvp Steve
   ```

4. **接受邀請**（被邀請的玩家）
   ```
   /duelaccept <發起者id>
   ```

5. **查看統計**（玩家）
   ```
   /BaiYaoduelstats KD
   ```

## 決鬥流程

1. **發起邀請**
   - 玩家 A 使用 `/duel <模式> <玩家B>` 發起邀請
   - 玩家 B 會收到邀請通知

2. **接受/拒絕邀請**
   - 玩家 B 使用 `/duelaccept <玩家A>` 接受邀請
   - 或使用 `/duelno` 拒絕邀請
   - 邀請會在 2 分鐘後自動過期

3. **決鬥開始**
   - 雙方玩家自動傳送到決鬥場地
   - 所有玩家自動恢復滿血、滿飽食度
   - 開始計時（根據模式設定的時間限制）

4. **決鬥結束**
   - 當一方全部死亡時，另一方獲勝
   - 當有玩家斷線時，另一方獲勝
   - 當時間到時，雙方平局
   - 所有玩家自動傳送到重生點（使用 `/spawn` 指令）

5. **結果廣播**
   - 決鬥結果會自動廣播到伺服器
   - 統計數據自動記錄

## 注意事項

1. 確保已安裝並配置 `Multiverse-Core` 插件
2. 地圖必須在 Multiverse 中已創建
3. 重生點座標必須在指定地圖內
4. 決鬥開始後，玩家會自動傳送到指定位置
5. 決鬥開始時，所有玩家會自動恢復滿血和滿飽食度
6. 當一方全部死亡或斷線時，另一方自動獲勝
7. 時間到時，決鬥會自動結束並判定為平局
8. 決鬥結束後，所有玩家會自動傳送到重生點
9. 玩家ID可以使用逗號分隔，例如：`player1,player2`
10. 決鬥邀請會在 2 分鐘後自動過期

## 統計系統

### 記錄的數據
- **擊殺數**：在決鬥中擊殺對手的次數
- **死亡數**：在決鬥中死亡的次數
- **斷線次數**：在決鬥中斷線的次數
- **KD 比率**：擊殺數除以死亡數（如果死亡數為 0，則顯示擊殺數）
- **對戰記錄**：與每個玩家的對戰勝負記錄
- **對戰歷史**：所有參與的決鬥記錄

### 查看統計
- `/BaiYaoduelstats KD` - 查看自己的擊殺死亡統計
- `/BaiYaoduelstats player <玩家id>` - 查看與指定玩家的對戰記錄
- `/BaiYaoduelstats matchhistory [頁碼]` - 查看對戰歷史（每頁 10 條）

## 技術細節

- 使用 Multiverse 的 `mvtp` 指令進行傳送
- 使用 EssentialsX 的 `broadcast` 指令進行廣播（可選）
- 使用 EssentialsX 的 `spawn` 指令傳送玩家到重生點（可選）
- 數據使用 YAML 格式存儲
- 支持多線程安全操作
- 邀請系統自動過期檢查（每秒檢查一次）

## 依賴插件

- **Multiverse-Core**（必需）- 用於世界傳送
- **EssentialsX**（可選）- 用於廣播和重生點傳送功能

## 開發者

BaiYao_610

## 版本

1.0.0

## 更新日誌

### v1.0.0
- 初始版本發布
- 支持決鬥邀請系統
- 支持多人隊伍對戰
- 支持逗號分隔玩家ID
- 自動治療玩家功能
- 自動傳送到重生點功能
- 時間限制平局處理
- 完整的統計系統
