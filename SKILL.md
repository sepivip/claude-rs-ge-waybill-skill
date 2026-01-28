---
name: rs-waybill
description: Integrate with the Georgian Revenue Service (RS.GE) Electronic Waybill SOAP web service. Use this skill when working with waybill creation, activation, closing, querying, goods management, transporter operations, invoices, templates, bar codes, car numbers, and all RS.GE WayBillService API methods.
---

# RS.GE Electronic Waybill Service Integration

## Service Overview

This skill provides integration with the Georgian Revenue Service (RS.GE) Electronic Waybill (ელექტრონული ზედნადები) SOAP Web Service.

- **WSDL/Endpoint**: `https://services.rs.ge/WayBillService/WayBillService.asmx`
- **Protocol**: SOAP XML Web Service
- **Authentication**: Service user credentials (`su`, `sp`) passed with every method call

### Test Credentials

| Parameter | Account 1 | Account 2 |
|-----------|-----------|-----------|
| Username (su) | `tbilisi` | `satesto2` |
| Password (sp) | `123456` | `123456` |
| TIN (tax ID) | `206322102` | `12345678910` |

## Waybill Types

| ID | Type |
|----|------|
| 1 | Internal transfer (შიდა გადაზიდვა) |
| 2 | Delivery with transportation (მიწოდება ტრანსპორტირებით) |
| 3 | Delivery without transportation (მიწოდება ტრანსპორტირების გარეშე) |
| 4 | Distribution - main waybill with sub-waybills (დისტრიბუცია) |
| 5 | Goods return (საქონლის უკან დაბრუნება) |
| 6 | Sub-waybill (ქვე-ზედნადები) |

## Waybill Statuses

| Code | Status |
|------|--------|
| 0 | Saved (შენახული) |
| 1 | Activated (აქტიური) |
| 2 | Completed/Closed (დასრულებული) |
| 8 | Sent to transporter (გადამზიდავთან გადაგზავნილი) |
| -1 | Deleted (წაშლილი) |
| -2 | Cancelled (გაუქმებული) |

## Workflow

1. **Create/Save** waybill with `save_waybill` (status=0)
2. **Activate** with `send_waybill` or `send_waybill_vd` (status=1, gets unique number)
3. **Close/Complete** with `close_waybill` or `close_waybill_vd` (status=2)
4. Optionally **cancel** with `ref_waybill` or **delete** with `del_waybill`
5. Optionally **create invoice** with `save_invoice`

For transporter flow: seller sends to transporter -> transporter fills fields with `save_waybill_transporter` -> activates with `send_waybill_transporter` -> closes with `close_waybill_transporter`.

## Implementation Guidelines

When implementing RS Waybill integration:

1. **All methods require `su` and `sp`** (service username and password) as the first two parameters.
2. **XML format**: The service uses XML nodes for complex data structures (waybills, goods lists, wood documents).
3. **Date format**: Use ISO format `yyyy-MM-ddTHH:mm:ss` (e.g., `2024-10-07T12:08:47`).
4. **Status filter format**: Comma-delimited with leading/trailing commas for single status: `,1,` for one status, `,1,2,` for multiple.
5. **Error handling**: Check `STATUS` in response XML. Negative values indicate errors. Use `get_error_codes` to get error descriptions.
6. **CATEGORY field**: Empty or `0` = normal, `1` = wood/timber.
7. **IS_MED field**: Empty or `0` = normal, `1` = medication.
8. **CHEK_BUYER_TIN / CHEK_DRIVER_TIN**: `0` = foreigner, `1` = Georgian citizen.
9. **TRAN_COST_PAYER**: `1` = buyer pays, `2` = seller pays.
10. **VAT_TYPE**: `0` = normal, `1` = zero-rated, `2` = non-taxable.

For full API method signatures and XML structures, see the reference document: `references/api-methods.md`
