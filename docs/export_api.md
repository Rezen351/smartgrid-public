# SmartGrid Export API

Dokumentasi resmi untuk mengonsumsi endpoint publik ekspor data telemetri historis dari platform SmartGrid.

## Base URL

```text
https://smartgrid.almuzky.my.id/api/v1/export
```

Semua endpoint export berada di bawah path prefix tersebut.

## Otentikasi

Export Service menggunakan JWT yang diterbitkan oleh Auth Service.

Role yang diizinkan:
- `admin`
- `operator`

### Mendapatkan access token

```bash
curl -sS -X POST \
  "https://smartgrid.almuzky.my.id/api/v1/auth/login" \
  -H "Content-Type: application/json" \
  -d '{
    "identifier": "USERNAME_ATAU_EMAIL",
    "password": "PASSWORD"
  }'
```

Respons berhasil:

```json
{
  "access_token": "eyJ...",
  "refresh_token": "...",
  "expires_in": 900
}
```

Gunakan nilai `access_token` pada header berikut:

```http
Authorization: Bearer ACCESS_TOKEN
```

Jangan menyimpan token di repository atau membagikannya di log publik.

## Health check

Endpoint ini tidak membutuhkan token:

```bash
curl -i "https://smartgrid.almuzky.my.id/api/v1/export/health"
```

Respons normal:

```json
{
  "success": true,
  "data": {
    "status": "ok"
  }
}
```

## Melihat node dan metric tersedia

```bash
curl -sS \
  -H "Authorization: Bearer ACCESS_TOKEN" \
  "https://smartgrid.almuzky.my.id/api/v1/export/nodes"
```

Contoh respons:

```json
{
  "success": true,
  "data": {
    "nodes": [
      {
        "node_id": "SmartGrid-01",
        "module_id": "d73f79bc-0757-4c42-8534-709ff9762a32",
        "metrics": ["reg504", "reg509"]
      }
    ]
  }
}
```

Gunakan nilai `node_id` dan `metrics` dari respons ini untuk menyusun query telemetry.

## Preview metadata

```bash
curl -sS -G \
  -H "Authorization: Bearer ACCESS_TOKEN" \
  --data-urlencode "node_id=SmartGrid-01" \
  --data-urlencode "metric=reg504" \
  --data-urlencode "from=2026-09-12T00:00:00Z" \
  --data-urlencode "to=2026-09-12T23:59:59Z" \
  "https://smartgrid.almuzky.my.id/api/v1/export/meta"
```

Contoh respons:

```json
{
  "success": true,
  "data": {
    "node_ids": ["SmartGrid-01"],
    "metrics": ["reg504"],
    "from": "2026-09-12T00:00:00Z",
    "to": "2026-09-12T23:59:59Z",
    "total": 120
  }
}
```

## Query telemetry sebagai JSON

```bash
curl -sS -G \
  -H "Authorization: Bearer ACCESS_TOKEN" \
  --data-urlencode "node_id=SmartGrid-01" \
  --data-urlencode "metric=reg504" \
  --data-urlencode "from=2026-09-12T00:00:00Z" \
  --data-urlencode "to=2026-09-12T23:59:59Z" \
  --data-urlencode "format=json" \
  --data-urlencode "limit=100" \
  "https://smartgrid.almuzky.my.id/api/v1/export/telemetry"
```

Contoh respons:

```json
{
  "success": true,
  "data": {
    "rows": [
      {
        "time": "2026-09-12T07:13:51.228871Z",
        "node_id": "SmartGrid-01",
        "module_id": null,
        "metric": "reg504",
        "value": 51.79999924
      }
    ],
    "total": 1,
    "has_more": false,
    "next_cursor": ""
  }
}
```

## Query beberapa node atau metric

```bash
curl -sS -G \
  -H "Authorization: Bearer ACCESS_TOKEN" \
  --data-urlencode "node_id=SmartGrid-01,SmartGrid-02" \
  --data-urlencode "metric=reg504,reg509" \
  --data-urlencode "format=json" \
  "https://smartgrid.almuzky.my.id/api/v1/export/telemetry"
```

Semua metric pada satu node:

```bash
curl -sS -G \
  -H "Authorization: Bearer ACCESS_TOKEN" \
  --data-urlencode "node_id=SmartGrid-01" \
  --data-urlencode "metric=*" \
  --data-urlencode "format=json" \
  "https://smartgrid.almuzky.my.id/api/v1/export/telemetry"
```

Semua node dan semua metric:

```bash
curl -sS -G \
  -H "Authorization: Bearer ACCESS_TOKEN" \
  --data-urlencode "node_id=*" \
  --data-urlencode "metric=*" \
  --data-urlencode "format=json" \
  "https://smartgrid.almuzky.my.id/api/v1/export/telemetry"
```

