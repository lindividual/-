#  神策分析埋点文档

## sdk接入-示例代码
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

## 事件埋点代码

### LoginSuccess(登录成功）
在fanc.js文件的登录方法中添加该事件。
```javascript
/**
         * 登录方法
         * @param form
         * @param type
         */
        doAjax: function (form, type, btnEle) {
            dataLoaderShow();

            //表单验证成功  提交
            var actionUrl = form.getAttribute("action");

            $.ajax({
                url: actionUrl,
                data: $(form).serializeArray(),
                type: "POST",
                timeout : 8000,
                dataType: "JSON",
                complete : function(XMLHttpRequest,status){ //请求完成后最终执行参数
                    if(status=='timeout'){//超时,status还有success,error等值的情况
                        if( $('.errortips').length>0){
                            $('.errortips').html("登录失败");
                        }else if($("#protocol-error").length>0){
                            $('#protocol-error').show().html("注册失败");
                        }
                    }
                },
                success: function (callBackData) {
                    dataLoaderHide();
                    $(btnEle).removeClass('btn-disable');
                    if (callBackData.code != "1") {
                        if (type == "login" || type=="alertLogin") {
                            $('.errortips').html(callBackData.message);
                        } else {
                            if (callBackData.code == "-55") {
                                $('#imgcode-error').show().html(callBackData.message);
                            } else if (callBackData.code == "-5") {
                                $('#mobilecode-error').show().html(callBackData.message);
                            } else {
                                $('#protocol-error').show().html(callBackData.message);
                            }
                        }

                    } else {
                        var sourceChannel = document.cookie.get("tdfrom");
                        //登录成功，获取cookie内的tdfrom，调用sa.track，上报登录成功事件。
                        if (btnEle == '#register-btn' || form = form#alertLoginForm){
                          sa.track('LoginSuccess', {
                            SourceChannel: sourceChannel
                          });
                        }else{
                        //注册成功，获取cookie内的tdfrom，调用sa.track，上报注册成功事件。
                          sa.track('RegisterSuccess', {
                            SourceChannel: sourceChannel
                          });
                        }
                        var jumpurl;
                        if(type=="alertLogin"){
                            window.top.location=document.URL;
                        }else{
                            window.top.location = $("#retUrl").val();
                        }
                    }
                }
            });
        },
```

### PERiskAssessment(完成风险评测）
在authentication.js文件中的riskResult方法中添加该事件
```javascript
//结果页面

    riskResult();

    function riskResult() {
        var myCount = $('.mycount').html(),
            testType = $('.testtype'),
            fitProduct = $('.fit-product');
 
        if (myCount <= 20) {
            testType.html('保守型');
            fitProduct.html('低');
        } else if (myCount >= 21 && myCount <= 34) {
            testType.html('稳健型');
            fitProduct.html('低、中低');
        } else if (myCount >= 35 && myCount <= 54) {
            testType.html('平衡型');
            fitProduct.html('低、中低、中等');
        } else if (myCount >= 55 && myCount <= 74) {
            testType.html('成长型');
            fitProduct.html('低、中低、中等、中高等');
        } else if (myCount >= 75 && myCount <= 100) {
            testType.html('进取型');
            fitProduct.html('低、中低、中等、中高及高');
        }
        
        sa.track('PERiskAssessment',{
          RiskScore: myCount,
          RiskType: testType
        })
    }

```
