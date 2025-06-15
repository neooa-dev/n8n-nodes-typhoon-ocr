# N8N-Tools

โปรเจ็กต์นี้เป็น **All-in-One Automation Stack** ที่รวม services ต่างๆ สำหรับการทำงานกับ N8N, AI, OCR และ Data Storage โดยใช้ Docker Compose

> 🚀 **ชุดเครื่องมือครบวงจรสำหรับ N8N Workflow Automation**

## 🔧 Services ที่รวมอยู่ในระบบ

### 🤖 **Core Automation**

- **N8N** (port 5678): Workflow automation platform หลัก
- **PostgreSQL** (internal): ฐานข้อมูลสำหรับ N8N workflows และข้อมูล
- **NCA-Toolkit** (port 8080): No-Code Architects Toolkit สำหรับ advanced automation

### 🧠 **AI & Machine Learning**

- **Ollama** (port 11434): Local LLM server สำหรับการประมวลผลภาษาและ AI
- **Qdrant** (port 6333/6334): Vector database สำหรับการค้นหาแบบ semantic
- **OpenTyphoon OCR** (port 8000): OCR engine สำหรับภาษาไทยและเอกสาร

### 💾 **Data & Storage**

- **MinIO** (port 9000/9001): S3-compatible object storage สำหรับไฟล์และข้อมูล

## ✨ **คุณสมบัติหลัก**

- 🚀 **One-Click Setup**: รัน `docker-compose up -d` แล้วใช้งานได้ทันที
- 🔧 **Environment-based Config**: จัดการ configuration ผ่าน `.env` file
- 🔒 **Auto MinIO Setup**: สร้าง buckets และ access keys อัตโนมัติ
- 💾 **Persistent Data**: ใช้ bind mounts เพื่อความปลอดภัยของข้อมูล
- 🌐 **Web Interfaces**: ทุก services มี web UI ใช้งานง่าย
- 📊 **Monitoring Ready**: มี test scripts สำหรับตรวจสอบสถานะ

## 🚀 Quick Start

### การติดตั้ง

```bash
# 1. Clone repository
git clone <repository-url>
cd n8n-tools

# 2. ตั้งค่า environment variables
cp env.template .env
nano .env  # แก้ไข TYPHOON_OCR_API_KEY (จำเป็น)

# 3. เริ่มระบบ
docker-compose up -d

# 4. ทดสอบระบบ
./scripts/test-services.sh
```

## 🌐 การเข้าถึง Services

| Service              | URL                    | Description                     |
| -------------------- | ---------------------- | ------------------------------- |
| **N8N**              | http://localhost:5678  | 🤖 Workflow automation platform |
| **MinIO Console**    | http://localhost:9001  | 💾 File storage management      |
| **NCA-Toolkit**      | http://localhost:8080  | 🛠️ Advanced automation tools    |
| **Qdrant Dashboard** | http://localhost:6334  | 🔍 Vector database UI           |
| **Typhoon OCR**      | http://localhost:8000  | 📄 OCR API endpoint             |
| **Ollama API**       | http://localhost:11434 | 🧠 Local LLM API                |
| **Qdrant API**       | http://localhost:6333  | 🔍 Vector database API          |
| **MinIO API**        | http://localhost:9000  | 💾 S3-compatible storage API    |

### 🔑 Default Credentials

- **MinIO Console**: `admin` / `miniopassword`
- **NCA-Toolkit**: API Key `bagaide`

