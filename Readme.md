# Hướng dẫn cài môi trường Data Mining trên VS Code (Miniconda + Jupyter)

## 1. Tổng quan

| Thành phần | Vai trò | Phiên bản dùng |
|-----------|---------|----------------|
| VS Code | Trình soạn thảo, chạy notebook `.ipynb` | Bản mới nhất |
| Miniconda3 (Windows 64-bit) | Quản lý Python và môi trường ảo | Bản mới nhất |
| Python (trong môi trường) | Ngôn ngữ chạy code | **3.10** |
| pandas, numpy, matplotlib, seaborn | Xử lý và trực quan hóa dữ liệu | Cài qua conda |
| ipykernel | Cầu nối để VS Code chạy notebook | Cài qua conda |

---

## 2. Extension cần cài trong VS Code

Mở tab Extensions bằng `Ctrl + Shift + X`, tìm và cài:

### Bắt buộc

| Extension | Tác giả | Lý do |
|-----------|---------|-------|
| **Python** | Microsoft | Nhận diện môi trường Python/Conda, chọn interpreter, gợi ý code (đi kèm Pylance) |
| **Jupyter** | Microsoft | Mở và chạy file `.ipynb`, có nút **Select Kernel**, **Run Cell** |

> Thiếu hai extension này thì VS Code không chạy được notebook và không thấy nút chọn kernel.

### Nên cài thêm (không bắt buộc)

| Extension | Lý do |
|-----------|-------|
| **Rainbow CSV** | Tô màu các cột trong file CSV, dễ đọc file dùng dấu `;` hoặc `,` |
| **Data Wrangler** | Xem và làm sạch DataFrame bằng giao diện bảng |
| **GitHub Copilot** | Gợi ý code (nếu dùng) |

---

## 3. Miniconda: là gì, vì sao dùng

### Các lý do chọn
1. **Tránh lỗi thư viện trên Windows**: numpy, pandas, matplotlib chứa code C/C++ đã biên dịch. Cài bằng `pip` đôi khi lỗi thiếu DLL, còn conda đóng gói sẵn nên ít lỗi hơn.
2. **Môi trường ảo tách biệt**: mỗi dự án có Python và thư viện riêng, không ảnh hưởng nhau.
3. **Chọn phiên bản Python tùy ý**: chỉ cần `python=3.10`, không phải gỡ và cài lại Python của máy.
4. **Phổ biến trong Data Science / ML**: nhiều tài liệu hướng dẫn theo conda, quản lý tốt các thư viện nặng (PyTorch, TensorFlow...).

### Miniconda hay Anaconda?
- **Anaconda**: cài sẵn hàng trăm gói, nặng vài GB.
- **Miniconda**: chỉ có phần lõi, nhẹ, cần gói nào thì cài gói đó.

### Vì sao chọn Python 3.10?
Phiên bản ổn định, tương thích tốt với hầu hết thư viện Machine Learning, Recommender System và Deep Learning hiện nay.

> **Lưu ý:** Miniconda mới nhất có thể cài sẵn Python 3.14 ở môi trường `base`. Điều này bình thường, vì ta không dùng `base` mà tạo môi trường riêng với Python 3.10.

### Không muốn dùng Conda?
Có thể dùng `venv` + `pip`:

```bash
python -m venv datamining_env
datamining_env\Scripts\activate
pip install pandas numpy matplotlib seaborn ipykernel
```

---

## 4. Cài đặt Miniconda

1. Tải bản **Miniconda3 Windows 64-bit** (file `.exe`) từ trang chủ Miniconda: <https://docs.conda.io/en/latest/miniconda.html>
2. Chạy file cài đặt:
   - **Next** → **I Agree** → **Just Me** → **Next**.
   - **Destination Folder**: để mặc định → **Next**.
   - Màn hình **Advanced Options**:
     - Tích **"Add Miniconda3 to my PATH environment variable"**.
     - Tích **"Register Miniconda3 as my default Python 3.x"**.
   - **Install** → **Finish**.
3. Mở terminal mới và kiểm tra:

```bash
conda --version
```

---

