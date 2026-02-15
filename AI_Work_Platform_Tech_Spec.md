# AI Work Platform - 技术实现规范

## 🔧 开发环境搭建

### 前置要求

- **Node.js**: >= 18.0.0
- **Python**: >= 3.10
- **Rust**: >= 1.70 (Tauri 需要)
- **Git**: >= 2.30

### 安装 Rust

```bash
# Linux/MacOS
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env

# Windows
# 下载并运行: https://rustup.rs/
```

### 安装 Tauri CLI

```bash
cargo install tauri-cli
```

---

## 📦 项目初始化

### 1. 创建 Tauri + Vue 3 项目

```bash
# 创建项目
npm create tauri-app@latest ai-work-platform

# 选择配置
✔ Project name · ai-work-platform
✔ Choose which language to use for your frontend · TypeScript / JavaScript
  ◉ TypeScript
  ○ JavaScript
✔ Choose your package manager · npm
✔ Choose your UI template · Vue 3
✔ Choose your UI flavor · TypeScript
```

### 2. 安装依赖

```bash
cd ai-work-platform
npm install
```

### 3. 安装 Vue 相关依赖

```bash
npm install vue-router pinia axios
npm install element-plus @element-plus/icons-vue
npm install @tauri-apps/api
```

### 4. 创建后端项目

```bash
mkdir backend
cd backend

# 创建虚拟环境
python3 -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# 安装依赖
pip install fastapi uvicorn sqlalchemy
pip install httpx python-multipart
pip install python-dotenv
pip install schedule
pip install openai anthropic

# 保存依赖
pip freeze > requirements.txt
```

---

## 🏗️ 前端项目结构详解

```
frontend/
├── src/
│   ├── main.ts                    # 应用入口
│   ├── App.vue                    # 根组件
│   ├── router/
│   │   └── index.ts               # 路由配置
│   ├── stores/
│   │   ├── user.ts                # 用户状态
│   │   ├── agent.ts               # Agent 状态
│   │   ├── flow.ts                # Flow 状态
│   │   └── skill.ts               # 技能状态
│   ├── components/
│   │   ├── common/
│   │   │   ├── Header.vue         # 顶部导航
│   │   │   ├── Sidebar.vue        # 侧边栏
│   │   │   └── Loading.vue        # 加载动画
│   │   ├── user/
│   │   │   └── UserProfile.vue    # 用户设置
│   │   ├── agent/
│   │   │   ├── AgentCard.vue      # Agent 卡片
│   │   │   ├── AgentList.vue      # Agent 列表
│   │   │   ├── AgentDetail.vue    # Agent 详情
│   │   │   └── AgentCreator.vue   # Agent 创建
│   │   ├── flow/
│   │   │   ├── FlowCard.vue       # Flow 卡片
│   │   │   ├── FlowList.vue       # Flow 列表
│   │   │   ├── FlowDetail.vue     # Flow 详情
│   │   │   └── FlowCreator.vue    # Flow 创建
│   │   └── skill/
│   │       ├── SkillCard.vue      # 技能卡片
│   │       ├── SkillList.vue      # 技能列表
│   │       └── SkillCreator.vue   # 技能创建
│   ├── views/
│   │   ├── Home.vue               # 首页
│   │   ├── UserSetup.vue          # 用户设置页
│   │   ├── AgentManagement.vue    # Agent 管理页
│   │   ├── FlowManagement.vue     # Flow 管理页
│   │   ├── SkillManagement.vue    # 技能管理页
│   │   ├── Monitor.vue            # 监控页
│   │   └── Settings.vue           # 设置页
│   ├── api/
│   │   ├── index.ts               # API 基础配置
│   │   ├── user.ts                # 用户 API
│   │   ├── agent.ts               # Agent API
│   │   ├── flow.ts                # Flow API
│   │   ├── skill.ts               # 技能 API
│   │   └── model.ts               # Model API
│   ├── types/
│   │   ├── user.ts                # 用户类型
│   │   ├── agent.ts               # Agent 类型
│   │   ├── flow.ts                # Flow 类型
│   │   ├── skill.ts               # 技能类型
│   │   └── model.ts               # Model 类型
│   ├── utils/
│   │   ├── tauri.ts               # Tauri 工具
│   │   ├── format.ts              # 格式化工具
│   │   └── validate.ts            # 验证工具
│   └── assets/
│       └── styles/
│           └── global.css         # 全局样式
├── src-tauri/
│   ├── src/
│   │   ├── main.rs                # Rust 主程序
│   │   ├── lib.rs                 # Tauri 命令
│   │   └── backend/
│   │       └── spawn.py           # 后端启动脚本
│   ├── tauri.conf.json            # Tauri 配置
│   └── Cargo.toml                 # Rust 依赖
└── package.json
```

