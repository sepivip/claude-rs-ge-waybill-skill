# RS.GE Waybill Skill

A [Claude Code](https://claude.com/claude-code) skill for integrating with the **Georgian Revenue Service (RS.GE) Electronic Waybill** SOAP Web Service.

## What it does

This skill provides Claude Code with full knowledge of the RS.GE WayBillService API, enabling it to help you:

- Create, activate, close, cancel, and delete electronic waybills
- Query seller and buyer waybill registries with filters
- Manage goods lists, barcodes, and measurement units
- Handle transporter operations (save, activate, close)
- Create tax invoices from waybills
- Work with waybill templates
- Manage distribution car numbers
- Look up entities by TIN/personal number

## API Coverage

All 35+ methods of the WayBillService are documented, including:

| Category | Methods |
|----------|---------|
| User Management | `update_service_user`, `get_service_users`, `chek_service_user` |
| Reference Data | `get_akciz_codes`, `get_waybill_types`, `get_waybill_units`, `get_trans_types`, `get_wood_types` |
| Waybill CRUD | `save_waybill`, `get_waybill` |
| Waybill Lists | `get_waybills`, `get_buyer_waybills`, `get_waybills_ex`, `get_buyer_waybills_ex`, `get_waybills_v1` |
| Lifecycle | `send_waybill`, `send_waybill_vd`, `close_waybill`, `close_waybill_vd`, `del_waybill`, `ref_waybill`, `confirm_waybill`, `reject_waybill` |
| Transporter | `save_waybill_transporter`, `send_waybill_transporter`, `close_waybill_transporter` |
| Invoices | `save_invoice` |
| Templates | `save_waybill_tamplate`, `get_waybill_tamplates`, `get_waybill_tamplate`, `delete_waybill_tamplate` |
| Barcodes | `save_bar_code`, `delete_bar_code`, `get_bar_codes` |
| Car Numbers | `save_car_numbers`, `delete_car_numbers`, `get_car_numbers` |
| Helpers | `get_name_from_tin`, `get_error_codes` |

## Installation

```bash
npx skills add https://github.com/sepivip/claude-rs-ge-waybill-skill --skill rs-waybill
```

## Service Endpoint

```
https://services.rs.ge/WayBillService/WayBillService.asmx
```

## Test Credentials

| Parameter | Account 1 | Account 2 |
|-----------|-----------|-----------|
| Username | `tbilisi` | `satesto2` |
| Password | `123456` | `123456` |
| TIN | `206322102` | `12345678910` |

## Waybill Types

| ID | Type |
|----|------|
| 1 | Internal transfer |
| 2 | Delivery with transportation |
| 3 | Delivery without transportation |
| 4 | Distribution (main + sub-waybills) |
| 5 | Goods return |
| 6 | Sub-waybill |

## License

Based on the official RS.GE WayBillService documentation (v4.0.4) published by the Information Technology Center of the Revenue Service of Georgia, 2012-2023.