## 5. Tạo môi trường và cài thư viện

### 5.1. Tạo môi trường

```bash
conda create -n datamining_env python=3.10 -y
```

### 5.2. Cài thư viện (khuyên dùng `-n`)

Dùng `-n tên_env` để chắc chắn cài đúng môi trường, kể cả khi `conda activate` chưa chạy được:

```bash
conda install -n datamining_env pandas numpy matplotlib seaborn ipykernel -y
```

> `jupyter` (JupyterLab, Qt...) khá nặng và **không cần** nếu chỉ chạy notebook trong VS Code. Chỉ cần `ipykernel`.

### 5.3. Kiểm tra đã cài đúng chỗ chưa

```bash
conda list -n datamining_env ipykernel
```

Dòng đầu phải là `...\miniconda3\envs\datamining_env` và build có `py310`.

### 5.4. Kích hoạt môi trường (khi dùng terminal)

```bash
conda activate datamining_env
```

Đầu dòng terminal sẽ hiện `(datamining_env)`.

---

## 6. Sửa lỗi `conda activate` trong PowerShell

Chạy **một lần**:

```powershell
conda init powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

- `conda init powershell`: cho PowerShell nhận lệnh `conda activate`.
- `Set-ExecutionPolicy ...`: cho phép PowerShell chạy script cấu hình của conda (chỉ áp dụng cho tài khoản của bạn, không cần quyền Admin).

Sau đó **đóng hẳn VS Code, mở lại**. Hoặc đổi terminal mặc định sang **Command Prompt**:
`Ctrl + Shift + P` → *Terminal: Select Default Profile* → *Command Prompt*.

---

## 7. Dùng môi trường trong VS Code

### Chọn kernel cho notebook
1. Mở **cả folder project** (File → Open Folder), rồi mở file `.ipynb`.
2. Bấm **Select Kernel** (góc trên bên phải notebook).
3. Chọn **Python Environments** → `datamining_env`.

### Chạy cell

| Thao tác | Phím tắt / Nút |
|----------|----------------|
| Chạy cell, đứng yên tại chỗ | `Ctrl + Enter` |
| Chạy cell, nhảy xuống cell kế | `Shift + Enter` |
| Chạy cell, tạo cell mới bên dưới | `Alt + Enter` |
| Chạy cell bằng chuột | Nút ▷ bên trái cell |
| Chạy tất cả | **Run All** trên thanh công cụ |
| Chạy các cell phía trên | **Execute Above** |

> Các cell phụ thuộc nhau, nên chạy theo thứ tự từ trên xuống (cell `import` trước, rồi mới tới `read_csv`).

### Cell import mẫu và kiểm tra

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

print("pandas:", pd.__version__)
print("numpy:", np.__version__)
```

---

## 8. Đọc dataset bằng đường dẫn tương đối

Làm việc nhóm nên dùng **đường dẫn tương đối** (đường dẫn kiểu `C:\Users\...` chỉ chạy trên máy người viết).

### Cấu trúc thư mục đề xuất

```
lab/
├── datamining-project.ipynb
├── data/
│   └── bank.csv
└── lab4/
    ├── dataset3.csv
    ├── dataset41.csv
    └── dataset42.csv
```

### Ví dụ

```python
df = pd.read_csv("data/bank.csv", sep=";")   # file phân tách bằng dấu ;
df = pd.read_csv("lab4/dataset3.csv")        # file phân tách bằng dấu ,
```

### Quy tắc cho cả nhóm
- Thống nhất cấu trúc thư mục (ví dụ dữ liệu để trong `data/`).
- Luôn mở **cả folder** `lab` trong VS Code.
- Dataset lớn thì không commit lên Git: thêm `data/` vào `.gitignore` và ghi link tải trong README.
- Có thể dùng `pathlib` cho an toàn giữa các hệ điều hành:

```python
from pathlib import Path
DATA_DIR = Path("data")
df = pd.read_csv(DATA_DIR / "bank.csv", sep=";")
```

### Kiểm tra khi báo `FileNotFoundError`

```python
import os
print(os.getcwd())
print(os.listdir())
```