---

## 🐍 后端项目结构详解

```
backend/
├── app/
│   ├── __init__.py
│   ├── main.py                    # FastAPI 主入口
│   ├── config.py                  # 配置管理
│   ├── dependencies.py            # 依赖注入
│   │
│   ├── api/
│   │   ├── __init__.py
│   │   ├── deps.py                # API 依赖
│   │   ├── router.py              # 路由聚合
│   │   ├── user.py                # 用户 API
│   │   ├── agent.py               # Agent API
│   │   ├── flow.py                # Flow API
│   │   ├── skill.py               # 技能 API
│   │   └── model.py               # Model API
│   │
│   ├── models/
│   │   ├── __init__.py
│   │   ├── user.py                # 用户数据模型
│   │   ├── agent.py               # Agent 数据模型
│   │   ├── flow.py                # Flow 数据模型
│   │   ├── skill.py               # 技能数据模型
│   │   └── model.py               # Model 数据模型
│   │
│   ├── schemas/
│   │   ├── __init__.py
│   │   ├── user.py                # 用户 Schema (Pydantic)
│   │   ├── agent.py               # Agent Schema
│   │   ├── flow.py                # Flow Schema
│   │   ├── skill.py               # 技能 Schema
│   │   └── model.py               # Model Schema
│   │
│   ├── services/
│   │   ├── __init__.py
│   │   ├── user_service.py        # 用户服务
│   │   ├── agent_service.py       # Agent 服务
│   │   ├── flow_service.py        # Flow 服务
│   │   ├── skill_service.py       # 技能服务
│   │   └── model_service.py       # Model 服务
│   │
│   ├── core/
│   │   ├── __init__.py
│   │   ├── config.py              # 核心配置
│   │   ├── database.py            # 数据库连接
│   │   ├── security.py            # 安全相关（加密等）
│   │   ├── logger.py              # 日志配置
│   │   │
│   │   ├── agent_engine.py        # Agent 引擎
│   │   ├── skill_manager.py       # 技能管理器
│   │   ├── flow_engine.py         # Flow 引擎
│   │   ├── model_manager.py       # Model 管理器
│   │   ├── tool_manager.py        # 工具管理器
│   │   ├── memory_manager.py      # 记忆管理器
│   │   └── scheduler.py           # 任务调度器
│   │
│   ├── db/
│   │   ├── __init__.py
│   │   ├── session.py             # 数据库 Session
│   │   ├── base.py                # 基础模型
│   │   └── init_db.py             # 初始化数据库
│   │
│   ├── utils/
│   │   ├── __init__.py
│   │   ├── file_utils.py          # 文件工具
│   │   ├── llm_client.py          # LLM 客户端
│   │   ├── skill_parser.py        # 技能解析器
│   │   └── flow_parser.py         # Flow 解析器
│   │
│   └── tools/                     # 内置工具（参考 OpenClaw）
│       ├── __init__.py
│       ├── web_search.py          # 网络搜索
│       ├── web_fetch.py           # 网页获取
│       ├── image_generate.py      # 图片生成
│       ├── video_generate.py      # 视频生成
│       ├── tts.py                 # 语音合成
│       └── file_ops.py            # 文件操作
│
├── agents/                        # Agent 数据目录
│   └── {agent_id}/
│       ├── SOUL.md
│       ├── IDENTITY.md
│       ├── RULES.md
│       ├── MEMORY.md
│       ├── SKILLS.md
│       └── CONFIG.json
│
├── skills/                        # 技能目录
│   ├── official/                  # 官方技能（只读）
│   │   ├── manhua_story/          # 漫剧故事生成
│   │   │   └── SKILL.md
│   │   ├── storyboard/            # 分镜脚本生成
│   │   │   └── SKILL.md
│   │   ├── character_dialogue/    # 角色对话生成
│   │   │   └── SKILL.md
│   │   ├── storyboard_to_image/   # 分镜转图片
│   │   │   └── SKILL.md
│   │   ├── voice_synthesis/       # 语音合成
│   │   │   └── SKILL.md
│   │   ├── video_generate/        # 视频生成
│   │   │   └── SKILL.md
│   │   └── bgm_recommend/         # 背景音乐推荐
│   │       └── SKILL.md
│   └── custom/                    # 用户自定义技能
│       └── {skill_id}/
│           └── SKILL.md
│
├── data/                          # 数据目录
│   ├── agents.db                  # Agent 数据库 (SQLite)
│   ├── users.db                   # 用户数据库
│   ├── flows.db                   # Flow 数据库
│   └── logs/                      # 日志目录
│       └── app.log
│
├── tests/                         # 测试目录
│   ├── __init__.py
│   ├── test_agent_engine.py
│   ├── test_flow_engine.py
│   └── test_skill_manager.py
│
├── scripts/                       # 脚本目录
│   ├── init_db.py                 # 初始化数据库
│   └── seed_data.py               # 种子数据
│
├── requirements.txt               # Python 依赖
├── .env.example                   # 环境变量示例
├── .env                           # 环境变量（不提交）
└── README.md
```

