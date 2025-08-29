# Authentication Mechanism for Uhub -- UGMS API Integration

## 1\. Purpose

To ensure secure, authenticated and tamper-proof API communication between Uhub and UGMS, both parties shall implement a digital signature-based mechanism using RSA key pairs. This allows the receiver to verify the authenticity and integrity of incoming requests.

## 2\. Security & Connection Requirements

### 2.1 Secure Transport & Architecture

- All communication must use HTTPS (TLS 1.2 or above).

- API is called in server-to-server (backend-to-backend) mode.

- IP addresses of both systems must be whitelisted mutually.

- Authentication method:

-         - RSA digital signature using SHA-256

-         - (Optional) Use of Client ID / Client Secret for system identification

-         - Payload signature included in each request

### 2.2 Environment Setup

Each party must provide connection info per environment:

| **Environment** | **Required Info** |
| --- | --- |
| **Sandbox / Test** | Base URL, IP address, Public Key |
| **Production** | Provided after successful UAT |

*Uhub will configure IP whitelisting and verify authentication credentials (e.g. public key) for each environment.*

## 3\. Key Pair and Signature

### 3.1 Key Generation

- Private Key: used to sign outgoing requests

- Public Key: shared with the other party to verify requests

**Standard: RSA 2048 bits, PKCS#1 v1.5 padding, SHA256 hash**

## 4\. Signing & Sending Requests

Step-by-step (Sender Side -- e.g., Uhub → UGMS)

### Step 1: Prepare Payload (JSON)

{
  "giftcode": "GC123456",
  "scheme_id": "SCHEME001",
  "quantity": 10
}

### Step 2: Convert to Sorted Query String

giftcode=GC123456**&**quantity=10**&**scheme_id=SCHEME001

### Step 3: Sign with Private Key (C# Sample)

using System.Security.Cryptography;
using System.Text;

string data = "giftcode=GC123456&quantity=10&scheme_id=SCHEME001";
string privateKeyPem = File.ReadAllText("private_key.pem");
byte[] dataBytes = Encoding.UTF8.GetBytes(data);

using var rsa = RSA.Create();
rsa.ImportFromPem(privateKeyPem.ToCharArray());

var signatureBytes = rsa.SignData(dataBytes, HashAlgorithmName.SHA256, RSASignaturePadding.Pkcs1);
string signatureBase64 = Convert.ToBase64String(signatureBytes);

### Step 4: Send Request

{
  "giftcode": "GC123456",
  "scheme_id": "SCHEME001",
  "quantity": 10,
  "signature": "ptjZEtRoQ/+7Xl/..."
}

## 5\. Verifying Incoming Requests

Step-by-step (Receiver Side -- e.g., UGMS ← Uhub)

### Step 1: Extract & Remove Signature

string signatureBase64 = requestBody["signature"];
requestBody.Remove("signature");

### Step 2: Reconstruct Sorted Query String

var sorted = requestBody.OrderBy(kvp => kvp.Key);
var queryString = string.Join("&", sorted.Select(kvp => $"{kvp.Key}={kvp.Value}"));

### Step 3: Verify Signature

string publicKeyPem = File.ReadAllText("uhub_public_key.pem");
byte[] dataBytes = Encoding.UTF8.GetBytes(queryString);
byte[] signatureBytes = Convert.FromBase64String(signatureBase64);

using var rsa = RSA.Create();
rsa.ImportFromPem(publicKeyPem.ToCharArray());

bool isValid = rsa.VerifyData(dataBytes, signatureBytes, HashAlgorithmName.SHA256, RSASignaturePadding.Pkcs1);

## 6\. HTTP Request Sample

POST /api/giftcode/sync HTTP/1.1
Content-Type: application/json

{
  "giftcode": "GC123456",
  "scheme_id": "SCHEME001",
  "quantity": 10,
  "signature": "ptjZEtRoQ/+7Xl/..."
}

## 7\. Key Management Responsibility

