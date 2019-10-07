# 类型

## 1.1 类型系统

AMQP类型系统定义了一组用于数据互操作的常用基本类型。通过基本类型定义的AMQP值可以被额外的语义信息注解为更复杂的类型。通过注解的形式，可以将非AMQP基本类型表示的外部类型与AMQP基本类型相关联。例如，URL通常表示为字符串，但是并非所有字符串都是有效的URL，并且许多编程语言或应用程序需要特定的类型来表示URL。 当一个值表示URL时，AMQP类型系统允许定义相关的注解代码来描述该值。

### 1.1.1 基本类型（Primitive Types）

有如下的基本类型：

|  类型   |  描述  |
|  ----  | ----  |
| null  | 空值 |
| boolean | 单元格 |
| ubyte | 0 到 2<sup>8</sup> - 1，闭区间 |
| ushort | 0 到 2<sup>16</sup> - 1，闭区间 |
| uint | 0 到 2<sup>32</sup> - 1，闭区间 |
| ulong | 0 到 2<sup>64</sup> - 1，闭区间 |
| byte | -2(<sup>7</sup>) 到 2<sup>7</sup> - 1，闭区间 |
| short | -2(<sup>15</sup>) 到 2<sup>15</sup> - 1，闭区间 |
| int | -2(<sup>31</sup>) 到 2<sup>31</sup> - 1，闭区间 |
| long | -2(<sup>63</sup>) 到 2<sup>63</sup> - 1，闭区间 |
| float | 32位浮点数 (IEEE 754-2008 binary32) |
| double | 64位浮点数 (IEEE 754-2008 binary64) |
| decimal32 | [32位十进制浮点数](https://en.wikipedia.org/wiki/Decimal32_floating-point_format) (IEEE 754-2008 decimal32) |
| decimal64 | [64位十进制浮点数](https://en.wikipedia.org/wiki/Decimal64_floating-point_format) (IEEE 754-2008 decimal64) |
| decimal128 | [128位十进制浮点数](https://en.wikipedia.org/wiki/Decimal128_floating-point_format) (IEEE 754-2008 decimal128) |
| char | 单个Unicode编码的字符 |
| timestamp | 时间戳 |
| uuid | 一个全局唯一的id (由RFC-4122章节4.1.2定义) |
| binary | 二进制数据 |
| string | Unicode字符串 |
| symbol | 约束领域内定义的符号值 |
| list |  支持多类型（多态）的列表 |
| map | 支持多类型（多态）的字典 |
| array | 单一类型的数组 |

### 1.1.2 自定义描述类型 (Described Types)

AMQP定义的基本类型可以直接表示大多数流行编程语言中存在的许多基本类型，可以直接用于交换基本数据。但在实际应用中，即使是最简单的应用程序也具有一组用于在程序内对概念建模的自定义类型。对于消息服务应用程序，要传递这些自定义类型的数据就需要先将其外部化（表示成AMQP格式）。

AMQP允许使用描述符（descriptor）对任何AMQP类型进行注解的方法。描述符关联了自定义类型和AMQP类型，它表明AMQP类型实际上是自定义类型的外部化表示。 AMQP类型及其描述符两者结合被称为自定义描述类型。

一个自定义描述类型包含两种不同的类型信息，它同时识别AMQP类型和自定义类型（以及它们之间的关系），因此该类型可以被从两个不同的角度进行解析：业务处理的应用程序可以将该类型理解为普通的自定义类型，从而根据业务领域的完整语义对每个类型进行解码和处理；非业务处理的应用程序仍可以将上述类型理解为AMQP类型，并对其进行解码和处理。