## 🏗️ Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                        N8N-Tools Stack                         │
├─────────────────────────────────────────────────────────────────┤
│  🤖 Core Automation Layer                                      │
│  ┌─────────────┐  ┌──────────────┐  ┌─────────────────────┐   │
│  │    N8N      │  │ PostgreSQL   │  │   NCA-Toolkit       │   │
│  │   :5678     │  │  (internal)  │  │      :8080          │   │
│  └─────────────┘  └──────────────┘  └─────────────────────┘   │
├─────────────────────────────────────────────────────────────────┤
│  🧠 AI & Processing Layer                                      │
│  ┌─────────────┐  ┌──────────────┐  ┌─────────────────────┐   │
│  │   Ollama    │  │    Qdrant    │  │  Typhoon OCR        │   │
│  │   :11434    │  │  :6333/6334  │  │      :8000          │   │
│  └─────────────┘  └──────────────┘  └─────────────────────┘   │
├─────────────────────────────────────────────────────────────────┤
│  💾 Storage Layer                                              │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                   MinIO S3                             │   │
│  │              :9000 (API) / :9001 (Console)            │   │
│  └─────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
```

### 🔄 การทำงานร่วมกัน

1. **N8N** เป็นตัวหลักในการสร้าง workflows และเชื่อมต่อกับ services อื่น
2. **Ollama** ให้บริการ AI/LLM สำหรับการประมวลผลภาษา
3. **Qdrant** จัดเก็บ vectors สำหรับการค้นหาแบบ semantic
4. **Typhoon OCR** ประมวลผลเอกสารและรูปภาพเป็นข้อความ
5. **MinIO** เป็น central storage สำหรับไฟล์และข้อมูล
6. **NCA-Toolkit** เสริมความสามารถ automation ขั้นสูง

## ⚙️ การตั้งค่า

### N8N

- Host: localhost (สามารถเปลี่ยนได้ผ่าน N8N_HOST)
- Port: 5678
- Protocol: http (สามารถเปลี่ยนได้ผ่าน N8N_PROTOCOL)
- สามารถใช้งาน Typhoon OCR ผ่าน Custom Node

### PostgreSQL

- Database: n8n
- Username: n8n
- Password: n8n
- Port: 5432 (internal)

### Ollama

- Port: 11434
- ข้อมูลโมเดลจะถูกเก็บใน volume `ollama_data`

### Qdrant

- API Port: 6333
- Dashboard Port: 6334
- ข้อมูลจะถูกเก็บใน volume `qdrant_data`

#### การสร้าง Collection ใน Qdrant

1. ผ่าน Dashboard (http://localhost:6334):

   - คลิก "Create Collection"
   - ตั้งชื่อ collection
   - เลือก Vector Size (ตามขนาดของ vector ที่จะเก็บ)
   - เลือก Distance Metric (Cosine, Euclidean, หรือ Dot)
   - คลิก "Create"

2. ผ่าน API:

```bash
curl -X PUT 'http://localhost:6333/collections/my_collection' \
  -H 'Content-Type: application/json' \
  -d '{
    "vectors": {
      "size": 384,
      "distance": "Cosine"
    }
  }'
```

3. ตัวอย่างการใช้งานใน N8N:

```json
{
  "method": "PUT",
  "url": "http://qdrant:6333/collections/my_collection",
  "authentication": "none",
  "body": {
    "vectors": {
      "size": 384,
      "distance": "Cosine"
    }
  }
}
```

4. การตรวจสอบ collection ที่มีอยู่:

```bash
curl 'http://localhost:6333/collections'
```

5. การลบ collection:

```bash
curl -X DELETE 'http://localhost:6333/collections/my_collection'
```

### OpenTyphoon OCR

- Port: 8000
- ต้องการ API Key จาก OpenTyphoon.ai (ตั้งค่าใน .env)
- รองรับการประมวลผลไฟล์ PDF และรูปภาพ
- มีการติดตั้ง poppler-utils สำหรับการประมวลผล PDF
- มีระบบ logging แสดงเวลาที่ใช้ในการประมวลผล

### MinIO

- API Port: 9000
- Console Port: 9001
- Username: admin (ค่าเริ่มต้น)
- Password: miniopassword (ค่าเริ่มต้น)
- ข้อมูลจะถูกเก็บใน `datas/minio/`
- รองรับ S3-compatible API
- มี Web Console สำหรับจัดการไฟล์

### NCA-Toolkit

- Port: 8080
- API Key: NCA_API_KEY (ค่าเริ่มต้น)
- เชื่อมต่อกับ MinIO สำหรับ object storage
- ข้อมูลจะถูกเก็บใน `datas/nca-toolkit/`
- รองรับการสร้าง automation workflows
- **S3 Configuration**: ใช้ access keys ที่กำหนดใน environment variables
- ไม่ต้องตั้งค่า S3 credentials ด้วยตนเอง

## การใช้งาน Typhoon OCR API

### การตั้งค่า API Key

ตั้งค่า API Key ในไฟล์ `.env`:

```env
TYPHOON_OCR_API_KEY=your_api_key_here
```

### การประมวลผลเอกสาร

```bash
curl -X POST "http://localhost:8000/process" \
  -F "file=@document.pdf" \
  -F "page_num=1" \
  -F "task_type=default"
