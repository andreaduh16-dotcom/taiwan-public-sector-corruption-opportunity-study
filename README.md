# 基層公務體系人員職務接觸財物型貪瀆案件之犯罪機會與廉政法遵風險

**Opportunity Structures and Integrity-Compliance Risks in Property-Related Corruption Cases Involving Frontline Public-Sector Personnel in Taiwan: A Judgment Content Analysis**

論文題目全名：基層公務體系人員職務接觸財物型貪瀆案件之犯罪機會與廉政法遵風險——以官方起訴統計與裁判內容分析為中心。本 repository 只含裁判內容分析的公開資料，不含官方起訴統計部分。

License: Not yet specified.

## Overview

基層公務人員因職務接觸公款、規費或各種財物，有時會把這些財物據為己有。這項研究想知道，在法院認定有罪的案件裡，行為人是怎麼接觸到財物的、犯行持續了多久、是怎麼被發現的，以及判決書對相關的內部控制有哪些記載。研究蒐集 2017 年 8 月至 2026 年 7 月間作成的貪污治罪條例第4條第1項第1款、第5條第1項第1款、第6條第1項第3款有罪判決，依事先訂定的 codebook 逐案編碼。判決書是法院為了認定犯罪事實而寫的文件，不是機關內控的調查報告，所以這份資料描述的是「判決怎麼寫」，而不是「機關實際怎麼運作」。

## Dataset

| 項目 | 數量 |
|---|---|
| 案件 | 39 件 |
| actor-case 觀察值 | 42 列 |
| §4Ⅰ①（侵占、竊取公用或公有財物） | 26 件案件（27 個 actor-case） |
| §6Ⅰ③（侵占職務上持有之非公用私有財物） | 13 件案件（15 個 actor-case） |
| §5Ⅰ①（擅提或截留公款） | 0 件。本搜尋策略及資料庫範圍內未找到符合判決 |

一個案件可有一名以上符合條件的行為人，每名行為人在每個案件中算一列。同一人若在不同案件各有犯行，各算一列。

## Repository Structure

| 檔案 | 說明 |
|---|---|
| `README.md` | 本檔 |
| `CHANGELOG.md` | 版本紀錄 |
| `METHODOLOGY.md` | 研究方法簡述 |
| `DATA_DICTIONARY.md` | 逐欄資料字典 |
| `CODEBOOK.md` | 正式編碼用的 codebook v2.2 |
| `REPRODUCIBILITY.md` | 如何核對資料完整性 |
| `SHA256SUMS.txt` | 各檔 SHA-256 雜湊值 |
| `data/coding_main_public.csv` | 42 列編碼資料（去除直接識別資訊） |
| `data/final_sample_public.csv` | 正式樣本清冊與納入依據 |
| `data/intercoder_check_public.csv` | 3 個 actor 的第二編碼者比對（內部一致性檢查） |
| `docs/search_strategy.md` | 搜尋資料來源、搜尋式與限制 |
| `docs/coding_quality_control.md` | 編碼品質控制流程與結果 |
| `docs/known_limitations.md` | 已知限制 |

研究者另保存逐欄證據長表，可供課程教師或研究審查時進一步核對，但不納入公開資料集。落選的候選案件名冊、排除案件清單與工作紀錄同樣不公開。

## Method

簡述見 [METHODOLOGY.md](METHODOLOGY.md)。重點：以事實來源判決的裁判日界定研究期間；以人事身分操作化「基層」，人事層級無法由判決或官方資料確認者排除；只依判決文字編碼，不推測；每個重要欄位附證據狀態。

`data/intercoder_check_public.csv` 只是 3 個 actor 的內部一致性檢查，不構成正式的 intercoder reliability estimate。

## Data Privacy

- 自然人姓名已從公開研究資料移除，自由文字中的人名改寫為「行為人」「他人」或職稱。
- 資料保留法院、裁判字號、裁判日期和機關名稱，以維持法律研究的可驗證性。
- 因此，這份資料是去除直接識別資訊的研究資料，不是不可逆的匿名資料。資料移除直接姓名，但保留公開裁判案號以維持研究可驗證性，因此本資料並非完全不可再識別之匿名資料。
- 使用者如重新連結公開裁判，仍可能辨識個案。
- 請勿利用本資料對個別行為人進行與研究目的無關的追蹤、騷擾或再識別。

## Interpretation Warning

- 樣本不代表犯罪率，也不能推論母體中各類案件的比例。
- CLASS_UNVERIFIABLE 的排除不代表該人不是基層，只代表公開資料不足以完成分類。
- 「未記載」不等於不存在。H 組各欄是輔助變項，不能拿來計算沒有盤點、稽核或對帳的案件比例。
- 資料不能用來推論某種流程造成犯罪，也沒有未犯罪的對照組。
- 不應把資料解讀為約僱、臨時或清潔人員的犯罪傾向較高。樣本偏向非官等制人員，部分原因是官等制與特殊人事體系的案件多半無法確認層級。

## AI-assisted research disclosure

研究過程中使用 AI 工具協助裁判搜尋、初步篩選、編碼、證據整理和品質檢查。AI 的判讀可能有誤，研究以 codebook、證據表、抽查和覆核程序降低風險，細節與限制見 `docs/coding_quality_control.md` 和 `docs/known_limitations.md`。研究問題、方法、納入規則、爭議案件的認定和最終分析由研究者控制。AI 工具不是作者。

## Citation

目前沒有正式發表資訊，也沒有 DOI。建議暫以下列格式引用，作者欄請研究者自行改為正式署名：

> andreaduh16-dotcom. (2026). *taiwan-public-sector-corruption-opportunity-study* (Version 1.0.0) [Data set]. GitHub. https://github.com/andreaduh16-dotcom/taiwan-public-sector-corruption-opportunity-study
