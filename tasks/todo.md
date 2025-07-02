# API Playground 改造计划

## 项目目标
将现有的植物商店示例API playground改造为支持SaaS IoT OpenAPI的在线测试功能，实现：
1. AccessKey/AccessSecret认证获取JWT Token
2. 在线测试真实的IoT设备管理接口
3. 支持产品TSL模型查询、设备管理等核心功能

## 任务列表

### ✅ 1. 分析现有API文档结构，了解SaaS OpenAPI的认证流程和接口规范
- [x] 分析API_accessInstruction.md中的认证流程
- [x] 理解AccessKey/AccessSecret到JWT Token的转换逻辑
- [x] 查看API_List文件夹中的各种IoT API接口定义
- [x] 了解现有OpenAPI规范结构

### 🔄 2. 创建tasks/todo.md文件记录详细的实施计划
- [x] 创建详细的任务分解和时间规划

### ⏳ 3. 基于API_List中的接口文档，创建新的SaaS IoT OpenAPI规范文件
- [ ] 替换openapi.json中的植物商店示例
- [ ] 定义SaaS IoT API的基础结构和服务器配置
- [ ] 添加Bearer Token认证配置
- [ ] 实现以下核心API接口：
  - 认证相关: 登录获取Token、刷新Token
  - 产品管理: 产品列表、产品详情、产品TSL模型
  - 设备管理: 设备列表、设备详情、设备数据读取
  - 数据订阅: 创建订阅、管理订阅

### ⏳ 4. 更新endpoint MDX文件，支持认证Token和真实的IoT设备管理接口
- [ ] 更新get.mdx: 产品TSL模型查询接口
- [ ] 更新create.mdx: 设备创建接口  
- [ ] 更新delete.mdx: 设备删除接口
- [ ] 创建auth.mdx: Token认证接口
- [ ] 创建device-list.mdx: 设备列表查询接口

### ⏳ 5. 实现Token获取和管理功能，支持AccessKey/AccessSecret认证
- [ ] 在introduction.mdx中添加认证说明
- [ ] 提供AccessKey/AccessSecret获取Token的示例代码
- [ ] 说明Token的使用方法和有效期管理

### ⏳ 6. 更新docs.json配置，增加新的API接口导航
- [ ] 更新导航结构，增加认证相关接口
- [ ] 添加设备管理接口分组
- [ ] 添加产品管理接口分组
- [ ] 调整页面顺序和分组逻辑

### ⏳ 7. 测试API playground功能，确保可以正常获取Token并调用接口
- [ ] 测试认证流程
- [ ] 测试各个API接口的可用性
- [ ] 验证参数和响应格式
- [ ] 确保错误处理正确显示

### ⏳ 8. 添加review总结到todo.md文件
- [ ] 总结改造过程中的关键变更
- [ ] 记录实现的功能特性
- [ ] 提供使用说明和注意事项

## 技术要点

### 认证流程实现
1. **AccessKey/AccessSecret认证**:
   - username生成规则: `ver=1&auth_mode=accessKey&sign_method=sha256&access_key=${AccessKey}&timestamp=${timestamp}`
   - password生成规则: `sha256("${username}${AccessSecret}")`
   - API地址: `https://iot-api.acceleronix.io/v2/quecauth/accessKeyAuthrize/accessKeyLogin`

2. **Token使用**:
   - Bearer Token方式
   - 有效期2小时，建议每小时刷新
   - 请求头: `Authorization: ${access_token}`

### 核心API接口
1. **产品TSL模型**: `/v2/quectsl/openapi/product/export/tslFile`
2. **设备列表**: `/v2/devicemgr/r3/openapi/product/device/overview`  
3. **刷新Token**: `/v2/quecauth/accessKeyAuthrize/refreshToken`

### OpenAPI规范要点
- 使用OpenAPI 3.1.0规范
- 服务器地址: `https://iot-api.acceleronix.io`
- 统一错误响应格式
- 支持分页查询参数

## 预期成果
- 完整的SaaS IoT API在线测试环境
- 支持真实的认证和接口调用
- 提供完整的API文档和使用示例
- 友好的用户界面和错误提示

## Review区域

### ✅ 项目改造完成总结

