## TableRec

**#include [<sdk_document_ai_api.h>]()**

| **接口名称** | **功能** |
| ---- | :--- |
| [sdk_table_rec_create](#api_tablerec_c_sdk_table_rec_create) | 创建文字检测、识别，方向的句柄 |
| [sdk_table_rec_infer](#api_tablerec_c_sdk_table_rec_infer) | 使用句柄中包含的模型进行表格识别 |
| [sdk_table_rec_release_table_result](#api_tablerec_c_sdk_table_rec_release_table_result) | 释放表格识别的内存 |


<a id = 'api_tablerec_c_sdk_table_rec_create'>`sdk_table_rec_create` </a>

```c++
da_result_t sdk_table_rec_create(
  const char* config,da_handle_t* handle);
```

创建表格识别句柄，需要先添加license

**示例：**

```c++
char *tablerec_config = new char[1024]();
snprintf(ocr_config, 1024, R"({"%s":"%s","%s":"%s","%s":"%s"})",
         CONST_OCR_DET_MODEL_KEY, "DA_Ocr_Detection_xxxxx.model",
         CONST_OCR_REC_MODEL_KEY, "DA_Ocr_Recgnization_xxxxx.model",
         CONST_Layout_Analysis_MODEL_KEY, "DA_Layout_Analysis_xxxxx.model"
    );
da_handle_t tablerec_handle = nullptr;
da_result_t tablerec_create_result = sdk_table_rec_create(tablerec_config, &tablerec_handle);

if(tablerec_create_result == E_DA_SUCCESS){
  //create tablerec handle successful.
}else{
  //something went wrong.
}
```

**参数**

| **变量名** | **输入/输出** | **描述**           |
| ---------- | ------------- | ------------------ |
| config     | [in]          | 输入模型类别及模型路径配置信息 |
| handle     | [out]         | 输入OCR句柄                    |

**响应**

正常返回E_DA_SUCCESS，否则返回[错误类型](./cplus_general_type)


<a id = 'api_tablerec_c_sdk_table_rec_infer'>`sdk_table_rec_infer` </a>

```c++
da_result_t
sdk_table_rec_infer(
  const da_handle_t* handle,
  da_image_t* image, 
  da_table_t* table_result, 
  da_table_cell_t** table_cells_result, 
  unsigned int* cell_count);
```

表格识别

**示例：**

```c++
  da_table_t table_result;
  da_table_cell_t* table_cells_result;
  unsigned int cell_count;
  da_result_t rec_result_t = sdk_table_rec_infer(&tablerec_handle, &image,&table_result, &table_cells_result,&cell_count);
  if (rec_result_t == E_DA_SUCCESS){
    std::cout << "sdk_table_rec_infer result:" << "table_type: " << table_result.table_type << "row: " << table_result.row_count << "col: " << table_result.col_count << std::endl;
  }else {
    std::cout << "sdk_table_rec_infer response : " << rec_result_t << std::endl;
  }
```

**参数**

| **变量名** | **输入/输出** | **描述**           |
| ---------- | ------------- | ------------------ |
| handle     | [in]          | 已初始化的表格识别句柄  |
| image      | [in]          | 输入的image        |
| table_result | [out]       | 返回表格识别的结果 |
| table_cells_result | [out]       | 返回表格单元格识别的结果 |
| cell_count | [out]       | 返回表格单元格识别的个数 |

**响应**

正常返回E_DA_SUCCESS，否则返回[错误类型](./cplus_general_type)


<a id = 'api_tablerec_c_sdk_table_rec_release_table_result'>`sdk_table_rec_release_table_result` </a>

```c++
da_result_t sdk_table_rec_release_table_result(
  da_table_t table_result);
```

释放表格识别的内存

**参数**

| **变量名** | **输入/输出** | **描述**           |
| ---------- | ------------- | ------------------ |
| table_result     | [in]          | 表格识别的保存结果  |

**响应**

正常返回E_DA_SUCCESS，否则返回[错误类型](./cplus_general_type)
