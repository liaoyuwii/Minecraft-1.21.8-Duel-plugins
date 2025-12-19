[README.md](https://github.com/user-attachments/files/24265410/README.md)
# BaiYaoDuel 插件

一個用於 Minecraft Paper 1.21.8 的決鬥插件。

## 功能特色

- 支持多種決鬥模式配置
- 自動記錄玩家統計（擊殺、死亡、斷線）
- 對戰歷史記錄
- 自動廣播決鬥結果
- 支持多人隊伍對戰
- 時間限制功能

## 安裝

1. 將編譯後的 `BaiYaoDuel-1.0.0.jar` 放入伺服器的 `plugins` 資料夾
2. 確保已安裝 `Multiverse-Core` 插件（用於世界傳送）
3. 重啟伺服器

編譯後的 JAR 文件將在 `target` 資料夾中。

## 指令說明

### 一般玩家指令

#### `/duel` 或 `/BaiYaoduel`
發起決鬥

**用法：**
```
/duel <模式> <玩家id> [己方其他玩家id...]
/BaiYaoduel <模式> <玩家id> [己方其他玩家id...]
```

**範例：**
```
/duel pvp Steve
/BaiYaoduel team 3v3 Alice Bob Charlie
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
- `players` - 每隊玩家數量
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
   ```

3. **發起決鬥**（玩家）
   ```
   /duel pvp Steve
   ```

4. **查看統計**（玩家）
   ```
   /BaiYaoduelstats KD
   ```

## 注意事項

1. 確保已安裝並配置 `Multiverse-Core` 插件
2. 地圖必須在 Multiverse 中已創建
3. 重生點座標必須在指定地圖內
4. 決鬥開始後，玩家會自動傳送到指定位置
5. 當一方全部死亡或斷線時，另一方自動獲勝
6. 時間到時，決鬥會自動結束（平局）

## 技術細節

- 使用 Multiverse 的 `mvtp` 指令進行傳送
- 使用 EssentialsX 的 `broadcast` 指令進行廣播
- 數據使用 YAML 格式存儲
- 支持多線程安全操作

## 開發者

BaiYao_610

## 版本

1.0.0

