# Codebook（v2.2）

> 正式編碼使用版本。
>
> 通則：(1) 只依裁判文字編碼，不推測；(2)「未記載」≠「不存在」；(3) 每個重要欄位附 evidence_status 與證據；(4) 不得以「監督不足」「原因」「高風險」等詞描述任何欄位；(5) 不得新增分析性解釋。

## 版本說明

本檔是正式編碼使用的 codebook v2.2，由前測版本（v1）經 v2、v2.1 修訂而成。與正式編碼有關的幾項說明如下。

1. 目標罪名的判斷以「主要事實編碼裁判所認定之目標罪名」為準；如已知後續審級撤銷或變更者，依後續維持之事實與法律認定處理。
2. 正式樣本為 39 件案件、42 個 actor-case 列。
3. 人事類型的處理：交通事業人員（業務佐）依《交通事業人員與交通行政人員相互轉任資格及年資提敘辦法》第3條記為「相當委任」，不記為「具有委任官等」；駐衛警察依《各機關學校團體駐衛警察設置管理辦法》第7條、第9條記為駐在單位僱用人員，`rank_verification_status` 為「非一般官等制／不適用」；清潔隊員依官方人事或職工工作規則資料確認非官等制後維持納入。
4. 刑法第10條第2項第2款之委託公務員，`public_official_type` 記為「委託公務員」。法院既已明確認定該身分，本研究不重新審查委託關係是否合法成立；判決未載具體委託依據者記入 `legal_issue_notes`。
5. 犯罪事實欄位（`number_of_acts`、`crime_period_*`、`amount_or_value`、`offense_method`、`offense_time_concealment`、`continuation_condition` 等）只就目標罪名的事實編碼；同一判決中的非目標罪（§5Ⅰ②、偽造文書、詐欺等）只記入 `co_offenses`。
6. H 組與發現方式的判準：H 組 actual 層不得以犯罪手法、行為人違反期限或規定的事實、單純職務分工、案發後清查、法院對機關管理的評價性敘述、證據清單的文件名稱作為依據；formal 層須是判決引述或描述的書面規定內容，僅「被告明知」「依規定」而未載內容者至多為「可直接確認」。發現方式記錄案件如何被發覺並依先後排列；「案經…移送偵查起訴」屬移送程序，不是發現方式。

## 1. 編碼單位與期間

- 編碼單位：**actor-case 列**（`actor_id`）。同案群內的每名符合條件之行為人一列；私人共犯與不符 actor 條件者（如 K127b）不編碼。
- 事實來源：`fact_source_judgment`（見 `data/final_sample_public.csv`）。期間以其裁判日判斷（2017-08-01～2026-07-31），不改用其他日期。各審級事實差異記 `appellate_fact_difference`，不換事實來源。

## 2. 通用證據狀態

| 值 | 定義 | 正例 | 反例 |
|---|---|---|---|
| 明確記載 | 裁判文字直接寫明 | 事實欄「共計侵占79萬575元」→ amount_or_value | 依薪資推測犯罪所得 |
| 可由裁判事實直接確認 | 未用本研究變項名稱，但依所載事實只能得到單一結論，無須額外假設 | 同一帳號收款並作廢收據→「收款與登帳未分離」 | 因犯罪持續兩年而認「無人盤點」 |
| 未記載 | 資料不足或需推測 | 全文未提盤點→custody_vs_inventory | 編成「無盤點制度」 |

不確定時：一律填「未記載」，`evidence_quote` 填「(無)」、`evidence_location` 填「全文未見」。

## 3. A 組：案件與行為人基本資料

| 變項 | 定義／允許值 | 規則 |
|---|---|---|
| case_id, case_cluster_id, actor_id | 沿用樣本清冊之研究編號 | actor_id 如 K47b、K90a |
| fact_source_judgment, fact_source_date, court, judgment_number, judgment_level | 文字；一審／二審／更審／三審 | 沿用清冊 |
| same_case_other_judgments | 同案其他審級與結果 | 文字 |
| appellate_fact_difference | 各審級事實／罪數／金額／論罪差異 | 「一致」「僅單審」「未比對」或說明 |
| crime_period_start / end | YYYY-MM-DD 或 YYYY-MM | 取裁判認定；只用於描述 |
| crime_period_basis | 附表／事實欄／替代依據 | 以替代依據者註明 |
| agency / department / job_title | 照判決 | 遮蔽者註明 |
| personnel_status | 見樣本清冊之 personnel_status | 照清冊，不得改寫為「委任」 |
| public_official_type | 任用公務員／授權公務員／委託公務員／unclear | K112 固定為委託公務員 |
| supervisor_status | 非主管（清冊已確認） | 不重新判斷；如全文出現與清冊相反之新事實，於 coding_notes 標示，不自行改樣本 |
| conviction_status | 有罪／有罪免刑／部分有罪 | 以事實來源判決為準 |

## 4. B 組：法律資料

