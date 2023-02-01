## OCR

**#include [<sdk_document_ai_api.h>]()**

| **接口名称** | **功能** |
| ---- | :--- |
| [sdk_ocr_ocr_create](#api_ocr_c_sdk_ocr_ocr_create) | 创建文字检测、识别，方向的句柄 |
| [sdk_ocr_ocr_infer](#api_ocr_c_sdk_ocr_ocr_infer) | 使用句柄中包含的模型进行文字的检测和识别（OCR全功能,根据输入的模型） |
| [sdk_ocr_det_infer](#api_ocr_c_sdk_ocr_det_infer) | 使用文字检测模型进行文字检测（OCR单功能） |
| [sdk_ocr_rec_infer](#api_ocr_c_sdk_ocr_rec_infer) | 使用文字识别模型进行文字识别（OCR单功能） |
| [sdk_ocr_det_release_result](#api_ocr_c_sdk_ocr_det_release_result) | 释放OCR检测的内存 |
| [sdk_ocr_rec_release_result](#api_ocr_c_sdk_ocr_rec_release_result) | 释放OCR识别的内存 |



<a id = 'api_ocr_c_sdk_ocr_ocr_create'>`sdk_ocr_ocr_create` </a>

```c++
da_result_t
sdk_ocr_ocr_create(
  const char* config,
  da_handle_t* handle
);
```

创建OCR句柄，需要先添加license

**示例：**

```c++
char *ocr_config = new char[1024]();
snprintf(ocr_config, 1024, R"({"%s":"%s","%s":"%s","%s":"%s"})",
         CONST_OCR_DET_MODEL_KEY, "DA_Ocr_Detection_xxxxx.model",
         CONST_OCR_REC_MODEL_KEY, "DA_Ocr_Recgnization_xxxxx.model",
         CONST_OCR_CLS_MODEL_KEY, "DA_Ocr_Cls_xxxxx.model"
    );
da_handle_t ocr_handle = nullptr;
da_result_t ocr_create_result = sdk_ocr_ocr_create(ocr_config, &ocr_handle);

if(ocr_create_result == E_DA_SUCCESS){
  //create ocr handle successful.
}else{
  //something went wrong.
}
```

**参数**

| **变量名** | **输入/输出** | **描述**                       |
| ---------- | ------------- | ------------------------------ |
| config     | [in]          | 输入模型类别及模型路径配置信息 |
| handle     | [out]         | 输入OCR句柄                    |

**响应**

正常返回E_DA_SUCCESS，否则返回[错误类型](./cplus_general_type)



<a id = 'api_ocr_c_sdk_ocr_ocr_infer'>`sdk_ocr_ocr_infer` </a>

```c++
da_result_t
sdk_ocr_ocr_infer(
  const da_handle_t* handle,
  da_image_t *image,
  da_ocr_det_t **det_result,
  da_ocr_rec_t **rec_result,
  int *result_count
);
```

OCR检测及识别

**示例：**

```c++
da_ocr_det_t *ocr_det_result = nullptr;
da_ocr_rec_t *ocr_rec_result = nullptr;
int ocr_result_count = 0;
da_result_t ocr_result_t = sdk_ocr_ocr_infer(&ocr_handle,&image,&ocr_det_result,&ocr_rec_result,&ocr_result_count);

if (ocr_result_t == E_DA_SUCCESS){
  for (int i = 0; i < ocr_result_count; ++i) {
    std::cout << "sdk_ocr_ocr_infer result [ " << i << "] Text:" << ocr_rec_result[i].text << ", confidence:"
                << ocr_rec_result[i].confidence << ", position:" <<  ocr_det_result[i] << std::endl;
  }
}else {
  std::cout << "sdk_ocr_ocr_infer response : " << ocr_result_t << std::endl;
}
```

**参数**

| **变量名**   | **输入/输出** | **描述**                                         |
| ------------ | ------------- | ------------------------------------------------ |
| handle       | [in]          | 已初始化的OCR句柄                                |
| image        | [in]          | 输入的image                                      |
| det_result   | [out]         | 返回文字检测的结果                               |
| rec_result   | [out]         | 返回文字识别的结果                               |
| result_count | [out]         | 返回结果的数量，文字检测和识别的结果数量绝对一致 |

**响应**

正常返回E_DA_SUCCESS，否则返回[错误类型](./cplus_general_type)



<a id = 'api_ocr_c_sdk_ocr_det_infer'>`sdk_ocr_det_infer` </a>

```c++
da_result_t
sdk_ocr_det_infer(
  const da_handle_t* handle,
  da_image_t *image,
  da_ocr_det_t **det_result,
  int *det_result_count
);
```

OCR检测

**示例：**

```c++
int det_result_count = 0;
da_ocr_det_t *det_result = nullptr;
da_result_t ocr_det_infer_result = sdk_ocr_det_infer(&ocr_det_handle, &image, &det_result,&det_result_count);

if (ocr_result_t == E_DA_SUCCESS){
  std::cout << "det infer count : " << det_result_count << std::endl;
  for (int i = 0; i < det_result_count; ++i) {
    std::cout << "position:" <<  det_result[i] << std::endl;
  }
}else {
  std::cout << "sdk_ocr_det_infer response : " << ocr_det_infer_result << std::endl;
}
```

**参数**

| **变量名**       | **输入/输出** | **描述**           |
| ---------------- | ------------- | ------------------ |
| handle           | [in]          | 已初始化的OCR句柄  |
| image            | [in]          | 输入的image        |
| det_result       | [out]         | 返回文字检测的结果 |
| det_result_count | [out]         | 返回结果的数量     |

**响应**

正常返回E_DA_SUCCESS，否则返回[错误类型](./cplus_general_type)



<a id = 'api_ocr_c_sdk_ocr_rec_infer'>`sdk_ocr_rec_infer` </a>

```c++
da_result_t
sdk_ocr_rec_infer(
  da_handle_t* handle,
  da_image_t *image,
  da_ocr_rec_t *rec_result
);
```

OCR检测

**示例：**

```c++
da_ocr_rec_t rec_result;
da_result_t rec_result_t = sdk_ocr_rec_infer(&ocr_rec_handle,&image,&rec_result);
if (rec_result_t == E_DA_SUCCESS){
  std::cout << "sdk_ocr_rec_infer result:" << std::string(rec_result.text) << ",confidence: " << rec_result.confidence << std::endl;
}else {
  std::cout << "sdk_ocr_rec_infer response : " << rec_result_t << std::endl;
}
```

**参数**

| **变量名** | **输入/输出** | **描述**           |
| ---------- | ------------- | ------------------ |
| handle     | [in]          | 已初始化的OCR句柄  |
| image      | [in]          | 输入的image        |
| rec_result | [out]         | 返回文字检测的结果 |

**响应**

正常返回E_DA_SUCCESS，否则返回[错误类型](./cplus_general_type)





<a id = 'api_ocr_c_sdk_ocr_det_release_result'>`sdk_ocr_det_release_result` </a>

```c++
da_result_t
sdk_ocr_det_release_result(
  da_ocr_det_t *det_result
);
```

释放OCR检测的内存



**参数**

| **变量名** | **输入/输出** | **描述**                                                   |
| ---------- | ------------- | ---------------------------------------------------------- |
| det_result | [in]          | 通过`sdk_ocr_det_infer`和`sdk_ocr_ocr_infer`输出的检测结果 |

**响应**

正常返回E_DA_SUCCESS，否则返回[错误类型](./cplus_general_type)



<a id = 'api_ocr_c_sdk_ocr_rec_release_result'>`sdk_ocr_rec_release_result` </a>

```c++
da_result_t
sdk_ocr_rec_release_result(
  da_ocr_rec_t *rec_result
);
```

释放OCR识别的内存



**参数**

| **变量名** | **输入/输出** | **描述**                              |
| ---------- | ------------- | ------------------------------------- |
| rec_result | [in]          | 通过`sdk_ocr_ocr_infer`输出的检测结果 |

**响应**

正常返回E_DA_SUCCESS，否则返回[错误类型](./cplus_general_type)





**完整全功能示例**

```c++
///add license for sdk
std::string license_string = "xxxxxxxxxxxxxxxxxxxxx";

da_result_t add_license_result = sdk_add_license(license_string.c_str());
if(add_license_result == E_DA_SUCCESS)
{
  //add license successful.
}else 
{
  std::cout << "license initialization error : " << add_license_result << std::endl;
}

///create ocr handle
char ocr_config[2048] = {};
snprintf(ocr_config, 2048, R"({"%s":"%s","%s":"%s","%s":"%s"})",
         CONST_OCR_DET_MODEL_KEY,
         "DA_Ocr_Detection_chinese_1.0.0.model"
         ,CONST_OCR_REC_MODEL_KEY,
         "DA_Ocr_Recognition_chinese_1.0.0.model"
         ,CONST_OCR_CLS_MODEL_KEY,
         "DA_Ocr_Classification_1.0.0.model"
        );

da_handle_t ocr_handle = nullptr;
da_result_t ocr_create_result = sdk_ocr_ocr_create(ocr_config, &ocr_handle);
delete 
if(ocr_create_result != E_DA_SUCCESS)
{
  std::cout << "create ocr failed :" << ocr_create_result << std::endl;
  return;
}

///create predict image
da_image_t image;
sdk_image_create("image.png", &image);

da_ocr_det_t *ocr_det_result = nullptr;
da_ocr_rec_t *ocr_rec_result = nullptr;
int ocr_result_count = 0;
da_result_t ocr_result_t = sdk_ocr_ocr_infer(&ocr_handle,&image,&ocr_det_result,&ocr_rec_result,&ocr_result_count);
if (ocr_result_t == E_DA_SUCCESS)
{
  for (int i = 0; i < ocr_result_count; ++i) 
  {
    std::cout << "sdk_ocr_ocr_infer result [ " << i << "] Text:" << ocr_rec_result[i].text << ", confidence:"
      << ocr_rec_result[i].confidence << std::endl ;
  }
}else
{
  std::cout << "sdk_ocr_ocr_infer failed : " << ocr_result_t << std::endl;
}

///release image resouces, when you no longer use them
sdk_image_release(&image);

///release handle resouces, when you no longer use them
da_result_t handle_release_result = sdk_handle_release(&ocr_handle);
std::cout << "sdk_handle_release result : " << handle_release_result << std::endl;

///release ocr infer result, when you no longer use them
sdk_ocr_det_release_result(ocr_det_result);
sdk_ocr_rec_release_result(ocr_rec_result);

```

