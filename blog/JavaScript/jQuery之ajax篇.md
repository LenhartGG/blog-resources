# jquery之ajax篇

## 一.jquery ajax设置header的两种方式

1.设置headers参数：

    headers: {
      // "Authorization":"Basic "+ auth,
      'Content-Type': 'application/json'
    },

2.设置在beforeSend方法中

	beforeSend: function (xhr) {
        xhr.setRequestHeader("tokenId", getTokenId());
    },


> 相应也需要后台配置headers，后台以node为例，配置如下：

	app.all('*', function(req, res, next) {
	  res.header("Access-Control-Allow-Origin", "*");
	  res.header("Access-Control-Allow-Headers", "Content-Type,Content-Length, Authorization, Accept,X-Requested-With, tokenId"); 
	  res.header("Access-Control-Allow-Methods","PUT,POST,GET,DELETE,OPTIONS"); 
	  res.header('Access-Control-Allow-Credentials', 'true'); 
	  res.header("X-Powered-By",' 3.2.1') 
	  next();
	});

## 二.jquery ajax设置data参数

	

    var srcData = {
        username: localStorage.username,
        password: localStorage.password,
        tokenId: localStorage.tokenId
    }
    $.ajax({
        url: '/users/logout',
        type: 'delete',
        data: srcData,
		## 设置传入data的格式为json
        dataType: 'json',


未完待续。。。