---

## 📝 核心模块实现

### 1. 用户设置模块

#### 后端：`app/api/user.py`

```python
from fastapi import APIRouter, Depends
from sqlalchemy.orm import Session
from app.schemas.user import UserProfile, UserProfileCreate
from app.db.session import get_db
from app.services.user_service import UserService

router = APIRouter()

@router.get("/profile", response_model=UserProfile)
async def get_profile(db: Session = Depends(get_db)):
    """获取用户配置"""
    service = UserService(db)
    return service.get_profile()

@router.post("/profile", response_model=UserProfile)
async def create_profile(
    profile: UserProfileCreate,
    db: Session = Depends(get_db)
):
    """创建或更新用户配置"""
    service = UserService(db)
    return service.create_or_update_profile(profile)

@router.put("/profile/{profile_id}", response_model=UserProfile)
async def update_profile(
    profile_id: str,
    profile: UserProfileCreate,
    db: Session = Depends(get_db)
):
    """更新用户配置"""
    service = UserService(db)
    return service.update_profile(profile_id, profile)
```

#### 前端：`src/views/UserSetup.vue`

```vue
<template>
  <div class="user-setup">
    <el-card>
      <template #header>
        <h2>欢迎！让我们先了解你</h2>
      </template>

      <el-form :model="form" :rules="rules" ref="formRef">
        <!-- 基本信息 -->
        <el-form-item label="姓名" prop="name">
          <el-input v-model="form.name" placeholder="请输入你的名字" />
        </el-form-item>

        <el-form-item label="职业" prop="profession">
          <el-input v-model="form.profession" placeholder="例如：内容创作者" />
        </el-form-item>

        <!-- 联系方式 -->
        <el-form-item label="邮箱" prop="email">
          <el-input v-model="form.email" placeholder="example@email.com" />
        </el-form-item>

        <!-- 工作偏好 -->
        <el-form-item label="工作风格" prop="response_style">
          <el-radio-group v-model="form.response_style">
            <el-radio label="简洁">简洁明了</el-radio>
            <el-radio label="详细">详细周全</el-radio>
            <el-radio label="幽默">幽默风趣</el-radio>
          </el-radio-group>
        </el-form-item>

        <!-- 技能标签 -->
        <el-form-item label="你擅长的技能">
          <el-select
            v-model="form.skills"
            multiple
            placeholder="选择你的技能"
          >
            <el-option
              v-for="skill in commonSkills"
              :key="skill"
              :label="skill"
              :value="skill"
            />
          </el-select>
        </el-form-item>

        <!-- 使用目标 -->
        <el-form-item label="使用平台的主要目的">
          <el-checkbox-group v-model="form.goals">
            <el-checkbox label="制作漫剧">制作漫剧</el-checkbox>
            <el-checkbox label="提升效率">提升工作效率</el-checkbox>
            <el-checkbox label="内容创作">内容创作</el-checkbox>
            <el-checkbox label="其他">其他</el-checkbox>
          </el-checkbox-group>
        </el-form-item>

        <el-form-item>
          <el-button type="primary" @click="submitForm">完成设置</el-button>
        </el-form-item>
      </el-form>
    </el-card>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive } from 'vue'
import { ElMessage } from 'element-plus'
import type { FormInstance, FormRules } from 'element-plus'
import { createUserProfile } from '@/api/user'

const formRef = ref<FormInstance>()
const form = reactive({
  name: '',
  profession: '',
  email: '',
  response_style: '简洁',
  skills: [] as string[],
  goals: [] as string[]
})

const commonSkills = [
  '文案写作',
  '视频剪辑',
  '图片设计',
  'AI工具使用',
  '数据分析',
  '项目管理'
]

const rules = {
  name: [{ required: true, message: '请输入姓名', trigger: 'blur' }],
  profession: [{ required: true, message: '请输入职业', trigger: 'blur' }],
  email: [
    { required: true, message: '请输入邮箱', trigger: 'blur' },
    { type: 'email', message: '邮箱格式不正确', trigger: 'blur' }
  ]
}

const submitForm = async () => {
  if (!formRef.value) return

  await formRef.value.validate(async (valid) => {
    if (valid) {
      try {
        await createUserProfile(form)
        ElMessage.success('设置完成！')
        // 跳转到首页
      } catch (error) {
        ElMessage.error('设置失败，请重试')
      }
    }
  })
}
</script>
```

