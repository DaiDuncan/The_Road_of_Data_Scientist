```c++
#include <regex>

```



## 简介

要创建正则表达式对象：

- 类模板 [Basic_regex 类](https://docs.microsoft.com/zh-cn/cpp/standard-library/basic-regex-class?view=msvc-160) 
- [regex](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#regex) 和 [wregex](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#wregex)之一
- 类型[regex_constants：： syntax_option_type](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-constants-class?view=msvc-160#syntax_option_type)的语法标志



要在文本中搜索正则表达式对象的匹配项：

- 模板函数 [regex_match](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-functions?view=msvc-160#regex_match) 和 [regex_search](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-functions?view=msvc-160#regex_search)
- 类型 [regex_constants：： match_flag_type](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-constants-class?view=msvc-160#match_flag_type)的匹配标志

这些函数通过使用类模板 [Match_results 类](https://docs.microsoft.com/zh-cn/cpp/standard-library/match-results-class?view=msvc-160) 及其专用化、 [cmatch](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#cmatch)、 [wcmatch](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#wcmatch)、 [smatch](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#smatch)和 [wsmatch](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#wsmatch)，以及类模板 [sub_match 类](https://docs.microsoft.com/zh-cn/cpp/standard-library/sub-match-class?view=msvc-160) 及其专用化、 [csub_match](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#csub_match) [wcsub_match](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#wcsub_match)、 [ssub_match](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#ssub_match)和 [wssub_match](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#wssub_match)，返回结果



要替换匹配正则表达式对象的文本：

- 模板函数 [regex_replace](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-functions?view=msvc-160#regex_replace)与类型 [regex_constants：： match_flag_type](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-constants-class?view=msvc-160#match_flag_type)的匹配标志一起使用



要循环访问正则表达式对象的多个匹配项：

- 使用类模板 [Regex_iterator 类](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-iterator-class?view=msvc-160) 和 [regex_token_iterator 类](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-token-iterator-class?view=msvc-160) 或其专用化、 [cregex_iterator](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#cregex_iterator)、 [sregex_iterator](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#sregex_iterator)、 [wcregex_iterator](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#wcregex_iterator)、 [wsregex_iterator](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#wsregex_iterator)、 [cregex_token_iterator](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#cregex_token_iterator)、 [sregex_token_iterator](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#sregex_token_iterator)、 [wcregex_token_iterator](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#wcregex_token_iterator)或 [wsregex_token_iterator](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#wsregex_token_iterator)，以及类型 [regex_constants：： match_flag_type](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-constants-class?view=msvc-160#match_flag_type)的匹配标志



# 类

| 类                                                           | 描述                                  |
| :----------------------------------------------------------- | :------------------------------------ |
| [basic_regex](https://docs.microsoft.com/zh-cn/cpp/standard-library/basic-regex-class?view=msvc-160) | 包装正则表达式。                      |
| [match_results](https://docs.microsoft.com/zh-cn/cpp/standard-library/match-results-class?view=msvc-160) | 包含一系列子匹配项。                  |
| [regex_constants](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-constants-class?view=msvc-160) | 包含各种类型的常量。                  |
| [regex_error](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-error-class?view=msvc-160) | 报告错误的正则表达式。                |
| [regex_iterator](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-iterator-class?view=msvc-160) | ==循环访问==匹配结果。                |
| [regex_traits](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-traits-class?view=msvc-160) | 描述用于匹配的元素的特征。            |
| [regex_traits](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-traits-char-class?view=msvc-160) | 描述用于匹配的的特征 **`char`** 。    |
| [regex_traits](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-traits-wchar-t-class?view=msvc-160) | 描述用于匹配的的特征 **`wchar_t`** 。 |
| [regex_token_iterator](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-token-iterator-class?view=msvc-160) | ==循环访问==子匹配项。                |
| [sub_match](https://docs.microsoft.com/zh-cn/cpp/standard-library/sub-match-class?view=msvc-160) | 介绍子匹配项。                        |





# 类型的定义

| 名称                                                         | 描述                                               |
| :----------------------------------------------------------- | :------------------------------------------------- |
| [cmatch](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#cmatch) | 的类型定义 **`char`** `match_results` 。           |
| [cregex_iterator](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#cregex_iterator) | 的类型定义 **`char`** `regex_iterator` 。          |
| [cregex_token_iterator](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#cregex_token_iterator) | 的类型定义 **`char`** `regex_token_iterator` 。    |
| [csub_match](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#csub_match) | 的类型定义 **`char`** `sub_match` 。               |
| [regex](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#regex) | 的类型定义 **`char`** `basic_regex` 。             |
| [smatch](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#smatch) | 的类型定义 `string` `match_results` 。             |
| [sregex_iterator](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#sregex_iterator) | 的类型定义 `string` `regex_iterator` 。            |
| [sregex_token_iterator](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#sregex_token_iterator) | 的类型定义 `string` `regex_token_iterator` 。      |
| [ssub_match](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#ssub_match) | 的类型定义 `string` `sub_match` 。                 |
| [wcmatch](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#wcmatch) | 的类型定义 **`wchar_t`** `match_results` 。        |
| [wcregex_iterator](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#wcregex_iterator) | 的类型定义 **`wchar_t`** `regex_iterator` 。       |
| [wcregex_token_iterator](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#wcregex_token_iterator) | 的类型定义 **`wchar_t`** `regex_token_iterator` 。 |
| [wcsub_match](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#wcsub_match) | 的类型定义 **`wchar_t`** `sub_match` 。            |
| [wregex](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#wregex) | 的类型定义 **`wchar_t`** `basic_regex` 。          |
| [wsmatch](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#wsmatch) | 的类型定义 `wstring` `match_results` 。            |
| [wsregex_iterator](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#wsregex_iterator) | 的类型定义 `wstring` `regex_iterator` 。           |
| [wsregex_token_iterator](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#wsregex_token_iterator) | 的类型定义 `wstring` `regex_token_iterator` 。     |
| [wssub_match](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-typedefs?view=msvc-160#wssub_match) | 的类型定义 `wstring` `sub_match` 。                |





# 函数

| 函数                                                         | 描述                                         |
| :----------------------------------------------------------- | :------------------------------------------- |
| [regex_match](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-functions?view=msvc-160#regex_match) | 与正则表达式完全匹配。                       |
| [regex_replace](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-functions?view=msvc-160#regex_replace) | **替换**匹配正则表达式。                     |
| [regex_search](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-functions?view=msvc-160#regex_search) | 搜索正则表达式匹配项。                       |
| [swap](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-functions?view=msvc-160#swap) | 交换 `basic_regex` 或 `match_results` 对象。 |





# 运算符

| 运算符                                                       | 描述                       |
| :----------------------------------------------------------- | :------------------------- |
| [operator = =](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-operators?view=msvc-160#op_eq_eq) | 比较各种对象，相等。       |
| [operator！ =](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-operators?view=msvc-160#op_neq) | 比较各种对象，不相等。     |
| [运算符<](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-operators?view=msvc-160#op_lt) | 比较各种对象，小于。       |
| [操作员<=](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-operators?view=msvc-160#op_gt_eq) | 比较各种对象，小于或等于。 |
| [运算符>](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-operators?view=msvc-160#op_gt) | 比较各种对象，大于。       |
| [运算符>=](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-operators?view=msvc-160#op_gt_eq) | 比较各种对象，大于或等于。 |
| [运算符<<](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex-operators?view=msvc-160#op_lt_lt) | 将 `sub_match` 插入流中。  |













# #参考文献

[Link: 微软|regex](https://docs.microsoft.com/zh-cn/cpp/standard-library/regex?view=msvc-160)