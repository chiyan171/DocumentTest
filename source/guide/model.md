## 模型介绍

DocumentAI中通常会用到一下模型，下面试对模型的简单介绍。

- ocr
  - 文字检测模型，命名规则为DA_Ocr_Detection_xxxxx.model，可得到文字的坐标。在OCR功能模块中使用
  - 文字识别模型，命名规则为DA_Ocr_Recgnization_xxxxx.model，可得到文字的信息。一般情况下，和文字检测模型联级使用，对检测到的文字坐标做文字识别。
  - 文字方向分类模型，命名规则为DA_Ocr_Detection_xxxxx.model。用于对文字识别模型的增强。和文字识别模型联级使用，若不需要，可不使用
  
- detection
  
  - 版面分析模型，命名规则为DA_Layout_Parser_xxxxx.model。对图片形式的文档进行区域划分，定位其中的关键区域，如表格、图片等
  
- image enhancement

  - 锐化并增强模型，命名规则为DA_Image_enhance_magic_color_xxxxx.model。 
  
- Table Recognition

  - 表格识别模型，命名规则为DA_Table_Recognition_xxxxx.model。 检测表格的每个单元格，可得到表格的行数，列数，及每个单元格的信息等等
  
  
  
  
  
  
  

  
  
  
  
  
  
  
  
  
  
  
  ​	