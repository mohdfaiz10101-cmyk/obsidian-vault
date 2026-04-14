# code-assistant - 核心记忆

**Agent ID:** `agent-caad9ac5-2a89-4d69-ab74-08379cce48f2`
**同步时间:** 2026-03-22 18:09:03

## Core Memory

```json
{
  "agent_type": "letta_v1_agent",
  "git_enabled": false,
  "blocks": [
    {
      "value": "I am a coding assistant with persistent memory. I remember past conversations, code patterns, debugging sessions, and project context across sessions. I help with NixOS configuration, Python, JavaScript, and system administration. I respond in Chinese.\n\n--- SYSTEM STATE (AUTO-SYNCED) ---\n[系统状态快照] 2026-03-14 00:24\n\n## NixOS Git 最近 3 次 Commit\naa2f605 docs: 更新 CLAUDE.md — Session 16 日志 + 知识图谱系统文档\nac5fd75 feat: 知识图谱统一搜索系统 + 辩论引擎 LiteLLM 修复\nc0b6c34 feat: AI 进化辩论系统 + 全盘挂载 + sudo PATH 修复\n\n## /mnt/data 目录结构 (1.8T)\n目录:\n  - Game\n  - 其他\n  - 资源\n  -町原打坐系列完整十大课程\n  1医\n  360Downloads\n  BaiduNetdiskDownload\n  BaiduSyncdisk\n  CloudMusic\n  DeathStrandingContent\n  Games\n  NoMansSkyPRUSENG12RUSENG11(2016VRSupportedUWP)(4.07)Portable\n  OSTotoFolder\n  Program Files (x86)\n  QQMusicCache\n  SdsZjds\n  Sea_of_Stars-Razor1911\n  SteamLibrary\n  System Volume Information\n  Tactics_Ogre_Reborn-FLT\n  UE_5.5\n  UniConverter\n  V Rising 26.10.22\n  WartalesLRUSENG5ENG(2021)(1.19852)Steam-Rip\n  WeChat Files\n  Wechat Files - 历史\n  XSBDownload\n  cache\n  config\n  leidian\n  msdownld.tmp\n  observations\n  sj\n  xdroid\n  三合至道\n  庞老师合辑\n  微信头像解读\n  本地资源库\n  盗贼遗产2\n大文件:\n  20220610061824274.zip (1MB)\n  Acrobat PRO DC 2023.003.20215 x64 m0nkrus.iso (1079MB)\n  VRisingPRUSENG11ENG(2022)(Build98031654DLC)Portable (2627MB)\n  ai-data.img (102400MB)\n  nix-overflow.img (51200MB)\n  ultimate-post-kit-pro-2_9_3.zip (2MB)\n  v2-6e250840b79769025735b3498ce9981d_b.jpg (0MB)\n  venus_images_V13.0.9.0.SKBCNXM_20220729.0000.00_12.0_cn_b67ee0c87f.tgz (5711MB)\n  zh-cn_windows_11_consumer_editions_version_21h2_updated_june_2023_x64_dvd_f8124fec.iso (5753MB)\n\n## 磁盘使用率\nFilesystem      Size  Used Avail Use% Mounted on\n/dev/nvme0n1p9   89G   84G  4.1G  96% /\n/dev/loop0       98G   44G   50G  47% /mnt/ai\n/dev/nvme0n1p2  252M  220M   33M  88% /boot\n/dev/sda4       932G  792G  141G  85% /mnt/data\n\n## Docker 容器\n运行中 (17):\n  openclaw\tUp 14 hours (healthy)\n  docker-nginx-1\tUp 32 hours\n  docker-api-1\tUp 32 hours\n  docker-worker_beat-1\tUp 32 hours\n  docker-worker-1\tUp 32 hours\n  docker-plugin_daemon-1\tUp 32 hours\n  docker-db_postgres-1\tUp 32 hours (healthy)\n  docker-redis-1\tUp 32 hours (healthy)\n  docker-web-1\tUp 32 hours\n  docker-weaviate-1\tUp 32 hours\n  docker-sandbox-1\tUp 32 hours (healthy)\n  docker-ssrf_proxy-1\tUp 32 hours\n  letta-proxy\tUp 32 hours\n  letta\tUp 32 hours\n  letta-db\tUp 32 hours (healthy)\n  chromadb\tUp 32 hours\n  n8n\tUp 32 hours\n\n## 失败的服务\n无失败服务\n\n## 网络状态\nwlp0s20f0u7u3:wifi:connected:PDCN-rtl8821cu\nwlp0s20f0u7u2u1:wifi:connected:PDCN - 客厅\ntailscale0:tun:connected (externally):tailscale0\nbr-37922624c2e4:bridge:connected (externally):br-37922624c2e4\nbr-3c464d8ad1bd:bridge:connected (externally):br-3c464d8ad1bd\nbr-4ae9fba0dab6:bridge:connected (externally):br-4ae9fba0dab6\nbr-56a3d8957842:bridge:connected (externally):br-56a3d8957842\nbr-5b8626b74594:bridge:connected (externally):br-5b8626b74594\nbr-ba2f91d94c8f:bridge:connected (externally):br-ba2f91d94c8f\nbr-ca297932616e:bridge:connected (externally):br-ca297932616e\ndocker0:bridge:connected (externally):docker0\nlo:loopback:connected (externally):lo\nvirbr0:bridge:connected (externally):virbr0\np2p-dev-wlp0s20f0u7u2u1:wifi-p2p:disconnected:\neno1:ethernet:unavailable:\nveth06e63c7:ethernet:unmanaged:\nveth09bb171:ethernet:unmanaged:\nveth19c0c89:ethernet:unmanaged:\nveth1a93e29:ethernet:unmanaged:\nveth1cce28a:ethernet:unmanaged:\nveth1f4dc45:ethernet:unmanaged:\nveth35a3635:ethernet:unmanaged:\nveth548575a:ethernet:unmanaged:\nveth56ff04e:ethernet:unmanaged:\nveth888913f:ethernet:unmanaged:\nveth897e84e:ethernet:unmanaged:\nveth8e087f3:ethernet:unmanaged:\nveth93cb111:ethernet:unmanaged:\nvethc0924e4:ethernet:unmanaged:\nvethc3d29ba:ethernet:unmanaged:\nvethc60813a:ethernet:unmanaged:\nvethc7adf53:ethernet:unmanaged:\nvethd2a2e9d:ethernet:unmanaged:\nvethdd76599:ethernet:unmanaged:\nvethe6f8556:ethernet:unmanaged:\nvethfb78eec:ethernet:unmanaged:\n",
      "limit": 20000,
      "project_id": null,
      "template_name": null,
      "is_template": false,
      "template_id": null,
      "base_template_id": null,
      "deployment_id": null,
      "entity_id": null,
      "preserve_on_migration": false,
      "label": "persona",
      "read_only": false,
      "description": "The persona block: Stores details about your current persona, guiding how you behave and respond. This helps you to maintain consistency and personality in your interactions.",
      "metadata": {},
      "hidden": null,
      "id": "block-cb53bdc5-736d-4ea0-b1aa-f30af4d31dc5",
      "created_by_id": null,
      "last_updated_by_id": null,
      "tags": []
    },
    {
      "value": "Charlie — NixOS 开发者，Intel H510 + RTX 3060 Ti，nixos-unstable flake。\n\n【操作偏好】\n- **网络稳定是最高优先级**：任何操作都不能断网，修复网络时绝不能先断开再重连\n- **永远不能移除/替换用户的浏览器**：可以加装备选浏览器，但绝不能动现有浏览器\n- Auto git commit when changes are reasonable\n- Auto run `ns` after modifying nix config files (configuration.nix, modules/*.nix, flake.nix, hardware-configuration.nix)\n- Keep CLAUDE.md and CONTEXT.md synced\n- Responds in Chinese (mixed traditional/simplified)\n- Input method: fcitx5-rime, default simplified (luna_pinyin.custom.yaml reset=1)\n- **每次输出完结果后，引导提醒待办事项**（用户要求）\n- **操作习惯整合规则**：扫描到的操作习惯统一写入 MEMORY.md，不要拆分成多个 topic 文件\n\n【输出风格】\n- **减少代码输出**：仅展示关键变更 diff，不输出完整文件。用文字说明改了什么\n- **进度可视化**：长任务用 `████████░░ 80%` 方块显示进度\n- **模型状态透明**：新会话开头报告当前模型名称\n- **提醒待办**：每次回复结尾引导提醒待办事项\n- **[硬性规则] 每次任务完成后必须自动更新 MEMORY.md + CLAUDE.md，无需用户提醒**\n\n【思维模式】\n### 问题切入方式\n- **根因优先**：不问\"怎么配置\"，先问\"为什么不能/为什么这样\"——理解根因比得到答案更重要\n- **系统视角**：看到单个问题时会自动联想到整个管道（ACP → Gemini → fan-out → LiteLLM）\n- **前提验证**：行动前先确认前提是否成立（\"你有 Gemini key 吗\"确认后才谈配置）\n\n### 决策风格\n- **整合优于分散**：偏好把东西整合到一起（单文件、统一入口），排斥碎片化管理\n- **实用优于完美**：能接受\"现在不行\"，但要知道根本原因和最快路径\n- **备着的思维**：即使现在用不上，也倾向于先把结构搭好（Gemini key 没有但想先配上）\n\n### 沟通偏好\n- **短问题 + 大意图**：用最少的字表达意图，期待 Claude 补全背景理解（\"acp能添加gemini clid吗\"）\n- **不需要铺垫**：直接给结论和操作路径，不需要\"首先...其次...最后...\"式的讲解\n- **接受限制**：直接说\"做不到\"比绕弯子解释更受欢迎，但要附上替代方案\n\n### 构建习惯\n- **渐进式积累**：喜欢把每次操作的经验沉淀下来（CLAUDE.md、MEMORY.md、thinking.md）\n- **系统自我进化**：不满足于工具能用，要让工具越来越懂自己（Hyper-OS、思维库）\n- **记录踩坑**：犯过的错必须写下来，防止重复\n\n---\n\n【磁盘安全】\n\n根分区 89G，当前约 88-90%，操作前必须检查 df -h /。",
      "limit": 20000,
      "project_id": null,
      "template_name": null,
      "is_template": false,
      "template_id": null,
      "base_template_id": null,
      "deployment_id": null,
      "entity_id": null,
      "preserve_on_migration": false,
      "label": "human",
      "read_only": false,
      "description": "The human block: Stores key details about the person you are conversing with, allowing for more personalized and friend-like conversation.",
      "metadata": {},
      "hidden": null,
      "id": "block-7e44c702-b6bd-4d1f-a4d8-2d5a5436eb3d",
      "created_by_id": null,
      "last_updated_by_id": null,
      "tags": []
    }
  ],
  "file_blocks": [],
  "prompt_template": ""
}
```