| 變項 | 定義／允許值 | 規則 |
|---|---|---|
| target_statute | 貪§4Ⅰ①／貪§5Ⅰ①／貪§6Ⅰ③ | **以主要事實編碼裁判所認定之目標罪名為準（非起訴法條）；如已知後續審級撤銷或變更者，依後續維持之事實與法律認定處理** |
| target_offense_subtype | 如「侵占公有財物」「竊取公有財物」「侵占職務上持有非公用私有財物」 | 照判決 |
| co_offenses | 同案其他罪名（偽造文書、§5Ⅰ②等） | 不入機會編碼；無→「未見」 |
| legal_issue_notes | 爭點備註（僅記法律爭點，不作制度評論） | 起訴與判決罪名不同、公務員身分爭點等 |

## 5. C 組：財物資料

| 變項 | 定義／允許值 | 規則 |
|---|---|---|
| property_type | 現金／支票匯款／器材材料（實物）／遺失物／扣押物／其他 | 照事實 |
| judgment_property_characterization | **法院**對財物之法律定性：公款／公有財物／公用財物／非公用私有財物／其他 | 照判決用語，不得自行改定性；各審不同記 appellate_fact_difference |
| property_origin_nature | 財物來源之**中性描述** | 如民眾繳納之規費、廠商繳回材料、拾得物、扣押物、押標金；**不依罪名分類** |
| property_description, amount_or_value, proceeds_amount | 文字／數字 | 以裁判載明為準；多筆不自行加總，除非註明「本研究加總」 |
| property_access_origin | 財物為何因職務進入行為人可接近／持有／保管／處理範圍 | 引職務內容、法規或職務命令；不做理論推論 |

## 6. D 組：職務與流程位置

`process_position`（可複選）：收款／提領／保管／出納／登帳／繳庫／清運／財物整理分類／財物移交／**發放**／**受理交存**／**領用**（領取或配發供職務使用之物）／其他（竊取類寫明「竊取自他人持有處」等）。已刪除：審核、核銷、驗收、申報／核銷型。
`actual_control_over_property`：是／否／未記載。`control_description`：文字。

## 7. E 組：犯罪實行

| 變項 | 規則 |
|---|---|
| offense_method | 行為人如何利用其接近／控制位置完成犯罪；**禁止只寫罪名**，寫具體動作 |
| offense_time_concealment | 犯行當下之掩飾（偽造收據、作廢收據、挪後補前、改帳、偽造簽名等）；裁判未載→未記載 |
| post_discovery_behavior | 被發覺後之行為（返還、勸說配合等）；**僅輔助**，不入主要分析 |
| number_of_acts | 數字或「未載」 |
| acts_consistency_across_instances | 一致／不一致／未比對／僅單審；附備註 |
| repeated_offense | yes／no／unclear |
| offense_duration | 文字 |
| accomplice_structure | 單獨（裁判明載）／有共犯（說明）／未見共犯記載 |

## 8. F 組：犯罪持續條件

`continuation_condition`：僅記**裁判有明文支持**之條件，如：長期單獨保管、收取後未即時入帳、財物缺乏即時盤點、可反覆接觸相同財物、得自行處分或搬運、共犯協助、其他。**不得**因犯罪持續很久而推論「監督不足」；無說明→「未記載」。`continuation_evidence_status`：同通用證據狀態。

## 9. G 組：發現方式

`discovery_method`（可複選，按先後註明）：主管發現／同仁發現／民眾檢舉／內部盤點／對帳發現／稽核／政風查察／警調或廉政調查／自首／其他／未記載。`discovery_description`：文字。`discovery_evidence_status`。

## 10. H 組：控制機制條件式變項——**輔助變項，不列入主要頻率統計**

變項：collection_vs_booking_separation、custody_vs_inventory、reconciliation、handover_control、supervisor_or_peer_monitoring、audit_or_inspection。每個變項兩欄：

- `<變項>__formal_rule_in_judgment`：判決所載之**書面規定**（法規、內規、作業要點）——明確記載／可直接確認／未記載
- `<變項>__actual_operation_in_judgment`：判決所認定之**實際運作**——明確記載／可直接確認／未記載
- 另附 `<變項>__content`（簡述內容與證據）。

硬性規定：「未記載」不得改編為「無」或「不存在」；不得計算或報告「X% 案件沒有盤點／稽核／對帳」；不入主要頻率統計；案發後之清查不算 audit_or_inspection；僅憑職務分工描述不足時填未記載。

## 11. 證據欄（長表）

逐欄證據長表（研究者另保存，未納入公開資料集）：actor_id, variable, value, evidence_status, evidence_quote（最短支持文字）, evidence_source（判決ID）, evidence_location（欄／節／附表）。無直接支持文字者不得硬編碼，值填「未記載」。

## 12. 編碼完整性

`full_text_verification_status`（complete／relevant_sections_complete／partial／summary_only）沿用 v2.1；正式編碼須為 complete 或 relevant_sections_complete。附表過長時，須在 `sections_note` 載明未逐列閱讀之範圍，且該範圍內之欄位（如逐筆日期）填「未記載」或註明依附表代表列／合計。
