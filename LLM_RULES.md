# 附录: AI/LLM 开发规则

本文档为 `VIBE_RULES.md` 的补充，适用于涉及与大型语言模型 (LLM) 直接交互的项目。当项目涉及LLM时，此处的规则必须被遵守。

---

## 1. 核心原则

*   **实时API测试 (Live API Testing)**: 所有涉及LLM交互的测试都必须针对真实的、实时的LLM API进行。严禁模拟（Mock）LLM的响应。
*   **提示词与模型解耦**:
    *   **提示词 (Prompts)**: 所有的LLM提示词都必须存储为外部的、带版本号的文件（例如 `summarize_v1.prompt.md`），严禁在源代码中硬编码。
    *   **模型名称 (Model Names)**: 具体的模型名称（例如 `qwen-turbo`）被视为配置项，必须从环境变量加载，不得硬编码。

---

## 2. 项目结构

*   **提示词文件位置**: 提示词文件必须与其直接使用的源代码模块并置存放。
    ```
    /src/
    |-- module_a/
    |   |-- feature_x.py
    |   |-- summarize_v1.prompt.md  #<-- 提示词文件
    ```

---

## 3. 运行时可靠性

*   **强制输出校验 (Mandatory Output Validation)**: 对于要求LLM返回的任何结构化数据（例如JSON），必须在运行时进行严格校验。流程如下：
    1.  **尽力请求 (Best-Effort Request)**: 如果LLM API提供结构化输出功能（例如 JSON mode），应优先使用。
    2.  **强制校验 (Mandatory Verification)**: 无论API功能如何，接收到的原始输出在被应用程序使用前，都必须通过一个严格的模式（Schema，例如使用 Pydantic 或 Zod）进行解析和验证。 