
### 11.1 MCP生态：扩展AI的能力边界

MCP（Model Context Protocol）是SOLO正式版中最重要的技术架构之一。它定义了一套标准化的协议，允许AI模型与外部工具和服务进行交互。通过MCP，SOLO的能力边界可以无限扩展，接入各种第三方服务、数据库、API和工具。

MCP的核心思想是协议标准化。在MCP出现之前，每个AI工具如果要接入外部服务，都需要编写专门的集成代码。这种点对点的集成方式不仅效率低下，而且难以维护。MCP通过定义统一的协议标准，让任何符合标准的服务都可以被AI工具无缝接入。


#### 11.1.1 MCP协议架构

MCP协议采用客户端-服务器架构。SOLO作为MCP客户端，通过标准化的协议与各种MCP服务器通信。每个MCP服务器封装了一种或多种能力，例如数据库访问、文件系统操作、API调用等。

MCP协议定义了三个核心概念：工具（Tools）、资源（Resources）和提示（Prompts）。工具是MCP服务器提供的可执行操作，例如查询数据库、发送HTTP请求等。资源是MCP服务器提供的数据源，例如文件内容、数据库记录等。提示是MCP服务器提供的预定义提示模板，用于引导AI完成特定任务。


#### 11.1.2 MCP生态全景

截至SOLO正式版发布，MCP生态已经拥有了超过11,000个公开的MCP Server，覆盖了开发、运维、数据分析、设计等多个领域。以下是一个分类概览：

| 类别 | Server数量 | 典型示例 | 主要用途 |
| --- | --- | --- | --- |
| 数据库 | 1,500+ | PostgreSQL MCP、MongoDB MCP、Redis MCP | 数据库查询、schema管理、数据迁移 |
| 开发工具 | 2,000+ | Git MCP、Docker MCP、Kubernetes MCP | 版本控制、容器管理、集群编排 |
| 云服务 | 1,800+ | AWS MCP、Azure MCP、GCP MCP | 云资源管理、服务部署、监控告警 |
| API集成 | 1,500+ | Slack MCP、GitHub MCP、Jira MCP | 消息通知、代码管理、项目管理 |
| 数据分析 | 800+ | Pandas MCP、Spark MCP、Airflow MCP | 数据处理、分析建模、工作流编排 |
| 文件处理 | 700+ | PDF MCP、Excel MCP、Image MCP | 文件解析、格式转换、内容提取 |
| AI/ML | 600+ | HuggingFace MCP、LangChain MCP | 模型管理、链式调用、推理服务 |
| 监控运维 | 500+ | Prometheus MCP、Grafana MCP | 指标采集、可视化、告警管理 |
| 安全 | 400+ | Vault MCP、SonarQube MCP | 密钥管理、代码扫描、安全审计 |
| 其他 | 1,200+ | 各种垂直领域MCP Server | 特定行业和场景的定制化需求 |

这个庞大的生态系统意味着，无论你的项目需要什么样的外部能力，大概率都能在MCP生态中找到合适的Server。你不需要自己编写集成代码，只需配置相应的MCP Server，SOLO就能自动获得对应的能力。


#### 11.1.3 MCP的技术优势

MCP协议带来了几个重要的技术优势：

标准化：统一的协议标准使得工具集成变得简单和可预测。开发者只需要学习一套协议，就能接入成千上万的服务。

安全性：MCP协议内置了权限控制和数据隔离机制。每个MCP Server只能访问被授权的资源，确保了数据安全。

可组合性：多个MCP Server的能力可以自由组合。你可以同时使用数据库MCP、Git MCP和Slack MCP，让SOLO在一次操作中完成数据查询、代码提交和消息通知。

可扩展性：任何人都可以开发新的MCP Server并分享到生态中。这种开放的扩展机制，确保了MCP生态的持续增长和演进。


### 11.2 实战：配置MCP Server

本节将通过几个具体的实战案例，展示如何在SOLO中配置和使用MCP Server。


#### 11.2.1 配置PostgreSQL MCP Server

假设你的项目使用PostgreSQL数据库，你希望SOLO能够直接查询数据库、管理数据表结构。以下是配置步骤：

步骤一：安装MCP Server

npm install -g @modelcontextprotocol/server-postgresql

步骤二：在SOLO中添加MCP配置

// 在SOLO的MCP配置文件中添加：

{

> "mcpServers": {

> "postgresql": {

> "command": "npx",

> "args": ["-y", "@modelcontextprotocol/server-postgresql",

> "postgresql://user:password@localhost:5432/mydb"]

}

}

}

步骤三：验证连接

