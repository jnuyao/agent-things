# Ch01：Function Calling — 让模型学会使用工具

> **Function Calling 的本质，不是让模型真的去执行代码，而是让它把“我想调用哪个工具、传什么参数”这个意图结构化地表达出来，剩下的执行权始终留在宿主程序手里。**

从 Ch00 往前走，这几乎是第一个必然出现的解法。

模型已经很会理解语言，也大致知道该怎么解决问题。  
真正缺的不是“智力”，而是“手”和“工具”。

## 1.1 问题引入：模型需要“手”和“工具”

假设你是一个后端工程师，同事问你：“今天北京天气怎么样？”

你知道该怎么查。  
打开天气 API，传入 `city=Beijing`，拿到结果。

但如果把这个问题丢给 GPT-4，它只能说：

> “我无法获取实时天气数据，因为我的训练数据截止到……”

这就是 Ch00 遗留的核心痛点：**LLM 是一个封闭系统，它只能输出文本，不能调用 API，不能查数据库，不能执行任何副作用操作。**

它像一个被锁在隔音房间里的天才。  
脑子很好使，但没有手，也没有工具。

把问题拆开，你会发现情况其实非常有意思：

| 能力 | LLM 有吗？ | 缺什么？ |
| --- | --- | --- |
| 理解用户意图 | ✅ 强项 | — |
| 知道该调用什么工具 | ✅ 可以推理 | — |
| 知道传什么参数 | ✅ 可以从上下文提取 | — |
| 真正执行调用 | ❌ 做不到 | 需要宿主系统帮忙 |
| 把结果整合成回答 | ✅ 强项 | — |

5 项能力里，模型已经具备了 4 项。  
它真正缺的，只是“执行”这最后一步。

所以解法思路就很清晰了：

> **不需要让模型真的去执行。只需要让它把“我想调用什么函数、传什么参数”这个意图结构化地表达出来，剩下的交给宿主程序。**

这就是 Function Calling 的核心思想。

从后端工程师的视角看，这很像一个智能化的 API Gateway。

Gateway 本身不处理业务逻辑，它只负责路由。  
根据请求内容，决定调用哪个下游服务，传什么参数。

LLM 在 Function Calling 里的角色，正是这样一个“智能路由器”。

---

## 1.2 历史背景：从“只会说”到“能动手”

Function Calling 不是凭空出现的，而是一系列尝试的结果。

### 2023.03 — ChatGPT Plugins：一次超前的尝试

