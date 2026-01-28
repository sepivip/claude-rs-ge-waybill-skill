# RS.GE WayBill Service - API Methods Reference

Endpoint: `https://services.rs.ge/WayBillService/WayBillService.asmx`

## Table of Contents

1. [Service User Management](#1-service-user-management)
2. [Reference Data (Catalogs)](#2-reference-data-catalogs)
3. [Waybill CRUD Operations](#3-waybill-crud-operations)
4. [Waybill Listing Methods](#4-waybill-listing-methods)
5. [Waybill Lifecycle (Activate/Close/Delete/Cancel)](#5-waybill-lifecycle)
6. [Transporter Operations](#6-transporter-operations)
7. [Invoice Operations](#7-invoice-operations)
8. [Waybill Templates](#8-waybill-templates)
9. [Bar Code Management](#9-bar-code-management)
10. [Car Number Management](#10-car-number-management)
11. [Helper Functions](#11-helper-functions)

---

## 1. Service User Management

### update_service_user
Update a service user's data.

```csharp
bool update_service_user(string user_name, string user_password, string ip, string name, string su, string sp)
```

**Parameters:**
- `user_name` - declaration user name
- `user_password` - declaration user password
- `ip` - IP address for service access
- `name` - object/shop name
- `su` - service username
- `sp` - service password

**Returns:** `true` if updated, `false` otherwise.

### get_service_users
Get list of service users.

```csharp
XmlNode get_service_users(string user_name, string user_password)
```

**Returns XML:**
```xml
<ServiceUsers>
  <ServiceUser>
    <ID>62</ID>
    <USER_NAME>tbilisi</USER_NAME>
    <UN_ID>731937</UN_ID>
    <IP>12.12.12.12</IP>
    <NAME>test_wb</NAME>
  </ServiceUser>
</ServiceUsers>
```

Fields: `ID` - service user ID, `USER_NAME` - username, `UN_ID` - taxpayer unique number, `IP` - IP address, `NAME` - shop name.

### chek_service_user
Check service user credentials.

```csharp
bool chek_service_user(string su, string sp, out int un_id, out int s_user_id)
```

**Returns:** `un_id` - taxpayer unique number, `s_user_id` - service user ID.

---

## 2. Reference Data (Catalogs)

### get_akciz_codes
Get excise goods codes.

```csharp
XmlNode get_akciz_codes(string su, string sp)
```

**Returns XML:**
```xml
<AKCIZ_CODES>
  <AKCIZ_CODE>
    <ID>224</ID>
    <TITLE>ეთილის სპირტი(2207)_ლიტრი_(2.6)</TITLE>
    <MEASUREMENT>ლიტრი</MEASUREMENT>
    <SAKON_KODI>2207</SAKON_KODI>
    <AKCIS_GANAKV>2.6</AKCIS_GANAKV>
  </AKCIZ_CODE>
</AKCIZ_CODES>
```

### get_waybill_types
Get waybill type catalog.

```csharp
XmlNode get_waybill_types(string su, string sp)
```

**Returns XML:**
```xml
<WAYBILL_TYPES>
  <WAYBILL_TYPE>
    <ID>6</ID>
    <NAME>ქვე-ზედნადები</NAME>
  </WAYBILL_TYPE>
</WAYBILL_TYPES>
```

### get_waybill_units
Get goods measurement units.

```csharp
XmlNode get_waybill_units(string su, string sp)
```

**Returns XML:**
```xml
<WAYBILL_UNITS>
  <WAYBILL_UNIT>
    <ID>1</ID>
    <NAME>ცალი</NAME>
  </WAYBILL_UNIT>
</WAYBILL_UNITS>
```

### get_trans_types
Get transportation types.

```csharp
XmlNode get_trans_types(string su, string sp)
```

**Returns XML:**
```xml
<TRANSPORT_TYPES>
  <TRANSPORT_TYPE>
    <ID>1</ID>
    <NAME>დასახელება</NAME>
  </TRANSPORT_TYPE>
</TRANSPORT_TYPES>
```

### get_wood_types
Get wood/timber types.

```csharp
XmlNode get_wood_types(string su, string sp)
```

**Returns XML:**
```xml
<WOOD_TYPES>
  <WOOD_TYPES>
    <ID>1</ID>
    <NAME>დასახელება</NAME>
    <DESCRIPTION>დასახელება</DESCRIPTION>
  </WOOD_TYPES>
</WOOD_TYPES>
```

---

## 3. Waybill CRUD Operations

### save_waybill
Create or update a waybill.

```csharp
XmlNode save_waybill(string su, string sp, XmlNode waybill)
```

**Input XML structure:**
```xml
<WAYBILL>
  <SUB_WAYBILLS></SUB_WAYBILLS>
  <GOODS_LIST>
    <GOODS>
      <ID>0</ID>                          <!-- 0 = new item -->
      <W_NAME>საქონლის სახელი</W_NAME>    <!-- required: goods name -->
      <UNIT_ID>1</UNIT_ID>                 <!-- required: unit ID -->
      <UNIT_TXT></UNIT_TXT>                <!-- required when UNIT_ID=99 ("other") -->
      <QUANTITY>5</QUANTITY>               <!-- required: quantity -->
      <PRICE>4</PRICE>                     <!-- required: price -->
      <STATUS>1</STATUS>                   <!-- 1=active, -1=delete this item -->
      <AMOUNT>20</AMOUNT>                  <!-- required: total amount -->
      <BAR_CODE>psp101</BAR_CODE>          <!-- required: barcode; for medicine: registration number -->
      <A_ID>0</A_ID>                       <!-- excise ID, 0 if not excise -->
      <VAT_TYPE>1</VAT_TYPE>              <!-- 0=normal, 1=zero-rated, 2=non-taxable -->
      <QUANTITY_EXT>5</QUANTITY_EXT>       <!-- optional: auxiliary quantity -->
      <WOOD_LABEL>asd</WOOD_LABEL>         <!-- wood label number -->
      <W_ID>1</W_ID>                       <!-- wood type ID -->
    </GOODS>
  </GOODS_LIST>
  <WOOD_DOCS_LIST>
    <WOODDOCUMENT>
      <ID>0</ID>
      <DOC_N>ხე1</DOC_N>
      <DOC_DATE>2012-03-09T12:15:21</DOC_DATE>
      <DOC_DESC>ლიცენზია</DOC_DESC>
      <STATUS>1</STATUS>
    </WOODDOCUMENT>
  </WOOD_DOCS_LIST>
  <ID>0</ID>                               <!-- 0 = new waybill -->
  <TYPE>2</TYPE>                            <!-- waybill type (1-6) -->
  <BUYER_TIN>12345678910</BUYER_TIN>       <!-- buyer TIN/personal number -->
  <CHEK_BUYER_TIN>0</CHEK_BUYER_TIN>      <!-- 0=foreigner, 1=Georgian -->
  <BUYER_NAME>გიორგი გიორგაძე</BUYER_NAME>
  <START_ADDRESS>თბილისი</START_ADDRESS>    <!-- transport start address -->
  <END_ADDRESS>თბილისი</END_ADDRESS>        <!-- transport end address -->
  <DRIVER_TIN>1</DRIVER_TIN>               <!-- driver personal number -->
  <CHEK_DRIVER_TIN></CHEK_DRIVER_TIN>     <!-- 0=foreigner, 1=Georgian -->
  <DRIVER_NAME>ირაკლი</DRIVER_NAME>
  <TRANSPORT_COAST>0</TRANSPORT_COAST>     <!-- transport cost -->
  <RECEPTION_INFO></RECEPTION_INFO>         <!-- supplier info -->
  <RECEIVER_INFO></RECEIVER_INFO>           <!-- receiver info -->
  <DELIVERY_DATE></DELIVERY_DATE>           <!-- fill before closing -->
  <STATUS>1</STATUS>                        <!-- 0=saved, 1=activated, 2=completed -->
  <SELER_UN_ID>731937</SELER_UN_ID>        <!-- seller unique number from chek_service_user -->
  <PAR_ID></PAR_ID>                         <!-- parent waybill ID for TYPE=6 -->
  <FULL_AMOUNT>35</FULL_AMOUNT>
  <CAR_NUMBER></CAR_NUMBER>
  <WAYBILL_NUMBER>0000014722</WAYBILL_NUMBER>
  <S_USER_ID>20350</S_USER_ID>
  <BEGIN_DATE>2012-03-09T12:15:21</BEGIN_DATE>  <!-- transport start date -->
  <TRAN_COST_PAYER>1</TRAN_COST_PAYER>    <!-- 1=buyer pays, 2=seller pays -->
  <TRANS_ID>1</TRANS_ID>                    <!-- transport type ID -->
  <TRANS_TXT>სატვირთო მანქანა</TRANS_TXT> <!-- text when TRANS_ID=4 ("other") -->
  <COMMENT></COMMENT>
  <CATEGORY></CATEGORY>                     <!-- empty/0=normal, 1=wood -->
  <IS_MED></IS_MED>                         <!-- empty/0=normal, 1=medicine -->
  <TRANSPORTER_TIN></TRANSPORTER_TIN>      <!-- transporter company TIN -->
</WAYBILL>
```

**Response XML:**
```xml
<RESULT>
  <STATUS>0</STATUS>          <!-- 0=success, negative=error code -->
  <ID>1551</ID>               <!-- waybill ID -->
  <GOODS_LIST>
    <GOODS>
      <ERROR>-1</ERROR>       <!-- error code per goods item -->
      <ID>0</ID>
      <W_NAME>ჩანთა</W_NAME>
      <!-- ... other fields ... -->
    </GOODS>
  </GOODS_LIST>
</RESULT>
```

### get_waybill
Get a single waybill by ID.

```csharp
XmlNode get_waybill(string su, string sp, int waybill_id)
```

**Returns:** Full WAYBILL XML including `SUB_WAYBILLS`, `GOODS_LIST`, `WOOD_DOCS_LIST`, and all waybill fields. Additional read-only fields: `CREATE_DATE`, `ACTIVATE_DATE`, `CLOSE_DATE`, `CUST_STATUS` (1=customs confirmed, 2=rejected), `CUST_NAME`.

---

## 4. Waybill Listing Methods

### get_waybills
Seller's waybill list (seller side).

```csharp
XmlNode get_waybills(string su, string sp, string itypes, string buyer_tin,
    string statuses, string car_number,
    DateTime begin_date_s, DateTime begin_date_e,
    DateTime create_date_s, DateTime create_date_e,
    string driver_tin,
    DateTime delivery_date_s, DateTime delivery_date_e,
    decimal full_amount, string waybill_number,
    DateTime close_date_s, DateTime close_date_e,
    string s_user_ids, string comment)
```

**Parameters:**
- `itypes` - waybill types, comma-separated
- `buyer_tin` - buyer TIN
- `statuses` - statuses comma-separated (e.g., `,1,` or `,1,2,`)
- `car_number` - vehicle number
- `begin_date_s/e` - transport start date range
- `create_date_s/e` - creation date range
- `driver_tin` - driver TIN
- `delivery_date_s/e` - delivery date range
- `full_amount` - total amount
- `waybill_number` - waybill number
- `close_date_s/e` - close date range
- `s_user_ids` - service user IDs, comma-separated
- `comment` - comment filter

**Returns:** `<WAYBILL_LIST>` with `<WAYBILL>` elements containing summary fields plus `BUYER_ST` (0=no status, 1=micro business, 2=small business).

### get_buyer_waybills
Buyer's waybill list (buyer side). Same parameters as `get_waybills` but with `seller_tin` instead of `buyer_tin`.

**Returns:** Same structure plus `SELLER_NAME`, `SELLER_TIN`, `SELLER_ST`.

### get_waybills_ex
Seller's waybill list with confirmation filter. Same as `get_waybills` plus:
- `is_confirmed` - `0`=unconfirmed, `1`=confirmed, `-1`=rejected

**Returns:** Same structure plus `is_confirmed` field.

### get_buyer_waybills_ex
Buyer's waybill list with confirmation filter. Same as `get_buyer_waybills` plus:
- `is_confirmed` - `0`=unconfirmed, `1`=confirmed, `-1`=rejected

**Returns:** Same structure plus `IS_CONFIRMED` field.

### get_waybills_v1
Get waybills by last update date range. Returns both bought and sold waybills for the authorized user with a specific company.

```csharp
XmlNode get_waybills_v1(string su, string sp, string? buyer_tin,
    DateTime last_update_date_s, DateTime last_update_date_e)
```

**Parameters:**
- `buyer_tin` - company TIN
- `last_update_date_s` - range start (e.g., `2024-10-07T12:08:47`)
- `last_update_date_e` - range end (e.g., `2024-10-07T14:34:49`)

**Constraints:** Maximum time range is 3 days.

---

## 5. Waybill Lifecycle

### send_waybill
Activate a waybill (start transportation).

```csharp
string send_waybill(string su, string sp, int waybill_id)
```
**Returns:** Waybill number (string).

### send_waybill_vd
Activate waybill with specific transport start date.

```csharp
string send_waybill_vd(string su, string sp, DateTime begin_date, int waybill_id)
```
**Returns:** Waybill number.

### confirm_waybill
Confirm a waybill.

```csharp
bool confirm_waybill(string su, string sp, int waybill_id)
```
**Returns:** `true` if confirmed, `false` otherwise.

### close_waybill
Close/complete a waybill.

```csharp
int close_waybill(string su, string sp, int waybill_id)
```
**Returns:** `1`=closed, `-1`=no, `-101`=not your waybill, `-100`=invalid credentials.

### close_waybill_vd
Close waybill with delivery date.

```csharp
int close_waybill_vd(string su, string sp, DateTime delivery_date, int waybill_id)
```
**Returns:** Same as `close_waybill`.

### del_waybill
Delete a waybill (only saved/non-activated).

```csharp
int del_waybill(string su, string sp, int waybill_id)
```
**Returns:** `1`=deleted, `-1`=no, `-101`=not yours, `-100`=invalid credentials.

### ref_waybill
Cancel an activated waybill.

```csharp
int ref_waybill(string su, string sp, int waybill_id)
```
**Returns:** `1`=cancelled, `-1`=no, `-101`=not yours, `-100`=invalid credentials.

### reject_waybill
Buyer rejects a waybill.

```csharp
bool reject_waybill(string su, string sp, int waybill_id)
```

---

## 6. Transporter Operations

### save_waybill_transporter
Transporter saves their fields on a waybill.

```csharp
int save_waybill_transporter(string su, string sp, int waybill_id,
    string car_number, string driver_tin, int chek_driver_tin,
    string driver_name, int trans_id, string trans_txt,
    string reception_info, string receiver_info)
```

### send_waybill_transporter
Transporter activates a waybill.

```csharp
int send_waybill_transporter(string su, string sp, int waybill_id,
    DateTime begin_date, out string waybill_number)
```

### close_waybill_transporter
Transporter closes a waybill.

```csharp
int close_waybill_transporter(string su, string sp, int waybill_id,
    string reception_info, string receiver_info, DateTime delivery_date)
```

**Note:** To send a waybill to a transporter, use `save_waybill` with the `TRANSPORTER_TIN` field set to the transporter company's TIN.

---

## 7. Invoice Operations

### save_invoice
Create a tax invoice from a waybill.

```csharp
int save_invoice(string su, string sp, int waybill_id, int in_inv_id, out int out_inv_id)
```

**Parameters:**
- `waybill_id` - waybill ID
- `in_inv_id` - existing invoice ID or `0` for new

**Returns:** `1`=created, `-1`=no, `-101`=not your waybill, `-100`=invalid credentials. `out_inv_id` = invoice number.

---

## 8. Waybill Templates

### save_waybill_tamplate
Save a waybill template.

```csharp
int save_waybill_tamplate(string su, string sp, string v_name, XmlNode waybill)
```

### get_waybill_tamplates
Get list of saved templates (names and IDs).

```csharp
int get_waybill_tamplates(string su, string sp, out XmlNode waybill_tamplates)
```

### get_waybill_tamplate
Get a specific template by ID.

```csharp
int get_waybill_tamplate(string su, string sp, int id, out XmlNode waybill_tamplate)
```

### delete_waybill_tamplate
Delete a template.

```csharp
int delete_waybill_tamplate(string su, string sp, int id)
```

**All return:** `1`=success, `-1`=fail, `-100`=invalid credentials.

---

## 9. Bar Code Management

### save_bar_code
Save a product barcode entry.

```csharp
int save_bar_code(string su, string sp, string bar_code, string goods_name,
    int unit_id, string unit_txt, int a_id)
```

### delete_bar_code
Delete a barcode entry.

```csharp
int delete_bar_code(string su, string sp, string bar_code)
```

### get_bar_codes
Get barcode list (filter by barcode).

```csharp
int get_bar_codes(string su, string sp, string bar_code, out XmlNode bar_codes)
```

**Returns XML fields:** `bar_code`, `goods_name`, `unit_id`, `unit_txt`, `a_id`.

---

## 10. Car Number Management

### save_car_numbers
Register a car number for distribution.

```csharp
int save_car_numbers(string su, string sp, string car_number)
```

### delete_car_numbers
Delete a registered car number.

```csharp
int delete_car_numbers(string su, string sp, string car_number)
```

### get_car_numbers
Get list of registered car numbers.

```csharp
int get_car_numbers(string su, string sp, XmlNode car_numbers)
```

---

## 11. Helper Functions

### get_name_from_tin
Get entity name by TIN or personal number.

```csharp
string get_name_from_tin(string su, string sp, string tin)
```
**Returns:** Buyer/entity name.

### get_error_codes
Get all error codes and descriptions.

```csharp
XmlNode get_error_codes(string su, string sp)
```

**Returns XML:**
```xml
<WAYBILL_TYPES>
  <WAYBILL_TYPE>
    <ID>-1000</ID>
    <TEXT>error description</TEXT>
    <TYPE>1</TYPE>   <!-- 1=waybill errors, 2=goods errors, 3=invoice errors -->
  </WAYBILL_TYPE>
</WAYBILL_TYPES>
```
