# GUI Runner

## 项目简介
基于Tkinter的可配置化GUI运行器，用于执行Python脚本和Node.js脚本。支持参数配置、多语言界面、配置文件管理等功能，适用于需要图形化操作的脚本执行场景。

## 功能特性
- **可视化参数配置**：支持文件路径选择、目录选择、复选框、下拉列表等多种参数类型
- **基于JSON文件的配置**：不是通过命令行参数，而是通过JSON文件进行配置，对依赖大量参数的脚本更友好
- **多语言支持**：通过简单的配置实现界面文本自定义（默认英文，可以换成其他语言）
- **配置管理**：支持配置文件加载/保存/另存为功能（JSON格式）
- **脚本执行**：根据脚本后缀自动决定使用python还是Node.js执行，子窗口执行脚本并实时显示日志输出，方便查看执行情况
- **跨平台支持**：基于Python标准库实现，兼容Windows/MacOS/Linux系统，但需要确保Tkinter库已安装

## 安装指南
```bash
pip install gui-runner
```
## 启动方式
1. 安装后命令行启动
    ```bash
    gui-runner --config=config.json --dict=dict.txt
    ```
2. 在脚本中引用相关模块并调用
    ```python
    # 启动示例
    import tkinter as tk
    from gui_runner import ConfigurableGUI

    def run_gui():
        root = tk.Tk()
        app = ConfigurableGUI(root, "dict.txt", "config.json")
        root.mainloop()

    if __name__ == "__main__":
        run_gui()
    ```

## 配置文件路径
* 主配置文件：```config.json```（可通过```--config```参数指定）
* 多语言字典文件：```language```（可通过```--dict```参数指定）

## 功能扩展
1. 添加新功能
    * 在```config.json```的```categories```数组中添加新分类
    * 每个分类配置下可以添加多个功能项（参考示例）
2. 配置参数界面
    * 创建JSON格式的界面配置文件（参考示例）
    * 支持参数类型：boolean, choice, array, file, directory, string, number, integer
    * 支持参数依赖关系
## 配置文件格式
### 主配置文件（config.json）
```json
{
  "main_title": "GUI标题",
  "categories": [
    {
      "name": "分类名称",
      "description": "分类描述",
      "functions": [
        {
          "name": "功能名称",
          "description": "功能描述",
          "script": "脚本路径",
          "config_file": "参数配置文件路径",
          "ui_config_file": "界面配置文件路径"
        }
      ]
    }
  ]
}
```
### 界面配置文件(function_config.json)
```json
{
  "notes": "注意事项说明",
  "groups": [
    {
      "name": "参数分组名称",
      "parameters": [
        {
          "name": "参数名",
          "label": "显示标签",
          "type": "参数类型",
          "tip": "提示信息",
          "default": "默认值",
          "choices": ["选项1", "选项2"],  // 仅choice类型需要
          "extensions": [".txt", ".csv"], // 仅file类型需要
          "item_type": "file", // 仅array类型需要
          "depends_on": "依赖参数名" // 依赖关系，只支持依赖boolean类型的参数
        }
      ]
    }
  ]
}
```
### 多语言文件编写
1. 创建文本文件（如dict.txt）
2. 格式要求
```
# 注释行（可选）
键名: 翻译文本
```
3. 文本文件示例（dict.txt）
```
# 主窗口文本
back_button: 返回
run_script_button: 运行脚本

# 错误信息
error_title: 错误
config_format_error: 配置文件格式错误
```
### 支持的多语言键
完整键列表参考```src/ui_dict.py```中的```_init_default_values```方法

## 目录结构
```
gui-runner/
├── src/                      # 源代码目录
│   ├── config_manager.py     # 配置文件管理
│   ├── gui_main.py           # 主窗口
│   ├── gui_subwindow.py      # 功能子窗口
│   ├── main.py               # 程序入口
│   ├── tooltip.py            # 工具提示组件
│   ├── ui_dict.py            # 多语言字典管理
│   └── utils.py              # 工具函数
├── config.json               # 主配置文件（示例）
├── setup.py                  # 安装脚本
└── README.md                 # 项目说明
```

## 许可证
本项目遵循[MIT许可证](LICENSE)。