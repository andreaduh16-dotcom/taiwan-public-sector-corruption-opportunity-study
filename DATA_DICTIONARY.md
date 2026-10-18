# 資料字典

三個資料檔都以 UTF-8（含 BOM）儲存。`level` 欄的 case 指案件層級，actor-case 指一個行為人在一個案件中的一列，multi-select 指可複選（以「；」分隔），auxiliary 指輔助變項。

## 使用前須知

- **未記載不等於不存在。**「未記載」只代表事實來源判決沒有寫，不能解讀為機關沒有該制度或行為人沒有該行為。
- **H 組（控制機制）是輔助變項（auxiliary = true）。**不列入主要頻率統計，也不能用來計算「沒有盤點／稽核的案件比例」。每個 H 變項分成「書面規定」和「實際運作」兩欄。
- 犯罪事實欄位（次數、期間、金額、方法等）只就目標罪名的事實編碼；同判決的非目標罪只記在 `co_offenses`。
- 公開版移除了逐欄證據、證據引文、H 組的內容描述欄、研究者備註和疑點欄。
- 自由文字欄位中的自然人姓名已改寫成「行為人」「他人」或職稱。

## data/coding_main_public.csv（42 列）

| 欄位 | 中文名稱 | level | 資料型態 | 允許值／格式 | 缺值意義 | 主要分析變項 |
|---|---|---|---|---|---|---|
| `case_id` | 案件編號（同 case_cluster_id） | case | string | 研究編號，如 C02、N05、K16 | — | 識別 |
| `case_cluster_id` | 案件群編號 | case | string | 同一犯行之各審級合為一群 | — | 識別 |
| `actor_id` | 行為人編號 | actor-case | string | 研究編號，如 K47b；同案多名符合條件者以字尾區分 | — | 識別 |
| `court` | 事實來源判決法院 | actor-case | string | 法院全名 | — | 識別 |
| `judgment_number` | 事實來源判決字號 | actor-case | string | 如「109年度上訴字第522號」 | — | 識別 |
| `fact_source_judgment` | 事實來源判決ID | actor-case | string | 司法院裁判ID格式 | — | 識別 |
| `fact_source_date` | 事實來源判決日 | actor-case | date | YYYY-MM-DD；2017-08-01至2026-07-31 | — | 是（期間判斷） |
| `judgment_level` | 事實來源審級 | actor-case | categorical | 一審／二審／更審／三審 | — | 否 |
| `same_case_other_judgments` | 同案其他審級 | actor-case | text | 審級、日期、結果 | 「判決文字未載」或未查 | 否 |
| `appellate_fact_difference` | 各審級事實差異 | actor-case | text | 「一致」「僅單審」「未比對」或說明 | 未比對＝未讀其他審級全文 | 否 |
| `crime_period_start` | 犯罪期間起日 | actor-case | date/text | YYYY-MM-DD 或 YYYY-MM；只就目標罪事實 | 未記載 | 是 |
| `crime_period_end` | 犯罪期間迄日 | actor-case | date/text | 同上 | 未記載 | 是 |
| `crime_period_basis` | 犯罪期間依據 | actor-case | text | 附表／事實欄／替代依據 | — | 否 |
| `agency` | 任職機關 | actor-case | text | 照判決 | — | 是 |
| `department` | 單位 | actor-case | text | 照判決 | 未記載 | 否 |
| `job_title` | 職稱／職務 | actor-case | text | 照判決 | 未記載 | 是 |
| `personnel_status` | 人事類型 | actor-case | categorical | 約僱／約用／約聘／定期僱用／臨時人員／技術單工（臨時人員契約）／清潔隊員（契約僱用、職工、或官方資料補入）／交通事業人員（業務佐）／駐衛警察（駐在單位僱用） | — | 是 |
| `public_official_type` | 刑法公務員類型 | actor-case | categorical | 任用公務員／授權公務員／委託公務員／unclear | unclear＝判決未引條文或未載類型 | 是 |
| `conviction_status` | 有罪結果 | actor-case | categorical | 有罪／有罪免刑／部分有罪 | — | 是 |
| `target_statute` | 目標罪名 | actor-case | categorical | 貪§4Ⅰ①／貪§6Ⅰ③（§5Ⅰ①在本搜尋範圍內無樣本） | — | 是 |
| `target_offense_subtype` | 目標罪名子類型 | actor-case | text | 如侵占公有財物、竊取公有財物、侵占職務上持有之非公用私有財物 | — | 是 |
| `co_offenses` | 同判決其他罪名 | actor-case | text | 非目標罪（§5Ⅰ②、偽造文書等）；不入機會編碼 | 未見 | 否 |
| `legal_issue_notes` | 法律爭點備註 | actor-case | text | 起訴與判決罪名不同、公務員身分爭點等 | — | 否 |
| `property_type` | 財物種類 | actor-case | categorical | 現金／支票匯款／器材材料（實物）／遺失物／扣押物／其他 | — | 是 |
| `judgment_property_characterization` | 法院對財物之定性 | actor-case | categorical | 公款／公有財物／公用財物／非公用私有財物／其他（照判決用語） | — | 是 |
| `property_origin_nature` | 財物來源（中性描述） | actor-case | text | 如民眾繳納之規費、廠商繳回材料、拾得物、扣押物 | — | 是 |
| `property_description` | 財物描述 | actor-case | text | 照判決 | — | 否 |
| `amount_or_value` | 金額或價值 | actor-case | text | 以判決載明為準，只就目標罪 | 未記載 | 是 |
| `proceeds_amount` | 犯罪所得 | actor-case | text | 以判決載明為準 | 未記載 | 否 |
| `property_access_origin` | 接觸財物的職務來源 | actor-case | text | 判決所載職務內容、法規或職務命令 | 未記載 | 是 |
| `process_position` | 流程位置 | multi-select | multi-select（以「；」分隔） | 收款／提領／保管／出納／登帳／繳庫／清運／財物整理分類／財物移交／發放／受理交存／領用／其他 | — | 是 |
| `process_position_other_detail` | 流程位置「其他」之說明 | actor-case | text | 自由文字 | 空白＝無 | 否 |
| `actual_control_over_property` | 行為時實際控制財物 | actor-case | categorical | 是／否／未記載 | 未記載 | 是 |
| `control_description` | 控制方式描述 | actor-case | text | 照判決 | 未記載 | 否 |
| `offense_method` | 犯罪方法 | actor-case | text | 具體動作，不只寫罪名 | 未記載 | 是 |
| `offense_time_concealment` | 犯行當下之掩飾 | actor-case | text | 偽造收據、作廢收據、挪後補前等 | 未記載＝判決未載 | 是 |
| `post_discovery_behavior` | 被發覺後之行為 | actor-case | text | 僅輔助 | 未記載 | 輔助 |
| `number_of_acts` | 目標罪犯行次數 | actor-case | number/text | 只計目標罪；各審不同者見 acts_consistency | 未載 | 是 |
| `acts_consistency_across_instances` | 次數歷審一致性 | actor-case | categorical | 一致／不一致／未比對／僅單審 | — | 否 |
| `repeated_offense` | 是否多次犯行 | actor-case | categorical | yes／no／unclear | unclear | 是 |
| `offense_duration` | 犯罪持續期間 | actor-case | text | 照判決 | 未記載 | 是 |
| `accomplice_structure` | 共犯結構 | actor-case | text | 單獨（裁判明載）／有共犯（說明）／未見共犯記載 | 「未見共犯記載」≠無共犯 | 是 |
| `continuation_condition` | 犯罪持續條件 | multi-select | text | 僅記裁判有明文支持者 | 未記載＝判決未寫，不得推論為監督不足 | 是 |
| `continuation_evidence_status` | 持續條件證據狀態 | actor-case | categorical | 明確記載／可由裁判事實直接確認／未記載 | — | 否 |
| `discovery_method` | 發現方式 | multi-select | multi-select（以「；」分隔，依先後） | 主管發現／同仁發現／民眾檢舉／內部盤點／對帳發現／稽核／政風查察／警調或廉政調查／自首／其他／未記載 | 未記載＝判決未載如何被發覺 | 是 |
| `discovery_description` | 發現經過描述 | actor-case | text | 照判決 | 未記載 | 否 |
| `discovery_evidence_status` | 發現方式證據狀態 | actor-case | categorical | 明確記載／可由裁判事實直接確認／未記載 | — | 否 |
| `full_text_verification_status` | 全文閱讀程度 | actor-case | categorical | complete／relevant_sections_complete | — | 品質欄位 |
| `collection_vs_booking_separation__formal_rule_in_judgment` | 收款與登帳是否分離：判決所載書面規定 | auxiliary | categorical | 明確記載／可直接確認／未記載 | 未記載＝判決未寫，不等於不存在 | 否（輔助） |
| `collection_vs_booking_separation__actual_operation_in_judgment` | 收款與登帳是否分離：判決認定之實際運作 | auxiliary | categorical | 明確記載／可直接確認／未記載 | 未記載＝判決未寫，不等於不存在 | 否（輔助） |
| `custody_vs_inventory__formal_rule_in_judgment` | 保管與盤點：判決所載書面規定 | auxiliary | categorical | 明確記載／可直接確認／未記載 | 未記載＝判決未寫，不等於不存在 | 否（輔助） |
| `custody_vs_inventory__actual_operation_in_judgment` | 保管與盤點：判決認定之實際運作 | auxiliary | categorical | 明確記載／可直接確認／未記載 | 未記載＝判決未寫，不等於不存在 | 否（輔助） |
| `reconciliation__formal_rule_in_judgment` | 對帳：判決所載書面規定 | auxiliary | categorical | 明確記載／可直接確認／未記載 | 未記載＝判決未寫，不等於不存在 | 否（輔助） |
| `reconciliation__actual_operation_in_judgment` | 對帳：判決認定之實際運作 | auxiliary | categorical | 明確記載／可直接確認／未記載 | 未記載＝判決未寫，不等於不存在 | 否（輔助） |
| `handover_control__formal_rule_in_judgment` | 交接機制：判決所載書面規定 | auxiliary | categorical | 明確記載／可直接確認／未記載 | 未記載＝判決未寫，不等於不存在 | 否（輔助） |
| `handover_control__actual_operation_in_judgment` | 交接機制：判決認定之實際運作 | auxiliary | categorical | 明確記載／可直接確認／未記載 | 未記載＝判決未寫，不等於不存在 | 否（輔助） |
| `supervisor_or_peer_monitoring__formal_rule_in_judgment` | 主管或同儕監控：判決所載書面規定 | auxiliary | categorical | 明確記載／可直接確認／未記載 | 未記載＝判決未寫，不等於不存在 | 否（輔助） |
| `supervisor_or_peer_monitoring__actual_operation_in_judgment` | 主管或同儕監控：判決認定之實際運作 | auxiliary | categorical | 明確記載／可直接確認／未記載 | 未記載＝判決未寫，不等於不存在 | 否（輔助） |
| `audit_or_inspection__formal_rule_in_judgment` | 稽核或查核：判決所載書面規定 | auxiliary | categorical | 明確記載／可直接確認／未記載 | 未記載＝判決未寫，不等於不存在 | 否（輔助） |
| `audit_or_inspection__actual_operation_in_judgment` | 稽核或查核：判決認定之實際運作 | auxiliary | categorical | 明確記載／可直接確認／未記載 | 未記載＝判決未寫，不等於不存在 | 否（輔助） |