💬 提示词：请连接PostgreSQL数据库，列出所有数据表及其结构，然后查询users表的前10条记录。

配置完成后，SOLO就能直接与你的PostgreSQL数据库交互了。你可以让SOLO执行各种数据库操作，如查询数据、创建表、修改schema等。


#### 11.2.2 配置GitHub MCP Server

GitHub MCP Server让SOLO能够直接与GitHub交互，管理仓库、Issue、Pull Request等。

// 安装

npm install -g @modelcontextprotocol/server-github

// 配置

{

> "mcpServers": {

> "github": {

> "command": "npx",

> "args": ["-y", "@modelcontextprotocol/server-github"],

> "env": {

> "GITHUB_TOKEN": "your-github-personal-access-token"

}

}

}

}

💬 提示词：请查看当前GitHub仓库中所有打开的Issue，按标签分类汇总，并为标记为bug的Issue创建对应的修复分支。

⚠️ 配置GitHub MCP Server时需要提供Personal Access Token。请确保Token只具有必要的权限，不要使用具有过多权限的Token。建议使用细粒度的Personal Access Token，只授予repo和issue权限。


### 11.3 Skills：封装常用工作流

Skills是SOLO提供的一种工作流封装机制。它允许你将常用的操作序列封装为一个可复用的技能，在需要时一键调用。Skills类似于IDE中的代码片段（Snippet），但功能更加强大，因为它不仅包含代码模板，还包含完整的执行逻辑。


#### 11.3.1 Skills的设计理念

在日常开发中，有很多操作是反复执行的。例如，每次创建新的API接口时，你都需要编写类似的代码结构：定义路由、编写控制器、添加验证、编写测试。虽然SOLO可以帮你生成这些代码，但每次都需要描述相同的需求，效率不高。

Skills解决了这个问题。你可以将创建API接口这个操作封装为一个Skill，定义好模板和参数。以后每次需要创建新的API接口时，只需调用这个Skill并传入参数即可。SOLO会按照Skill的定义自动执行所有步骤。


#### 11.3.2 创建自定义Skill

创建一个Skill需要定义以下几个要素：

名称：Skill的唯一标识符，例如create-rest-api。

描述：Skill的功能描述，帮助用户理解这个Skill的用途。

参数：Skill接受的输入参数，例如接口名称、请求方法、数据字段等。

步骤：Skill的执行步骤，每一步都是一个具体的操作。

模板：代码模板，使用参数占位符。

以下是一个创建REST API接口的Skill示例：

// Skill: create-rest-api

{

> "name": "create-rest-api",

> "description": "创建标准的REST API接口，包含路由、控制器、验证和测试",

> "parameters": [

{ "name": "resourceName", "type": "string", "description": "资源名称" },

{ "name": "fields", "type": "array", "description": "资源字段列表" },

{ "name": "authRequired", "type": "boolean", "description": "是否需要认证" }

],

> "steps": [

> "创建路由文件 routes/{resourceName}.js",

> "创建控制器文件 controllers/{resourceName}Controller.js",

> "创建验证文件 validators/{resourceName}Validator.js",

> "创建测试文件 tests/{resourceName}.test.js",

> "在server.js中注册新路由"

]

}


#### 11.3.3 使用Skill

创建好Skill后，使用非常简单：

💬 提示词：使用create-rest-api技能，创建一个商品(Product)管理接口，字段包括：name(字符串, 必填), price(数字, 必填), description(字符串, 可选), stock(数字, 必填)。需要认证。

💡 SOLO社区已经分享了大量的预置Skills，你可以直接导入使用。在SOLO的Skills市场中，可以按类别浏览和搜索Skills，找到适合你工作流的技能。


### 11.4 Rules：定制AI的行为规则

Rules是SOLO中用于定制AI行为的机制。通过定义规则，你可以让SOLO在生成代码时遵循特定的编码规范、架构模式和业务约束。Rules分为两种类型：User Rules（用户规则）和Project Rules（项目规则）。


#### 11.4.1 User Rules：全局行为定制

User Rules是全局生效的规则，适用于你所有的项目。它们定义了你的个人偏好和通用规范。例如：

// User Rules 示例

{

> "rules": [

> "所有代码注释使用中文",

> "使用TypeScript而非JavaScript",

> "使用函数式编程风格，避免使用class",

> "变量命名使用camelCase，常量使用UPPER_SNAKE_CASE",

> "每个函数不超过50行",

> "所有异步操作使用async/await，不使用.then()"

]

}

