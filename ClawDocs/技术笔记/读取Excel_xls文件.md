# 读取 Excel .xls 文件

> 场景：需要读取老版本的 .xls 二进制格式 Excel 文件（非 .xlsx）

---

## 问题背景

Python 环境没有 xlrd/pandas 等库，无法直接读取 .xls 文件。

## 解决方案

使用 Node.js 的 `xlsjs` 库（别名 xlsx）。

### 安装

```bash
npm install xlsjs
```

### 读取代码

```javascript
const XLS = require('xlsjs');
const workbook = XLS.readFile('path/to/file.xls');

// 获取所有 sheet 名称
console.log('Sheets:', workbook.SheetNames);

// 读取第一个 sheet 并转为 CSV 格式
const sheet = workbook.Sheets[workbook.SheetNames[0]];
const data = XLS.utils.sheet_to_csv(sheet, {blankrows: false});
console.log(data);

// 也可以指定 sheet 名称
const sheet2 = workbook.Sheets['SheetName'];
```

### 遍历所有行

```javascript
const lines = data.split('\n');
for (let i = 0; i < lines.length; i++) {
    console.log(`Line ${i}:`, lines[i]);
}
```

---

## 应用场景

- 读取招标文件的附件（招标需求一览表等）
- 处理老旧 Excel 格式
- 批量提取表格数据

---

## 备选方案

### Python（如果有 xlrd）

```python
import xlrd
wb = xlrd.open_workbook('file.xls')
sheet = wb.sheet_by_name('Sheet1')
for row in range(sheet.nrows):
    print(sheet.row_values(row))
```

### 使用 ssconvert（Linux 命令行）

```bash
ssconvert input.xls output.csv
```