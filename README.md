# dsh-plugin-shared

`dsh-plugin-shared` 是同一工作区里几个 DSH 插件共用的**跨模块契约层**。它没有 Cordis 入口，
本身不是可加载的插件，只被其它模块以相对路径引入（`../../_shared/*.mjs`）。

这样做的理由：归档投递、主体信封这类东西只要存在两份实现，两边迟早会不一致——摘要算法尤其如此。
所以摘要与校验只在这里写一次，两边永远算得出同一个值。

| 文件 | 作用 | 主要导出 |
|---|---|---|
| `contracts.mjs` | ActMem / MemPalace / Laputa / Evolution 共用的跨域信封 | `envelope`、`assertEnvelope`、`isSubjectId`、`normalizeProvenance` |
| `archive-contract.mjs` | ActMem（发送方）与 MemPalace（接收方）之间的归档投递契约；摘要的**唯一**实现 | `archiveDigest`、`canonicalJson` |
| `ipc.mjs` | 组件内部注册与调用；域错误码只对拥有该前缀的组件可读 | `registerComponentInternal`、`callComponent` |
| `paths.mjs` | 项目本地 DSH home 是组件唯一可用的默认根 | `projectHome`、`resolveComponentPath`、`prepareStateDir` |
| `subject.mjs` | 主体解析；拿不到可信身份时**失败关闭**，不凭空造一个 | `resolveSubject`、`subjectFromEnv` |
| `qq-feed.mjs` | 用有界行缓冲异步扫描一对固定的消息流文件 | `scanQqFeedSnapshot` |

使用方：`dsh-local-memory`、`dsh-mempalace`（主体语义另见 `dsh-evolution`、`dsh-laputa`）。

## 来源与许可

本仓库原为 `dsh-superagent-template` 工作区中的一个模块，现拆出来独立建仓维护。

- **来源**：本项目原创，未移植第三方代码。
- **许可**：MIT。
