# Hypit Video Studio — GPT 驱动的视频创作实验

本仓库由 GPT 通过 GitHub 连接器维护，使用 GitHub Actions Ubuntu runner 安装和运行 Hypit；无需用户在 Windows 本地安装 Codex、Docker 或 Hypit。

## 已通过的真实验收（2026-11-14）

| 实验 | 结果 | 证据 |
|---|---|---|
| Hypit CLI 0.3.2 安装与版本检查 | PASS | [安装检查 Run #3](https://github.com/Justalittlebittt/hypit-video-studio/actions/runs/43562452354) |
| 《别咬了！》7镜头 SVML 编译 | PASS | [镜头检查及计划 Run](https://github.com/Justalittlebittt/hypit-video-studio/actions/runs/43563350102) |
| S02 咬薯片 + S05 月球宇航员需求图 | PASS，2个 Seedance 2 mini 请求 | 同上 |
| 全部7镜头生成任务计划 | PASS，7个独立请求 | 同上 |
| 不调用生成模型的本地 MP4 渲染 | PASS | [Hypit Build Run](https://github.com/Justalittlebittt/hypit-video-studio/actions/runs/43563917727) |
| 本地渲染导出媒体检查 | PASS：H.264 360×640、3.000秒、AAC | 同上，artifact `hypit-free-render-mp4` |

本地渲染测试的真实 Build ID：
`bld_20261114T055730589Z_90DBBF1A30`

**重要边界：** 已经证明 CLI、SVML 编译、两阶段 Run 计划、本地渲染与导出。**没有**调用 Seedance、没有生成真人广告镜头，没有制作完成29秒成片，也没有产生视频模型费用。3秒 `CRUNCH!` 文字短片只是基础设施验收，并非最终广告。

## 《别咬了！》项目位置

`projects/chip-ad-001/`

- `author.svml`：7个分镜及独立视频生成资源
- `recipes.svs`：画面/文字样式
- `two-takes.svrun`：关键镜头生成计划
- `seven-takes.svrun`：7镜头资源生成计划
- `render-smoke.svml` + `render-smoke.svrun`：已成功运行的无收费模型本地渲染测试
- `hypit.runtime.json`：本地媒体处理及 HTML 渲染服务配置

## 下一步

1. 先对真实 S02 / S05 镜头选择视频生成 Provider，确定模型、账户、单镜头价格、输入限制。
2. 未经用户同意，不调用收费的视频生成或自动重试。
3. 在生成真实镜头后，再把完成的素材、咔嚓音效、宇航员及妻子台词、片尾文案组装成29秒成片。
4. 已验收输出应通过 Hypit 显式 Candidate / Build Result 复用，而不是全部重新生成。

### 本地重现

在具备 Node 22、npm、FFmpeg、Chromium/Chrome 及联网能力的环境：

```bash
npm install -g @hypit/hypit@0.3.2
cd projects/chip-ad-001
npm install --no-save @fontsource-variable/inter
hypit check author.svml
hypit plan two-takes.svrun
hypit plan seven-takes.svrun
hypit runtime use hypit.runtime.json
hypit runtime up
hypit build render-smoke.svrun --follow
```

最后一条 Build **只对无外部生成模型的渲染烟测工程执行**；不要直接运行 `two-takes.svrun` 或 `seven-takes.svrun` 的 Build，直到配置和授权 Provider、预算。