---

### 2. Agent 自动生成模块

#### 后端：`app/services/agent_service.py`

```python
from typing import Dict, Any
from app.utils.llm_client import LLMClient
from app.utils.file_utils import write_file
import json
import os

class AgentService:
    def __init__(self, db):
        self.db = db
        self.llm_client = LLMClient()

    async def generate_agent(self, job_description: Dict[str, Any]) -> Dict[str, Any]:
        """根据职位描述生成 Agent 配置"""

        # 构建生成提示词
        prompt = self._build_generation_prompt(job_description)

        # 调用 LLM 生成 Agent 配置
        response = await self.llm_client.chat(
            prompt=prompt,
            model="gpt-4o"  # 使用强模型生成
        )

        # 解析生成的配置
        agent_config = self._parse_agent_config(response)

        # 创建 Agent 目录
        agent_id = agent_config["agent_id"]
        agent_dir = f"agents/{agent_id}"
        os.makedirs(agent_dir, exist_ok=True)

        # 写入配置文件
        self._write_agent_files(agent_dir, agent_config)

        # 保存到数据库
        self._save_to_database(agent_config)

        return agent_config

    def _build_generation_prompt(self, job_description: Dict[str, Any]) -> str:
        """构建生成提示词"""
        return f"""
        请根据以下职位描述，生成一个完整的 Agent 配置：

        职位名称：{job_description['job_title']}
        工作内容：{job_description['job_description']}
        技能要求：{job_description['skills_required']}
        工作风格：{job_description['work_style']}
        输出要求：{job_description['output_requirements']}

        请生成以下文件的内容：
        1. IDENTITY.md - 身份信息（姓名、职位、简介）
        2. SOUL.md - 灵魂（性格、价值观、行为准则）
        3. RULES.md - 行为准则和安全边界
        4. SKILLS.md - 推荐的技能列表
        5. CONFIG.json - 默认配置（推荐模型、工具权限）

        推荐模型考虑以下因素：
        - 中文任务优先推荐 GLM-4
- 创意任务优先推荐 Claude-3.5
- 通用任务推荐 GPT-4o-mini

        推荐技能从以下官方技能中选择：
        - 漫剧故事生成
        - 分镜脚本生成
        - 角色对话生成
        - 分镜转图片
        - 语音合成
        - 视频生成

        请以 JSON 格式返回，包含以下字段：
        {{
          "agent_id": "uuid",
          "identity": "IDENTITY.md 内容",
          "soul": "SOUL.md 内容",
          "rules": "RULES.md 内容",
          "skills": ["技能1", "技能2"],
          "config": {{"default_model": "模型名称"}}
        }}
        """

    def _parse_agent_config(self, response: str) -> Dict[str, Any]:
        """解析 LLM 返回的配置"""
        import uuid
        import json

        try:
            config = json.loads(response)
            config["agent_id"] = str(uuid.uuid4())
            return config
        except json.JSONDecodeError:
            # 如果解析失败，返回默认配置
            return self._get_default_config()

    def _write_agent_files(self, agent_dir: str, config: Dict[str, Any]):
        """写入 Agent 配置文件"""
        write_file(f"{agent_dir}/IDENTITY.md", config["identity"])
        write_file(f"{agent_dir}/SOUL.md", config["soul"])
        write_file(f"{agent_dir}/RULES.md", config["rules"])
        write_file(f"{agent_dir}/SKILLS.md", "\n".join(config["skills"]))

        # CONFIG.json
        config_data = {
            "default_model": config["config"]["default_model"],
            "tools_enabled": ["web_search", "tts", "image_generate"],
            "created_at": datetime.datetime.now().isoformat()
        }
        write_file(f"{agent_dir}/CONFIG.json", json.dumps(config_data, indent=2))

    def _get_default_config(self) -> Dict[str, Any]:
        """获取默认配置"""
        import uuid
        return {
            "agent_id": str(uuid.uuid4()),
            "identity": "# IDENTITY.md\n- **姓名**: AI 助手\n- **职位**: 通用助手",
            "soul": "# SOUL.md\n乐于助人，诚实可靠。",
            "rules": "# RULES.md\n1. 遵守法律法规\n2. 保护用户隐私",
            "skills": [],
            "config": {"default_model": "gpt-4o-mini"}
        }
```