User Rules的优势在于一次配置，全局生效。你不需要在每个项目中重复设置相同的规则。当你开始一个新项目时，SOLO会自动应用你的User Rules，确保生成的代码符合你的个人风格。


#### 11.4.2 Project Rules：项目级约束

Project Rules是项目级别的规则，只对特定项目生效。它们通常定义了项目特定的技术约束和业务规则。例如：

// Project Rules 示例（.solo/rules.json）

{

> "rules": [

> "使用Prisma作为ORM，不使用其他ORM",

> "API响应格式统一为 { code, message, data }",

> "所有API接口需要进行参数验证",

> "数据库查询使用分页，每页最多20条",

> "敏感数据（密码、token）不能记录到日志中",

> "所有错误需要通过统一的错误处理中间件处理",

> "Git提交信息格式：type(scope): description"

]

}

Project Rules通常由团队共同制定，并提交到代码仓库中。这样，所有使用SOLO的团队成员都会遵循相同的规则，确保代码风格和架构的一致性。


#### 11.4.3 Rules的优先级和冲突处理

当User Rules和Project Rules存在冲突时，Project Rules优先。这是因为在团队协作中，项目级的规范应该优先于个人偏好。SOLO会在检测到规则冲突时发出警告，提醒你注意。

Rules不仅影响代码生成，还影响SOLO的所有行为，包括代码审查、重构建议、测试生成等。通过精心设计Rules，你可以让SOLO成为一个完全符合你团队规范的AI编程助手。


### 11.5 实战：创建自定义智能体

SOLO允许用户创建自定义智能体，这是SOLO生态中最具创造力的功能之一。自定义智能体可以封装特定的领域知识和工作流，成为你在特定领域的专属AI助手。


#### 11.5.1 自定义智能体的组成

一个自定义智能体由以下几个部分组成：

系统提示词：定义智能体的角色、能力和行为准则。这是智能体的灵魂，决定了它的性格和专业方向。

知识库：智能体可以关联特定的知识库，包括文档、代码示例、最佳实践等。这些知识会在智能体工作时作为参考。

工具集：智能体可以配置特定的MCP Server，获得特定的工具能力。例如，一个数据分析智能体可以配置数据库MCP和可视化MCP。

Skills：智能体可以预配置特定的Skills，在需要时快速调用。

Rules：智能体可以有自己的Rules，确保输出符合特定规范。


#### 11.5.2 创建一个React开发智能体

以下是一个完整的自定义智能体创建过程，我们将创建一个专注于React开发的智能体。

步骤一：定义系统提示词

💬 提示词：你是一个React开发专家智能体。你的专长包括：React Hooks、状态管理（Redux/Zustand）、性能优化、组件设计模式。你生成的代码遵循以下原则：使用函数组件和Hooks、使用TypeScript、遵循单一职责原则、组件粒度适中、使用CSS Modules或Tailwind CSS进行样式管理。你总是优先考虑性能和可维护性。

步骤二：配置工具集

为React开发智能体配置以下MCP Server：

{

> "mcpServers": {

> "filesystem": { "command": "npx", "args": ["-y", "@mcp/server-filesystem"] },

> "npm": { "command": "npx", "args": ["-y", "@mcp/server-npm"] },

> "browser": { "command": "npx", "args": ["-y", "@mcp/server-browser"] }

}

}

步骤三：添加预置Skills

为智能体添加以下预置Skills：create-react-component（创建React组件）、create-react-hook（创建自定义Hook）、create-redux-slice（创建Redux Slice）、create-api-service（创建API服务层）。

步骤四：设置Rules

{

> "rules": [

> "组件文件使用PascalCase命名",

> "自定义Hook以use开头",

> "Props接口使用组件名+Props命名",

> "使用React.memo优化纯展示组件",

> "使用useCallback/useMemo避免不必要的重渲染",

> "所有副作用放在useEffect中"

]

}

步骤五：测试和发布

创建完成后，你可以先在本地测试这个智能体，确保它的行为符合预期。测试通过后，你可以选择将其发布到SOLO的智能体市场，供其他用户使用。

💬 提示词：使用React专家智能体，创建一个用户登录表单组件。要求：包含邮箱和密码输入框、记住我复选框、登录按钮、表单验证（邮箱格式、密码最少6位）、加载状态和错误提示。使用Tailwind CSS进行样式设计。


### 11.6 智能体生态数据

SOLO的智能体生态已经发展成为一个庞大而活跃的社区。截至正式版发布，以下是生态的一些关键数据：

