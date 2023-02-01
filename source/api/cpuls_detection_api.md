## Detection

**#include [<sdk_document_ai_api.h>]()**

| **接口名称** | **功能** |
| ---- | :--- |
| [sdk_detection_create](#api_ocr_c_sdk_detection_create) | 根据模型创建目标检测句柄 |
| [sdk_detection_infer](#api_ocr_c_sdk_detection_infer) | 使用句柄中包含的模型进行目标检测 |
| [sdk_detection_release_result](#api_ocr_c_sdk_detection_release_result) | 释放目标检测的内存 |



<a id = 'api_ocr_c_sdk_detection_create'>`sdk_detection_create` </a>

```c++
da_result_t
sdk_detection_create(
  const char* config,
  da_handle_t* handle
);
```

创建目标检测的句柄，需要先添加license

**示例：**

```c++
char detection_config[1024] = {};
snprintf(detection_config, 1024, R"({"%s":"%s"})",
         CONST_LAYOUT_PARSER_MODEL_KEY,"DA_Layout_Parser_1.0.0.model"
        );
da_handle_t detection_handle = nullptr;
da_result_t detection_create_result = sdk_detection_create(detection_config,&detection_handle);

if(detection_create_result == E_DA_SUCCESS){
  //create detection handle successful.
}else{
  //something went wrong.
}
```

**参数**

| **变量名** | **输入/输出** | **描述**                       |
| ---------- | ------------- | ------------------------------ |
| config     | [in]          | 输入模型类别及模型路径配置信息 |
| handle     | [out]         | 输出句柄                       |

**响应**

正常返回E_DA_SUCCESS，否则返回[错误类型](./cplus_general_type)





<a id = 'api_ocr_c_sdk_detection_infer'>`sdk_detection_infer` </a>

```c++
da_result_t
sdk_detection_infer(
  da_handle_t* handle, 
  da_image_t *image,
  da_detection_t **detection_result,
  int *detection_count
);
```

目标检测推理

**示例：**

```c++
da_image_t image;
sdk_image_create("image.png", &image);
da_detection_t *detection_result = nullptr;
int detection_count;
da_result_t detection_infer_result = sdk_detection_infer(&detection_handle,&image,&detection_result,&detection_count);
if (detection_infer_result == E_DA_SUCCESS ){
  for (int i = 0; i < detection_count; ++i) {
    std::cout << "name : " << detection_result[i].object << " , confidence: " << detection_result[i].confidence <<
      ", position : [" << detection_result[i].position.left << ","
      << detection_result[i].position.top << ","
      << detection_result[i].position.right << ","
      << detection_result[i].position.bottom << "]" <<
      std::endl;
  }

  sdk_detection_release_result(detection_result);
}else {
  std::cout << "sdk_detection_infer result : " << layout_parser_result << std::endl;
}
```

**参数**

| **变量名**       | **输入/输出** | **描述**             |
| ---------------- | ------------- | -------------------- |
| handle           | [in]          | 已初始化的句柄       |
| image            | [in]          | 输入的image          |
| detection_result | [out]         | 返回目标检测的结果   |
| detection_count  | [out]         | 返回标检测结果的数量 |

**响应**

正常返回E_DA_SUCCESS，否则返回[错误类型](./cplus_general_type)



<a id = 'api_ocr_c_sdk_detection_release_result'>`sdk_detection_release_result` </a>

```c++
da_result_t
sdk_detection_release_result(
  da_detection_t *detection_result
);
```

释放目标检测的内存

**参数**

| **变量名**       | **输入/输出** | **描述**                                |
| ---------------- | ------------- | --------------------------------------- |
| detection_result | [in]          | 通过`sdk_detection_infer`输出的检测结果 |

**响应**

正常返回E_DA_SUCCESS，否则返回[错误类型](./cplus_general_type)