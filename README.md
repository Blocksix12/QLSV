# 📱 QLSV - Quản Lý Sinh Viên

<div align="center">

![Android](https://img.shields.io/badge/Android-3DDC84?style=for-the-badge&logo=android&logoColor=white)
![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-07405E?style=for-the-badge&logo=sqlite&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-yellow.svg? style=for-the-badge)

**Ứng dụng Android quản lý sinh viên với SQLite Database**

[Tính năng](#-tính-năng) • [Cài đặt](#-cài-đặt) • [Kiến trúc](#-kiến-trúc) • [Screenshots](#-screenshots) • [Đóng góp](#-đóng-góp)

</div>

---

## 📋 Mục lục

- [Giới thiệu](#-giới-thiệu)
- [Tính năng](#-tính-năng)
- [Công nghệ](#-công-nghệ)
- [Yêu cầu](#-yêu-cầu)
- [Cài đặt](#-cài-đặt)
- [Kiến trúc](#-kiến-trúc)
- [Cấu trúc dự án](#-cấu-trúc-dự-án)
- [Database Schema](#-database-schema)
- [Security](#-security)
- [Git Configuration](#-git-configuration)
- [Contributing](#-contributing)
- [Roadmap](#-roadmap)
- [License](#-license)

---

## 🎯 Giới thiệu

**QLSV (Quản Lý Sinh Viên)** là ứng dụng Android native được phát triển bằng Java, sử dụng SQLite để lưu trữ dữ liệu sinh viên.  Ứng dụng áp dụng kiến trúc Clean Architecture với các layer rõ ràng, dễ bảo trì và mở rộng.

### 🌟 Điểm nổi bật

- ✅ **Clean Architecture** - Tách biệt các layer:  Data, Domain, UI
- ✅ **SQLite Database** - Lưu trữ local, không cần internet
- ✅ **CRUD Operations** - Đầy đủ chức năng Thêm/Sửa/Xóa/Xem
- ✅ **Material Design** - Giao diện đẹp, thân thiện
- ✅ **Security** - Tên database được mã hóa Base64
- ✅ **Logging** - System log chi tiết cho debugging

---

## ✨ Tính năng

### 📌 Quản lý Sinh viên

- ➕ **Thêm** sinh viên mới với validation
- ✏️ **Sửa** thông tin sinh viên
- 🗑️ **Xóa** sinh viên với xác nhận
- 👁️ **Xem** danh sách đầy đủ
- 🔍 **Tìm kiếm** theo MSSV, tên
- 📊 **Sắp xếp** linh hoạt

### 📱 Giao diện

- 📋 **RecyclerView** hiển thị danh sách
- ➕ **Activity thêm** sinh viên
- ✏️ **Activity sửa** thông tin
- 🗑️ **Dialog** xác nhận xóa
- 📱 **Responsive** design

---

## 🛠️ Công nghệ

### Core
- **Java** - 100%
- **Android SDK** - Min 21, Target 33
- **SQLite** - Local database

### Architecture
- **Clean Architecture**
- **Repository Pattern**
- **UseCase Pattern**
- **MVVM** (optional)

### UI
- **Material Components**
- **RecyclerView**
- **CardView**
- **ConstraintLayout**

---

## 💻 Yêu cầu

### Development
- Android Studio Arctic Fox+
- JDK 8 or 11
- Gradle 7.0+
- Min SDK:  21 (Android 5.0)
- Target SDK: 33 (Android 13)

### Device
- Android 5.0+
- RAM: 2GB+
- Storage: 50MB

---

## 🚀 Cài đặt

### Bước 1: Clone

```bash
git clone https://github.com/NvkhoaDev54/QLSV.git
cd QLSV
```

### Bước 2: Mở Android Studio

1. **File** → **Open**
2. Chọn thư mục `QLSV`
3. Chờ Gradle sync

### Bước 3: Cấu hình Database

Database name được mã hóa trong `app/build.gradle`:

```gradle
android {
    defaultConfig {
        buildConfigField "String", "DB_NAME_ENCODED", "\"UVxMU1YuZGI=\""
        // Decoded:  "QLSV.db"
    }
}
```

Để đổi tên:

```bash
# Encode tên mới
echo -n "NewName. db" | base64

# Cập nhật build.gradle
buildConfigField "String", "DB_NAME_ENCODED", "\"<base64_result>\""
```

### Bước 4: Thêm Database (Optional)

```
app/src/main/assets/databases/QLSV.db
```

> ⚠️ File `.db` được gitignore

### Bước 5: Build & Run

```bash
# Debug build
./gradlew assembleDebug

# Install
adb install app/build/outputs/apk/debug/app-debug.apk
```

Hoặc nhấn **Run** (▶️) trong Android Studio

---

## 🏗️ Kiến trúc

### Clean Architecture

```
┌─────────────────────────────────────┐
│      Presentation Layer             │
│  (Activity, Fragment, Adapter)      │
└─────────────┬───────────────────────┘
              │
┌─────────────▼───────────────────────┐
│         Domain Layer                 │
│    (Model, UseCase, Interface)      │
└─────────────┬───────────────────────┘
              │
┌─────────────▼───────────────────────┐
│          Data Layer                  │
│   (DAO, DatabaseHelper, Repo)       │
└─────────────────────────────────────┘
```

### Package Structure

```
com.example.qlsv/
├── data/
│   ├── db/
│   │   ├── DatabaseHelper.java
│   │   └── DatabaseProvider.java
│   └── dao/
│       └── SinhVienDAO.java
│
├── domain/
│   ├── model/
│   │   └── SinhVien.java
│   └── usecase/
│       ├── GetAllSinhVienUseCase.java
│       ├── AddSinhVienUseCase. java
│       ├── UpdateSinhVienUseCase. java
│       └── DeleteSinhVienUseCase.java
│
├── service/
│   └── SinhVienService.java
│
├── ui/
│   ├── list/
│   ├── add/
│   ├── edit/
│   └── delete/
│
└── util/
    ├── AppLogger.java
    └── Constants.java
```

---

## 📂 Cấu trúc dự án

```
QLSV/
├── README.md                          # ✅ Documentation
├── LICENSE                            # ✅ MIT License
├── . gitignore                         # ✅ Ignore rules
├── settings.gradle                    # ✅ Modules
├── build.gradle                       # ✅ Root build
│
├── app/
│   ├── build.gradle                   # ✅ App build (DB_NAME_ENCODED)
│   ├── proguard-rules.pro            # ✅ ProGuard
│   │
│   └── src/
│       ├── main/
│       │   ├── AndroidManifest.xml    # ✅ Manifest
│       │   │
│       │   ├── java/com/example/qlsv/
│       │   │   ├── App.java           # ✅ Application
│       │   │   ├── data/              # ✅ Data layer
│       │   │   ├── domain/            # ✅ Domain layer
│       │   │   ├── service/           # ✅ Services
│       │   │   ├── ui/                # ✅ UI components
│       │   │   └── util/              # ✅ Utilities
│       │   │
│       │   ├── res/
│       │   │   ├── layout/            # ✅ XML layouts
│       │   │   ├── values/            # ✅ Resources
│       │   │   ├── drawable/          # ✅ Drawables
│       │   │   └── mipmap/            # ✅ Icons
│       │   │
│       │   └── assets/
│       │       └── databases/
│       │           └── QLSV. db        # ❌ GITIGNORE
│       │
│       ├── androidTest/               # ✅ Tests
│       └── test/                      # ✅ Unit tests
│
└── gradle/                            # ✅ Gradle wrapper
```

**Legend:**
- ✅ = Commit vào Git
- ❌ = Gitignore (sensitive/generated)

---

## 🗄️ Database Schema

### Table: `SinhVien`

```sql
CREATE TABLE SinhVien (
    mssv TEXT PRIMARY KEY NOT NULL,
    hoTen TEXT NOT NULL,
    ngaySinh TEXT,
    gioiTinh TEXT,
    diaChi TEXT,
    soDienThoai TEXT,
    email TEXT,
    lop TEXT,
    khoa TEXT,
    namNhapHoc INTEGER,
    trangThai TEXT DEFAULT 'Đang học',
    createdAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Indexes

```sql
CREATE INDEX idx_hoTen ON SinhVien(hoTen);
CREATE INDEX idx_lop ON SinhVien(lop);
CREATE INDEX idx_khoa ON SinhVien(khoa);
CREATE INDEX idx_trangThai ON SinhVien(trangThai);
```

### Sample Data

```sql
INSERT INTO SinhVien VALUES
('SV001', 'Nguyễn Văn A', '2003-01-15', 'Nam', 'Hà Nội', 
 '0901234567', 'nva@example.com', '20DTHC1', 'CNTT', 2020, 'Đang học',
 CURRENT_TIMESTAMP, CURRENT_TIMESTAMP);
```

---

## 🔒 Security

### Database Encoding

```gradle
// app/build.gradle
buildConfigField "String", "DB_NAME_ENCODED", "\"UVxMU1YuZGI=\""
```

```java
// DatabaseProvider.java
public static String getDecodedDatabaseName() {
    String encoded = BuildConfig.DB_NAME_ENCODED;
    byte[] decoded = Base64.decode(encoded, Base64.DEFAULT);
    return new String(decoded, StandardCharsets.UTF_8);
}
```

### ProGuard

```proguard
# proguard-rules.pro
-keep class com.example.qlsv.domain.model.** { *; }
-keep class com.example.qlsv.data.dao.** { *; }
-keep class com.example.qlsv.data.db.** { *; }
```

---

## 🔧 Git Configuration

### `.gitignore`

```gitignore
# Built files
*.apk
*.aab
*. dex
*.class

# Gradle
.gradle/
build/
local.properties

# IDE
.idea/
*. iml

# Database
*. db
*.db-shm
*.db-wal
app/src/main/assets/databases/*. db

# Keystore
*. jks
*.keystore

# Logs
*.log
```

### Commit Convention

```
<type>:  <subject>

feat: Add new feature
fix: Fix bug
docs: Update docs
style: Format code
refactor: Refactor
test: Add tests
chore:  Update deps
```

---

## 🤝 Contributing

### Quy trình

1. **Fork** repo
2. **Clone** về máy
3. **Create branch**:  `git checkout -b feature/amazing`
4. **Commit**: `git commit -m "feat: Add amazing feature"`
5. **Push**: `git push origin feature/amazing`
6. **Create Pull Request**

### Code Standards

- Java Naming Conventions
- 4 spaces indentation
- Max line:  120 chars
- Javadoc cho public methods
- Unit tests

### Commit Message

```
feat: Add student search
fix: Fix database connection
docs: Update README
style:  Format code
test: Add DAO tests
```

---

## 🗺️ Roadmap

### v1.1. 0
- [ ] Advanced search
- [ ] Export Excel/PDF
- [ ] Import from CSV

### v1.2.0
- [ ] Grade management
- [ ] Statistics
- [ ] Charts

### v2.0.0
- [ ] Server sync (REST API)
- [ ] Authentication
- [ ] Push notifications
- [ ] Dark mode

---

## 📄 License

MIT License

```
Copyright (c) 2025 NvkhoaDev54

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

## 👨‍💻 Tác giả

**NvkhoaDev54**

- 🐙 GitHub: [@NvkhoaDev54](https://github.com/NvkhoaDev54)
- 📧 Email: nvkhoadev54@example.com
- 💼 LinkedIn: [Your Profile](https://linkedin.com/in/yourprofile)

---

## 🙏 Cảm ơn

- [Android Developers](https://developer.android.com/)
- [SQLite](https://www.sqlite.org/)
- [Material Design](https://material.io/)
- [Stack Overflow](https://stackoverflow.com/)

---

## 📞 Liên hệ & Hỗ trợ

- 📧 Email: nvkhoadev54@example.com
- 🐛 Issues: [GitHub Issues](https://github.com/NvkhoaDev54/QLSV/issues)
- 💬 Discussions: [GitHub Discussions](https://github.com/NvkhoaDev54/QLSV/discussions)

---

<div align="center">

**⭐ Nếu thấy project hữu ích, hãy cho 1 star nhé!  ⭐**

Made with ❤️ by [NvkhoaDev54](https://github.com/NvkhoaDev54)

</div>