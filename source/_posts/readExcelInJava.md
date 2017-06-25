---
title: Java读取Excel
date: 2017/6/25
---

引入jar包
```
<dependency>
	<groupId>org.apache.poi</groupId>
	<artifactId>poi</artifactId>
	<version>3.16</version>
</dependency>
<dependency>
	<groupId>org.apache.poi</groupId>
	<artifactId>poi-ooxml</artifactId>
	<version>3.16</version>
</dependency>
```

读取方法

```
public static List<Map<String, String>> readExcel(String filePath) throws Exception {
    String fileType = filepath.substring(filepath.lastIndexOf(".") + 1, filepath.length());
    InputStream is = null;
    Workbook wb = null;
    try {
        is = new FileInputStream(filepath);
         
        if (fileType.equals("xls")) {
            wb = new HSSFWorkbook(is);
        } else if (fileType.equals("xlsx")) {
            wb = new XSSFWorkbook(is);
        } else {
            throw new Exception("读取的不是excel文件");
        }
        
	List<Map<String, String>> result = new ArrayList<Map<String, String>>();// 对应excel文件

        //默认获取第一个sheet,如果需要获取全部sheet,再套一层循环
	Sheet sheet = wb.getSheetAt(0);

	List<String> titles = new ArrayList<String>();// 放置所有的标题
	int rowSize = sheet.getLastRowNum() + 1;
	for (int j = 0; j < rowSize; j++) {// 遍历行
		Row row = sheet.getRow(j);
		if (row == null) {// 略过空行
			continue;
		}
		int cellSize = row.getLastCellNum();// 行中有多少个单元格，也就是有多少列
		if (j == 0) {// 第一行是标题行
			for (int k = 0; k < cellSize; k++) {
				Cell cell = row.getCell(k);
				titles.add(cell.toString());
			}
		} else {// 其他行是数据行
			Map<String, String> rowMap = new HashMap<String, String>();// 对应一个数据行
			for (int k = 0; k < titles.size(); k++) {
				Cell cell = row.getCell(k);
				String key = titles.get(k);
				String value = null;
				if (cell != null) {
					value = cell.toString();
				}
				rowMap.put(key, value);
			}
			result.add(rowMap);
		}
	}

	return result;
	} catch (FileNotFoundException e) {
		throw e;
	} finally {
		if (wb != null) {
			wb.close();
		}
		if (is != null) {
			is.close();
		}
	}
}
```
