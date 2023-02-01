## Windows中文路径编码

此文档提供一种在windows平台下解决传入中文路径时模型或者图片无法加载的问题的方法

此问题的本质为字符串的编码问题，我们只需要将传入的字符串进行编码格式的转换即可解决此问题，以下为一个简单的字符编码类的声明与实现以及使用实例代码。


**头文件**
```c++
#include <cstdint>
#include <iostream>

#ifdef _WIN
#include <windows.h>
#endif

#define DA_CODEPAGE_DefANSI 0

class UTF8Decoder {
public:
  UTF8Decoder() = default;
  ~UTF8Decoder() = default;

  explicit UTF8Decoder(const std::string& src_str);

  void Input(uint8_t byte);
  void AppendCodePoint(uint32_t ch);
  void ClearStatus() { m_PendingBytes = 0; }
  std::wstring GetResult() const { return m_Buffer; }

  std::string ToDefAnsi() const;

private:
  int m_PendingBytes = 0;
  uint32_t m_PendingChar = 0;
  std::wstring m_Buffer;
};
```

**源码**
```c++
UTF8Decoder::UTF8Decoder(const std::string& src_str)
{
  for (const auto& item : src_str)
    Input(item);
}

void UTF8Decoder::AppendCodePoint(uint32_t ch)
{
  m_Buffer.push_back(static_cast<wchar_t>(ch));
}

void UTF8Decoder::Input(uint8_t byte)
{
  if (byte < 0x80) {
    m_PendingBytes = 0;
    m_Buffer.push_back(byte);
  } else if (byte < 0xc0) {
    if (m_PendingBytes == 0) {
      return;
    }
    m_PendingBytes--;
    m_PendingChar |= (byte & 0x3f) << (m_PendingBytes * 6);
    if (m_PendingBytes == 0) {
      AppendCodePoint(m_PendingChar);
    }
  } else if (byte < 0xe0) {
    m_PendingBytes = 1;
    m_PendingChar = (byte & 0x1f) << 6;
  } else if (byte < 0xf0) {
    m_PendingBytes = 2;
    m_PendingChar = (byte & 0x0f) << 12;
  } else if (byte < 0xf8) {
    m_PendingBytes = 3;
    m_PendingChar = (byte & 0x07) << 18;
  } else if (byte < 0xfc) {
    m_PendingBytes = 4;
    m_PendingChar = (byte & 0x03) << 24;
  } else if (byte < 0xfe) {
    m_PendingBytes = 5;
    m_PendingChar = (byte & 0x01) << 30;
  } else {
    m_PendingBytes = 0;
  }
}

std::string UTF8Decoder::ToDefAnsi() const
{
  if (m_Buffer.empty())
    return {};

  auto new_len = WideCharToMultiByte(DA_CODEPAGE_DefANSI, 0, m_Buffer.data(), 
                                     m_Buffer.size(), nullptr, 0, nullptr, nullptr);

  std::string ansi_str;
  ansi_str.resize(new_len);
  WideCharToMultiByte(DA_CODEPAGE_DefANSI, 0, m_Buffer.data(), m_Buffer.size(),
                      &ansi_str[0], ansi_str.size(), nullptr, nullptr);
  return ansi_str;
}
```

**代码示例**
```c++
  auto chinese_path = "path/with/中文";
  UTF8Decoder decoder(chinese_path);
  auto path = decoder.ToDefAnsi();
  // now you can use this path string normally
  ...
```