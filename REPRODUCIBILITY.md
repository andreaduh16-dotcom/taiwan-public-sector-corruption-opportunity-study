# 核對與重現

這個 repository 提供的是編碼後的資料，不包含原始搜尋結果和判決全文，所以不能從頭重跑搜尋與編碼。可以做的是核對資料檔是否完整、數字是否和文件一致，以及回到公開裁判查證個案。

## 1. 核對檔案完整性

在 repository 根目錄執行：

```
sha256sum -c SHA256SUMS.txt
```

## 2. 核對樣本數字

```python
import csv
rows = list(csv.DictReader(open("data/coding_main_public.csv", encoding="utf-8-sig")))
assert len(rows) == 42
assert len({r["case_cluster_id"] for r in rows}) == 39
n41 = [r for r in rows if r["target_statute"] == "貪§4Ⅰ①"]
n63 = [r for r in rows if r["target_statute"] == "貪§6Ⅰ③"]
assert len(n41) == 27 and len(n63) == 15
assert len({r["case_cluster_id"] for r in n41}) == 26
assert len({r["case_cluster_id"] for r in n63}) == 13
assert not any("5Ⅰ①" in r["target_statute"] for r in rows)
```

## 3. 回到公開裁判查證

每列保留法院、裁判字號、`fact_source_judgment` 和裁判日期，可以到司法院裁判書系統查詢原判決。編碼值以 `CODEBOOK.md` 的定義為準。逐欄證據長表不公開；需要核對時，請聯繫研究者。

## 4. 搜尋的重現

搜尋式和篩選條件列在 `docs/search_strategy.md`。資料庫內容會隨時間更新，重跑搜尋不一定得到完全相同的結果。
