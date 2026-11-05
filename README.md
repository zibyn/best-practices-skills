# best-practices-skills

不绑定任何品牌的开发框架与基础设施最佳实践 skills，可作为 Claude Code 与 Codex 插件安装。

## 为什么有这个仓库

Next.js、React、Supabase、Postgres 等生态已经有官方或社区维护的 best-practices skills，但它们大多带有明显的品牌特征：默认推荐自家托管平台、SDK、付费服务，或把某个厂商的约定当作通用规范。

本仓库把这些 skills 重写为**无品牌依赖**的版本：

- 只保留框架和技术本身的通用使用范式；
- 删除厂商推广、平台绑定和专有服务的默认推荐；
- 必须提到具体厂商时，把它作为可选项写，而不是默认方案；
- 每个重写的 skill 都在下方[来源](#来源)中注明原始出处。

## 目录结构

```text
skills/
  <skill-name>/
    SKILL.md          # skill 入口，含 name / description frontmatter
    references/       # 按需加载的参考资料（可选）
    scripts/          # 辅助脚本（可选）
```

每个 skill 一个目录，名称使用 kebab-case，按技术命名（如 `react`、`postgres`），而不是按厂商命名。

## 安装

### Claude Code

```text
/plugin marketplace add zibyn/best-practices-skills
/plugin install best-practices-skills@best-practices-skills
```

### Codex

把仓库作为插件安装，或直接把需要的 skill 复制到 Codex 的 skills 目录：

```bash
git clone https://github.com/zibyn/best-practices-skills.git
cp -r best-practices-skills/skills/<skill-name> ~/.codex/skills/
```

## 迁移与合并 skill

新增 skill 时，给出原 skill 的 GitHub 地址（或路径），按以下流程处理：

1. **读取原 skill**：`SKILL.md` 及其引用的全部文件。
2. **判断去向**：仓库已有同技术的 skill 则合并，否则新建目录。
3. **去品牌化重写**：移除厂商推广、平台绑定和专有服务的默认推荐，保留通用范式。
4. **合并去重**：与已有内容冲突时保留更通用、更新的做法，删除重复条目。
5. **登记来源**：在下方来源表中补一行，注明原地址、许可证和处理方式。

## 来源

本仓库中的 skill 由以下来源重写或合并而来。原作品版权归原作者所有，使用遵循各自许可证。

| Skill | 原始来源 | 许可证 | 处理方式 |
| ----- | -------- | ------ | -------- |
| _暂无_ | | | |

## 贡献

欢迎提交 PR。新增或修改 skill 时请：

- 保持内容与具体厂商无关；
- 在来源表中注明出处；
- 确认原 skill 的许可证允许修改和再分发。