## data/final_sample_public.csv（42 列）

| 欄位 | 中文名稱 | level | 資料型態 | 允許值／格式 | 缺值意義 | 主要分析變項 |
|---|---|---|---|---|---|---|
| `actor_id` | 行為人編號 | actor-case | string | 研究編號，如 K47b；同案多名符合條件者以字尾區分 | — | 識別 |
| `case_cluster_id` | 案件群編號 | case | string | 同一犯行之各審級合為一群 | — | 識別 |
| `court` | 事實來源判決法院 | actor-case | string | 法院全名 | — | 識別 |
| `judgment_number` | 事實來源判決字號 | actor-case | string | 如「109年度上訴字第522號」 | — | 識別 |
| `fact_source_date` | 事實來源判決日 | actor-case | date | YYYY-MM-DD；2017-08-01至2026-07-31 | — | 是（期間判斷） |
| `target_statute` | 目標罪名 | actor-case | categorical | 貪§4Ⅰ①／貪§6Ⅰ③（§5Ⅰ①在本搜尋範圍內無樣本） | — | 是 |
| `personnel_status` | 人事類型 | actor-case | categorical | 約僱／約用／約聘／定期僱用／臨時人員／技術單工（臨時人員契約）／清潔隊員（契約僱用、職工、或官方資料補入）／交通事業人員（業務佐）／駐衛警察（駐在單位僱用） | — | 是 |
| `agency` | 任職機關 | actor-case | text | 照判決 | — | 是 |
| `job_title` | 職稱／職務 | actor-case | text | 照判決 | 未記載 | 是 |
| `public_official_type` | 刑法公務員類型 | actor-case | categorical | 任用公務員／授權公務員／委託公務員／unclear | unclear＝判決未引條文或未載類型 | 是 |
| `rank_verification_status` | 官等／層級確認方式 | actor-case | categorical | 非一般官等制／不適用；特殊人事體系比照確認 | — | 是 |
| `equivalent_level_result` | 相當層級 | actor-case | categorical | 相當委任（C01）；非官等制（K89）；其餘空白 | 空白＝不適用 | 否 |
| `full_text_verification_status` | 全文閱讀程度 | actor-case | categorical | complete／relevant_sections_complete | — | 品質欄位 |
| `screening_basis` | 納入依據（簡化） | actor-case | text | 統一格式的說明 | — | 否 |

## data/intercoder_check_public.csv（81 列）

3 個 actor（C03、K42、K89）的比對，共 27 個變項。agreement 欄是逐字相同與否，包含自由文字欄位，因此不能直接當作一致率。類別判斷的比較結果（45 格中 40 格一致）見 `docs/coding_quality_control.md`。

| 欄位 | 中文名稱 | level | 資料型態 | 允許值／格式 | 缺值意義 | 主要分析變項 |
|---|---|---|---|---|---|---|
| `actor_id` | 行為人編號 | actor-case | string | 研究編號，如 K47b；同案多名符合條件者以字尾區分 | — | 識別 |
| `variable` | 比對的變項 | — | string | codebook 變項名 | — | — |
| `coder_A_value` | 編碼者A的值（覆核修正後的最終值） | — | text | — | — | — |
| `coder_B_value` | 編碼者B（獨立第二編碼者）的值 | — | text | — | — | — |
| `agreement` | 是否一致 | — | categorical | yes／no（文字欄位完全相同才算 yes） | — | — |
| `resolution` | 處理方式 | — | text | 以正式編碼值為準；K42部分欄位採編碼者B之目標罪事實範圍 | — | — |