---

### 3. Flow 自然语言解析模块

#### 后端：`app/utils/flow_parser.py`

```python
import re
from typing import Dict, List, Any
from app.utils.llm_client import LLMClient

class FlowParser:
    def __init__(self):
        self.llm_client = LLMClient()

    async def parse_natural_language(self, description: str) -> Dict[str, Any]:
        """将自然语言描述解析为结构化 Flow"""

        # 构建解析提示词
        prompt = f"""
        请将以下工作流描述解析为结构化的 Flow 定义：

        {description}

        要求：
        1. 识别涉及的 Agent（根据职位名称）
        2. 识别每个 Agent 的任务
        3. 识别任务之间的依赖关系
        4. 确定每个任务的输入和输出

        请以 JSON 格式返回：
        {{
          "flow_id": "uuid",
          "name": "Flow 名称",
          "description": "Flow 描述",
          "agents": [
            {{
              "agent_id": "agent_id",
              "agent_name": "Agent 名称",
              "tasks": [
                {{
                  "task_id": "task_id",
                  "name": "任务名称",
                  "input": "输入描述",
                  "output": "输出描述",
                  "skills": ["技能1", "技能2"]
                }}
              ]
            }}
          ],
          "connections": [
            {{"from": "task_id_1", "to": "task_id_2"}}
          ]
        }}
        """

        # 调用 LLM 解析
        response = await self.llm_client.chat(
            prompt=prompt,
            model="gpt-4o"
        )

        # 解析 JSON
        import json
        import uuid

        try:
            flow = json.loads(response)
            flow["flow_id"] = str(uuid.uuid4())
            return flow
        except json.JSONDecodeError:
            # 解析失败，返回错误
            raise ValueError("无法解析工作流描述")

    def validate_flow(self, flow: Dict[str, Any]) -> bool:
        """验证 Flow 的完整性"""
        if not flow.get("agents"):
            return False

        # 检查任务依赖关系
        task_ids = []
        for agent in flow["agents"]:
            for task in agent["tasks"]:
                task_ids.append(task["task_id"])

        for conn in flow.get("connections", []):
            if conn["from"] not in task_ids or conn["to"] not in task_ids:
                return False

        return True
```

