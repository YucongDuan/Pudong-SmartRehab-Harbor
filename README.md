# DIKWP‑Mesh 4.1 Pudong SmartRehab Harbor

## 浦东光明中医医院人形康复智慧港与中西医转化医学协同平台 v1.0.0

这是一个依据用户提供的冯煜院长访谈与公开机构、康复、科研治理资料形成的**可直接运行工程原型**。系统把访谈中相互关联的发展方向映射为可配置、可审计、可分阶段验证的运行对象；不把访谈中的未来设想自动升级为临床有效性、监管许可或医院正式实施状态。

系统默认只装载合成患者、仿真设备、模型样本和演示工作流。临床、设备、围手术期和研究模块均保留人工门控，不执行自治诊断、处方、穴位/针刺参数生成、设备对人执行、出院决定、真实受试者招募或研究伦理判断。

## 1. 直接运行

### 1.1 本地运行

要求：Python 3.10 或更高版本。

```bash
python run.py serve
```

浏览器打开：

```text
http://127.0.0.1:8791
```

默认演示登录：

```text
用户名：admin
密码：mesh41-demo
```

正式运行代码只使用 Python 标准库、SQLite 和原生 JavaScript，不需要安装第三方 Python 包，不调用外部大模型 API。

### 1.2 Windows

```bat
start_windows.bat
```

### 1.3 Linux / macOS

```bash
chmod +x start.sh
./start.sh
```

### 1.4 Docker

```bash
docker compose up --build
```

浏览器打开 `http://127.0.0.1:8791`。

## 2. 系统覆盖的七个关键组合

| 组合 | 运行能力 | 默认边界 |
|---|---|---|
| 人形康复智慧港 | 六轴评估、Goal、设备会话、遥测、结果闭环、停止门控 | 合成/仿真会话；不授权设备对人执行 |
| 大师手法数字化实验室 | 力—位—时轨迹采集、版本、注释、RMSE 对照、人工复核 | 仿体或合成对象；不形成“名家水平”总分 |
| 六轴康复与疗愈环境 | 四个 ICF 来源限定轴，加患者意图/体验和疗愈暴露/反应 | 是本项目工作流配置，不是康复概念的普遍定义 |
| CM‑ERAS 质量闭环 | 围手术期阶段、人工批准检查点、结局字段、残差 | 不生成医嘱、方药、穴位、麻醉方案或出院决定 |
| 内分泌类器官研究治理 | 假设、版本、伦理、HGR、样本、门控、里程碑 | 研究治理容器，不形成治疗承诺 |
| 脑机接口/自体神经束治理 | 高敏感协议、设备、伦理、样本、停止状态和不良事件容器 | 不执行真实植入或受试者招募 |
| 肝硬化自体细胞研究治理 | 队列、样本、随访和结局的可追溯记录 | 不推导疗效，不替代正式临床研究系统 |

## 3. 四院区协同映射

系统将访谈中的院区表达保存为来源限定的运行角色：

- 罗山路院区：精品中医名医诊疗中心运行角色；
- 川沙院区：纯中医服务枢纽运行角色；
- 光明老院区（惠南方向）：中西医结合与 CM‑ERAS 枢纽运行角色；
- 光明中医医院智慧康复场景：高科技、人形康复、人机交互与转化研究运行角色。

这些是**工程映射**，不代表院方正式组织架构、科室授权、采购或项目立项。

## 4. 浏览器工作台

登录后包含九个工作区：

1. 运行总览；
2. 院区与战略组合；
3. 六轴康复闭环；
4. 人机交互与设备；
5. 大师手法数字化；
6. CM‑ERAS；
7. 转化研究治理；
8. DIKWP×DIKWP 意图驱动关系网；
9. 证据、边界与审计。

首次启动且数据库为空时装载 19 个演示资源，并生成确定性康复和手法遥测。再次启动不会覆盖已有记录。

## 5. 演示工作流角色

默认密码均为 `mesh41-demo`。

| 用户名 | 工作流角色 | 主要范围 |
|---|---|---|
| `admin` | admin | 全部演示权限 |
| `leader` | hospital_leader | 战略组合、运行总览、审计 |
| `rehabdoc` | rehab_physician | 康复评估、目标、CM‑ERAS、FHIR |
| `tcmdoc` | tcm_clinician | 中医临床记录、手法复核、CM‑ERAS |
| `therapist` | rehab_therapist | 康复会话、仿真设备、手法采集 |
| `engineer` | device_engineer | 设备、校准、遥测、手法实验 |
| `research` | research_pi | 研究协议、状态迁移、合成样本 |
| `ethics` | ethics_secretary | 伦理审查工作流记录 |
| `quality` | quality_manager | 质量、审计、FHIR |
| `data` | data_manager | 遥测、数据和导出 |
| `patient` | patient_proxy | 受限的本人目标/打卡工作流 |

