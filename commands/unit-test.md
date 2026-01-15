运行单元测试。

## 参数说明

格式: `/unit-test [路径]`
- 指定文件或目录运行测试
- 省略则运行全部测试

## Instructions

1. 检测项目测试框架:
   - JavaScript/TypeScript: jest, vitest, mocha
   - Python: pytest, unittest
   - 其他: 根据项目配置判断

2. 运行测试:
   - 如果提供了路径，运行指定测试
   - 如果未提供，运行全部测试

3. 输出测试结果:
   ```
   ## 测试结果
   - 通过: X
   - 失败: Y
   - 总计: Z
   - 覆盖率: XX% (如可用)
   ```

4. 如果测试失败:
   - 分析失败原因
   - 提供修复建议

现在运行测试: $ARGUMENTS