| **Party** | **Signs Requests With** | **Verifies Incoming Requests With** |
| --- | --- | --- |
| **Uhub** | Uhub Private Key | UGMS Public Key |
| **UGMS** | UGMS Private Key | Uhub Public Key |

## Integration Description

1.     API Sync GiftCode

| **Trường** | **Kiểu dữ liệu** | **Bắt buộc** | **Mô tả** |
| --- | --- | --- | --- |
| gift_code | string | Có | Mã quà, không để trống, tối đa 50 ký tự |
| gift_name | string | Có | Tên quà, không để trống, tối đa 100 ký tự |
| gift_type | string | Có | Loại quà, không để trống, tối đa 100 ký tự |
| uom | string | Có | Đơn vị tính, không để trống, tối đa 100 ký tự |
| gift_group | string | Có | Nhóm quà, không để trống, tối đa 100 ký tự |
| **Example** |  |
| curl -X POST https://yourdomain.com/api/v1/gift/create \ |  |
| -H "Content-Type: application/json" \ |  |
| -d '{ |  |
| "gift_code": "GFT001", |  |
| "gift_name": "Áo thun Unilever", |  |
| "gift_type": "vật phẩm", |  |
| "uom": "Cái" |  |
| }' |  |
|  |  |  |  |  |

2.     API Sync Scheme (Schemeinfo)

| **Trường** | **Kiểu dữ liệu** | **Bắt buộc** | **Mô tả** |
| --- | --- | --- | --- |
| scheme_id | string | Có | Mã scheme/ không rỗng/ duy nhất |
| scheme_name | string | Có | Tên scheme, không rỗng |
| start_date | string | Có | Định dạng: "YYYY-MM-DD" |
| end_date | string | Có | Định dạng: "YYYY-MM-DD", ≥ start_date |
| requester | string | Có | Tên người tạo chương trình |
| list_gift | array | Có | Danh sách quà tặng, ít nhất 1 phần |

**Example:**

| curl -X POST https://yourdomain.com/api/v1/scheme/create \ |
| --- |
| -H "Content-Type: application/json" \ |
| -d '{ |
| "scheme_id": "SCHM001", |
| "scheme_name": "Chương trình Tết 2025", |
| "start_date": "2025-12-01", |
| "end_date": "2026-01-15", |
| "requester": "Nguyễn Văn A", |
| "list_gift": [ |
| { |
| "gift_code": "GFT001", |
| "gift_name": "Lì xì", |
| "quantity": 500 |
| }, |
| { |
| "gift_code": "GFT002", |
| "gift_name": "Bánh quy", |
| "quantity": 300 |
| } |
| ] |
| }' |

3.     API Sync Store (Customer)

| **Trường** | **Kiểu dữ liệu** | **Bắt buộc** | **Mô tả** |
| --- | --- | --- | --- |
| customer_id | string | Có | Mã cửa hàng (Store), không rỗng, duy nhất |
| customer_name | string | Có | Tên cửa hàng, không rỗng |
| ship_to_code | string | Có | Mã địa chỉ cửa hàng |
| ship_to_name | string | Có | Tên địa chỉ cửa hàng |
| province | string | Có | Tỉnh thành cửa hàng |

**Example:**

| curl -X POST https://yourdomain.com/api/v1/customer/create \ |
| --- |
| -H "Content-Type: application/json" \ |
| -d '{ |
| "customer_id": "CUS123", |
| "customer_name": "Aeon", |
| "ship_to_code": "ST456", |
| "ship_to_name": "Aeon", |
| "province": "Hà Nội" |
| }' |

4.     API Sync Thông tin phiếu xuất kho đến kho Store- customer (Customer - Giao dịch nhận)

