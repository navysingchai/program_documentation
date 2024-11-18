# documentation : database.class.php & program.class.php

## start
```
include('./backend/classes/Database.class.php');
include('./backend/classes/ProgramV2.class.php');
$Program = new Program();
```
> [!WARNING]
> warning the file name of class program we have two versions, the first version is Program.class.php and last version is ProgramV2.class.php, in this document will descript detail of "ProgramV2".

## insert 
have two styles.
### 1. create array associative array variable and variable (undata structure variable) then assign.
```
$tbl = "table_name";
$param = [];
$param['column_name'] = "value";
$param['column_name'] = "value";
$param['column_name'] = "value";
$insert = $Program->insert($tbl, $param);
```
### 2. assign associative array and variable in function.
```
$insert = $Program->insert(
  "table_name",
  [
    "column_name" => "value",
    "column_name" => "value",
    "column_name" => "value"
  ]
);
```

## handle last insert id.
```
$last_insert_id = $insert['lastInsertID'];
```

## update
have two styles.
### 1. create array associative array variable and variable (undata structure variable) then assign.

```
$sql = "UPDATE table_name SET column_name_1 = :param1, column_name_2 = 'hello' WHERE condition_column = :condition";
$param = [];
$param['param1'] = "value";
$param['condition'] = "condition_value";
$update = $Program->update($sql, $param);
```

### 2. assign associative array and variable in function.
```
$sql = "UPDATE table_name SET column_name_1 = :param1, column_name_2 = 'hello' WHERE condition_column = :condition";
$update = $Program->update(
  $sql,
  [
    'param1' => "value",
    'condition' => "condition_value"
  ]
);
```
> [!IMPORTANT]
> update and select method, some value can skip bind paramation.

## select for one row (haven't "where" condition).
```
$sql = "SELECT * FROM table_name";
$select = $Program->selectOne($sql);
```

## select for one row (have "where" condition).
```
$sql = "SELECT * FROM table_name WHERE column_id = :id AND isActive = 'Y'";
$param = []
$param['id'] = 12
$select = $Program->selectOne($sql, $param);
echo $select['column_name']
```
or
```
$sql = "SELECT * FROM table_name WHERE column_id = :id AND isActive = 'Y'";
$select = $Program->selectOne(
  $sql,
  [
    'id' => 12
  ]
);
echo $select['column_name']
```
> [!IMPORTANT]
> selectOne method if use "SELECT * FROM" it will return the first row
> Example for get the row that is most value "SELECT * FROM table_name ORDER BY id DESC".

## select for several row (haven't "where" condition).
```
$sql = "SELECT * FROM table_name";
$select = $Program->selectAll($sql);
if ($select) {  
  foreach ($select as $row) {
    echo $row['column_name']
  }
}
```
## select for several row (haven "where" condition).
```
$sql = "SELECT * FROM table_name WHERE column_id = :id AND isActive = 'Y'";
$param = [];
$param['id'] = 10;
$select = $Program->selectAll($sql, $param);
if ($select) {  
  foreach ($select as $row) {
    echo $row['column_name']
  }
}
```
or
```
$sql = "SELECT * FROM table_name WHERE column_id = :id AND isActive = 'Y'";
$select = $Program->selectAll(
  $sql,
  [
    'id' => 10
  ]
);
if ($select) {  
  foreach ($select as $row) {
    echo $row['column_name']
  }
}
```
> [!IMPORTANT]
> for all "select" method if it not found row in table. it's will return 'false'. so we can check it before use.

## upload file.
```
$File = $_FILES['file_name'];
$Path = "./<folder_name>/";
$AllowedExtensions = ['png', 'pdf', 'webp'];
$ReferenceName = "file name that need";
$uploadfile = $Program->UploadFile($File, $Path, $AllowedExtensions, $ReferenceName);
$file_name_after_upload = $uploadfile['filename'];
```
### descript of this code.
1. File : is the input type file in html, if the file that upload have an error it will show this exception.
```
'status' => 'error',
'type' => 'fileErr',
'errType' => 'fileErr'
```
2. Path : this method will check directory. so if it's not found the directory it will show this exception.
```
'status' => 'error',
'type' => 'path',
'msg' => 'not found path'
```
3. AllowedExtensions : use the map array for define type of file for need to upload,
> [!NOTE]
> we can use ***$AllowedExtensions = null*** for use default allow extensions ['jpg', 'jpeg', 'png', 'webp'].

4. ReferenceName : is the new file name after uploaded.
5. store method UploadFile and access key 'filename' for get file name after uploaded.
6. if upload file is fail (is not file error, not found directory, file type) it will show this exception.
```
'status' => 'error',
'type' => "upload",
'msg' => "upload"
```
7. if user choose wrong file type it will show this exception.
```
'status' => 'error',
'type' => 'fileType',
'msg' => 'fileType'
```