这些身份仅是可审计的工作流标签，不是统一身份认证、执业资格核验或电子签名。

## 6. 命令行

```bash
python run.py serve
python run.py seed
python run.py health
python run.py catalog-summary
python run.py verify-audit
python run.py export-fhir rehab_episode REHAB-STROKE-001 --output outputs/rehab_fhir_bundle.json
python run.py import-telemetry-csv --device-id DEV-HUMANOID-001 --session-id CSV-DEMO-001 --input examples/device_telemetry.csv --subject-kind phantom
```

使用自定义数据库或端口：

```bash
SMART_HARBOR_DB=/secure/path/harbor.db \
SMART_HARBOR_HOST=127.0.0.1 \
SMART_HARBOR_PORT=8791 \
SMART_HARBOR_TOKEN_SECRET='replace-with-a-random-secret' \
python run.py serve
```

Windows PowerShell：

```powershell
$env:SMART_HARBOR_DB="D:\smart-harbor\harbor.db"
$env:SMART_HARBOR_TOKEN_SECRET="replace-with-a-random-secret"
python run.py serve
```

## 7. API 快速检查

健康检查无需登录：

```bash
curl http://127.0.0.1:8791/api/health
```

登录：

```bash
curl -s -X POST http://127.0.0.1:8791/api/auth/login \
  -H 'Content-Type: application/json' \
  -d '{"username":"admin","password":"mesh41-demo"}'
```

主要 API 契约位于 `openapi.yaml`；完整端点说明见 `docs/09_api_reference.md`。

## 8. 数据与审计

SQLite 默认位置：

```text
var/pudong_smartrehab_harbor.db
```

每次资源写入都保存：

- 资源类型和标识；
- 当前修订号；
- 规范化 JSON 的 SHA‑256；
- 操作者工作流标签；
- 操作类型和时间；
- 前序审计哈希与当前事件哈希。

验证审计链：

```bash
python run.py verify-audit
```

写操作采用乐观并发控制，调用方必须提供 `expected_revision`，防止静默覆盖。

## 9. DIKWP‑Mesh 4.1 运行约束

系统不为外部中医、康复、机器人或研究概念生成脱离来源的普遍定义。运行对象由表达、来源、观察者、时间、范围、单位、证据状态、意图来源、关系边和残差构成。

全部 25 个 DIKWP×DIKWP 有向通道作为关系地址开放：

```text
D→D … D→P
I→D … I→P
K→D … K→P
W→D … W→P
P→D … P→P
```

系统按当前意图激活子图，并通过节点删除和边删除反事实测试提取 `intent_conditioned_kernel`。该问题核只适用于当前数据、当前意图、当前来源和当前时间边界。

可靠性分开记录：

```text
Semantic Stability: S4–S0
Mesh Examination: M4–M0
Evidence Reliability:
Solid / Supported / Provisional / Borrowed / Hollow / Contested / Blocked
```

没有来源明确的权重时，不生成单一总分。

## 10. 质量验证

运行完整 QA：

```bash
python tools/run_qa.py
```

运行核心测试：

```bash
SMART_HARBOR_QUIET=1 PYTHONPATH=. python -m unittest discover -s tests -v
```

QA 包括：Python 编译、19 项单元/API 测试、JSON Schema 解析与示例验证、数据目录完整性、前端 JavaScript 语法、医疗边界静态审计、本地 HTTP 端到端和审计链校验。

## 11. 目录

```text
smart_harbor/     后端、领域逻辑、门控、Mesh、FHIR、CLI
web/              原生浏览器工作台
data/             来源登记、战略、康复、设备、CM‑ERAS、研究目录
schemas/          JSON Schema 2020-12
examples/         示例资源、FHIR、Mesh、设备遥测 CSV/JSON
openapi.yaml      API 契约
scripts/          启动与发行辅助脚本
tools/            QA、清单与打包工具
tests/            单元与 HTTP 验收测试
docs/             追溯矩阵、架构、工作流、数据、治理、部署、验收
outputs/          QA 与验收生成物
var/              本地 SQLite 运行目录
```

## 12. 明确边界

本发行包是：

```text
公共信息定制
合成数据验证
仿真/仿体设备验证
临床人员在环
离线优先
可直接运行工程原型
```

本发行包不是：

```text
医院委托、认可、采购、验收或正式上线系统
已经接入 HIS/EMR/LIS/PACS/统一身份/电子签名的系统
已经完成真实患者临床验证或医疗器械注册的系统
可自治诊断、处方、针刺参数、设备控制或出院决策的系统
真实受试者招募、伦理审批或人类遗传资源行政许可系统
实时号源、床位、厂商设备或真实生物样本平台
```

详细边界见 `docs/11_clinical_research_boundary.md`。