---

### 4. Agent 工作引擎

#### 后端：`app/core/agent_engine.py`

```python
import asyncio
from typing import Dict, Any, Optional
from app.core.model_manager import ModelManager
from app.core.skill_manager import SkillManager
from app.core.tool_manager import ToolManager
from app.core.memory_manager import MemoryManager

class AgentEngine:
    def __init__(self, agent_id: str):
        self.agent_id = agent_id
        self.agent_dir = f"agents/{agent_id}"

        # 加载 Agent 配置
        self.config = self._load_config()
        self.identity = self._load_file("IDENTITY.md")
        self.soul = self._load_file("SOUL.md")
        self.rules = self._load_file("RULES.md")
        self.skills = self._load_file("SKILLS.md")

        # 初始化管理器
        self.model_manager = ModelManager(self.config["default_model"])
        self.skill_manager = SkillManager()
        self.tool_manager = ToolManager(self.config["tools_enabled"])
        self.memory_manager = MemoryManager(self.agent_id)

    async def process_message(
        self,
        message: str,
        context: Optional[Dict[str, Any]] = None
    ) -> str:
        """处理用户消息"""

        # 1. 加载记忆
        memories = self.memory_manager.get_relevant_memories(message)

        # 2. 构建提示词
        prompt = self._build_prompt(message, memories, context)

        # 3. 加载技能
        skills = self.skill_manager.load_skills(self.skills)

        # 4. 调用 LLM
        response = await self.model_manager.chat(
            prompt=prompt,
            skills=skills,
            tools=self.tool_manager.get_available_tools()
        )

        # 5. 处理工具调用
        if response.get("tool_calls"):
            tool_results = await self._execute_tool_calls(response["tool_calls"])
            # 将工具结果追加到提示词，重新调用
            prompt = self._append_tool_results(prompt, tool_results)
            response = await self.model_manager.chat(prompt=prompt)

        # 6. 保存记忆
        self.memory_manager.save_memory(message, response["content"])

        return response["content"]

    def _build_prompt(
        self,
        message: str,
        memories: str,
        context: Optional[Dict[str, Any]]
    ) -> str:
        """构建提示词"""
        prompt = f"""
        {self.identity}

        {self.soul}

        {self.rules}

        记忆：
        {memories}

        用户消息：
        {message}
        """

        if context:
            prompt += f"\n\n上下文：\n{context}"

        return prompt

    async def _execute_tool_calls(self, tool_calls: list) -> list:
        """执行工具调用"""
        results = []
        for call in tool_calls:
            tool_name = call["name"]
            tool_args = call["arguments"]
            result = await self.tool_manager.execute(tool_name, **tool_args)
            results.append({
                "tool": tool_name,
                "result": result
            })
        return results

    def _load_config(self) -> Dict[str, Any]:
        """加载配置"""
        import json
        with open(f"{self.agent_dir}/CONFIG.json", "r") as f:
            return json.load(f)

    def _load_file(self, filename: str) -> str:
        """加载文件"""
        with open(f"{self.agent_dir}/{filename}", "r") as f:
            return f.read()
```

---

## 🔄 IPC 通信设计

### Tauri 命令定义（Rust）

```rust
// src-tauri/src/lib.rs
use tauri::State;
use std::sync::Mutex;

// 后端进程句柄
struct BackendHandle {
    process: Option<std::process::Child>,
}

#[tauri::command]
async fn start_backend(handle: State<Mutex<BackendHandle>>) -> Result<(), String> {
    // 启动 Python 后端
    let mut cmd = std::process::Command::new("python3");
    cmd.arg("backend/main.py");
    cmd.arg("--port").arg("8000");

    match cmd.spawn() {
        Ok(child) => {
            let mut handle = handle.lock().unwrap();
            handle.process = Some(child);
            Ok(())
        }
        Err(e) => Err(e.to_string()),
    }
}

#[tauri::command]
async fn stop_backend(handle: State<Mutex<BackendHandle>>) -> Result<(), String> {
    let mut handle = handle.lock().unwrap();
    if let Some(mut child) = handle.process.take() {
        child.kill().map_err(|e| e.to_string())?;
    }
    Ok(())
}

#[tauri::command]
async fn check_backend_status() -> Result<bool, String> {
    // 检查后端是否运行
    let client = reqwest::Client::new();
    match client.get("http://localhost:8000/health").send().await {
        Ok(_) => Ok(true),
        Err(_) => Ok(false),
    }
}

#[cfg_attr(mobile, tauri::mobile_entry_point)]
pub fn run() {
    tauri::Builder::default()
        .manage(Mutex::new(BackendHandle {
            process: None,
        }))
        .invoke_handler(tauri::generate_handler![
            start_backend,
            stop_backend,
            check_backend_status
        ])
        .run(tauri::generate_context!())
        .expect("error while running tauri application");
}
```

