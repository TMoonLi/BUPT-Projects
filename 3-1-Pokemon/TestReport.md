﻿# 宠物小精灵对战游戏 测试报告

> 2014211306 李俊宁 2014211288
>
> 2016年 12月 22日

## 1. 范围

### 1.1. 项目概述

#### a. 被测软件名称

- 宠物小精灵对战游戏

#### b. 版本

- v0.1

#### c. 被测软件用途

- 适合作为简单的多人互动游戏，从而能为更多的玩家服务；
- 作为 程序设计实践 学习和实践的项目；

#### d. 被测软件的组成及功能

依赖库：

- *[ORM Lite](https://github.com/BOT-Man-JL/ORM-Lite)*
- *[EggAche GL](https://github.com/BOT-Man-JL/EggAche-GL)*
- *[json](https://github.com/nlohmann/json)*

程序模块：

- Socket 模块，负责基本的 Socket 通信；
- Pokemon 模块，负责与宠物小精灵相关的属性、战斗逻辑；
- Server 模块，负责服务器业务逻辑的实现；
- Client 模块，负责客户端协议的实现；
- GUI Client 模块，负责客户端业务逻辑的实现，即最终用户使用的部分；

测试模块：

- Pokemon Test 模块，负责 Pokemon 模块的单元测试；
- Client Test 模块，负责除 GUI 相关以外业务逻辑的单元测试；
- GUI Client 模块，负责综合测试；

### 1.2. 测试概述

#### Test 1: Pokemon Test

- 被测对象：Pokemon 模块
  - 小精灵对战
  - 小精灵升级
- 测试环境：自动化测试

#### Test 2: Client Test

- 被测对象：Socket 模块，Server 模块（部分），Client 模块
  - 用户管理
  - 查看用户/小精灵
  - 房间系统
- 测试环境：自动化测试

#### Test 3: Integrate Test

- 被测对象：所有模块
  - 图形界面 注册、登录用户
  - 图形界面 查询用户/小精灵
  - 图形界面 房间系统
  - 图形界面 游戏 / 游戏结果
- 测试环境：真实环境

## 2. 测试结果

### 2.1. Test 1: Pokemon Test

#### 2.1.1. Test 1.1: 小精灵对战

- 综述：小精灵进行回合制对战；
- 初始化：生成 2 个 `Pikachu` 和一个 `Charmander`；
- 步骤：
  1. `Pikachu1` 先手对 `Charmander`，轮流调用 `Attack` 函数到一方胜利；
  2. `Charmander` 先手对 `Pikachu1`，轮流调用 `Attack` 函数到一方胜利；
  3. `Charmander` 先手对 `Pikachu2`，轮流调用 `Attack` 函数到一方胜利；
  4. `Pikachu2` 先手对 `Charmander`，轮流调用 `Attack` 函数到一方胜利；
  5. `Pikachu1` 先手对 `Pikachu2`，轮流调用 `Attack` 函数到一方胜利；
  6. `Pikachu2` 先手对 `Pikachu1`，轮流调用 `Attack` 函数到一方胜利；
  7. 每轮对战结束后，恢复小精灵生命值；
- 预期结果与评估标准：
  - 由于 `Attack` 函数使用 Device Random 产生随机数，
    所以结果不能用 `assert` 自动判断；
  - 根据每次 `Attack` 的结果判断 `_GetDamagePoint` 正确性；
  - 根据每次 `GetHP` `GetLevel` `GetExp` 的结果判断中间过程是否正确；
- 实测结果：与预期结果一致；
- 用例终止条件：本测试用例的全部测试步骤被执行
  或因某种原因导致测试步骤无法执行（异常终止）；
- 设计人员：李俊宁
- 执行状态：完成
- 执行结果：通过
- 执行人员：李俊宁

#### 2.1.1. Test 1.1: 小精灵升级

- 综述：小精灵通过回合制对战，获取经验并升级；
- 初始化：生成 2 个 `Pikachu` 和一个 `Charmander`；
- 步骤：
  1. `Pikachu1` 先手对 `Charmander`，轮流调用 `Attack` 函数到一方胜利；
  2. `Charmander` 先手对 `Pikachu1`，轮流调用 `Attack` 函数到一方胜利；
  3. `Charmander` 先手对 `Pikachu2`，轮流调用 `Attack` 函数到一方胜利；
  4. `Pikachu2` 先手对 `Charmander`，轮流调用 `Attack` 函数到一方胜利；
  5. `Pikachu1` 先手对 `Pikachu2`，轮流调用 `Attack` 函数到一方胜利；
  6. `Pikachu2` 先手对 `Pikachu1`，轮流调用 `Attack` 函数到一方胜利；
  7. 每轮对战结束后，恢复小精灵生命值；
- 预期结果与评估标准：
  - 由于 `Attack` 函数使用 Device Random 产生随机数，
    所以结果不能用 `assert` 自动判断；
  - 根据每次 `Attack` 的结果判断 获取经验 / 升级 正确性；
  - 根据每次 `GetHP` `GetLevel` `GetExp` 的结果判断中间过程是否正确；
- 实测结果：与预期结果一致；
- 用例终止条件：本测试用例的全部测试步骤被执行
  或因某种原因导致测试步骤无法执行（异常终止）；
- 设计人员：李俊宁
- 执行状态：完成
- 执行结果：通过
- 执行人员：李俊宁

### 2.2. Test 2: Client Test

#### 2.2.1. Test 2.1: 用户管理

- 综述：注册、登录、注销相关功能的测试；
- 初始化：
  - 冷启动服务器，使用无数据的数据库（由服务器初始化）；
  - 同时启动 3 个 `Client`，连接到该服务器；
- 步骤：
  1. `Client1` 使用 "Johnny", "John" 作为用户名、密码注册；
  2. `Client1` 使用 "Johnny", "Johnny" 作为用户名、密码注册；
  3. `Client1` 使用 "Johnny", "Johnny" 作为用户名、密码注册；
  4. `Client1` 使用 "Johnny", "Johnny" 作为用户名、密码登录；
  5. `Client1` 使用 "Johnny", "Johnny" 作为用户名、密码登录；
  6. `Client1` 使用 "Johnny", "Hack" 作为用户名、密码登录；
  7. `Client1` 登出；
  8. `Client1` 登出；
  9. `Client2` 使用 "John", "LeeLee" 作为用户名、密码注册；
  10. `Client2` 使用 "John", "LeeLee" 作为用户名、密码登录；
  11. `Client3` 使用 "BOT", "ManMan" 作为用户名、密码注册；
  12. `Client3` 使用 "BOT", "ManMan" 作为用户名、密码登录；
- 预期结果与评估标准：
  1. 注册失败，服务器返回 "Empty UID or Pwd's Size < 6"；
  2. 注册成功，服务器返回 "Registered successfully"；
  3. 注册失败，服务器返回 "UserID has been Taken"；
  4. 登录成功，服务器返回 `Johnny` 的用户模型；
  5. 登录成功，服务器返回 `Johnny` 的用户模型，之前登录失效；
  6. 登录失败，服务器返回 "Bad Login Attempt"；
  7. 登出成功，服务器返回 "Logout Successfully"；
  8. 登出失败，服务器返回 "You haven't Login"；
  9. 注册成功，服务器返回 "Registered successfully"；
  10. 登录成功，服务器返回 `John` 的用户模型；
  11. 注册成功，服务器返回 "Registered successfully"；
  12. 登录成功，服务器返回 `BOT` 的用户模型；
  13. 实测结果应与预期一致；
- 实测结果：与预期结果一致；
- 用例终止条件：本测试用例的全部测试步骤被执行
  或因某种原因导致测试步骤无法执行（异常终止）；
- 设计人员：李俊宁
- 执行状态：完成
- 执行结果：通过
- 执行人员：李俊宁

#### 2.2.2. Test 2.2: 查看用户/小精灵

- 综述：查看用户/小精灵 相关功能的测试；
- 初始化：
  - 热启动服务器；
  - 使用完成 Test 2.1 后得到的数据库 和 3 个 `Client`；
- 步骤：
  1. `Client1` 查询小精灵；
  2. `Client1` 查询用户；
  3. `Client2` 查询小精灵；
  4. `Client3` 查询用户；
- 预期结果与评估标准：
  1. 查询失败，服务器返回 "You haven't Login"；
  2. 查询失败，服务器返回 "You haven't Login"；
  3. 查询成功，服务器返回 9个 小精灵模型；
  4. 查询成功，服务器返回 3个 用户模型，"Johnny" 离线，"John" "BOT" 在线；
  5. 实测结果应与预期一致；
- 实测结果：与预期结果一致；
- 用例终止条件：本测试用例的全部测试步骤被执行
  或因某种原因导致测试步骤无法执行（异常终止）；
- 设计人员：李俊宁
- 执行状态：完成
- 执行结果：通过
- 执行人员：李俊宁

#### 2.2.3. Test 2.3: 房间系统

- 综述：房间系统 相关功能的测试；
- 初始化：
  - 热启动服务器；
  - 使用完成 Test 2.2 后得到的数据库 和 3 个 `Client`；
- 步骤：
  1. `Client2` 使用小精灵 `1`，进入 "Hello" 房间；
  2. `Client2` 使用小精灵 `5`，进入 "Hello" 房间；
  3. `Client2` 离开房间；
  4. `Client2` 使用小精灵 `5`，进入 "Hello" 房间；
  5. `Client2` 使用小精灵 `5`，进入 "Hello2" 房间；
  6. `Client3` 使用小精灵 `8`，进入 "Hello2" 房间；
  7. `Client3` 查询所有房间；
  8. `Client3` 离开房间；
  9. `Client3` 离开房间；
  10. `Client3` 使用小精灵 `8`，进入 "Hello" 房间；
  11. `Client3` 查询所有房间；
  12. `Client3` 准备完成，并查询房间内玩家状态；
  13. `Client2` 准备完成，并查询房间内玩家状态；
  14. `Client2` 离开房间；
  15. `Client2` 使用小精灵 `5`，进入 "Hello" 房间；
  16. `Client2` 准备未完成，并查询房间内玩家状态；
  17. `Client2` 查询所有房间；
  18. `Client3` 离开房间；
- 预期结果与评估标准：
  1. 进入失败，服务器返回 "It's NOT your Pokemon"；
  2. 进入成功，服务器返回 地图尺寸 300 * 200；
  3. 离开成功，服务器返回 "Left the Room"；
  4. 进入成功，服务器返回 地图尺寸 300 * 200；
  5. 进入失败，服务器返回 "Already in a Room"；
  6. 进入成功，服务器返回 地图尺寸 300 * 200；
  7. 查询成功，服务器返回 2 个房间，"Hello", "Hello2"，均未开始游戏；
  8. 离开成功，服务器返回 "Left the Room"；
  9. 离开失败，服务器返回 "Not in a Room"；
  10. 进入成功，服务器返回 地图尺寸 300 * 200；
  11. 查询成功，服务器返回 1 个房间，"Hello" 未开始游戏；
  12. 准备成功，服务器返回 2 个玩家，"John" 未准备，"BOT" 准备完成；
  13. 准备成功，服务器返回 2 个玩家，"John", "BOT" 均准备完成；
  14. 离开成功，服务器返回 "Left the Room"；
  15. 进入失败，服务器返回 "Room is in Game"；
  16. 查询失败，服务器返回 "Not in a Room"；
  17. 查询成功，服务器返回 1 个房间，"Hello" 已开始游戏；
  18. 离开成功，服务器返回 "Left the Room"；
  19. 实测结果应与预期一致；
- 实测结果：与预期结果一致；
- 用例终止条件：本测试用例的全部测试步骤被执行
  或因某种原因导致测试步骤无法执行（异常终止）；
- 设计人员：李俊宁
- 执行状态：完成
- 执行结果：通过
- 执行人员：李俊宁

### 2.3. Test 3: Integrate Test

#### 2.3.1. Test 3.1: 图形界面 注册、登录用户

- 综述：图形界面 注册、登录用户 相关功能的测试；
- 初始化：
  - 冷启动服务器，使用无数据的数据库（由服务器初始化）；
  - 同时启动 2 个 `Client`，连接到该服务器；
- 步骤：
  1. 
- 预期结果与评估标准：
  1. 
- 实测结果：与预期结果一致；
- 用例终止条件：本测试用例的全部测试步骤被执行
  或因某种原因导致测试步骤无法执行（异常终止）；
- 设计人员：李俊宁
- 执行状态：完成
- 执行结果：通过
- 执行人员：李俊宁

#### 2.3.2. Test 3.2: 图形界面 查询用户/小精灵


#### 2.3.3. Test 3.3: 图形界面 房间系统


#### 2.3.4. Test 3.4: 图形界面 游戏 / 游戏结果