```

Parameters:

- `file`: ไฟล์ที่ต้องการประมวลผล (PDF หรือรูปภาพ)
- `page_num`: หน้าที่ต้องการประมวลผล (ค่าเริ่มต้น: 1)
- `task_type`: ประเภทงาน (ค่าเริ่มต้น: "default")

### ตรวจสอบสถานะ API

```bash
curl "http://localhost:8000/health"
```

### รูปแบบผลลัพธ์

```json
{
  "result": "ข้อความที่ได้จากการประมวลผล OCR",
  "processing_time": {
    "minutes": 1,
    "seconds": 30,
    "total_seconds": 90.5
  }
}
```

## การใช้งาน Typhoon OCR ใน N8N

### การติดตั้ง Custom Node

1. เปิด N8N ที่ http://localhost:5678
2. ไปที่ Settings > Community Nodes
3. คลิก "Install a community node"
4. ใส่ URL ของ repository นี้
5. คลิก "Install"

### การใช้งานใน Workflow

1. เพิ่ม node "Typhoon OCR" ใน workflow
2. เลือก Operation:
   - Process Document: สำหรับประมวลผลเอกสาร
3. ตั้งค่าพารามิเตอร์:
   - File: URL หรือ base64 ของไฟล์ที่ต้องการประมวลผล

### ตัวอย่างการใช้งานใน N8N

#### 1. การประมวลผลไฟล์จาก URL

1. เพิ่ม node "HTTP Request" เพื่อดาวน์โหลดไฟล์
2. เพิ่ม node "Typhoon OCR" เพื่อประมวลผล
3. ตั้งค่า:
   - File: URL ของไฟล์

#### 2. การประมวลผลไฟล์จาก Form

1. เพิ่ม node "Webhook" เพื่อรับไฟล์จาก form
2. เพิ่ม node "Typhoon OCR" เพื่อประมวลผล
3. ตั้งค่า:
   - File: ข้อมูลไฟล์จาก form

#### 3. การประมวลผลหลายหน้า PDF

1. เพิ่ม node "HTTP Request" เพื่อดาวน์โหลด PDF
2. เพิ่ม node "Function" เพื่อวนลูปแต่ละหน้า
3. เพิ่ม node "Typhoon OCR" เพื่อประมวลผลแต่ละหน้า

## Data Storage

โปรเจ็กต์นี้ใช้ bind mounts แทน Docker volumes เพื่อให้ข้อมูลจัดเก็บในโฟลเดอร์ `datas/` ภายในโปรเจ็กต์:

- `datas/n8n/`: เก็บข้อมูลการตั้งค่าและ workflows ของ N8N
- `datas/postgres/`: เก็บข้อมูล PostgreSQL
- `datas/ollama/`: เก็บโมเดลและข้อมูลของ Ollama
- `datas/qdrant/`: เก็บข้อมูล vector ของ Qdrant
- `datas/typhoon-ocr/`: เก็บข้อมูลและไฟล์ที่ประมวลผลด้วย OCR
- `datas/minio/`: เก็บข้อมูล object storage ของ MinIO
- `datas/nca-toolkit/`: เก็บข้อมูลและ configuration ของ NCA-Toolkit
- `datas/shared/`: (ไม่ใช้แล้ว) เก็บข้อมูลที่แชร์ระหว่าง services

### ข้อดีของ Bind Mounts

- **Easy Backup**: สามารถ backup ข้อมูลได้ง่ายโดยคัดลอกโฟลเดอร์ `datas/`
- **Direct Access**: เข้าถึงข้อมูลได้โดยตรงจากไฟล์ระบบ
- **Portability**: ย้ายโปรเจ็กต์ไปที่อื่นได้พร้อมข้อมูล
- **Version Control**: สามารถเลือกว่าข้อมูลส่วนไหนจะ commit ใน git (โฟลเดอร์ `datas/` ถูก ignore โดยค่าเริ่มต้น)

## การหยุด Services

```bash
docker-compose down
```

หากต้องการลบข้อมูลทั้งหมด ให้ลบโฟลเดอร์ `datas/`:

```bash
docker-compose down
rm -rf datas/
```

**หมายเหตุ**: เนื่องจากใช้ bind mounts การลบ containers ด้วย `docker-compose down` จะไม่ลบข้อมูลใน `datas/` ทำให้ข้อมูลปลอดภัยจากการลบโดยไม่ตั้งใจ

## การใช้งาน MinIO

### การเข้าถึง MinIO Console

1. เปิดเบราว์เซอร์ไปที่ http://localhost:9001
2. ใส่ username: `admin` และ password: `miniopassword`
3. สร้าง bucket ใหม่หรือจัดการไฟล์ที่มีอยู่

### การใช้งาน MinIO API

```bash
# ตรวจสอบ bucket ที่มีอยู่
curl http://localhost:9000