| **Trường** | **Kiểu dữ liệu** | **Bắt buộc** | **Mô tả** |
| --- | --- | --- | --- |
| ship_to_code | string | Có | Mã địa chỉ cửa hàng |
| receive_date | string | Có | Định dạng: "YYYY-MM-DD" |
| scheme_id | string | Có | Mã scheme |
| shipment_num | string | Có | Mã giao dịch |
| sup_code | string | Có | Mã supplier giao |
| trans_code | string | Có | Mã đơn vị vận tải giao |
| gift_list | array | Có | Danh sách quà |
| **Trong ****gift_list[]**: |  |  |  |
| **Trường** | **Kiểu dữ liệu** | **Bắt buộc** | **Mô tả** |
| gift_code | string | Có | Mã quà tặng |
| quantity | integer | Có | Số lượng ≥ 0 |

**Example:**

| curl -X POST https://yourdomain.com/api/v1/utop/receive-gift \ |
| --- |
| -H "Content-Type: application/json" \ |
| -d '{ |
| "ship_to_code": "ST001", |
| "receive_date": "2025-07-04", |
| "scheme_id": "SCHM001", |
| "shipment_num": "Số Giao Dịch", |
| "sup_code": "mã supplier giao", |
| "trans_code": "mã đơn vị vận tải giao", |
| "gift_list": [ |
| { |
| "gift_code": "GFT001", |
| "quantity": 100 |
| }, |
| { |
| "gift_code": "GFT002", |
| "quantity": 50 |
| } |
| ] |
| }' |

5.     API Sync Thông tin phiếu điều chỉnh kho Store - customer(Customer - Giao dịch điều chỉnh)

| **Trường** | **Kiểu dữ liệu** | **Bắt buộc** | **Mô tả** |
| --- | --- | --- | --- |
| ship_to_code | string | Có | Mã địa chỉ cửa hàng |
| receive_date | string | Có | Định dạng: "YYYY-MM-DD" |
| scheme_id | string | Có | Mã scheme |
| shipment_num | string | Có | Mã giao dịch |
| sup_code | string | Có | Mã supplier giao |
| trans_code | string | Có | Mã đơn vị vận tải giao |
| gift_list | array | Có | Danh sách quà |
| **Trong ****gift_list[]**: |  |  |  |
| **Trường** | **Kiểu dữ liệu** | **Bắt buộc** | **Mô tả** |
| gift_code | string | Có | Mã quà tặng |
| quantity | integer | Có | <0 |

**Example:**

| curl -X POST https://yourdomain.com/api/v1/utop/receive-gift \ |
| --- |
| -H "Content-Type: application/json" \ |
| -d '{ |
| "ship_to_code": "ST001", |
| "receive_date": "2025-07-04", |
| "scheme_id": "SCHM001", |
| "shipment_num": "Số Giao Dịch", |
| "sup_code": "mã supplier giao", |
| "trans_code": "mã đơn vị vận tải giao", |
| "gift_list": [ |
| { |
| "gift_code": "GFT001", |
| "quantity": -20 |
| }, |
| { |
| "gift_code": "GFT002", |
| "quantity": -10 |
| } |
| ] |
| }' |

6.     API Sync Thông tin số lượng đã sử dụng theo giftCode và scheme

| **Trường** | **Kiểu dữ liệu** | **Bắt buộc** | **Mô tả** |
| --- | --- | --- | --- |
| ship_to_code | string | Có | Mã địa chỉ cửa hàng |
| scheme_id | string | Có | Mã scheme |
| gift_list | array | Có | Danh sách quà |
| **Trong ****gift_list[]**: |  |  |  |
| **Trường** | **Kiểu dữ liệu** | **Bắt buộc** | **Mô tả** |
| gift_code | string | Có | Mã quà tặng |
| quantity | integer | Có | Số lượng - + |

**Example:**

| curl -X POST https://ugift.vn/api/v1/utop/actual_delivery_gift \ |
| --- |
| -H "Content-Type: application/json" \ |
| -d '{ |
| "ship_to_code": "ST001", |
| "scheme_id": "SCHM001", |
| "gift_list": [ |
| { |
| "gift_code": "GFT001", |
| "quantity": 20 |
| }, |
| { |
| "gift_code": "GFT002", |
| "quantity": -10 |
| } |
| ] |
| }' |