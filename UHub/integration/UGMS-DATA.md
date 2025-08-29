## GiftCode
| No. | FieldId | Description | DataType | Mandatory |
| --- | --- | --- | --- | --- |
|  | Gift Code |  | string | x |
|  | Gift Name |  | string | x |
|  | Gift Type |  | string | x |
|  | Standard Policy (%) (%) |  | num |  |
|  | Packing Per Case |  | string | x |
|  | UOM |  | string | x |
|  | Color |  | string |  |
|  | Grossweight per case (kg) |  | string | x |
|  | Size (LxWxH)cm |  | num | x |
|  | Description |  | string |  |
|  | Netweight per pc (g) |  | string | x |
|  | Dimension per case (LxWxH)cm |  | num | x |
|  | Gift group |  | string | x |
|  | Inactive |  | bool |  |
|  | Vat |  | num | x |
|  | Pallet |  | num |  |
|  | Expiry Warning Time |  | num |  |
|  | Auto Move To Damage Time |  | num |  |

## SchemeInfo

### header

| No. | FieldId | Description | DataType | Mandatory |
| --- | --- | --- | --- | --- |
|  | PO No. |  | string |  |
|  | Dummy |  | string |  |
|  | Scheme ID |  | string | x |
|  | Scheme Name |  | string | x |
|  | Start Date |  | date | x |
|  | End Date |  | date | x |
|  | Budget owner email |  | string | x |
|  | Requester email |  | string | x |
|  | Planner email |  | string | x |
|  | Vô hiệu kế hoạch giao hàng |  | bool |  |
|  | GRN |  | string |  |
|  | INV |  | string |  |
|  | Category |  | string | x |
|  | Channel |  | string | x |
|  | Mechanic |  | string |  |

### Danh sách các gift của scheme
| No. | FieldId | Description | DataType | Mandatory |
| --- | --- | --- | --- | --- |
|  | Gift Code |  | string | x |
|  | Gift Name |  | string | x |
|  | Gift Type |  | string |  |
|  | Supplier |  | string | x |
|  | Brand |  | string | x |
|  | QTY |  | num | x |
|  | Unit price |  | num | x |

## Customer

| No. | FieldId | Description | DataType | Mandatory |
| --- | --- | --- | --- | --- |
|  | Customer ID |  | string | x |
|  | Customer Name |  | string | x |
|  | Ship To Code |  | string | x |
|  | Ship To Name |  | string | x |
|  | Province |  | string | x |
|  | Region |  | string | x |
|  | Sale Chanel |  | string | x |
|  | Customer Address |  | string |  |
|  | Contact Point Address |  | string |  |
|  | Contact Point Phone Number |  | string |  |
|  | Contact Point Email |  | string | x |
|  | K/A |  | string |  |
|  | Receiver's Name |  | string |  |
|  | Receiver's Phone Number |  | string |  |
|  | Sales Supervisor's Name |  | string |  |
|  | Sales Supervisor's Phone Number |  | string |  |
|  | Contract Date |  | date |  |
|  | Close |  | bool |  |
|  | Priority |  | num | x |
|  | Customer Type |  | string | x |

## Customer-GiaoDichNhan
### header
| No. | FieldId | Description | DataType | Mandatory |
| --- | --- | --- | --- | --- |
|  | Ship To Code |  | string | x |
|  | Rec data |  | date | x |
|  | scheme id |  | string | x |
|  | Trans code |  | string | x |


### detail
| No. | FieldId | Description | DataType | Mandatory |
| --- | --- | --- | --- | --- |
|  | item code |  | string | x |
|  | quantity |  | num | x |
|  | expire data |  | date |  |

## UTOP-DieuChinhKho

| là api mà utop sẽ gửi lại số lượng hàng cần điều chỉnh |
| --- |
| ví dụ:<br>giảm số lượng hàng bị hư hại tại 1 store nào đó |

### header
| No. | FieldId | Description | DataType | Mandatory |
| --- | --- | --- | --- | --- |
|  | shiptocode | nơi cần điều chỉnh số lượng hàng bị hư hại | string | x |
|  | transaction_date | ngày điều chỉnh | date | x |
|  | schemeid | mã chương trình scheme | string | x |
|  | number_no | số phiếu điều chỉnh của utop (nếu có) | string |  |

### detail
| No. | FieldId | Description | DataType | Mandatory |
| --- | --- | --- | --- | --- |
|  | giftcode | code hàng được điều chỉnh | string | x |
|  | quanlity | số lượng hàng bị hư, hàng bị bể | number | x |
|  | reason | lý do: hàng hư hỏng, hàng bể..... | string | x |

## UTOP-LuanChuyenStore

| là api mà utop sẽ gửi lại các thông tin trong quá trình luân chuyển hàng giữa các store |
| --- |
| ví dụ:<br>chuyển hàng từ store a sang store b |

### header
| No. | FieldId | Description | DataType | Mandatory |
| --- | --- | --- | --- | --- |
|  | shiptocode (from) | nơi thực hiện chuyển đi | string | x |
|  | transaction_date | ngày điều chỉnh | date | x |
|  | schemeid | mã chương trình scheme | string | x |
|  | number_no | số phiếu điều chỉnh của utop (nếu có) | string |  |
|  | shiptocode (to) | nơi thực hiện nhận hàng chuyển đến | string | x |

### detail
| No. | FieldId | Description | DataType | Mandatory |
| --- | --- | --- | --- | --- |
|  | giftcode | code hàng được chuyển | string | x |
|  | quanlity | số lượng hàng bị chuyển | number | x |
|  | reason | lý do: lý do chuyển (nếu có) | string |  |