# translate-tw-doc

將台灣政府文件（中文）翻譯成英文，並自動產生格式化的 Word (.docx) 檔案，供美國移民申請（I-485）使用。

## 支援文件類型

| 文件 | 中文名稱 |
|------|----------|
| 出生證明書 | 出生證明書 |
| 替代役退役證明書（補發） | 替代役退役證明書補發證明 |

## 功能

- 依照原始文件格式，產生帶有表格、色彩標示的英文翻譯 Word 檔
- 自動附上 Certification by Translator 頁
- 可選擇性附加原始文件掃描圖片頁

## 使用方式

### 環境需求

- [uv](https://github.com/astral-sh/uv)（Python 套件管理）
- python-docx
- pymupdf（僅在需要將 PDF 轉成圖片時使用）

### 步驟

1. 複製 `scripts/` 資料夾中對應的腳本
2. 填入翻譯後的資料（見下方 Data Dict 格式）
3. 執行腳本

```bash
uv run --link-mode=copy --with python-docx --with pymupdf build_birth_cert.py
uv run --link-mode=copy --with python-docx --with pymupdf build_military.py
```

> **注意：** 若工作目錄位於 OneDrive，請加上 `--link-mode=copy` 參數。執行前請先關閉已開啟的 Word 檔案，以免 PermissionError。

---

## 出生證明書（build_birth_cert.py）

### Data Dict 格式

```python
person_birth = dict(
    father_name            = "WANG, CHIH-MING",
    father_dob             = "March 10, 1975 (ROC Year 64)",
    father_id              = "A123456789",
    father_occ             = "N/A",
    father_place_of_origin = "Taipei City, Taiwan Province",
    mother_name            = "LIN, CHUN-CHIAO",
    mother_dob             = "July 22, 1977 (ROC Year 66)",
    mother_id              = "B223456789",
    mother_occ             = "N/A",
    mother_place_of_origin = "Kaohsiung City, Taiwan Province",
    household_reg          = "No. 100, Da'an Road, Da'an District, Taipei City",
    child_id               = "A234567890",
    # child_name           = "WANG, XIAO-MING  [Illegible seal]",  # 若文件上有新生兒姓名
    sex                    = "Male",     # "Male" 或 "Female"
    dob_time               = "April 15, 2000 (ROC Year 89), at 10:30 AM",
    total_live_births      = "1",
    gestational_age        = "39 weeks (full term)",
    birth_weight           = "3,200 g",
    birth_type_num         = 1,          # 1=單胎 2=雙胞胎 3=三胞胎 4=其他多胞胎
    birth_male_count       = 1,
    birth_female_count     = 0,
    # birth_female_selected= False,      # 女嬰單胎時設為 False（避免勾選）
    # birth_female_display = "__",       # 女嬰單胎時顯示 "__"
    birth_order_in_multiple= None,
    birth_location_type_num= 1,          # 1=醫院 2=診所 3=助產院 4=家中 5=其他
    birth_location_addr    = "No. 7, Da'an Road, Da'an District, Taipei City",
    delivery_by_num        = 1,          # 1=醫師 2=助產士 3=其他
    # delivery_seal_note   = "[Illegible seal]",
    special_conditions     = "Normal",
    physician_name         = "LIN, YI-SHENG",
    med_cert_no            = "Yi-Zi No. 001234",
    hospital_name          = "Example Hospital",
    med_license_no         = "Example-Health-Yi No. 0001",
    med_facility_addr      = "No. 7, Da'an Road, Da'an District, Taipei City",
    physician_date         = "April 17, 2000 (ROC Year 89)",
    p2_cert_statement      = "This photocopy corresponds to the original document.",
    p2_office              = "Example Household Registration Office, Taipei City",
    p2_director            = "ZHANG, ZHU-REN  [Official seal]",
    p2_date                = "January 1, 2026 (ROC Year 115)",
    p2_serial              = "Example District Household Transcript No. (A) 000001",
)
```

---

## 替代役退役證明書（build_military.py）

### Data Dict 格式

```python
person_military = dict(
    name           = "WANG, DA-MING",
    dob            = "April 15, 2000 (ROC Year 89)",
    id_no          = "A123456789",
    enlistment     = "July 1, 2022 (ROC Year 111)",
    discharge      = "Effective from midnight, June 30, 2023 (ROC Year 112)",
    service_type   = "Educational Service Corps",
    service_unit   = "Non-supervisory Cadre",
    discharge_cert = "No. 11200001",
    reason         = "Completion of Service Term",
    official       = "ZHANG, ZHU-REN",
    issue_date     = "January 1, 2026 (ROC Year 115)",
    ref_no         = "Example-District-Yi No. 001 (Military Service)",
)
```

---

## 翻譯規則

### 姓名羅馬拼音（Wade-Giles，全大寫，姓在前）

格式：`SURNAME, GIVEN-NAME`（例：王大明 → `WANG, DA-MING`）

### 民國年換算

公式：**民國年 = 西元年 - 1911**

格式：`Month DD, YYYY (ROC Year NN)`

### 地址格式

```
No. [號], [路/街] Road/Street, [里/村] Village, [鎮/鄉/市] Township/City, [縣] County, Taiwan Province
```

### 印章標記

| 情況 | 標記 |
|------|------|
| 清晰官方印章 | `[Official seal]` |
| 清晰診所印章 | `[Clinic seal]` |
| 模糊不清 | `[Illegible seal]` |

---

## Claude Code Skill

本專案同時作為 [Claude Code](https://claude.ai/code) 的 skill，放置於專案的 `.claude/skills/translate-tw-doc/` 目錄下，可透過 `/translate-tw-doc` 指令呼叫，引導使用者逐步完成翻譯與文件產生。

## License

MIT