# อัปโหลดไฟล์ (ต้องใช้ S3 client เช่น aws-cli หรือ mc)
mc config host add minio http://localhost:9000 admin miniopassword
mc mb minio/nca-toolkit
mc cp file.txt minio/nca-toolkit/
```

## การใช้งาน NCA-Toolkit

### การเข้าถึง Web Interface

เปิดเบราว์เซอร์ไปที่ http://localhost:8080 เพื่อใช้งาน No-Code Architects Toolkit

### การใช้งานร่วมกับ N8N

NCA-Toolkit สามารถเชื่อมต่อกับ N8N ได้ผ่าน:

1. **HTTP Request Node**: เรียกใช้ API ของ NCA-Toolkit
2. **S3 Integration**: ใช้ MinIO เป็น storage backend
3. **Workflow Automation**: สร้าง automation ที่ซับซ้อนขึ้น

### Environment Variables

Docker Compose จะโหลด environment variables จากไฟล์ `.env` อัตโนมัติ โดยทุก services มีการตั้งค่า `env_file: - .env` ไว้

สามารถปรับแต่งการตั้งค่าผ่านไฟล์ `.env`:

```env
# N8N Configuration
N8N_HOST=localhost
N8N_PROTOCOL=http

# Typhoon OCR Configuration
TYPHOON_OCR_API_KEY=your_typhoon_ocr_api_key_here

# MinIO Settings
MINIO_ROOT_USER=admin
MINIO_ROOT_PASSWORD=miniopassword
S3_BUCKET_NAME=nca-toolkit
MINIO_BUCKET_PUBLIC=false

# NCA-Toolkit Settings (Simplified)
NCA_API_KEY=NCA_API_KEY
S3_REGION=None

# S3/MinIO Access Keys - กำหนดใน .env
S3_ACCESS_KEY=nca-toolkit-access-key
S3_SECRET_KEY=nca-toolkit-secret-key-12345
S3_ENDPOINT=http://minio:9000
```

### การ Setup อัตโนมัติของ MinIO

โปรเจ็กต์นี้มี **MinIO initialization service** (`minio-init`) ที่จะทำการ setup อัตโนมัติ:

1. **รอให้ MinIO พร้อมใช้งาน**: ใช้ healthcheck เพื่อตรวจสอบสถานะ
2. **สร้าง Bucket**: สร้าง bucket ตามชื่อที่กำหนดใน `S3_BUCKET_NAME`
3. **ตั้งค่า Public Access** (ถ้าต้องการ): ใช้ `MINIO_BUCKET_PUBLIC=true`
4. **สร้าง Access Key**: สร้าง access key ใหม่สำหรับ API usage
5. **บันทึกข้อมูล**: เก็บ access keys ไว้ในไฟล์สำหรับอ้างอิง
6. **ใช้ Environment Keys**: ใช้ keys ที่กำหนดไว้ใน environment variables

### การใช้งาน S3 Keys ใน NCA-Toolkit

NCA-Toolkit จะใช้ MinIO access keys จาก environment variables:

1. **ใช้ Static Keys**: ใช้ keys ที่กำหนดไว้ใน `.env` file
2. **Keys ที่ตั้งไว้**: S3_ACCESS_KEY และ S3_SECRET_KEY จาก environment
3. **MinIO User Creation**: MinIO init จะสร้าง user ตาม keys ที่กำหนด
4. **Shared Configuration**: ทั้ง MinIO และ NCA-Toolkit ใช้ keys เดียวกัน

### การดู MinIO Access Keys

หลังจากเริ่ม services แล้ว สามารถดู access keys ที่สร้างได้ด้วย:

```bash
# ดู logs ของ minio-init เพื่อเห็น access keys
docker-compose logs minio-init

# ดู logs ของ nca-toolkit เพื่อเห็นการโหลด keys
docker-compose logs nca-toolkit

# ดูไฟล์ที่บันทึกไว้
docker-compose exec minio-init cat /tmp/minio-keys.txt