## Pagination dengan cursor

Jika respons memiliki:

```json
{
  "has_more": true,
  "next_cursor": "CURSOR_TOKEN"
}
```

Kirim `next_cursor` sebagai parameter `cursor` pada request berikutnya. Filter `node_id`, `metric`, `from`, dan `to` harus tetap sama.

```bash
curl -sS -G \
  -H "Authorization: Bearer ACCESS_TOKEN" \
  --data-urlencode "node_id=SmartGrid-01" \
  --data-urlencode "metric=reg504" \
  --data-urlencode "from=2026-09-12T00:00:00Z" \
  --data-urlencode "to=2026-09-12T23:59:59Z" \
  --data-urlencode "format=json" \
  --data-urlencode "limit=100" \
  --data-urlencode "cursor=CURSOR_TOKEN" \
  "https://smartgrid.almuzky.my.id/api/v1/export/telemetry"
```

Untuk format JSON, cursor berada di `data.next_cursor`. Untuk format CSV, cursor berada pada response header `X-Export-Next-Cursor`.

## Download telemetry sebagai CSV

Format CSV adalah format default dan dikembalikan sebagai file attachment:

```bash
curl -sS -G \
  -H "Authorization: Bearer ACCESS_TOKEN" \
  --data-urlencode "node_id=SmartGrid-01" \
  --data-urlencode "metric=reg504" \
  --data-urlencode "from=2026-09-12T00:00:00Z" \
  --data-urlencode "to=2026-09-12T23:59:59Z" \
  --data-urlencode "format=csv" \
  -o telemetry.csv \
  "https://smartgrid.almuzky.my.id/api/v1/export/telemetry"
```

CSV memiliki bentuk wide table. Contoh:

```csv
time,node_id,module_id,reg504
2026-09-12T07:13:51.228871Z,SmartGrid-01,,51.79999924
```

Periksa header cursor jika data lebih banyak daripada halaman saat ini:

```bash
curl -sS -D headers.txt -G \
  -H "Authorization: Bearer ACCESS_TOKEN" \
  --data-urlencode "node_id=SmartGrid-01" \
  --data-urlencode "format=csv" \
  -o telemetry.csv \
  "https://smartgrid.almuzky.my.id/api/v1/export/telemetry"

grep -i "X-Export-Next-Cursor" headers.txt
```

## Format waktu

Format yang didukung:

```text
2026-09-12T07:00:00Z       RFC3339
2026-09-12                 tanggal UTC
1789196400                 Unix timestamp dalam detik
```

Rentang export maksimum adalah **366 hari**. Untuk hasil yang konsisten, gunakan RFC3339 dengan timezone UTC (`Z`).

## Parameter query

| Parameter | Wajib | Keterangan |
|---|---:|---|
| `node_id` | Ya | Satu node, beberapa node dipisah koma, atau `*` untuk semua node. |
| `metric` | Tidak | Satu metric, beberapa metric dipisah koma, atau `*` untuk semua metric. Default `*`. |
| `from` | Tidak | Waktu awal: RFC3339, `YYYY-MM-DD`, atau Unix timestamp detik. Default 24 jam terakhir. |
| `to` | Tidak | Waktu akhir: RFC3339, `YYYY-MM-DD`, atau Unix timestamp detik. Default waktu sekarang. |
| `format` | Tidak | `json` atau `csv`. Default `csv`. |
| `limit` | Tidak | Jumlah baris per halaman. Default `10000`, maksimum `100000`. |
| `cursor` | Tidak | Cursor dari respons halaman sebelumnya. |

Metric harus menggunakan nama yang tersimpan di kolom `metric`, misalnya `reg504`, bukan nama `source_key` MQTT jika keduanya berbeda.

## Error umum

### 400 Bad Request

Parameter tidak valid, `node_id` tidak diisi, atau rentang waktu melebihi 366 hari.

```json
{
  "success": false,
  "error": {
    "code": "BAD_REQUEST",
    "message": "node_id is required"
  }
}
```

### 401 Unauthorized

Token tidak ada, tidak valid, atau sudah kedaluwarsa.

```json
{
  "success": false,
  "error": {
    "code": "UNAUTHORIZED",
    "message": "missing or invalid Authorization header"
  }
}
```

### 403 Forbidden

JWT valid, tetapi role user bukan `admin` atau `operator`.

```json
{
  "success": false,
  "error": {
    "code": "FORBIDDEN",
    "message": "forbidden: insufficient role"
  }
}
```

### 500 Internal Server Error

Export Service gagal membaca data historis. Periksa status layanan melalui log platform.

## OpenAPI

Spesifikasi OpenAPI dapat diakses tanpa token:

```bash
curl -sS "https://smartgrid.almuzky.my.id/api/v1/export/openapi"
```
