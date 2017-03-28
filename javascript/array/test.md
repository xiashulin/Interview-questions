## 接口说明    
ETag提供2个接口:     
* 获取自选股（包括自选股和自选股分组）getMyStock    
* 更新自选股（同上）updateMyStock      

## 测试用例    

### 获取自选股    
接口方法名：getMyStock    
测试目的：测试是否能获取到最新版本的自选股信息    
参考信息：自选股接口改造文档，PStockETag.jce    

#### 用例1    
* 说明： <b>未注册用户首次</b>获取自选股，首次获取，传递的版本号应该为-1  
* 入参：   
```javascript  
let _gpsReq = new pstock.GPSReq();
_gpsReq.username = 'aslin';
_gpsReq.inc = true;
_gpsReq.version = '-1';
```    
* 预期输出：返回statusCode为0，表示正常响应。会当作新用户返回，返回该新用户自选股版本号为0，并且有1个编号为0的自选股分组，其附带2个自选股，编号分别为00001和399001，如：    
```javascript  
{
    statusCode: 0,
    version: '0',
    groupItems: [{
        groupid: 0,
        username: 'aslin',
        groupname: '自选股',
        priority: 0,
        status: 1,
        deletetime: 0,
        updatetime: 0,
        createtime: 1490667604
    }],
    items: [{
        scode: '000001',
        platform: 'pc',
        groupid: 0,
        marketid: 1,
        priority: 0,
        status: 1,
        deletetime: 0,
        updatetime: 0,
        createtime: 1490667604
    }, {
        scode: '399001',
        platform: 'pc',
        groupid: 0,
        marketid: 0,
        priority: 1,
        status: 1,
        deletetime: 0,
        updatetime: 0,
        createtime: 1490667604
    }]
}
```    
* 实际输出：符合预期结果     

#### 用例2     
* 说明：不输入版本号参数或输入非正常的版本号     
* 入参：    
```javascript  
// 没有版本号
let _gpsReq = new pstock.GPSReq();
_gpsReq.username = 'aslin';
_gpsReq.inc = true;

// 错误的版本号
let _gpsReq = new pstock.GPSReq();
_gpsReq.username = 'aslin';
_gpsReq.inc = true;
_gpsReq.version = '0.1';
```    
* 预期输出：返回statusCode为-1001，表示参数错误。如：     
```javascript
{
    statusCode: -1001,
    version: '0',
    groupItems: [],
    items: []
}
```    
* 实际输出：符合预期    

#### 用例3     
* 说明：不输入username或username为空字符串   
* 入参：    
```javascript
// 没有username
let _gpsReq = new pstock.GPSReq();
_gpsReq.inc = true;
_gpsReq.version = '1';

// 空的username
let _gpsReq = new pstock.GPSReq();
_gpsReq.username = '';
_gpsReq.inc = true;
_gpsReq.version = '1';
```    
* 预期输出：返回statusCode为-1001，表示参数错误。如：    
```javascript    
{
    statusCode: -1001,
    version: '0',
    groupItems: [],
    items: []
}
```    
* 实际输出：符合预期     

#### 用例4   
* 说明：客户端版本号低于服务端版本号   
* 入参：    
```javascript
let _gpsReq = new pstock.GPSReq();
_gpsReq.username = 'aslin';
_gpsReq.inc = true;
_gpsReq.version = '1';
```  
* 预期输出：全量正常输出服务端的用户自选股信息，并返回最新的版本号。如：     
```javascript   
{
    statusCode: 0,
    version: '222',
    groupItems: [{
        groupid: 0,
        username: 'aslin',
        groupname: '自选股',
        priority: 0,
        status: 1,
        deletetime: 0,
        updatetime: 1490613338703,
        createtime: 1490613338703
    }, {
        groupid: 3,
        username: 'aslin',
        groupname: '自选股2',
        priority: 4,
        status: 1,
        deletetime: 0,
        updatetime: 1490603277504,
        createtime: 1490603277504
    }],
    items: [{
        scode: '299008',
        platform: 'pc',
        groupid: 1,
        marketid: 1,
        priority: 1,
        status: 1,
        deletetime: 0,
        updatetime: 1490613218674,
        createtime: 1490613218674
    }, {
        scode: '000001',
        platform: 'pc',
        groupid: 0,
        marketid: 1,
        priority: 2,
        status: 1,
        deletetime: 1490613338703,
        updatetime: 1490613338703,
        createtime: 0
    }]
}
```      
* 实际输出：符合预期      

#### 用例5      
* 说明：获取不包含自定义分组的自选股，即入参inc为false。    
* 入参：      
```javascript    
let _gpsReq = new pstock.GPSReq();
_gpsReq.username = 'aslin';
_gpsReq.inc = false;
_gpsReq.version = '13';
```    
* 预期输出：全量正常输出用户没有自定义分组的自选股信息，并返回最新的版本号。如：     
```javascript   
{
    statusCode: 0,
    version: '222',
    groupItems: [{
        groupid: 0,
        username: 'aslin',
        groupname: '自选股',
        priority: 0,
        status: 1,
        deletetime: 0,
        updatetime: 1490613338703,
        createtime: 1490613338703
    }, {
        groupid: 3,
        username: 'aslin',
        groupname: '自选股2',
        priority: 4,
        status: 1,
        deletetime: 0,
        updatetime: 1490603277504,
        createtime: 1490603277504
    }],
    items: [{
        scode: '000001',
        platform: 'pc',
        groupid: 0,
        marketid: 1,
        priority: 2,
        status: 1,
        deletetime: 1490613338703,
        updatetime: 1490613338703,
        createtime: 0
    }]
}
// 自定义分组的自选股没有输出出来
```     
* 实际输出：符合预期     