### 前端调用（Vue 3）

```typescript
// src/utils/tauri.ts
import { invoke } from '@tauri-apps/api/tauri'

export async function startBackend() {
  try {
    await invoke('start_backend')
    console.log('Backend started')
  } catch (error) {
    console.error('Failed to start backend:', error)
    throw error
  }
}

export async function stopBackend() {
  try {
    await invoke('stop_backend')
    console.log('Backend stopped')
  } catch (error) {
    console.error('Failed to stop backend:', error)
    throw error
  }
}

export async function checkBackendStatus(): Promise<boolean> {
  try {
    const status = await invoke<boolean>('check_backend_status')
    return status
  } catch (error) {
    console.error('Failed to check backend status:', error)
    return false
  }
}
```

### 应用启动流程

```typescript
// src/main.ts
import { createApp } from 'vue'
import { createPinia } from 'pinia'
import router from './router'
import App from './App.vue'
import { startBackend, checkBackendStatus } from './utils/tauri'

const app = createApp(App)
app.use(createPinia())
app.use(router)

app.mount('#app')

// 启动后端
startBackend().then(() => {
  console.log('Application ready')
}).catch((error) => {
  console.error('Failed to start application:', error)
})
```

---

## 📦 打包和分发

### Windows 打包

```bash
# 构建前端
npm run build

# 构建 Tauri 应用
npm run tauri build

# 生成的安装程序
# src-tauri/target/release/bundle/nsis/AI-Work-Platform_1.0.0_x64-setup.exe
```

### Tauri 配置（`src-tauri/tauri.conf.json`）

```json
{
  "build": {
    "beforeDevCommand": "npm run dev",
    "beforeBuildCommand": "npm run build",
    "devUrl": "http://localhost:5173",
    "frontendDist": "../dist"
  },
  "package": {
    "productName": "AI Work Platform",
    "version": "1.0.0"
  },
  "tauri": {
    "allowlist": {
      "all": false,
      "shell": {
        "all": false,
        "open": true
      },
      "process": {
        "all": false,
        "relaunch": true,
        "exit": true
      }
    },
    "bundle": {
      "active": true,
      "targets": ["msi", "nsis"],
      "identifier": "com.aiwork.platform",
      "icon": [
        "icons/32x32.png",
        "icons/128x128.png",
        "icons/128x128@2x.png",
        "icons/icon.icns",
        "icons/icon.ico"
      ]
    },
    "security": {
      "csp": null
    },
    "updater": {
      "active": false
    }
  }
}
```

---

## 🚀 启动和运行

### 开发模式

```bash
# 终端 1：启动前端
npm run tauri dev

# 终端 2：启动后端（手动启动，方便调试）
cd backend
source venv/bin/activate
python3 main.py --port 8000 --reload
```

### 生产模式

```bash
# 构建安装程序
npm run tauri build

# 运行安装程序
# AI-Work-Platform_1.0.0_x64-setup.exe

# 应用会自动启动后端服务
```

---

## 🧪 测试

### 后端测试

```bash
cd backend
pytest tests/ -v
```

### 前端测试

```bash
npm run test
```

---

## 📝 API 文档

启动后端后，访问：
- Swagger UI: http://localhost:8000/docs
- ReDoc: http://localhost:8000/redoc

---

**文档创建时间**：2026-02-15
**版本**：v1.0