> **Chú ý:** code tải từ Kaggle có đường dẫn `/kaggle/input/...` và `import kagglehub` chỉ chạy được trên Kaggle. Khi chạy local, hãy xóa cell mẫu của Kaggle và đổi sang đường dẫn local. Dùng `Ctrl + F` tìm chữ `kaggle` trong notebook để sửa hết.

---

## 9. Các lỗi đã gặp và cách xử lý

| Lỗi | Nguyên nhân | Cách xử lý |
|-----|-------------|------------|
| `Running cells ... requires the ipykernel package` | `ipykernel` chưa cài vào `datamining_env` (thường cài nhầm vào `base`) | `conda install -n datamining_env ipykernel -y`, rồi đóng mở lại VS Code |
| `conda list` hiện `environment at ...\miniconda3` (không có `envs\datamining_env`) | `conda activate` chưa thành công, đang ở `base` | Làm theo mục 6, hoặc luôn dùng `-n datamining_env` |
| `IncompleteRead` / `Connection broken` khi cài | Mạng bị đứt lúc tải gói | Chạy lại lệnh; hoặc `conda clean --packages --tarballs -y` rồi chạy lại; hoặc cài từng nhóm nhỏ; bỏ `jupyter` cho nhẹ |
| `ModuleNotFoundError: No module named 'kagglehub'` | Cell mẫu của Kaggle chạy trên máy local | Xóa cell đó, không cần cài kagglehub |
| `FileNotFoundError` khi `read_csv` | Đường dẫn sai hoặc notebook đứng sai thư mục | Dùng đường dẫn tương đối, kiểm tra bằng `os.getcwd()` |
| `NameError: name 'pd' is not defined` | Chưa chạy cell import | Chạy cell `import pandas as pd` trước |
| `conda is not recognized` | Chưa nhận PATH | Mở **Anaconda Prompt**, chạy `conda init powershell` và `conda init cmd.exe`, rồi mở lại VS Code |
| Lỗi "Terms of Service" | Conda yêu cầu chấp nhận điều khoản | `conda tos accept` |
| Không thấy `datamining_env` trong danh sách kernel | Kernel chưa đăng ký | `python -m ipykernel install --user --name datamining_env` rồi mở lại VS Code |

---

## 10. Các lệnh Conda hữu ích

| Lệnh | Công dụng |
|------|-----------|
| `conda env list` | Xem danh sách môi trường |
| `conda activate <tên_env>` | Kích hoạt môi trường |
| `conda deactivate` | Thoát môi trường hiện tại |
| `conda install -n <tên_env> <gói>` | Cài gói vào đúng môi trường |
| `conda list -n <tên_env>` | Xem các gói đã cài |
| `conda env remove -n <tên_env>` | Xóa môi trường |

> Gói không có trên conda thì dùng `python -m pip install <gói>` trong môi trường đã activate, nhưng nên ưu tiên `conda install` trước để tránh xung đột.

---

## 11. Mở rộng: Recommender System và Deep Learning

Nên **tạo môi trường riêng** để tránh xung đột phiên bản:

```bash
conda create -n dl_env python=3.10 -y
conda install -n dl_env pandas numpy matplotlib seaborn ipykernel scikit-learn -y
```

| Nhóm | Thư viện gợi ý | Ghi chú |
|------|----------------|---------|
| Nền tảng | `scikit-learn`, `scipy` | Ma trận thưa, SVD, NMF, độ tương đồng |
| Recommender System | `scikit-surprise`, `implicit`, `recbole` | Trên Windows nên cài `scikit-surprise` từ `conda-forge`; `lightfm` hay lỗi, có thể thử trong WSL2 |
| Deep Learning | **PyTorch** | Lấy đúng lệnh cài tại <https://pytorch.org/get-started/locally/> |

- TensorFlow từ bản 2.11 không còn hỗ trợ GPU trên Windows native (cần WSL2). Nếu chưa bắt buộc, nên dùng PyTorch.
- Kiểm tra GPU:

```python
import torch
print(torch.__version__)
print("CUDA available:", torch.cuda.is_available())
```