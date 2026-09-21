# HERO-68343 测试报告

## 功能测试

### 1. Ordering Limitation Settings 配置页面打开验证

#### 1.1. 复现 Bug：大客户 HQ 环境下页面无法打开

**Prerequisite(s):**
1. 登录 Infrasys POS Platform（Backend）
2. 使用 ihg/hyatt/marriott 等大客户 HQ 账号，menu_items 表记录数 > 100000
3. PHP memory_limit 配置为 128M

**Step(s):**
1. 进入 POS System → Ordering Setup → Config by Location
2. 找到 "Ordering Limitation Settings" 配置项
3. 点击打开该配置

**Reproduction Result(s):**
1. 页面无法正常加载，显示 PHP Fatal Error
2. error.log 记录：`Allowed memory size of 134217728 bytes exhausted (tried to allocate 4194312 bytes)`
3. 堆栈跟踪指向 `App::shutdown()` 阶段，触发 fatal error handler

**Fix Result(s):**
1. 页面正常打开，无 PHP Fatal Error
2. 后台 error.log 无新增内存耗尽错误记录
3. 页面加载时间在可接受范围内（< 10秒）

#### 1.2. 小账号环境下页面正常打开

**Prerequisite(s):**
1. 登录 Infrasys POS Platform（Backend）
2. 使用普通小账号，menu_items 表记录数 < 10000

**Step(s):**
1. 进入 POS System → Ordering Setup → Config by Location
2. 找到 "Ordering Limitation Settings" 配置项
3. 点击打开该配置

**Reproduction Result(s):**
1. 页面正常打开，无异常
2. 小账号环境不受此 Bug 影响

**Fix Result(s):**
1. 页面正常打开，无异常
2. 修复不影响小账号的正常功能

### 2. Ordering Limitation Settings 新增配置验证

#### 2.1. 大客户 HQ 下新增配置记录

**Prerequisite(s):**
1. 已在 ihg/hyatt/marriott 等大客户 HQ 环境
2. "Ordering Limitation Settings" 配置页面可正常打开（已修复）

**Step(s):**
1. 点击 "Add New" 新增配置
2. 设置 Apply To = Target location
3. 选择 Outlet = For a specific outlet
4. 选择具体 Target outlet
5. 设置 Support = Yes
6. 点击 "Save" 保存

**Reproduction Result(s):**
1. （Bug 复现阶段：页面无法打开，此步骤无法执行）

**Fix Result(s):**
1. 配置保存成功，页面提示 Save Successfully
2. 新增配置在列表中可见，字段值正确
3. 重新打开配置，数据持久化完整

### 3. 关联配置 by Location 功能验证

#### 3.1. 其他 Config by Location 配置项不受影响

**Prerequisite(s):**
1. 大客户 HQ 环境
2. 修复已部署

**Step(s):**
1. 依次打开 Config by Location 下的其他配置项
2. 检查是否都能正常打开、查看、保存

**Reproduction Result(s):**
1. Ordering Limitation Settings 无法打开（内存耗尽）
2. 其他配置项正常（不受影响）

**Fix Result(s):**
1. 所有 Config by Location 配置项均可正常打开
2. 查看、编辑、保存功能均正常工作
3. 无回归缺陷引入

#### 3.2. 涉及 Menu Item 查询的其他页面验证

**Prerequisite(s):**
1. 大客户 HQ 环境，menu_items > 100000 条
2. 修复已部署

**Step(s):**
1. 打开依赖 Menu Item 列表的 Platform 配置页面
2. 验证页面加载是否正常

**Reproduction Result(s):**
1. 仅 Ordering Limitation Settings 报内存耗尽
2. 其他页面正常（使用了不同的 SQL 查询方式）

**Fix Result(s):**
1. 所有涉及 Menu Item 的 Platform 配置页面加载正常
2. 无新增内存相关错误
3. 修复方案（SQL 优化或分批查询）有效降低内存占用

### 4. 数据库层验证

#### 4.1. SQL 查询性能验证

**Prerequisite(s):**
1. 大客户 HQ 数据库环境
2. menu_items > 100000 条，menu_gb_overrides > 750000 条关联记录

**Step(s):**
1. 执行原 First SQL：SELECT item_id, item_code, item_name_l1, ... FROM menu_items WHERE item_status <> 'd' ORDER BY item_code
2. 检查 Second SQL 是否生成超长 IN 子句
3. 验证修复后 SQL 查询方式（分批 / LIMIT / 优化 JOIN）

**Reproduction Result(s):**
1. First SQL 返回 100000+ item_id
2. Second SQL 的 IN(...) 列表超过 750000 字符
3. PHP 在构造 / 执行 Second SQL 时内存溢出（128M 限制下无法分配额外 4M）

**Fix Result(s):**
1. Second SQL 不再生成超长 IN 子句（改为分批查询或 WHERE item_id IN (SELECT ...) 子查询或其他优化方式）
2. 单条 SQL 执行时内存占用 < 128M
3. 数据库层面 SQL 执行时间可接受（< 5秒）

## 兼容性测试

### 5. Platform 浏览器兼容性

#### 5.1. Chrome 浏览器验证

**Prerequisite(s):**
1. Chrome 最新稳定版
2. 大客户 HQ 测试环境，修复已部署

**Step(s):**
1. 登录 Platform Backend
2. 打开 Ordering Limitation Settings 配置页
3. 执行新增、查看、保存操作

**Reproduction Result(s):**
1. 页面无法打开

**Fix Result(s):**
1. 页面正常加载，功能正常

#### 5.2. Firefox 浏览器验证

**Prerequisite(s):**
1. Firefox 最新稳定版
2. 大客户 HQ 测试环境，修复已部署

**Step(s):**
1. 登录 Platform Backend
2. 打开 Ordering Limitation Settings 配置页
3. 执行新增、查看、保存操作

**Fix Result(s):**
1. 页面正常加载，功能正常

#### 5.3. Microsoft Edge 浏览器验证

**Prerequisite(s):**
1. Microsoft Edge 最新稳定版
2. 大客户 HQ 测试环境，修复已部署

**Step(s):**
1. 登录 Platform Backend
2. 打开 Ordering Limitation Settings 配置页
3. 执行新增、查看、保存操作

**Fix Result(s):**
1. 页面正常加载，功能正常

## 测试环境信息

### 6. 本地测试环境

1. OS：macOS / Windows 11
2. 浏览器：Chrome, Firefox, Edge
3. PHP 版本：运行环境一致
4. 数据库：使用测试数据集（模拟大客户 100000+ menu_items）

### 7. HQ 测试环境

1. 大客户：ihg / hyatt / marriott
2. PHP memory_limit：128M（生产值）
3. Database：menu_items > 100000 条，menu_gb_overrides > 750000 条

### 8. ER 测试环境

1. PHP memory_limit：默认配置
2. 用于回归测试确认无副作用

## Appendix

### 验收标准（来自 Release Notes）

**Bug Fixing: [Jira-68343] Platform setup page (HQ) - Unable to open config by location setup "Ordering Limitation Settings"**

**Descriptions:**
When the item data volume is excessively large, the system is unable to open the "Ordering Limitation Settings" for config by location normally.

**Expected result:**
The Ordering Limitation Settings configuration page opens normally. It is fixed in current version.

**Setup:**
Add a setup under Config by Location
Go to Infrasys POS platform: POS System → Ordering Setup → Config by Location
Select setup "Ordering Limitation Settings"
Click "Add New" to add new record
Apply To = Target location
Outlet = For a specific outlet
Outlet = Target outlet (available only if "Outlet" is chosen in the row "Apply To")
Support = Yes
Click "Save" to save record
