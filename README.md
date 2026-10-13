# Stem Flow

基于浏览器与摄像头手势的四轨音乐混音原型。用户可用手势调节 Vocal、Bass、Drums 与 Other 的音量，并进入 FX 页面调节 Reverb、Delay 和 Filter。

当前测试版本：[`Stem_Flow_Final.html`](Stem_Flow_Final.html)

## 快速开始

1. 在桌面浏览器中打开 `Stem_Flow_Final.html`。
2. 点击 **Enable Camera**，允许摄像头权限，等待 Camera 与 Hand model 就绪。
3. 选择 **Intro** 学习操作，或选择 **Play song** 直接进入混音。
4. 点击 **Start**，将手保持在镜头中，等待倒计时结束。

建议使用本地服务器以获得更稳定的摄像头权限：

```sh
python3 -m http.server 8000
```

然后访问 <http://localhost:8000/Stem_Flow_Final.html>。手势识别模型从 jsDelivr 加载，因此首次运行需要网络连接。

## 手势规则

| 操作 | 手势 |
| --- | --- |
| 调节音量 | 在目标音轨区域捏合拇指和食指，上下移动；松开后保留数值。 |
| 进入 FX | 在目标音轨区域竖起拇指并保持：拇指明显向上、其余四指卷曲、拇指与食指明确分离。 |
| 调节效果 | 在 Reverb、Delay 或 Filter 区域捏合并上下移动。 |
| 退出 FX | 在右下角 Quit 区域使用相同的 Thumbs Up 手势并保持。 |
| 教学下一步 | 完成当前练习后，在 Next 上先松开，再捏合并保持。 |

### 判定优先级

`Pinch`（拇指＋食指捏合）优先于 `Thumbs Up`。当拇指与食指接近时，系统不会开始 Thumbs Up 的切换判定；只有拇指上扬、其余四指卷曲且拇指／食指距离明显时，才会触发 FX 进入或退出。原有的手势稳定时间、切换冷却、拖动恢复、教学与防误触逻辑保持不变。

## 区域映射

| 位置 | 主界面 | FX 界面 |
| --- | --- | --- |
| 左上 | Vocal / Vocals | Reverb |
| 左下 | Bass | Delay |
| 右上 | Drums | Filter |
| 右下 | Other | Quit |

快捷键：`Space` 暂停／继续；`M` 在正式混音时清除全部效果。

## 测试重点

- 用左右手、不同距离和略偏左／右的角度反复捏合，确认不会误进入 FX。
- 用四指卷曲、拇指与食指分离的手势测试 Thumbs Up，确认可稳定进入／退出 FX。
- 完成 Intro 后测试连续调节音量与三种效果；检查显示、动画与声音是否同步。
- 短暂移出镜头后重新入镜，再测试双手；检查是否出现数值跳变、卡住或意外切换。

完整测试说明：

- 中文：[GUIDEBOOK.html](GUIDEBOOK.html)
- English：[GUIDEBOOK_EN.html](GUIDEBOOK_EN.html)

## 文件说明

| 文件 | 用途 |
| --- | --- |
| `Stem_Flow_Final.html` | 当前测试入口；内嵌最新视觉、音频、UI 与手势判定。 |
| `GUIDEBOOK.html` | 最新中文测试指南。 |
| `GUIDEBOOK_EN.html` | 最新英文测试指南。 |
| `Stem_Flow_Intro.html` | 旧版可运行包，用于比较和旧版回归测试。 |
| `stem-intro-controls.js` | 旧版手势稳定、防误触和教学流程的可编辑源代码。 |
| `build-stem-intro.cjs` | 将旧版手势源重新嵌入 `Stem_Flow_Intro.html` 并检查语法。 |
| `test-*.cjs` | 不依赖摄像头的旧版模拟回归测试。 |

## 旧版构建与回归测试

以下命令只适用于 `Stem_Flow_Intro.html`；当前视觉版的界面与手势逻辑均内嵌在 `Stem_Flow_Final.html` 中。

```sh
node build-stem-intro.cjs
node test-stem-intro.cjs
node test-gesture-safety.cjs
node test-feedback-unit.cjs
```

`test-reverb-render.cjs` 还需要先安装 `@napi-rs/canvas`。
