# 游戏自动化机器人

这是一个基于图像识别和内存读取的游戏自动化脚本，支持日常任务自动完成。

## 功能特性

- 图像识别：基于OpenCV的模板匹配技术
- 自动操作：键盘鼠标模拟操作
- 状态管理：有限状态机管理任务流程
- 配置灵活：支持自定义脚本和参数

## 技术栈

- Go语言
- OpenCV图像识别库
- Robotgo输入控制库
- JSON配置文件

## 安装依赖

```bash
go mod init game-automation-bot
go get -u gocv.io/x/gocv
go get github.com/go-vgo/robotgo
```

## 使用方法

```bash
# 编译
go build -o game-automation-bot main.go

# 运行
./game-automation-bot
```

## 项目结构

```
game-automation-bot/
├── main.go          # 主程序入口
├── config/          # 配置文件
├── images/          # 图片模板
├── scripts/         # 自动化脚本
└── utils/           # 工具函数
```

## 配置说明

配置文件定义了脚本的执行流程、识别模板路径、操作坐标等信息。

## 注意事项

- 请确保遵守游戏使用条款
- 适度使用，避免影响游戏平衡
- 根据具体游戏调整识别参数