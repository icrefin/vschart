# 图表测试套件

这个目录包含vschart-python的独立测试文件，每个文件测试特定的图表功能。

## 测试文件说明

| 文件名 | 测试功能 |
|--------|----------|
| `test_line_series.py` | 线系列添加、配置和移除 |
| `test_trend_lines.py` | 趋势线添加、配置和移除 |
| `test_fibonacci.py` | 斐波那契回撤线配置 |
| `test_rectangles.py` | 矩形添加、样式配置和删除 |
| `test_markers.py` | 标记添加、样式配置和移除 |
| `test_volume_markers.py` | 成交量标记添加、样式配置和移除 |
| `test_vertical_lines.py` | 垂直线添加、样式配置和移除 |
| `test_background_color.py` | 背景色设置和移除 |
| `test_chart_options.py` | 图表选项配置 |
| `test_candlestick_options.py` | 蜡烛图样式选项配置 |
| `test_volume_options.py` | 成交量样式选项配置 |
| `test_line_options.py` | 线条样式选项配置 |
| `test_volume_profile.py` | 成交量分布图创建和配置 |
| `test_legend.py` | 图例创建、配置和移除 |
| `test_all.py` | 运行所有测试 |

## 使用方法

### 运行单个测试

```bash
# 运行特定测试
python tests/test_line_series.py

# 使用自定义参数
python tests/test_trend_lines.py --host localhost --port 8080 --delay 2.0

# 交互模式（按空格键继续）
python tests/test_markers.py --interactive
```

### 运行所有测试

```bash
# 运行所有测试
python tests/test_all.py

# 跳过某些测试
python tests/test_all.py --skip-tests test_fibonacci test_rectangles

# 只运行指定测试
python tests/test_all.py --run-tests test_line_series test_trend_lines

# 交互模式运行所有测试
python tests/test_all.py --interactive
```

## 命令行参数

所有测试文件支持以下命令行参数：

- `--host`: 图表服务器主机地址 (默认: localhost)
- `--port`: 图表服务器端口 (默认: 8080)
- `--days`: 生成数据的交易日数 (默认: 100)
- `--delay`: 操作间隔时间，秒 (默认: 1.0)
- `--interactive`: 交互模式，按空格键继续
- `--log-level`: 日志级别 (DEBUG, INFO, WARNING, ERROR)

## 测试流程

每个测试文件遵循以下流程：

1. **创建图表实例** - 连接到图表服务器
2. **生成测试数据** - 创建随机蜡烛图和成交量数据
3. **运行测试** - 执行特定功能的测试
4. **清理资源** - 断开图表连接

## 测试功能示例

### 线系列测试
- 添加线系列
- 设置线条样式
- 更新数据
- 移除线系列

### 标记测试
- 添加不同形状的标记
- 配置标记颜色和大小
- 设置标记文本
- 批量移除标记

### 图例测试
- 创建图例
- 更新图例内容
- 调整图例位置
- 配置图例样式

## 注意事项

1. **服务器连接**: 确保图表服务器正在运行
2. **端口配置**: 使用正确的端口连接服务器
3. **交互模式**: 在交互模式下需要手动按空格键继续
4. **数据生成**: 测试数据是随机生成的，每次运行可能不同
5. **清理机制**: 每个测试前会自动清除图表元素

## 开发指南

### 添加新测试

1. 创建新的测试文件，格式为 `test_*.py`
2. 实现测试函数，接受标准参数
3. 使用 `test_utils.py` 中的工具函数
4. 在 `test_all.py` 的 `TEST_MODULES` 列表中添加新测试

### 测试结构模板

```python
def test_new_feature(candlestick_series, volume_series, 
                    candlestick_data, volume_data, 
                    delay, interactive_mode):
    """测试新功能"""
    # 实现测试逻辑
    pass

def main():
    """主函数"""
    args = parse_test_arguments("测试描述")
    # 设置环境并运行测试
```

## 故障排除

### 常见问题

1. **连接失败**: 检查图表服务器是否运行
2. **端口冲突**: 使用不同的端口号
3. **测试超时**: 增加 `--delay` 参数值
4. **元素未清除**: 检查 `clear_chart_elements` 函数