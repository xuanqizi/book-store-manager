# 某书店书刊出租和零售管理系统
已部署到服务端，链接为http://49.232.26.148:7007/ 
因服务器重装，上述链接已无法访问
## 数据库
+ **book: 记录书本或报刊的基本信息**
  + ISBN: 书本的ISBN号码
  + title: 书本的标题
  + author: 书本的作者
  + type: 书本的类型（0为报刊, 1为书籍）
  + number: 书本库存
  + price: 书本售价
+ **customer: 记录顾客信息**
  + cid: 顾客id(纯数字)
  + name: 顾客姓名
  + address: 顾客地址(可不填)
+ **operator: 记录操作员信息**
  + username: 操作员用户名(登录用)
  + password: 操作员账号的密码
  + name: 操作员姓名
+ **order: 记录出售订单信息**
  + OrderID: 订单号
  + OperatorID: 操作员ID(对应operator表的username列)
  + amount: 订单金额
  + cid: 顾客ID
  + time: 订单时间
+ **rent: 记录出借订单信息** (每本书分开记录)
  + OrderID: 订单号
  + OperatorID: 操作员ID
  + ISBN: 书本的ISBN号
  + rent_time: 借出时间
  + return_date: 返还时间（若为NULL即未还）
  + due_date: 返还期限
  + cid: 顾客ID
+ **sell: 记录每本书的出售情况** (每本书分开记录)
  + key: 自增列，无特殊意义
  + ISBN: 书本ISBN号
  + cid: 顾客ID
  + OrderID: 订单号(书本是在哪个订单被卖出的)
  + OperatorID: 操作员ID
  + time: 出售时间

## 服务端API
1. **purchase**
  + 实现进货功能(即库存补充以及新增商品)
  + 方法: POST (FormData)
  + 数据: ISBN, title, author, number, price, type
2. **depot**
  + 实现查看库存功能
  + 方法: GET
3. **querysell**
  + 查询零售订单记录
  + 方法: GET
  + 返回值: [{OrderID, CustomerID, CustomerName, time, operator, amount}]
4. **recordsell**
  + 记录零售数据
  + 方法: POST (FormData)
  + 数据: CustomerID, OperatorID, isbn[], amount
  + 可能返回值: "Finish"
5. **newCustomer**
  + 新用户注册
  + 方法: POST (FormData)
  + 数据: CustomerID, name, address
  + 可能返回值: "success" "ID existed"
6. **signup**
  + 操作员注册账号
  + 方法: POST (FormData)
  + 数据: username, password, name
  + 可能返回值: "success" "Username existed"
7. **login**
  + 操作员账号登录
  + 方法: POST (FormData)
  + 数据: username, password
  + 可能返回值: {username, name} "Username Not Exist" "Wrong Password."
8. **recordrent**
  + 记录书籍出借
  + 方法: POST (FormData)
  + 数据: OperatorID, CustomerID, ISBN
  + 可能返回值: "success" "Customer Not Found."
9. **recordret**
  + 记录书籍退还
  + 方法: POST (FormData)
  + 数据: OperatorID, OrderID
  + 可能返回值: "success" "OrderID Not Found."
10. **queryrent**
  + 查询书籍出借信息
  + 方法: GET
  + 可能返回值: {'rentList': rentList} 包含订单号, 顾客id, <br>
  操作员, 出借时间, 应还时间, 归还时间(未还为None)
11. **querybook**
  + 根据时间查询书籍销售和出租情况
  + 方法：POST(FormData)
  + 数据：ISBN, begin, end


