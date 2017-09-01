# 
神策分析埋点文档

##
sdk接入-示例代码
```javascript
<script>
(function(para) {
  var p = para.sdk_url, n = para.name, w = window, d = document, s = 'script',x = null,y = null;
  w['sensorsDataAnalytic201505'] = n;
  w[n] = w[n] || function(a) {return function() {(w[n]._q = w[n]._q || []).push([a, arguments]);}};
  var ifs = ['track','quick','register','registerPage','registerOnce','clearAllRegister','trackSignup', 'trackAbtest', 'setProfile','setOnceProfile','appendProfile', 'incrementProfile', 'deleteProfile', 'unsetProfile', 'identify','login','logout','trackLink','clearAllRegister','getAppStatus'];
  for (var i = 0; i < ifs.length; i++) {
    w[n][ifs[i]] = w[n].call(null, ifs[i]);
  }
  if (!w[n]._t) {
    x = d.createElement(s), y = d.getElementsByTagName(s)[0];
    x.async = 1;
    x.src = p;
    y.parentNode.insertBefore(x, y);
    w[n].para = para;
  }
})({
  // SDK 文件的存放地址
  sdk_url: '.../sensorsdata.min.js',
  name: 'sa',
  // 数据上报地址
  web_url: 'https://sensordata.tdw.cn/?project=production',
  // 配置分发地址
  server_url: 'https://sensorslog.tdw.cn/sa?project=production',
  heatmap:{}
});
//若用户已登录，将用户的团贷网userName设置为用户的loginID。PS:本段代码仅供参考。
var user_id = jaaulde.utils.cookies.get("TDWUserName");
  if (user_id != null) {
     sa.login(user_id);
  }
//启动自动跟踪
sa.quick('autoTrack');
</script>
```

##
事件埋点代码

###
LoginSuccess(登录成功）
```javascript
sa.track('LoginSuccess', {
        ProductId: '123456'，
        ProductCatalog: "Laptop Computer",
        ProductName: "MacBook Pro",
        ProductPrice: 123.45
    });
```

