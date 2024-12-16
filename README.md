# Test Performance (CPU, RAM)

## 1. Test CPU

```bash
$i = 1; while ($true) { $i++ }

```

## 2. Test RAM

### Sử dụng tệp lớn để tải RAM:

```bash
fsutil file createnew testfile.txt 5000000000
```
Sử dụng Notepad++ để mở file này

### Sử dụng Python 

Liên tục thêm dữ liệu để đảm bảo sử dụng hết RAM:

```bash
big_list = []
try:
    while True:
        big_list.extend(["A" * 1024 * 1024] * 100) 
        print(f"Danh sach hien tai co {len(big_list)} phan tu.")
except MemoryError:
    print("Het bo nho!")
input("Nhan Enter de thoat...")

```

### Sử dụng PowerShell

```bash
Add-Type -TypeDefinition @"
using System;
public class MemoryHog {
    public static string[] GrowMemory(int size) {
        string[] largeArray = new string[size];
        for (int i = 0; i < size; i++) {
            largeArray[i] = new string('A', 1024 * 1024);  // 1MB of 'A'
        }
        return largeArray;
    }
}
"@

$bigList = @()
try {
    while ($true) {
        $bigList += [MemoryHog]::GrowMemory(100)  # Add 100 * 1MB strings
        [GC]::Collect()  # Force garbage collection to keep memory usage updated
        $usedMemory = [System.GC]::GetTotalMemory($false)
        Write-Host "Danh sach hien tai co $($bigList.Count) phan tu. Dung luong bo nho hien tai: $([math]::Round($usedMemory / 1MB, 2)) MB"
    }
} catch {
    Write-Host "Het bo nho!"
}
Read-Host "Nhan Enter de thoat..."

```


