---
name: translate-tw-doc
description: Translate Taiwan ROC official documents (birth certificate, military discharge) from Chinese to English and generate formatted Word (.docx) files for USCIS immigration filing (I-485).
license: MIT. LICENSE.txt has complete terms.
---

# Taiwan Document Translation & Formatting

Translate a Taiwan ROC official document from Chinese to English and generate a formatted Word (.docx) file for USCIS immigration filing (I-485).

Supported document types:
- **Birth Certificate** (出生證明書)
- **Substitute Military Service Discharge Certificate** (替代役退役證明書)

## Usage

```
/translate-tw-doc <document-type> <person-name>
```

Examples:
```
/translate-tw-doc birth-cert "Yi-Wei Chen"
/translate-tw-doc sms "Yi-Wei Chen"
```

---

## Step 1 — Collect Chinese Content

Ask the user to provide the Chinese text from the document. They can provide fields in **any order**. Guide them with the required fields list below.

### Birth Certificate Required Fields

Tell the user:
> 請提供以下出生證明書的中文內容（順序不限）：

| Field | Chinese Label |
|-------|---------------|
| Father's name | 父親姓名 |
| Father's DOB | 父親出生日期 |
| Father's ID | 父親身分證字號 |
| Father's occupation | 父親職業 |
| Father's place of origin | 父親籍貫 |
| Mother's name | 母親姓名 |
| Mother's DOB | 母親出生日期 |
| Mother's ID | 母親身分證字號 |
| Mother's occupation | 母親職業 |
| Mother's place of origin | 母親籍貫 |
| Household registration address | 戶籍地址 |
| Child's ID | 新生兒身分證字號 |
| Child's name (if on document) | 新生兒姓名 |
| Sex | 性別 |
| Date & time of birth | 出生日期時間 |
| Total live births by mother | 活產總數 |
| Gestational age | 懷孕週數 |
| Birth weight | 出生體重 |
| Birth type (single/twins/etc.) | 胎別 |
| Male/Female count | 男/女人數 |
| Birth location type & address | 出生地點及地址 |
| Delivery attended by | 接生者 |
| Special conditions | 母嬰特殊狀況 |
| Physician/Midwife name | 醫師或助產士姓名 |
| Physician certificate no. | 醫師證書號碼 |
| Hospital/Clinic name | 醫院診所或助產院名稱 |
| Medical facility license no. | 開業執照號碼 |
| Medical facility address | 醫療機構地址 |
| Physician signature date | 醫師簽章日期 |
| **Page 2** — Certification statement | 謄本認證聲明 |
| Issuing office | 發行機關 |
| Director name | 主任姓名 |
| Certification date | 認證日期 |
| Document serial no. | 文件編號 |
| Any seals/stamps visible | 印章註記 |

### Military Record Required Fields

Tell the user:
> 請提供以下替代役退役證明書的中文內容（順序不限）：

| Field | Chinese Label |
|-------|---------------|
| Name | 姓名 |
| Date of birth | 出生日期 |
| National ID no. | 身分證字號 |
| Enlistment date | 入營日期 |
| Discharge date | 退役日期 |
| Service type | 服役類別 |
| Service unit | 服役單位 |
| Discharge certificate no. | 退役證書號碼 |
| Reason for discharge | 退役原因 |
| Authorizing official | 核定人 |
| Issue date | 發證日期 |
| Document reference no. | 文號 |

---

## Step 2 — Translate to English

Apply these translation conventions:

### Name Romanization (Wade-Giles, ALL CAPS, Surname first)

Format: `SURNAME, GIVEN-NAME` (e.g., 陳奕維 → `CHEN, YI-WEI`)

Common surnames:
| Chinese | Romanized |
|---------|-----------|
| 陳 | CHEN |
| 黃 | HUANG |
| 郭 | GUO |
| 蔣 | CHIANG |
| 李 | LEE |
| 吳 | WU |
| 林 | LIN |
| 張 | CHANG |
| 劉 | LIU |
| 王 | WANG |

### ROC Year Conversion

Formula: **ROC Year = Western Year - 1911**

Format: `"Month Day, Year (ROC Year NN)"`
Example: 民國78年10月12日 → `"October 12, 1989 (ROC Year 78)"`

With time: `"October 12, 1989 (ROC Year 78), at 9:58 PM"`

### Address Format

```
No. [號], [路/街] Road/Street, [里/村] Village, [鎮/鄉/市] Township/City, [縣] County, Taiwan Province
```

| Chinese | English |
|---------|---------|
| 鄰 | Neighborhood N |
| 里 / 村 | Village |
| 鎮 / 鄉 | Township |
| 市 | City |
| 縣 | County |
| 省 | Taiwan Province |

### Document-Specific Terms

| Chinese | English |
|---------|---------|
| 單胎 | Single birth |
| 雙胞胎 | Twins |
| 三胞胎 | Triplets |
| 四胞胎以上 | Other multiple births |
| 醫院 | Hospital |
| 診所 | Clinic |
| 助產院(所) | Birthing Center |
| 醫師 | Physician |
| 助產士 | Midwife |
| 替代役 | Substitute Military Service |
| 退役 | Discharge |
| 教育服務役 | Educational Service Corps |