2023 年 3 月，OpenAI 发布了 ChatGPT Plugins，被媒体称为 “AI 的 App Store 时刻”：  
[ChatGPT plugins](https://openai.com/blog/chatgpt-plugins)

它允许第三方开发者编写插件，让 ChatGPT 访问实时信息、执行计算、调用第三方服务。

方向是对的，但实现并不理想：

- 插件发现机制混乱，用户不知道什么时候会触发哪个插件
- 模型调用插件的成功率不稳定
- 开发者接入成本高，需要写 OpenAPI spec 和额外描述文件

Plugins 证明了一件事：

> 模型确实需要连接外部工具。  
> 但实现方式需要更底层、更稳定、更可控。

### 2023.05 — Prompt Engineer 登上风口

2023 年 5 月，世界经济论坛把 Prompt Engineer 列为新兴职业榜首：  
[World Economic Forum](https://www.weforum.org/stories/2023/05/ai-prompt-engineer-jobs/)

这个现象的本质是：**当模型只能通过自然语言交互时，“如何写好 prompt”就成了最关键的技能。**

但 Prompt Engineering 有天然天花板。

你可以用自然语言让模型“尽量输出 JSON”。  
却很难靠 prompt 精确约束一个 API 调用的参数 schema。

这恰恰是 Function Calling 要解决的问题。

### 2023.06.13 — Function Calling 正式发布

2023 年 6 月 13 日，OpenAI 在 GPT-3.5-turbo 和 GPT-4 上正式发布 Function Calling：  
[Function calling and other API updates](https://openai.com/blog/function-calling-and-other-api-updates)

这是一个分水岭。

核心突破在于：

> **模型第一次能够以结构化 JSON 的形式输出“调用意图”。**

不是自由文本。  
不是 Markdown 代码块。  
而是 API 层面可以消费的结构化结果。

这意味着：

- 开发者可以用 JSON Schema 定义工具接口
- 模型可以从可用工具里推理该调用哪个
- 模型可以抽取参数并按结构返回
- 程序可以稳定解析，而不是“猜测模型想干嘛”

### 2024.08 — Structured Outputs：从“大概率正确”到“严格合规”

早期 Function Calling 有一个痛点：模型输出的 JSON 大概率符合 schema，但不是 100% 保证。

它偶尔仍会出现：

- 字段名拼错
- 类型不对
- 缺少必填字段

2024 年 8 月，OpenAI 发布 Structured Outputs：  
[Introducing Structured Outputs in the API](https://openai.com/index/introducing-structured-outputs-in-the-api/)

它通过在解码阶段加入约束，保证输出严格符合预定义的 JSON Schema。

从工程视角看，这一步意义极大。

早期 FC 更像“通常靠谱”。  
Structured Outputs 开始把它推向“可以纳入正式系统契约”。

---

## 1.3 核心概念：Function Calling 到底怎么工作

### 1.3.1 整体工作流

Function Calling 的完整流程其实很清楚：

```text
用户输入
  ->
宿主程序把用户消息和 tools schema 一起发给模型
  ->
模型推理后返回：要调用哪个函数 + 参数是什么
  ->
宿主程序解析这个结构化结果
  ->
宿主程序真正执行函数
  ->
宿主程序把函数结果再发回模型
  ->
模型把结果整理成最终回答
```

这套链路里最关键的一点是：

> **模型从头到尾都没有真正执行过任何函数。**

它只做了一件事：

> **输出一段结构化数据，表达自己的调用意图。**

真正的执行权始终在你的代码手里。

### 1.3.2 Tools Schema 设计

当你调用 LLM API 时，需要通过 `tools` 参数告诉模型有哪些工具可用。

每个工具定义的本质，就是一个接口契约。  
通常它会包含：

- 工具名称
- 工具描述
- 参数 schema
- 必填字段
- 枚举约束

这里有三个特别重要的设计点：

**1. description 极其重要**

它不是给人看的注释，而是模型决策的重要依据。  
你写得越清楚，模型越知道“什么时候该调用这个工具”。

**2. required 字段决定边界**

它告诉模型哪些参数必须提供，哪些可以省略。  
这直接影响调用结果是否可执行。

**3. enum 约束减少自由发挥**

参数值越可枚举，模型出错空间越小。  
这和传统后端接口设计里的“缩小输入空间”是同一个原则。

如果用一个最常见的“查询天气”工具来举例，一个简化后的 tools schema 会长这样：

```json
{
  "type": "function",
  "function": {
    "name": "get_weather",
    "description": "查询指定城市的当前天气。当用户询问天气、温度或是否下雨时使用。",
    "parameters": {
      "type": "object",
      "properties": {
        "city": {
          "type": "string",
          "description": "城市名，例如 Beijing、Shanghai、Singapore"
        },
        "unit": {
          "type": "string",
          "enum": ["celsius", "fahrenheit"],
          "description": "温度单位，默认使用 celsius"
        }
      },
      "required": ["city"]
    }
  }
}
```

这个例子里有几个很关键的点：

- `name` 决定了宿主程序最后会执行哪个工具
- `description` 告诉模型“什么时候该用这个工具”
- `properties` 描述了参数结构
- `required` 告诉模型哪些参数缺了就不能执行
- `enum` 约束了允许的取值范围

如果用户问：

> 北京今天天气怎么样？

模型在看完这份 schema 之后，就应该能推理出两件事：

- 该调用 `get_weather`
- 应该传入 `city = "Beijing"`

从后端工程师的角度看，这和 gRPC 的 `.proto`、Thrift 的 IDL、或者 REST API 的 OpenAPI schema 没有什么本质区别。

Function Calling 做的事情，本质上就是：

> **给模型一个它也能读懂的接口定义。**

### 1.3.3 底层机制：特殊 Token 与受约束解码

Function Calling 在底层并不神秘。

可以把它理解成两件事的组合：

**1. 特殊 Token**

当模型决定调用工具时，它输出的并不再是普通聊天文本，而是一种带控制语义的结构化片段。  
API 层据此知道：“这不是普通回复，而是一个工具调用意图。”

**2. 受约束解码**

Structured Outputs 的关键就在这里。

普通文本生成是在整个词表上采样下一个 token。  
受约束解码则会根据当前 schema，动态屏蔽所有不合法的 token。

如果某个字段要求是整数，那么在那个位置上，非数字 token 就不会被允许生成。

从工程角度看，这几乎像给 LLM 输出过程加了一层“语法检查器”。

### 1.3.4 演进：从不可靠到确定性

| 阶段 | 时间 | 特征 | 可靠性 |
| --- | --- | --- | --- |
| Prompt Hacking | 2022-2023 | 在 prompt 中要求模型输出 JSON | 经常格式错误 |
| Function Calling v1 | 2023.06 | API 层面支持 tools 参数 | 大多数时候可用 |
| Structured Outputs | 2024.08 | 解码层约束 + strict 模式 | 可以纳入严格工程约束 |

这个演进过程非常像后端系统熟悉的那条路：

从“约定一下大家尽量照做”，  
走向“接口契约可验证、可约束、可依赖”。

---

## 1.4 如果不写代码，怎么在脑海里跑通一次 Function Calling

第一版里，这一章不提供代码。

因为这里最重要的不是学会某个 SDK 的写法，而是先建立一个稳定的工程心智模型。

你可以在脑子里跑一遍最简单的例子：

用户说：

> 北京今天天气怎么样？

这时候系统内部其实发生的是五步：

1. 程序先告诉模型：“你手头有一个叫 `get_weather` 的工具，它接收 `city` 参数。”
2. 模型理解用户是在问天气，于是判断应该调用 `get_weather`
3. 模型再从用户问题里抽出参数，得到 `city = Beijing`
4. 宿主程序真正去调用天气服务
5. 程序把结果塞回模型，模型再把它组织成自然语言回答

如果把这个过程再展开一点，你会发现：**程序和模型之间其实经历了两次往返。**

下面用一个“简化后的真实数据交互”来示意一次完整流程。  
注意：不同厂商和 SDK 的字段名会不同，但核心结构基本类似。

### 第一次请求：程序把用户问题和工具定义一起发给模型

```json
{
  "messages": [
    {
      "role": "user",
      "content": "北京今天天气怎么样？"
    }
  ],
  "tools": [
    {
      "type": "function",
      "function": {
        "name": "get_weather",
        "description": "查询指定城市的当前天气",
        "parameters": {
          "type": "object",
          "properties": {
            "city": { "type": "string" },
            "unit": { "type": "string", "enum": ["celsius", "fahrenheit"] }
          },
          "required": ["city"]
        }
      }
    }
  ]
}
```

这一步，程序实际上是在告诉模型三件事：

- 用户问了什么
- 当前有哪些工具可用
- 每个工具长什么样、该怎么调用

### 第一次响应：模型返回“调用意图”

```json
{
  "role": "assistant",
  "tool_calls": [
    {
      "id": "call_001",
      "type": "function",
      "function": {
        "name": "get_weather",
        "arguments": "{\"city\":\"Beijing\",\"unit\":\"celsius\"}"
      }
    }
  ]
}
```

这一刻特别关键。

模型没有直接回答天气。  
它只是说：

> 我建议调用 `get_weather`，参数是 `city = Beijing`，`unit = celsius`。

### 中间执行：宿主程序真正去调用外部服务

这一步完全不在模型内部发生。

程序会：

1. 解析 `tool_calls`
2. 找到本地注册的 `get_weather`
3. 调用真实天气 API 或内部服务
4. 拿到执行结果

假设真实工具返回的是：

```json
{
  "city": "Beijing",
  "temperature": 22,
  "unit": "celsius",
  "condition": "晴"
}
```

### 第二次请求：程序把工具结果再发回模型

```json
{
  "messages": [
    {
      "role": "user",
      "content": "北京今天天气怎么样？"
    },
    {
      "role": "assistant",
      "tool_calls": [
        {
          "id": "call_001",
          "type": "function",
          "function": {
            "name": "get_weather",
            "arguments": "{\"city\":\"Beijing\",\"unit\":\"celsius\"}"
          }
        }
      ]
    },
    {
      "role": "tool",
      "tool_call_id": "call_001",
      "content": "{\"city\":\"Beijing\",\"temperature\":22,\"unit\":\"celsius\",\"condition\":\"晴\"}"
    }
  ]
}
```

这一步的含义是：

- 你上一轮要求调用 `get_weather`
- 这个工具现在已经执行完了
- 这是工具返回的真实结果

### 第二次响应：模型把结果组织成自然语言

```json
{
  "role": "assistant",
  "content": "北京今天天气晴，气温 22 摄氏度。"
}
```

直到这一步，用户才真正看到自然语言回答。

把整件事压缩成最短的话，其实就是：

```text
第一次往返：模型决定“该调什么”
中间步骤：程序执行“具体怎么调”
第二次往返：模型整理“最后怎么说”
```

如果把它压缩成一句话，就是：

> **模型负责决定“该做什么”，程序负责执行“怎么做”。**

这个区分非常关键。  
因为它意味着安全性、权限、执行环境、失败恢复，依旧全部掌握在工程系统手里。

---

## 1.5 深入剖析：几个反直觉的洞察

### 洞察 1：Function Calling 不是“模型在执行代码”

这是最常见的误解。

很多人会说：“GPT 已经能调 API 了。”  
更准确的说法应该是：

> **模型已经能稳定表达调用意图了。**

真正的 HTTP 请求、数据库查询、文件操作，依然都是你的代码在做。

这个区分非常重要，因为它意味着：

- 安全性在你手里
- 权限控制在你手里
- 执行环境在你手里
- 错误处理也在你手里

所以 Function Calling 不是“模型拿到了 root 权限”。  
它只是拿到了一个结构化的“请求麦克风”。

### 洞察 2：工具描述其实也是 Prompt

很多人会把 `tools` 里的 schema 当成“纯技术配置”，但它其实也会影响模型行为。

因为当你传入工具定义时，这些定义本身会进入模型上下文。  
这意味着三件事：

- 工具描述会消耗 token
- 描述质量会影响调用准确率
- 工具数量太多会增加模型的选择负担

所以工具不是“越多越好”。  
真正好的设计，是把当前上下文里最相关、最必要的工具暴露给模型。

### 洞察 3：Prompt Engineer 的热潮，本质上是一个过渡阶段

2023 年 Prompt Engineer 成为热门职业，本质反映的是一个非常具体的时代背景：

> 当模型只能靠自然语言驱动时，“写 prompt”就是最高杠杆的技能。

但 Function Calling 开始改变这件事。

因为它把很多原本只能靠 prompt 勉强约束的事情，转移到了更结构化的接口层。

这不意味着 prompt 不重要了。  
工具的 `description` 本身仍然是 prompt。

但它确实意味着：  
**纯粹依赖 prompt 技巧的时代，开始向接口设计、schema 设计、协议设计让位。**

### 洞察 4：这是一条从“非确定性”走向“可控性”的工程化道路

后端工程师习惯的是确定性系统。  
同样的输入，应该得到同样的结果。

LLM 的世界则天然带着非确定性。  
同样的 prompt，不同次生成可能略有不同。

Function Calling，尤其是 Structured Outputs，代表了一条很重要的路线：

> **在模型的非确定性之上，先划出一块可控、可验证、可集成的确定性边界。**

模型负责“决策”。  
程序负责“执行”。  
协议负责把这两者桥接起来。

这是整个 AI 应用工程里非常重要的一步。

---

## 1.6 遗留痛点：为什么 Function Calling 还不够

Function Calling 解决了“模型无法使用工具”的问题，但它很快暴露出新的限制。

### 痛点 1：协议碎片化

不同厂商的 Function Calling 接口并不统一。

| 厂商 | 工具定义格式 | 调用返回格式 | 特殊要求 |
| --- | --- | --- | --- |
| OpenAI | `tools` + JSON Schema | `tool_calls` | 需要关联 `tool_call_id` |
| Anthropic | 类似 `tools` | `tool_use` content block | role 处理不同 |
| Google | `function_declarations` | `function_call` part | 嵌套结构不同 |
| 开源模型 | 各自实现 | 不统一 | 可靠性参差不齐 |

也就是说，你为一家写的代码，迁到另一家往往得重写。

这里暴露的不是“模型不够强”，而是：

> **缺少统一的工具调用协议。**

这直接引出后面的 MCP。

### 痛点 2：能力仍然很窄

当前的 Function Calling 本质上还是单次调用模型。

它并不天然擅长：

- 多步骤编排
- 条件分支
- 长链路状态
- 跨步骤记忆

换句话说，它能让模型“伸手拿工具”，但还不能让模型“自己规划整个任务流程”。

这就是为什么后面会走向 Agent。

### 痛点 3：知识问题仍然存在

Function Calling 给了模型“手”，但没有给它“外部记忆”。

比如：

- 公司内部文档它仍然不知道
- 昨天发布的新闻它仍然不知道
- 用户的长期偏好它仍然不知道

这意味着另一个方向必须出现：

> **在推理时为模型动态注入外部知识。**

这直接引出 RAG。

### 两条路径会在后面汇合

从 Ch00 的三大缺陷出发，到这里你已经能看见两条很清晰的补强路径：

```text
LLM 的三大结构性缺陷
  ->
一条路解决“无法行动” -> Function Calling
一条路解决“知识截止” -> RAG
  ->
两条路继续往前走
  ->
最终都需要协议、编排、状态和系统化能力
```

Function Calling 给了模型“手”。  
RAG 给了模型“外部知识”。  
而后面的 MCP、Agent、Memory、Agent OS，则是在把这些零散能力真正拼成系统。

---

## 1.7 本章小结

Function Calling 可以被压缩成一句话：

> **它让模型第一次能够以结构化方式表达“我要调用哪个工具、传什么参数”，从而把纯文本生成推进到真正可执行的 AI 应用。**

这一章最重要的三个认知是：

- 模型不执行函数，它只声明调用意图
- 真正的执行权、安全边界和错误处理都在宿主程序手里
- Function Calling 不是终点，它只是模型连接外部世界的第一座桥

下一章会走另一条补强路径：

**Ch02：RAG — 当模型不知道时，怎么让它先去查资料。**
