## 通用接口

**#include [<sdk_document_ai_api.h>]()**

| **接口名称** | **功能** |
| ---- | :--- |
| [sdk_add_license](#api_general_sdk_add_license) | 添加license字符串 |
| [sdk_add_license_file](#api_general_sdk_add_license_file) | 添加license文件 |
| [sdk_version](#api_general_sdk_version) | 获取sdk版本号 |
| [sdk_model_info](#api_general_sdk_model_info) | 获取模型信息 |
| [sdk_image_create](#api_general_sdk_image_create) | 根据图片路径创建图像指针并分配内存 |
| [sdk_image_create_by_buffer](#api_general_sdk_image_create_by_buffer) | 根据图片数据创建图像指针并分配内存 |
| [sdk_image_crop](#api_general_sdk_image_crop) | 图像扣取 |
| [sdk_image_save](#api_general_sdk_image_save) | 图像保存 |
| [sdk_image_release](#api_general_sdk_image_release) | 释放图像数据 |
| [sdk_handle_release](#api_general_sdk_handle_release) | 释放模型句柄 |
|  |  |

<br /><br /><br />
<a id = 'api_general_sdk_add_license'>`sdk_add_license` </a>

```c++
da_result_t
sdk_add_license(
  const char* license
);
```

设置license。若license不正确，所有非通用的接口都不能正常使用

**示例：**

```c++
da_result_t add_license_result = sdk_add_license("xxxxxxxxxxxx");
if(add_license_result == E_DA_SUCCESS){
  //add license successful.
}else{
  //something went wrong.
}
```

**参数**

| **变量名** | **输入/输出** | **描述**      |
| ---------- | ------------- | ------------- |
| license    | [in]          | License字符串 |

**响应**

正常返回E_DA_SUCCESS，否则返回[错误类型](./cplus_general_type)

<br /><br /><br />
<a id = 'api_general_sdk_add_license_file'>`sdk_add_license_file` </a>

```c++
da_result_t sdk_add_license_file(const char* license_path);
```

设置license。若license不正确，所有非通用的接口都不能正常使用

**示例：**

```c++
da_result_t add_license_result = sdk_add_license("license_file_path");
if(add_license_result == E_DA_SUCCESS){
  //add license successful.
}else{
  //something went wrong.
}
```

**参数**

| **变量名** | **输入/输出** | **描述**      |
| ---------- | ------------- | ------------- |
| license_path    | [in]          | License文件路径 |

**响应**

正常返回E_DA_SUCCESS，否则返回[错误类型](./cplus_general_type)


<br /><br /><br />
<a id = 'api_general_sdk_version'>`sdk_version` </a>

```c++
da_result_t
sdk_version(
  char* version
);
```

获取SDK版本号

**示例：**

```c++
char version[10];
sdk_version(version);
std::cout << "sdk version :" << std::string(version) << std::endl;
```



**参数**

| **变量名** | **输入/输出** | **描述**    |
| ---------- | ------------- | ----------- |
| version    | [out]         | SDK版本指针 |

**响应**

正常返回E_DA_SUCCESS，否则返回[错误类型](./cplus_general_type)


<br /><br /><br />
<a id = 'api_general_sdk_model_info'>`sdk_model_info` </a>

```c++
da_result_t
sdk_model_info(
  const char* model_path,
  char* model_info
);
```

获取模型信息，调用前需要验证license

**示例：**

```c++
char model_info[256];
da_result_t acquire_model_version_result = sdk_model_info("xxxxxxxxxx.model",model_info);
if (acquire_model_version_result == E_DA_SUCCESS){
  std::cout << "model info:" << std::string(model_info) << std::endl;
}else {
  //something went wrong.
}
```



**参数**

| **变量名**    | **输入/输出** | **描述**     |
| ------------- | ------------- | ------------ |
| model_path    | [in]          | 模型路径     |
| model_info | [out]         | 模型信息指针 |

**响应**

正常返回E_DA_SUCCESS，否则返回[错误类型](./cplus_general_type)


<br /><br /><br />
<a id = 'api_general_sdk_image_create'>`sdk_image_create` </a>

```c++
da_result_t
sdk_image_create(
  const char* image_path,
  da_image_t *image
);
```

根据图片路径创建图像指针并分配内存。若不使用，需要[释放](#api_general_sdk_image_release)该指针，否则会内存泄漏

**示例：**

```c++
da_image_t image;
da_result_t create_image_result = sdk_image_create("xxxxxxx.png", &image);
if(create_image_result == E_DA_SUCCESS){
  //create image successful
}else {
  //something went wrong.
}
```

**参数**

| **变量名** | **输入/输出** | **描述**             |
| ---------- | ------------- | -------------------- |
| image_path | [in]          | 图片路径             |
| image      | [out]         | 输出图像指针所在地址 |

**响应**

正常返回E_DA_SUCCESS，否则返回[错误类型](./cplus_general_type)


<br /><br /><br />
<a id = 'api_general_sdk_image_create_by_buffer'>`sdk_image_create_by_buffer` </a>

```c++
da_result_t
sdk_image_create_by_buffer(
  int width,
  int height,
  void* data,
  da_pixel_format_e format,
  da_image_t *image
);
```

根据图片路径创建图像指针并分配内存。若不使用，需要[释放](#api_general_sdk_image_release)该指针，否则会内存泄漏

**示例：**

```c++
da_image_t image;
da_result_t create_image_result = sdk_image_create_by_buffer(32,32,image_data_point,DA_PIX_FMT_BGR888, &image);
if(create_image_result == E_DA_SUCCESS){
  //create image successful
}else {
  //something went wrong.
}
```

**参数**

| **变量名** | **输入/输出** | **描述**             |
| ---------- | ------------- | -------------------- |
| width      | [in]          | 图片宽度             |
| height     | [in]          | 输出图像指针所在地址 |
| data       | [in]          | 图片数据             |
| format     | [in]          | 图片格式             |
| image      | [out]         | 输出图像指针所在地址 |

**响应**

正常返回E_DA_SUCCESS，否则返回[错误类型](./cplus_general_type)


<br /><br /><br />
<a id = 'api_general_sdk_image_crop'>`sdk_image_crop` </a>

```c++
da_result_t
sdk_image_crop(
  const da_image_t* image_src,
  const da_rect_t *crop_area,
  da_image_t *image_dst
);
```

图像扣取。若不使用，需要[释放](#api_general_sdk_image_release)该指针，否则会内存泄漏

**示例：**

```c++
da_image_t image;
sdk_image_create("xxxxx.png", &image);
da_rect_t rect = {left,top,right,bottom};
da_image_t crop_image;
da_result_t crop_image_result = sdk_image_crop(&image, &rect, &crop_image);
if(crop_image_result == E_DA_SUCCESS){
  //crop image successful
}else {
  //something went wrong.
}
```

**参数**

| **变量名** | **输入/输出** | **描述**             |
| ---------- | ------------- | -------------------- |
| image_src  | [in]          | 输入图像指针所在地址 |
| crop_area  | [in]          | 扣图区域             |
| image_dst  | [out]         | 输出图像指针所在地址 |

**响应**

正常返回E_DA_SUCCESS，否则返回[错误类型](./cplus_general_type)


<br /><br /><br />
<a id = 'api_general_sdk_image_save'>`sdk_image_save` </a>

```c++
da_result_t
sdk_image_save(
  const da_image_t* image,
  const char* save_path
);
```

图像保存到指定路径，使用的程序需要在该路径有读写权限

**示例：**

```c++
da_image_t image;
sdk_image_create("xxxxx.png", &image);
da_result_t save_image_result = sdk_image_save(&image,"xxxxxxx.png");
if(save_image_result == E_DA_SUCCESS){
  //save image successful
}else {
  //something went wrong.
}
```

**参数**

| **变量名** | **输入/输出** | **描述**             |
| ---------- | ------------- | -------------------- |
| image      | [in]          | 输入图像指针所在地址 |
| save_path  | [in]          | 图片保存的路径       |

**响应**

正常返回E_DA_SUCCESS，否则返回[错误类型](./cplus_general_type)


<br /><br /><br />
<a id = 'api_general_sdk_image_release'>`sdk_image_release` </a>

```c++
da_result_t
sdk_image_release(
  const da_image_t *image
);
```

释放图像数据

**示例：**

```c++
da_image_t image;
// do something with image and release it final
da_result_t release_image_result = sdk_image_release(&image);
if(release_image_result == E_DA_SUCCESS){
  //release image successful
}else {
  //something went wrong.
}
```

**参数**

| **变量名** | **输入/输出** | **描述**             |
| ---------- | ------------- | -------------------- |
| image      | [in]          | 输入图像指针所在地址 |

**响应**

正常返回E_DA_SUCCESS，否则返回[错误类型](./cplus_general_type)


<br /><br /><br />
<a id = 'api_general_sdk_handle_release'>`sdk_handle_release` </a>

```c++
da_result_t
sdk_handle_release(
  const da_handle_t* handle
);
```

释放已初始化的句柄

**示例：**

```c++
da_handle_t handle = nullptr;
// do something with handle and release it final
da_result_t handle_release_result = sdk_handle_release(&handle);
if(handle_release_result == E_DA_SUCCESS){
  //release handle successful
}else {
  //something went wrong.
}
```

**参数**

| **变量名** | **输入/输出** | **描述**       |
| ---------- | ------------- | -------------- |
| handle     | [in]          | 已初始化的句柄 |

**响应**

正常返回E_DA_SUCCESS，否则返回[错误类型](./cplus_general_type)