### Seal/Stamp Annotations

| Situation | Annotation |
|-----------|------------|
| Clear official seal | `[Official seal]` |
| Clear clinic/hospital seal | `[Clinic seal]` |
| Illegible seal | `[Illegible seal]` |
| Readable stamp with text | `[Seal: "translated text"]` |

---

## Step 3 — Present Translation for Confirmation

Display the **complete** translated result to the user in a readable table format. Ask:

> 請確認以上翻譯是否正確。確認後我會產生 Python data dict 並執行 script。

Wait for explicit confirmation. Make corrections if needed.

---

## Step 4 — Generate Data Dict

### Birth Certificate Data Dict

```python
person_birth = dict(
    father_name            = "SURNAME, GIVEN-NAME",
    father_dob             = "Month DD, YYYY (ROC Year NN)",
    father_id              = "ID_NUMBER",
    father_occ             = "N/A",
    father_place_of_origin = "County, Taiwan Province",
    mother_name            = "SURNAME, GIVEN-NAME",
    mother_dob             = "Month DD, YYYY (ROC Year NN)",
    mother_id              = "ID_NUMBER",
    mother_occ             = "N/A",
    mother_place_of_origin = "County, Taiwan Province",
    household_reg          = "Full address",
    child_id               = "ID_NUMBER",
    # child_name           = "SURNAME, GIVEN-NAME",  # if visible on document
    sex                    = "Male",
    dob_time               = "Month DD, YYYY (ROC Year NN), at H:MM AM/PM",
    total_live_births      = "N",
    gestational_age        = "NN weeks (full term)",
    birth_weight           = "N,NNN g",
    birth_type_num         = 1,       # 1=Single, 2=Twins, 3=Triplets, 4=Other
    birth_male_count       = 1,
    birth_female_count     = 0,
    # birth_female_selected= False,   # for female single birth
    # birth_female_display = "__",    # for female single birth
    birth_order_in_multiple= None,
    birth_location_type_num= 1,       # 1=Hospital, 2=Clinic, 3=Birthing Center, 4=Home, 5=Other
    birth_location_addr    = "Full address",
    delivery_by_num        = 1,       # 1=Physician, 2=Midwife, 3=Other
    # delivery_seal_note   = "[Illegible seal]",  # if seal visible
    special_conditions     = "Normal",
    physician_name         = "SURNAME, GIVEN-NAME",
    hospital_name          = "Hospital/Clinic Name",
    med_cert_no            = "Certificate number",
    med_license_no         = "License number",
    med_facility_addr      = "Full address",
    physician_date         = "Month DD, YYYY (ROC Year NN)",
    p2_cert_statement      = "This photocopy corresponds to the original document.",
    p2_office              = "Office Name",
    p2_director            = "SURNAME, GIVEN-NAME  [Official seal]",
    p2_date                = "Month DD, YYYY (ROC Year NN)",
    p2_serial              = "Serial number",
)
```

**Checkbox rules:**
- count = 0 → checkbox □, display `__`
- count > 0 → checkbox ■, display count
- Female single birth child: add `birth_female_selected=False, birth_female_display="__"`

### Military Data Dict

```python
person_military = dict(
    name           = "SURNAME, GIVEN-NAME",
    dob            = "Month DD, YYYY (ROC Year NN)",
    id_no          = "ID_NUMBER",
    enlistment     = "Month DD, YYYY (ROC Year NN)",
    discharge      = "Effective from midnight, Month DD, YYYY (ROC Year NN)",
    service_type   = "Service Type",
    service_unit   = "Service Unit",
    discharge_cert = "No. NNNNNNNN",
    reason         = "Completion of Service Term",
    official       = "SURNAME, GIVEN-NAME",
    issue_date     = "Month DD, YYYY (ROC Year NN)",
    ref_no         = "Reference number",
)
```

---

## Step 5 — Write Script & Execute

### For Birth Certificate

Create a Python script (or append to existing `build_translations.py`) that:
1. Imports `build_birth_cert` function from `build_birth_cert.py`
   OR contains the builder inline
2. Calls `build_birth_cert(person_birth)`
3. Calls `add_translator_cert(doc, "Birth Certificate (出生證明書)")`
4. Optionally calls `add_original_pages(doc, [...])` if scans exist
5. Saves the .docx

### For Military Record

Same pattern using `build_military()` from `build_military.py`.

### Run Command

```
uv run --link-mode=copy --with python-docx --with pymupdf <script.py>
```

> Note: `--link-mode=copy` is required for OneDrive directories.
> Close any open Word documents first to avoid PermissionError.

---

## Reference Scripts

Scripts are bundled in this skill's `scripts/` folder:

| Script | Path |
|--------|------|
| Birth cert builder | `${CLAUDE_SKILL_DIR}/scripts/build_birth_cert.py` |
| SMS builder | `${CLAUDE_SKILL_DIR}/scripts/build_military.py` |

Copy the relevant script to your working directory, fill in the data dict, and run it.
