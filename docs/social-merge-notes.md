# Social Page Merge Notes

这份说明用于把当前社交页能力合并到同伴正在修改的主文件中。假设对方是在现有主文件基础上继续开发，而不是直接覆盖整个 `preview.html`。

## 需要给同伴的文件

1. `preview.html`
2. `social-page.html`

建议：
- `preview.html` 作为真实集成版本
- `social-page.html` 作为单独视觉参考页

## 优先级

如果同伴只收一个文件，给：

`preview.html`

如果同伴既要看主逻辑，又想单独参考社交页布局，给：

1. `preview.html`
2. `social-page.html`

## 需要合并的模块

### 1. 底部导航 `BottomNav`

需要确保底部导航中包含：

1. `探索`
2. `社交`
3. `规划`
4. `我的`

关键点：
- `社交` 使用 `MessageCircle` 图标
- 点击后跳转 `social`

### 2. 社交页组件 `ScreenSocial`

这是本次最核心的页面。

页面结构顺序：

1. 标题区
2. 推荐用户
3. 最近私聊
4. 我加入的群组

当前推荐用户卡保留的信息只有：

1. 用户头像
2. 用户名
3. 在线/最近活跃时间
4. 用户标签
5. 一个契合标签
6. 三个按钮：`跳过`、`去私聊`、`喜欢`

### 3. 路由注册

在主应用路由中增加：

`social`

并确保有：

`{currentPage === 'social' && <ScreenSocial navigate={navigate} />}`

### 4. 历史栈返回逻辑

如果同伴当前项目里的返回还是“固定跳首页”，需要把这部分一起合并。

在 `App` 中保留：

1. `pageHistory`
2. `navigate(page)`
3. `goBack()`

并把以下页面的左上角返回都改为 `goBack`：

1. `ScreenPlaceDetail`
2. `ScreenInterestUsers`
3. `ScreenWriteReview`
4. `ScreenGroup`
5. `ScreenChat`
6. `ScreenProfileGamification`

## 依赖的数据块

同伴如果不是整文件覆盖，需要把这些数据一并带上：

1. `interestUsers`
2. `socialData`

如果主文件里已经有类似数据结构，可直接复用，不必逐字照抄。

## 推荐的最小合并路径

### 方案 A：最快

适合：同伴想直接拿可运行版本。

做法：

1. 直接参考或覆盖 `preview.html`
2. 重点检查自己分支里是否有其他并行修改

### 方案 B：更稳

适合：同伴已经在主文件中做了不少改动，不想整文件覆盖。

按顺序合并：

1. 合并 `BottomNav` 的 `社交` 入口
2. 合并 `ScreenSocial`
3. 合并 `socialData`、`interestUsers` 依赖数据
4. 在 `App` 中注册 `social` 路由
5. 如果需要修复返回，再合并 `pageHistory / navigate / goBack`

## 建议直接告诉同伴的话

可以直接发下面这段：

```text
请优先参考 preview.html 里的这几块：
1. BottomNav：新增了“社交”入口
2. ScreenSocial：这是新的社交页主实现
3. App：里面有 social 路由，以及 pageHistory/goBack 返回逻辑
4. 数据依赖：interestUsers、socialData

如果你只想先合社交页视觉，可以先看 social-page.html；
如果你要把功能真正接进主 demo，请以 preview.html 为准。
```

## 当前社交页的最终确认口径

1. 不保留顶部“发起群组”入口
2. 推荐用户卡不使用大图头部
3. 推荐用户卡只保留头像、标签、契合信息与私聊动作
4. 最近私聊优先级高于群组
5. 我加入的群组放在最下面
6. 推荐用户区域已压缩，保证首屏能看到群组区块