# ดู keys ที่กำหนดใน environment
cat .env | grep S3_
```

### การใช้งาน MinIO กับ Applications อื่น

Access keys ที่สร้างใหม่สามารถนำไปใช้กับ applications อื่นได้:

```env
# ใช้ access keys ที่สร้างจาก minio-init
S3_ACCESS_KEY=[generated_access_key]
S3_SECRET_KEY=[generated_secret_key]
S3_ENDPOINT=http://localhost:9000
S3_BUCKET_NAME=nca-toolkit
```

**หมายเหตุ**: S3_ACCESS_KEY, S3_SECRET_KEY และ S3_BUCKET_NAME ถูกกำหนดใน environment variables และใช้ร่วมกันระหว่าง MinIO และ NCA-Toolkit

## 🔧 System Flow

1. **MinIO starts** → รัน MinIO server
2. **Health check** → ตรวจสอบว่า MinIO พร้อมแล้ว
3. **minio-init runs** → ทำการ setup อัตโนมัติ:
   - เชื่อมต่อ MinIO
   - สร้าง bucket
   - สร้าง access keys
   - บันทึกข้อมูลใน `/tmp/minio-keys.txt`
   - สร้าง MinIO user ตาม environment keys
4. **nca-toolkit starts** → เริ่ม NCA-Toolkit พร้อม environment keys:
   - ใช้ S3 credentials จาก environment variables
   - ไม่ต้องรอ MinIO keys generation
   - ใช้ static keys ที่กำหนดไว้ใน .env
   - เริ่มแอปพลิเคชันได้ทันที

## 🎯 System Benefits

N8N-Tools Stack ให้ประโยชน์ครบวงจรสำหรับ automation workflows:

### 🚀 **Technical Benefits**

- **Auto-Configuration**: MinIO และ NCA-Toolkit มีการ setup อัตโนมัติ
- **Environment-based**: จัดการ configuration ผ่าน `.env` file เพียงไฟล์เดียว
- **Integrated Storage**: ทุก services ใช้ MinIO เป็น central storage
- **One-Command Deployment**: รัน `docker-compose up -d` แล้วใช้งานได้ทันที

### 💡 **Automation Capabilities**

- **Document Processing**: OCR + AI analysis + Vector storage
- **Multi-modal AI**: Text, Images, และ Documents ในระบบเดียว
- **Workflow Orchestration**: N8N เชื่อมต่อทุก services เข้าด้วยกัน
- **Scalable Architecture**: เพิ่ม/ลด services ได้ตามความต้องการ

### 🔧 **Use Cases**

- **Document Intelligence**: OCR → LLM Analysis → Vector Search
- **Content Management**: File Upload → Processing → Storage → Retrieval
- **AI Workflows**: Multi-step AI processing chains
- **Data Pipeline**: ETL processes with AI enhancement

## 🧪 การทดสอบระบบ

หลังจากเริ่ม services แล้ว สามารถทดสอบการทำงานได้ด้วย:

```bash
# ทดสอบ services ทั้งหมด
./scripts/test-services.sh

# หรือทดสอบแต่ละ service แยก
curl http://localhost:9000/minio/health/live    # MinIO health
curl http://localhost:8080/v1/toolkit/test      # NCA-Toolkit
curl http://localhost:5678                      # N8N
curl http://localhost:6333                      # Qdrant
curl http://localhost:8000/health               # Typhoon OCR
```

## 🚀 การเริ่มต้นใช้งาน

### Quick Start

```bash
# 1. คัดลอก environment template
cp env.template .env

# 2. แก้ไข API keys (จำเป็นเฉพาะ TYPHOON_OCR_API_KEY)
nano .env

# 3. เริ่ม services ทั้งหมด (Docker Compose จะโหลด .env อัตโนมัติ)
docker-compose up -d

# 4. รอให้ initialization เสร็จ (ประมาณ 2-3 นาที)
docker-compose logs -f minio-init

# 5. ทดสอบการทำงาน
./scripts/test-services.sh
```

**หมายเหตุ**: S3 credentials ได้รับการกำหนดไว้ใน .env แล้ว และใช้ร่วมกันระหว่าง MinIO และ NCA-Toolkit

### ตรวจสอบ MinIO Keys

```bash
# ดู access keys ที่สร้างใหม่
docker-compose logs minio-init | grep -E "(Access Key|Secret Key)"

# ดู NCA-Toolkit startup logs
docker-compose logs nca-toolkit | head -20

# ดู environment keys
cat .env | grep S3_
```
