# 大学物理实验 LaTeX 报告 Skill

这是一个面向 `Codex`、`Claude Code` 及同类智能编程代理的公开 skill 仓库，用来根据实验指导书、课程要求、老师补充材料和原始数据照片，生成符合课程规范的 LaTeX 实验报告。

仓库**内置了课程切分资料 PDF**。因此安装时应当把**整个仓库目录**安装到 skills 目录中，而不是只复制单个 markdown 文件。

GitHub 仓库地址：

`https://github.com/tangentiiii/physics-lab-report-skill`

## 功能概览

- 内置课程资料：
  - `绪论课ppt.pdf`
  - 3 份课程基础知识 PDF
  - 7 份主题实验指导 PDF
- 自动定位当前实验对应的主题指导书
- 在必要时自动补读基础知识 PDF
- 优先遵循 `绪论课ppt.pdf` 中的实验报告规范
- 保守识别实验数据照片，并在分析前要求用户确认
- 支持从原始数据照片中裁剪出波形图、电路图、光路图等局部图像
- 生成 `report.tex`、`data.tex`、线性回归脚本与图像
- 生成可直接上传到 Overleaf 编译的项目结构

## 安装方式

由于本仓库包含内置 PDF 资料，请直接克隆**整个仓库**到 skill 目录。

### Codex

```bash
git clone --depth=1 https://github.com/tangentiiii/physics-lab-report-skill.git \
  ~/.codex/skills/physics-lab-report-skill
```

### Claude Code

```bash
git clone --depth=1 https://github.com/tangentiiii/physics-lab-report-skill.git \
  ~/.claude/skills/physics-lab-report-skill
```

安装后重启代理，使其重新发现新 skill。

如果已经安装过旧版本，可以在对应目录执行：

```bash
git pull
```

## 使用方式

典型提示词：

- “用这个实验的数据照片和老师 PPT 写一份 LaTeX 实验报告。”
- “读取仓库里的实验指导书，并结合我的原始数据生成简要报告。”
- “先识别数据，再让我确认，最后写完整实验报告。”
- “请按照绪论课 ppt 的要求处理不确定度、有效位数和线性回归。”
- “如果波形图在原始照片里，请先裁剪出来再插入报告。”
- “生成一个我上传到 Overleaf 就能直接编译的实验报告项目。”

也可以显式调用：

```text
使用 $physics-lab-report-skill 为这个实验生成 LaTeX 报告。
```

## 用户需要额外提供什么

仓库已经自带课程资料，但用户通常还需要在当前工作目录额外放入：

- 实验数据记录表照片或扫描件
- 老师下发的对应实验指导 PPT
- 单独的波形图、电路图、示意图或包含这些内容的原始照片
- 如有必要，用户自己的 LaTeX 模板或现有数据文件

也就是说，**课程共性资料内置在仓库中，个性化材料由用户本地提供**。

## 推荐工作流

1. 在一个单独的工作文件夹中放入你的实验相关材料。
2. 至少准备好原始数据照片；如果老师发了补充 PPT，也一并放入。
3. 让代理使用本 skill 先阅读仓库内置指导书和本地补充材料。
4. 让代理先识别并展示 `data.tex`，确认无误后再继续。
5. 让代理生成完整的 LaTeX 项目，包括 `report.tex`、`data.tex`、`figures/` 和必要的回归图。
6. 将整个生成后的项目文件夹上传到 Overleaf 编译。

## 输出结果通常包含什么

一个典型的生成结果通常包括：

- `report.tex`
- `data.tex`
- `figures/`
- 可选的 `scripts/`

其中：

- `report.tex` 是主报告文件
- `data.tex` 保存整理后的实验数据表
- `figures/` 保存原始照片、裁剪图、波形图、电路图和回归图
- `scripts/` 可保存线性回归或数据处理脚本，但 LaTeX 编译本身不应依赖这些脚本现场运行

## 仓库结构

```text
physics-lab-report-skill/
├── SKILL.md
├── README.md
├── LICENSE
├── .gitignore
├── agents/
│   └── openai.yaml
├── assets/
│   ├── brief-report-template.tex
│   ├── full-report-template.tex
│   └── data-template.tex
├── course-materials/
│   ├── 绪论课ppt.pdf
│   ├── 01_课程基础知识_测量误差及数据处理的基础知识.pdf
│   ├── 02_课程基础知识_电磁学实验基本仪器.pdf
│   ├── 03_课程基础知识_光学实验预备知识.pdf
│   ├── 04_主题实验_三线摆法和扭摆法测刚体的转动惯量.pdf
│   ├── 05_主题实验_准稳态法测不良导体的导热系数和比热.pdf
│   ├── 06_主题实验_霍尔效应及磁阻效应研究.pdf
│   ├── 07_主题实验_高温超导材料转变温度的测定.pdf
│   ├── 08_主题实验_示波器原理及应用.pdf
│   ├── 09_主题实验_透镜焦距的测量.pdf
│   └── 10_主题实验_迈克尔逊干涉光路的搭建和应用.pdf
└── references/
    ├── bundled-course-materials.md
    ├── material-discovery.md
    └── numerical-rules.md
```

## 设计原则

- 课程公共资料可以内置，但私人实验报告、私人数据、私人照片不能放入仓库。
- `绪论课ppt.pdf` 是报告格式、不确定度、有效位数、作图规范的首要依据。
- 老师补充 PPT 可以补充实验细节，也可能覆盖旧版实验步骤或仪器常数。
- 数据识别必须保守，不能擅自编造、修正或补全模糊数据。
- 如果波形图、电路图等已经是单独文件，可直接引用；如果它们嵌在原始照片中，应先裁剪出单独图片再插入报告。
- 生成的 LaTeX 工程应避免绝对路径、本地专属字体、本地脚本依赖和需要 shell-escape 的方案，以保证 Overleaf 可直接编译。
- 在用户确认 `data.tex` 之前，不得进入正式计算与成文阶段。

## 适用范围与限制

- 本仓库适用于已经拥有实验指导书、课程要求和原始数据的用户。
- 本 skill 可以帮助整理、提取、计算和成文，但不会替用户伪造缺失数据。
- 如果图片内容模糊、裁剪边界不清、两次识别冲突，代理应停止猜测并要求用户人工确认。
- 如果你所在课程的实验报告规范与本仓库内置资料不同，请优先以你自己的课程要求为准，并让代理读取你的本地规则文件。

## 更新

如果你已经通过 Git 安装了本 skill，后续可以进入对应目录执行：

```bash
git pull origin main
```

如果你的默认分支不是 `main`，请改成实际分支名。

## License

MIT