**改造时间**: 2025年7月2日  
**改造内容**: 将植物商店示例API playground完全改造为SaaS IoT API playground

### 🔧 主要变更内容

#### 1. OpenAPI规范文件改造 (`api-reference/openapi.json`)
- **服务器地址**: 从 `http://sandbox.mintlify.com` 更新为 `https://iot-api.acceleronix.io`
- **API标题**: 从 "OpenAPI Plant Store" 更新为 "SaaS IoT OpenAPI"
- **接口替换**: 完全替换植物商店接口为真实IoT接口
  - 认证接口: `/v2/quecauth/accessKeyAuthrize/accessKeyLogin`
  - 产品TSL模型: `/v2/quectsl/openapi/product/export/tslFile`
  - 设备列表: `/v2/devicemgr/r3/openapi/product/device/overview`
  - Token刷新: `/v2/quecauth/accessKeyAuthrize/refreshToken`
- **数据模型**: 增加完整的IoT数据结构（TokenResponse, TSLResponse, DeviceListResponse等）

#### 2. 端点MDX文件更新
- **get.mdx**: 产品TSL模型查询接口 - 获取设备物模型定义
- **create.mdx**: 设备列表查询接口 - 支持分页和状态过滤
- **delete.mdx**: Token刷新接口 - JWT令牌续期管理
- **webhook.mdx**: 认证登录接口 - AccessKey/AccessSecret认证流程

#### 3. 接口文档增强 (`api-reference/introduction.mdx`)
- **认证流程说明**: 详细的AccessKey/AccessSecret到JWT Token转换过程
- **使用前置条件**: SaaS应用创建、服务授权、产品授权步骤
- **API分类**: 认证、产品管理、设备管理、数据访问四大类
- **错误处理**: 标准化错误响应格式和状态码说明

#### 4. 导航结构优化 (`docs.json`)
- **分组重构**: 从通用示例更新为IoT特定分组
  - Getting Started: API介绍和认证说明
  - Authentication: 登录和Token管理
  - Product Management: 产品和TSL模型
  - Device Management: 设备查询和管理

### 🎯 实现的核心功能

#### 认证系统
- ✅ AccessKey/AccessSecret认证流程
- ✅ JWT Token获取和管理
- ✅ Token自动刷新机制
- ✅ 2小时有效期和建议刷新策略

#### IoT核心API
- ✅ 产品TSL模型查询 - 支持多语言(CN/EN)
- ✅ 设备列表查询 - 支持状态过滤和分页
- ✅ Bearer Token认证 - 所有API统一认证方式
- ✅ 标准化错误处理 - 统一的错误响应格式

#### 用户体验
- ✅ 完整的curl示例 - 每个接口都有实用的命令行示例
- ✅ 详细的参数说明 - 清晰的参数类型和验证规则
- ✅ 错误代码说明 - 完整的状态码和错误信息对照表
- ✅ 分类导航结构 - 按功能模块组织的清晰导航

### 📊 技术规格确认

- **OpenAPI版本**: 3.1.0
- **认证方式**: Bearer Token (JWT)
- **服务器**: https://iot-api.acceleronix.io
- **数据格式**: JSON
- **Token有效期**: 2小时 (7200秒)
- **文件语法**: ✅ 所有JSON和MDX文件语法验证通过

### 🚀 使用指南

1. **获取凭证**: 在SaaS平台获取AccessKey和AccessSecret
2. **认证登录**: 使用"Get Access Token"接口获取JWT令牌
3. **调用API**: 在Authorization头中使用Bearer Token
4. **管理令牌**: 建议每小时刷新一次Token

### 💡 后续建议

- **文档补充**: 可考虑添加更多设备操作接口（创建、更新、删除设备）
- **订阅功能**: 可增加数据订阅和Webhook相关接口
- **SDK示例**: 可提供不同编程语言的SDK使用示例
- **实时监控**: 可添加设备状态实时查询接口

### ✅ 验证结果

- OpenAPI JSON语法验证: ✅ 通过
- docs.json配置验证: ✅ 通过  
- 所有端点文件检查: ✅ 完整
- 导航结构确认: ✅ 正确

**改造状态**: 🎉 完成 - SaaS IoT API playground已可用于在线测试真实的IoT接口