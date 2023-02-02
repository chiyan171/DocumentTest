## ImageProcess

**#include [<sdk_image_process_api.h>]()**

| **接口名称** | **功能** |
| ---- | :--- |
| [sdk_image_magic_color_create](#api_ocr_c_sdk_image_magic_color_create) | 创建对图像锐化并增强的句柄 |
| [sdk_image_magic_color_infer](#api_ocr_c_sdk_image_magic_color_infer) | 使用句柄中包含的模型进行图像锐化并增强 |
| [sdk_image_document_scan_create](#api_ocr_c_sdk_image_document_scan_create) | 创建文档切边矫正的句柄 |
| [sdk_image_document_scan_infer](#api_ocr_c_sdk_image_document_scan_infer) | 使用句柄中包含的模型进行文档切边矫正 |




<a id = 'api_ocr_c_sdk_image_magic_color_create'>`sdk_image_magic_color_create` </a>

```c++
da_result_t 
sdk_image_magic_color_create(
  const char* config, 
  da_handle_t* handle
);
```

创建图像锐化并增强句柄，需要先添加license

**示例：**

```c++
char config[2048] = {};
auto model_path = "model_save_path/xxxxx.model";
snprintf(config, 2048, R"({"%s":"%s"})",
          CONST_MAGIC_COLOR_MODEL_KEY, 
          model_path.c_str()
);

da_handle_t handle;
auto res = sdk_image_magic_color_create(config, &handle);
if(res == E_DA_SUCCESS){
  //create magic color handle successful.
}else{
  //something went wrong.
}
```

**参数**

| **变量名** | **输入/输出** | **描述**                       |
| ---------- | ------------- | ------------------------------ |
| config     | [in]          | 输入模型类别及模型路径配置信息 |
| handle     | [out]         | 输入magic color句柄                    |

**响应**

正常返回E_DA_SUCCESS，否则返回[错误类型](./cplus_general_type)


<a id = 'api_ocr_c_sdk_image_magic_color_infer'>`sdk_image_magic_color_infer` </a>

```c++
da_result_t 
sdk_image_magic_color_infer(
  da_handle_t* handle, 
  da_image_t* input_img, 
  da_image_t* output_img
);
```

图像锐化并增强

**示例：**

```c++
da_handle_t handle;
da_image_t image;
da_image_t out;
auto res = sdk_image_magic_color_infer(&handle, &image, &out);

if(res == E_DA_SUCCESS){
  //magic color predict successful.
}else{
  //something went wrong.
}

sdk_handle_release(&handle);
sdk_image_release(&image);
sdk_image_release(&out);
```

**参数**

| **变量名** | **输入/输出** | **描述**                       |
| ---------- | ------------- | ------------------------------ |
| handle     | [in]          | 已初始化的magci color句柄 |
| input_img     | [in]         | 输入图片                    |
| output_img     | [out]         | 输出图片                    |

**响应**

正常返回E_DA_SUCCESS，否则返回[错误类型](./cplus_general_type)


<a id = 'api_ocr_c_sdk_image_document_scan_create'>`sdk_image_document_scan_create` </a>

```c++
da_result_t 
sdk_image_document_scan_create(
  const char* config, 
  da_handle_t* handle
);
```

创建文档切边矫正句柄，需要先添加license

**示例：**

```c++
char config[2048] = {};
auto document_model_path = "model_save_path/xxxxx.model";
auto corner_model_path = "model_save_path/xxxxx.model";
snprintf(config, 2048, R"({"%s":"%s","%s":"%s"})",
         CONST_DOCUMENT_MODEL_KEY, document_model_path.c_str(),
         CONST_CORNER_MODEL_KEY, corner_model_path.c_str()
);

da_handle_t handle;
auto res = sdk_image_document_scan_create(config, &handle);
if(res == E_DA_SUCCESS){
  //create document scan handle successful.
}else{
  //something went wrong.
}
```

**参数**

| **变量名** | **输入/输出** | **描述**                       |
| ---------- | ------------- | ------------------------------ |
| config     | [in]          | 输入模型类别及模型路径配置信息 |
| handle     | [out]         | 输入document scan句柄                    |

**响应**

正常返回E_DA_SUCCESS，否则返回[错误类型](./cplus_general_type)


<a id = 'api_ocr_c_sdk_image_document_scan_infer'>`sdk_image_document_scan_infer` </a>

```c++
da_result_t 
sdk_image_document_scan_infer(
  da_handle_t*   handle, 
  da_image_t*    input_img, 
  da_document_t* output);
```

文档切边矫正

**示例：**

```c++
da_handle_t handle;
da_image_t image;
da_document_t doc_scan_output;
auto res = sdk_image_document_scan_infer(&handle, &image, &doc_scan_output);

if(res == E_DA_SUCCESS){
  //document scan predict successful.
}else{
  //something went wrong.
}

sdk_handle_release(&handle);
sdk_image_release(&doc_scan_output.image);
```

**参数**

| **变量名** | **输入/输出** | **描述**                       |
| ---------- | ------------- | ------------------------------ |
| handle     | [in]          | 已初始化的document scan句柄 |
| input_img     | [in]         | 输入图片                    |
| output     | [out]         | 输出图片内主体文档的位置信息（左上角，右上角，右下角，左下角）以及文档矫正后的图片                    |

**响应**

正常返回E_DA_SUCCESS，否则返回[错误类型](./cplus_general_type)