| 指标 | 数据 | 说明 |
| --- | --- | --- |
| 自定义智能体总数 | 365,000+ | 用户创建并分享的智能体 |
| MCP Server总数 | 11,000+ | 公开可用的MCP Server |
| 预置Skills数量 | 5,000+ | 社区贡献的Skills模板 |
| 活跃贡献者 | 50,000+ | 每月活跃的智能体/Skill创作者 |
| 日均使用次数 | 2,000,000+ | 所有智能体的日均调用次数 |
| 覆盖领域 | 100+ | 涵盖开发、设计、数据、运维等领域 |
| 最受欢迎类别 | 前端开发 | 约占总使用量的25% |
| 增长趋势 | 月均15% | 智能体数量的月均增长率 |

这些数据充分说明了SOLO智能体生态的活力和规模。365,000多个自定义智能体意味着，无论你的工作领域是什么，都能找到适合的智能体来辅助你的工作。而月均15%的增长率则表明，这个生态还在快速扩张中，未来将会有更多更专业的智能体涌现。

智能体生态的成功，很大程度上归功于SOLO开放的架构设计。低门槛的智能体创建方式、便捷的分享机制、完善的评价体系，共同构成了一个良性循环的生态。创作者因为用户的反馈而不断改进自己的智能体，用户因为高质量的智能体而更加依赖这个平台。


### 11.7 高级工程化的最佳实践

MCP、Skills和Rules三大功能共同构成了SOLO的高级工程化体系。要将这些功能发挥到最大价值，需要遵循一些最佳实践。


#### 11.7.1 MCP使用最佳实践

按需配置：只配置项目实际需要的MCP Server。过多的MCP Server会增加SOLO的决策负担，降低响应速度。建议根据项目需求，选择3-5个最相关的MCP Server。

安全第一：配置MCP Server时，注意保护敏感信息。数据库密码、API密钥等应该通过环境变量传递，而不是硬编码在配置文件中。

定期更新：MCP Server的维护者会定期发布更新，修复Bug和添加新功能。建议定期检查并更新你使用的MCP Server。


#### 11.7.2 Skills使用最佳实践

从社区开始：在创建自定义Skill之前，先搜索SOLO的Skills市场，看看是否已有满足需求的Skill。社区中有很多高质量的Skills可以直接使用。

参数化设计：创建Skill时，尽量使用参数化设计。一个好的Skill应该像函数一样，通过参数来适应不同的场景，而不是硬编码特定的值。

文档完善：为你的Skill编写清晰的使用文档，包括参数说明、使用示例和注意事项。这不仅能帮助其他用户，也能帮助未来的你自己。


#### 11.7.3 Rules使用最佳实践

团队协作：在团队项目中，Project Rules应该由团队共同讨论制定，并纳入代码审查流程。确保所有成员都理解并同意这些规则。

渐进式添加：不要一次性添加过多的Rules。建议从最核心的几条规则开始，随着项目的演进逐步添加。过多的规则可能会限制SOLO的灵活性。

定期审查：定期审查Rules的有效性，移除过时的规则，添加新的规则。规则应该随着项目的发展和团队的成长而演进。

本章小结

本章深入介绍了SOLO的高级工程化体系，包括MCP生态、Skills系统和Rules机制。MCP通过标准化的协议，让SOLO能够接入11,000多个外部服务，极大地扩展了AI的能力边界。Skills系统让你能够封装常用的工作流，实现一键调用。Rules机制则让你能够定制AI的行为，确保生成的代码符合团队规范。

通过创建自定义智能体的实战案例，我们看到了这三个功能如何协同工作，构建出强大的领域专属AI助手。365,000多个自定义智能体的生态数据，充分证明了这一体系的开放性和生命力。掌握高级工程化的最佳实践，将帮助你构建出更加高效、规范和可扩展的AI编程工作流。


# 第四部分 独立化与多端协同

SOLO的进化并没有止步于IDE插件。随着用户群体的不断扩大，SOLO团队发现了一个关键洞察：SOLO的用户不只是开发者。产品经理、运营人员、数据分析师、设计师、甚至学生和教师，都在使用SOLO完成各种非编程的工作。这一发现催生了SOLO独立端的诞生，也开启了MTC（More Than Coding）模式的新篇章。

本部分将全面介绍SOLO独立端的产品形态和核心功能，深入解析MTC模式的设计理念和应用场景，并详细展示移动端上线带来的三端协同体验。从PC到Web再到移动端，SOLO正在构建一个全场景的AI工作平台。

特别值得关注的是语音交互功能的引入。通过与Insta360的合作，SOLO实现了高质量的语音输入和输出，让用户可以通过自然语言与AI进行流畅的对话。这不仅解放了双手，更开启了一种全新的AI交互范式。

