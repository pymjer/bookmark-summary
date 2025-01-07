Title: 接入 Xinference 部署的本地模型 | Dify

URL Source: https://docs.dify.ai/zh-hans/development/models-integration/xinference

Markdown Content:
接入 Xinference 部署的本地模型 | Dify
===============
 

[![Image 5](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F977870688-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fcollections%252FJ0tNUAIHn7AqDs8XgD8X%252Ficon%252FSh1lHa9aRiRhUAYG2sdf%252FSquare%2520%25C2%25B7%2520512.png%3Falt%3Dmedia%26token%3Dea2fb2cc-b80c-4621-999d-48d4c0ddcc7b&width=32&dpr=4&quality=100&sign=d3c0284&sv=2)![Image 6](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F977870688-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fcollections%252FJ0tNUAIHn7AqDs8XgD8X%252Ficon%252FSh1lHa9aRiRhUAYG2sdf%252FSquare%2520%25C2%25B7%2520512.png%3Falt%3Dmedia%26token%3Dea2fb2cc-b80c-4621-999d-48d4c0ddcc7b&width=32&dpr=4&quality=100&sign=d3c0284&sv=2) Dify](https://docs.dify.ai/zh-hans)

简体中文

[English](https://docs.dify.ai/development/models-integration/xinference?fallback=true)[简体中文](https://docs.dify.ai/zh-hans/development/models-integration/xinference?fallback=true)[日本語](https://docs.dify.ai/ja-jp/development/models-integration/xinference?fallback=true)

Search...

Ctrl + K

简体中文

[English](https://docs.dify.ai/development/models-integration/xinference?fallback=true)[简体中文](https://docs.dify.ai/zh-hans/development/models-integration/xinference?fallback=true)[日本語](https://docs.dify.ai/ja-jp/development/models-integration/xinference?fallback=true)

*   入门
    
    *   [欢迎使用 Dify](https://docs.dify.ai/zh-hans)
        
        *   [特性与技术规格](https://docs.dify.ai/zh-hans/getting-started/readme/features-and-specifications)
            
        *   [模型供应商列表](https://docs.dify.ai/zh-hans/getting-started/readme/model-providers)
            
        
    *   [云服务](https://docs.dify.ai/zh-hans/getting-started/cloud)
        
    *   [社区版](https://docs.dify.ai/zh-hans/getting-started/install-self-hosted)
        
        *   [Docker Compose 部署](https://docs.dify.ai/zh-hans/getting-started/install-self-hosted/docker-compose)
            
        *   [本地源码启动](https://docs.dify.ai/zh-hans/getting-started/install-self-hosted/local-source-code)
            
        *   [宝塔面板部署](https://docs.dify.ai/zh-hans/getting-started/install-self-hosted/bt-panel)
            
        *   [单独启动前端 Docker 容器](https://docs.dify.ai/zh-hans/getting-started/install-self-hosted/start-the-frontend-docker-container)
            
        *   [环境变量说明](https://docs.dify.ai/zh-hans/getting-started/install-self-hosted/environments)
            
        *   [常见问题](https://docs.dify.ai/zh-hans/getting-started/install-self-hosted/faq)
            
        
    *   [Dify Premium](https://docs.dify.ai/zh-hans/getting-started/dify-premium)
        
*   手册
    
    *   [模型](https://docs.dify.ai/zh-hans/guides/model-configuration)
        
        *   [增加新供应商](https://docs.dify.ai/zh-hans/guides/model-configuration/new-provider)
            
        *   [预定义模型接入](https://docs.dify.ai/zh-hans/guides/model-configuration/predefined-model)
            
        *   [自定义模型接入](https://docs.dify.ai/zh-hans/guides/model-configuration/customizable-model)
            
        *   [接口方法](https://docs.dify.ai/zh-hans/guides/model-configuration/interfaces)
            
        *   [配置规则](https://docs.dify.ai/zh-hans/guides/model-configuration/schema)
            
        *   [负载均衡](https://docs.dify.ai/zh-hans/guides/model-configuration/load-balancing)
            
        
    *   [构建](https://docs.dify.ai/zh-hans/guides/application-orchestrate)
        
        *   [创建应用](https://docs.dify.ai/zh-hans/guides/application-orchestrate/creating-an-application)
            
        *   [聊天助手](https://docs.dify.ai/zh-hans/guides/application-orchestrate/conversation-application)
            
        *   [Agent](https://docs.dify.ai/zh-hans/guides/application-orchestrate/agent)
            
        *   [应用工具箱](https://docs.dify.ai/zh-hans/guides/application-orchestrate/app-toolkits)
            
            *   [敏感内容审查](https://docs.dify.ai/zh-hans/guides/application-orchestrate/app-toolkits/moderation-tool)
                
            
        
    *   [工作流](https://docs.dify.ai/zh-hans/guides/workflow)
        
        *   [关键概念](https://docs.dify.ai/zh-hans/guides/workflow/key-concept)
            
        *   [变量](https://docs.dify.ai/zh-hans/guides/workflow/variables)
            
        *   [节点说明](https://docs.dify.ai/zh-hans/guides/workflow/node)
            
            *   [开始](https://docs.dify.ai/zh-hans/guides/workflow/node/start)
                
            *   [LLM](https://docs.dify.ai/zh-hans/guides/workflow/node/llm)
                
            *   [知识检索](https://docs.dify.ai/zh-hans/guides/workflow/node/knowledge-retrieval)
                
            *   [问题分类](https://docs.dify.ai/zh-hans/guides/workflow/node/question-classifier)
                
            *   [条件分支](https://docs.dify.ai/zh-hans/guides/workflow/node/ifelse)
                
            *   [代码执行](https://docs.dify.ai/zh-hans/guides/workflow/node/code)
                
            *   [模板转换](https://docs.dify.ai/zh-hans/guides/workflow/node/template)
                
            *   [文档提取器](https://docs.dify.ai/zh-hans/guides/workflow/node/doc-extractor)
                
            *   [列表操作](https://docs.dify.ai/zh-hans/guides/workflow/node/list-operator)
                
            *   [变量聚合](https://docs.dify.ai/zh-hans/guides/workflow/node/variable-aggregator)
                
            *   [变量赋值](https://docs.dify.ai/zh-hans/guides/workflow/node/variable-assigner)
                
            *   [迭代](https://docs.dify.ai/zh-hans/guides/workflow/node/iteration)
                
            *   [参数提取](https://docs.dify.ai/zh-hans/guides/workflow/node/parameter-extractor)
                
            *   [HTTP 请求](https://docs.dify.ai/zh-hans/guides/workflow/node/http-request)
                
            *   [工具](https://docs.dify.ai/zh-hans/guides/workflow/node/tools)
                
            *   [结束](https://docs.dify.ai/zh-hans/guides/workflow/node/end)
                
            *   [直接回复](https://docs.dify.ai/zh-hans/guides/workflow/node/answer)
                
            
        *   [快捷键](https://docs.dify.ai/zh-hans/guides/workflow/shortcut-key)
            
        *   [编排节点](https://docs.dify.ai/zh-hans/guides/workflow/orchestrate-node)
            
        *   [文件上传](https://docs.dify.ai/zh-hans/guides/workflow/file-upload)
            
        *   [异常处理](https://docs.dify.ai/zh-hans/guides/workflow/error-handling)
            
            *   [预定义异常处理逻辑](https://docs.dify.ai/zh-hans/guides/workflow/error-handling/predefined-nodes-failure-logic)
                
            *   [错误类型](https://docs.dify.ai/zh-hans/guides/workflow/error-handling/error-type)
                
            
        *   [附加功能](https://docs.dify.ai/zh-hans/guides/workflow/additional-features)
            
        *   [预览与调试](https://docs.dify.ai/zh-hans/guides/workflow/debug-and-preview)
            
            *   [预览与运行](https://docs.dify.ai/zh-hans/guides/workflow/debug-and-preview/yu-lan-yu-yun-hang)
                
            *   [单步调试](https://docs.dify.ai/zh-hans/guides/workflow/debug-and-preview/step-run)
                
            *   [对话/运行日志](https://docs.dify.ai/zh-hans/guides/workflow/debug-and-preview/log)
                
            *   [检查清单](https://docs.dify.ai/zh-hans/guides/workflow/debug-and-preview/checklist)
                
            *   [运行历史](https://docs.dify.ai/zh-hans/guides/workflow/debug-and-preview/history)
                
            
        *   [应用发布](https://docs.dify.ai/zh-hans/guides/workflow/publish)
            
        *   [变更公告：图片上传被替换为文件上传](https://docs.dify.ai/zh-hans/guides/workflow/bulletin)
            
        
    *   [知识库](https://docs.dify.ai/zh-hans/guides/knowledge-base)
        
        *   [创建知识库&上传文档](https://docs.dify.ai/zh-hans/guides/knowledge-base/create-knowledge-and-upload-documents)
            
        *   [从 Notion 导入数据](https://docs.dify.ai/zh-hans/guides/knowledge-base/sync-from-notion)
            
        *   [从网页导入数据](https://docs.dify.ai/zh-hans/guides/knowledge-base/sync-from-website)
            
        *   [知识库管理与文档维护](https://docs.dify.ai/zh-hans/guides/knowledge-base/knowledge-and-documents-maintenance)
            
        *   [在应用内集成知识库](https://docs.dify.ai/zh-hans/guides/knowledge-base/integrate-knowledge-within-application)
            
        *   [召回测试/引用归属](https://docs.dify.ai/zh-hans/guides/knowledge-base/retrieval-test-and-citation)
            
        *   [通过 API 维护知识库](https://docs.dify.ai/zh-hans/guides/knowledge-base/maintain-dataset-via-api)
            
        *   [连接外部知识库](https://docs.dify.ai/zh-hans/guides/knowledge-base/connect-external-knowledge-base)
            
        *   [外部知识库 API](https://docs.dify.ai/zh-hans/guides/knowledge-base/external-knowledge-api-documentation)
            
        
    *   [工具](https://docs.dify.ai/zh-hans/guides/tools)
        
        *   [快速接入工具](https://docs.dify.ai/zh-hans/guides/tools/quick-tool-integration)
            
        *   [高级接入工具](https://docs.dify.ai/zh-hans/guides/tools/advanced-tool-integration)
            
        *   [工具配置](https://docs.dify.ai/zh-hans/guides/tools/tool-configuration)
            
            *   [Google](https://docs.dify.ai/zh-hans/guides/tools/tool-configuration/google)
                
            *   [Bing](https://docs.dify.ai/zh-hans/guides/tools/tool-configuration/bing)
                
            *   [SearchApi](https://docs.dify.ai/zh-hans/guides/tools/tool-configuration/searchapi)
                
            *   [StableDiffusion](https://docs.dify.ai/zh-hans/guides/tools/tool-configuration/stable-diffusion)
                
            *   [Dall-e](https://docs.dify.ai/zh-hans/guides/tools/tool-configuration/dall-e)
                
            *   [Perplexity Search](https://docs.dify.ai/zh-hans/guides/tools/tool-configuration/perplexity)
                
            *   [AlphaVantage 股票分析](https://docs.dify.ai/zh-hans/guides/tools/tool-configuration/alphavantage)
                
            *   [Youtube](https://docs.dify.ai/zh-hans/guides/tools/tool-configuration/youtube)
                
            *   [SearXNG](https://docs.dify.ai/zh-hans/guides/tools/tool-configuration/searxng)
                
            *   [Serper](https://docs.dify.ai/zh-hans/guides/tools/tool-configuration/serper)
                
            *   [SiliconFlow (支持 Flux 绘图)](https://docs.dify.ai/zh-hans/guides/tools/tool-configuration/siliconflow)
                
            *   [ComfyUI](https://docs.dify.ai/zh-hans/guides/tools/tool-configuration/comfyui)
                
            
        
    *   [发布](https://docs.dify.ai/zh-hans/guides/application-publishing)
        
        *   [发布为公开 Web 站点](https://docs.dify.ai/zh-hans/guides/application-publishing/launch-your-webapp-quickly)
            
            *   [Web 应用的设置](https://docs.dify.ai/zh-hans/guides/application-publishing/launch-your-webapp-quickly/web-app-settings)
                
            *   [文本生成型应用](https://docs.dify.ai/zh-hans/guides/application-publishing/launch-your-webapp-quickly/text-generator)
                
            *   [对话型应用](https://docs.dify.ai/zh-hans/guides/application-publishing/launch-your-webapp-quickly/conversation-application)
                
            
        *   [嵌入网站](https://docs.dify.ai/zh-hans/guides/application-publishing/embedding-in-websites)
            
        *   [基于 APIs 开发](https://docs.dify.ai/zh-hans/guides/application-publishing/developing-with-apis)
            
        *   [基于前端组件再开发](https://docs.dify.ai/zh-hans/guides/application-publishing/based-on-frontend-templates)
            
        
    *   [标注](https://docs.dify.ai/zh-hans/guides/annotation)
        
        *   [日志与标注](https://docs.dify.ai/zh-hans/guides/annotation/logs)
            
        *   [标注回复](https://docs.dify.ai/zh-hans/guides/annotation/annotation-reply)
            
        
    *   [监测](https://docs.dify.ai/zh-hans/guides/monitoring)
        
        *   [集成外部 Ops 工具](https://docs.dify.ai/zh-hans/guides/monitoring/integrate-external-ops-tools)
            
            *   [集成 LangSmith](https://docs.dify.ai/zh-hans/guides/monitoring/integrate-external-ops-tools/integrate-langsmith)
                
            *   [集成 Langfuse](https://docs.dify.ai/zh-hans/guides/monitoring/integrate-external-ops-tools/integrate-langfuse)
                
            
        *   [数据分析](https://docs.dify.ai/zh-hans/guides/monitoring/analysis)
            
        
    *   [扩展](https://docs.dify.ai/zh-hans/guides/extension)
        
        *   [API 扩展](https://docs.dify.ai/zh-hans/guides/extension/api-based-extension)
            
            *   [使用 Cloudflare Workers 部署 API Tools](https://docs.dify.ai/zh-hans/guides/extension/api-based-extension/cloudflare-workers)
                
            *   [敏感内容审查](https://docs.dify.ai/zh-hans/guides/extension/api-based-extension/moderation)
                
            
        *   [代码扩展](https://docs.dify.ai/zh-hans/guides/extension/code-based-extension)
            
            *   [外部数据工具](https://docs.dify.ai/zh-hans/guides/extension/code-based-extension/external-data-tool)
                
            *   [敏感内容审查](https://docs.dify.ai/zh-hans/guides/extension/code-based-extension/moderation)
                
            
        
    *   [协同](https://docs.dify.ai/zh-hans/guides/workspace)
        
        *   [发现](https://docs.dify.ai/zh-hans/guides/workspace/app)
            
        *   [邀请与管理成员](https://docs.dify.ai/zh-hans/guides/workspace/invite-and-manage-members)
            
        
    *   [管理](https://docs.dify.ai/zh-hans/guides/management)
        
        *   [应用管理](https://docs.dify.ai/zh-hans/guides/management/app-management)
            
        *   [团队成员管理](https://docs.dify.ai/zh-hans/guides/management/team-members-management)
            
        *   [个人账号管理](https://docs.dify.ai/zh-hans/guides/management/personal-account-management)
            
        *   [订阅管理](https://docs.dify.ai/zh-hans/guides/management/subscription-management)
            
        
*   动手实验室
    
    *   [初级](https://docs.dify.ai/zh-hans/workshop/basic)
        
        *   [如何搭建 AI 图片生成应用](https://docs.dify.ai/zh-hans/workshop/basic/build-ai-image-generation-app)
            
        *   [AI Agent 实战：搭建个人在线旅游助手](https://docs.dify.ai/zh-hans/workshop/basic/travel-assistant)
            
        
    *   [中级](https://docs.dify.ai/zh-hans/workshop/intermediate)
        
        *   [使用文件上传搭建文章理解助手](https://docs.dify.ai/zh-hans/workshop/intermediate/article-reader)
            
        *   [使用知识库搭建智能客服机器人](https://docs.dify.ai/zh-hans/workshop/intermediate/customer-service-bot)
            
        *   [ChatFlow 实战：搭建 Twitter 账号分析助手](https://docs.dify.ai/zh-hans/workshop/intermediate/twitter-chatflow)
            
        
*   社区
    
    *   [寻求支持](https://docs.dify.ai/zh-hans/community/support)
        
    *   [成为贡献者](https://docs.dify.ai/zh-hans/community/contribution)
        
    *   [为 Dify 文档做出贡献](https://docs.dify.ai/zh-hans/community/docs-contribution)
        
*   研发
    
    *   [后端](https://docs.dify.ai/zh-hans/development/backend)
        
        *   [DifySandbox](https://docs.dify.ai/zh-hans/development/backend/sandbox)
            
            *   [贡献指南](https://docs.dify.ai/zh-hans/development/backend/sandbox/contribution)
                
            
        
    *   [模型接入](https://docs.dify.ai/zh-hans/development/models-integration)
        
        *   [接入 Hugging Face 上的开源模型](https://docs.dify.ai/zh-hans/development/models-integration/hugging-face)
            
        *   [接入 Replicate 上的开源模型](https://docs.dify.ai/zh-hans/development/models-integration/replicate)
            
        *   [接入 Xinference 部署的本地模型](https://docs.dify.ai/zh-hans/development/models-integration/xinference)
            
        *   [接入 OpenLLM 部署的本地模型](https://docs.dify.ai/zh-hans/development/models-integration/openllm)
            
        *   [接入 LocalAI 部署的本地模型](https://docs.dify.ai/zh-hans/development/models-integration/localai)
            
        *   [接入 Ollama 部署的本地模型](https://docs.dify.ai/zh-hans/development/models-integration/ollama)
            
        *   [接入 LiteLLM 代理的模型](https://docs.dify.ai/zh-hans/development/models-integration/litellm)
            
        *   [接入 GPUStack 进行本地模型部署](https://docs.dify.ai/zh-hans/development/models-integration/gpustack)
            
        
*   阅读更多
    
    *   [应用案例](https://docs.dify.ai/zh-hans/learn-more/use-cases)
        
        *   [如何训练出专属于“你”的问答机器人？](https://docs.dify.ai/zh-hans/learn-more/use-cases/train-a-qa-chatbot-that-belongs-to-you)
            
        *   [教你十几分钟不用代码创建 Midjourney 提示词机器人](https://docs.dify.ai/zh-hans/learn-more/use-cases/create-a-midjoureny-prompt-word-robot-with-zero-code)
            
        *   [构建一个 Notion AI 助手](https://docs.dify.ai/zh-hans/learn-more/use-cases/build-an-notion-ai-assistant)
            
        *   [如何在几分钟内创建一个带有业务数据的官网 AI 智能客服](https://docs.dify.ai/zh-hans/learn-more/use-cases/create-an-ai-chatbot-with-business-data-in-minutes)
            
        *   [使用全套开源工具构建 LLM 应用实战：在 Dify 调用 Baichuan 开源模型能力](https://docs.dify.ai/zh-hans/learn-more/use-cases/practical-implementation-of-building-llm-applications-using-a-full-set-of-open-source-tools)
            
        *   [手摸手教你把 Dify 接入微信生态](https://docs.dify.ai/zh-hans/learn-more/use-cases/dify-on-wechat)
            
        *   [使用 Dify 和 Twilio 构建 WhatsApp 机器人](https://docs.dify.ai/zh-hans/learn-more/use-cases/dify-on-whatsapp)
            
        *   [将 Dify 应用与钉钉机器人集成](https://docs.dify.ai/zh-hans/learn-more/use-cases/dify-on-dingtalk)
            
        *   [使用 Dify 和 Azure Bot Framework 构建 Microsoft Teams 机器人](https://docs.dify.ai/zh-hans/learn-more/use-cases/dify-on-teams)
            
        *   [如何让 LLM 应用提供循序渐进的聊天体验？](https://docs.dify.ai/zh-hans/learn-more/use-cases/how-to-make-llm-app-provide-a-progressive-chat-experience)
            
        *   [如何将 Dify Chatbot 集成至 Wix 网站？](https://docs.dify.ai/zh-hans/learn-more/use-cases/how-to-integrate-dify-chatbot-to-your-wix-website)
            
        *   [如何连接 AWS Bedrock 知识库？](https://docs.dify.ai/zh-hans/learn-more/use-cases/how-to-connect-aws-bedrock)
            
        
    *   [扩展阅读](https://docs.dify.ai/zh-hans/learn-more/extended-reading)
        
        *   [什么是 LLMOps？](https://docs.dify.ai/zh-hans/learn-more/extended-reading/what-is-llmops)
            
        *   [什么是数组变量？](https://docs.dify.ai/zh-hans/learn-more/extended-reading/what-is-array-variable)
            
        *   [检索增强生成（RAG）](https://docs.dify.ai/zh-hans/learn-more/extended-reading/retrieval-augment)
            
            *   [混合检索](https://docs.dify.ai/zh-hans/learn-more/extended-reading/retrieval-augment/hybrid-search)
                
            *   [重排序](https://docs.dify.ai/zh-hans/learn-more/extended-reading/retrieval-augment/rerank)
                
            *   [召回模式](https://docs.dify.ai/zh-hans/learn-more/extended-reading/retrieval-augment/retrieval)
                
            
        *   [提示词编排](https://docs.dify.ai/zh-hans/learn-more/extended-reading/prompt-engineering)
            
        *   [如何使用 JSON Schema 让 LLM 输出遵循结构化格式的内容？](https://docs.dify.ai/zh-hans/learn-more/extended-reading/how-to-use-json-schema-in-dify)
            
        
    *   [常见问题](https://docs.dify.ai/zh-hans/learn-more/faq)
        
        *   [本地部署相关](https://docs.dify.ai/zh-hans/learn-more/faq/install-faq)
            
        *   [LLM 配置与使用](https://docs.dify.ai/zh-hans/learn-more/faq/llms-use-faq)
            
        
*   政策
    
    *   [开源许可证](https://docs.dify.ai/zh-hans/policies/open-source)
        
    *   [用户协议](https://docs.dify.ai/zh-hans/policies/agreement)
        
        *   [服务条款](https://dify.ai/terms)
        *   [隐私政策](https://dify.ai/privacy)
        

[Powered by GitBook](https://www.gitbook.com/?utm_source=content&utm_medium=trademark&utm_campaign=CdDIVDY6AtAz028MFT4d)

On this page

*   [部署 Xinference](https://docs.dify.ai/zh-hans/development/models-integration/xinference#bu-shu-xinference)
*   [开始部署](https://docs.dify.ai/zh-hans/development/models-integration/xinference#kai-shi-bu-shu)

[Edit on GitHub](https://github.com/langgenius/dify-docs/blob/main/zh_CN/development/models-integration/xinference.md)

1.  [研发](https://docs.dify.ai/zh-hans/development)
3.  [模型接入](https://docs.dify.ai/zh-hans/development/models-integration)

接入 Xinference 部署的本地模型
=====================

[Xorbits inference](https://github.com/xorbitsai/inference) 是一个强大且通用的分布式推理框架，旨在为大型语言模型、语音识别模型和多模态模型提供服务，甚至可以在笔记本电脑上使用。它支持多种与GGML兼容的模型,如 chatglm, baichuan, whisper, vicuna, orca 等。 Dify 支持以本地部署的方式接入 Xinference 部署的大型语言模型推理和 embedding 能力。

[](https://docs.dify.ai/zh-hans/development/models-integration/xinference#bu-shu-xinference)

部署 Xinference


---------------------------------------------------------------------------------------------------------------

### 

[](https://docs.dify.ai/zh-hans/development/models-integration/xinference#kai-shi-bu-shu)

开始部署

部署 Xinference 有两种方式，分别为[本地部署](https://github.com/xorbitsai/inference/blob/main/README_zh_CN.md#%E6%9C%AC%E5%9C%B0%E9%83%A8%E7%BD%B2)和[分布式部署](https://github.com/xorbitsai/inference/blob/main/README_zh_CN.md#%E5%88%86%E5%B8%83%E5%BC%8F%E9%83%A8%E7%BD%B2)，以下以本地部署为例。

1.  首先通过 PyPI 安装 Xinference：
    
    Copy
    
    ```
    $ pip install "xinference[all]"
    ```
    
2.  本地部署方式启动 Xinference：
    
    Copy
    
    ```
    $ xinference-local
    2023-08-20 19:21:05,265 xinference   10148 INFO     Xinference successfully started. Endpoint: http://127.0.0.1:9997
    2023-08-20 19:21:05,266 xinference.core.supervisor 10148 INFO     Worker 127.0.0.1:37822 has been added successfully
    2023-08-20 19:21:05,267 xinference.deploy.worker 10148 INFO     Xinference worker successfully started.
    ```
    
    Xinference 默认会在本地启动一个 worker，端点为：`http://127.0.0.1:9997`，端口默认为 `9997`。 默认只可本机访问，可配置 `-H 0.0.0.0`，非本地客户端可任意访问。 如需进一步修改 host 或 port，可查看 xinference 的帮助信息：`xinference-local --help`。
    
    > 使用 Dify Docker 部署方式的需要注意网络配置，确保 Dify 容器可以访问到 Xinference 的端点，Dify 容器内部无法访问到 localhost，需要使用宿主机 IP 地址。
    
3.  创建并部署模型
    
    进入 `http://127.0.0.1:9997` 选择需要部署的模型和规格进行部署，如下图所示：
    
    ![Image 7](https://docs.dify.ai/~gitbook/image?url=https%3A%2F%2F1288284732-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FCdDIVDY6AtAz028MFT4d%252Fuploads%252Fgit-blob-1da79199a6fdc32848d828fc0a647d724d7845c6%252Fimage.png%3Falt%3Dmedia&width=768&dpr=4&quality=100&sign=c927dc66&sv=2)
    
    由于不同模型在不同硬件平台兼容性不同，请查看 [Xinference 内置模型](https://inference.readthedocs.io/en/latest/models/builtin/index.html) 确定创建的模型是否支持当前硬件平台。
    
4.  获取模型 UID
    
    从上图所在页面获取对应模型的 ID，如：`2c886330-8849-11ee-9518-43b0b8f40bea`
    
5.  模型部署完毕，在 Dify 中使用接入模型
    
    在 `设置 > 模型供应商 > Xinference` 中填入：
    
    *   模型名称：`vicuna-v1.3`
        
    *   服务器 URL：`http://<Machine_IP>:9997` **替换成你的机器 IP 地址**
        
    *   模型 UID：`2c886330-8849-11ee-9518-43b0b8f40bea`
        
    
    "保存" 后即可在应用中使用该模型。
    

Dify 同时支持将 [Xinference embed 模型](https://github.com/xorbitsai/inference/blob/main/README_zh_CN.md#%E5%86%85%E7%BD%AE%E6%A8%A1%E5%9E%8B) 作为 Embedding 模型使用，只需在配置框中选择 `Embeddings` 类型即可。

如需获取 Xinference 更多信息，请参考：[Xorbits Inference](https://github.com/xorbitsai/inference/blob/main/README_zh_CN.md)

[Previous接入 Replicate 上的开源模型](https://docs.dify.ai/zh-hans/development/models-integration/replicate)[Next接入 OpenLLM 部署的本地模型](https://docs.dify.ai/zh-hans/development/models-integration/openllm)

Last updated 1 month ago
