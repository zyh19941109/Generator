## Generator

### 异步操作

> 1.回调

```
		$.ajax({
		  	url: xxx,
		  	dataType: 'json',
		  	success(data1){
				$.ajax({
					url: xxx,
					dataType: 'json',
					success(data2){
						$.ajax({
							url: xxx,
							dataType: 'json',
							success(data3){
								//完事儿
							},
							error(){
								alert('错了');
							}
						});
					},
					error(){
						alert('错了');
					}
				});
		  	},
		  	error(){
		  		alert('错了');
		  	}
		});
```

> 2.Promise

```
		Promise.all([
		  	$.ajax({url: xxx, dataType: 'json'}),
		  	$.ajax({url: xxx, dataType: 'json'}),
		  	$.ajax({url: xxx, dataType: 'json'})
		]).then(results=>{
		  	//完事儿
		}, err=>{
		  	alert('错了');
		});
```

> 3.generator

```
		runner(function *(){
		  	let data1=yield $.ajax({url: xxx, dataType: 'json'});
		  	let data2=yield $.ajax({url: xxx, dataType: 'json'});
		  	let data3=yield $.ajax({url: xxx, dataType: 'json'});
		
		  	//完事儿
		});
```

> 带逻辑-普通回调

```
		$.ajax({url: 'getUserData', dataType: 'json', success(userData){
		  	if(userData.type=='VIP'){
				$.ajax({url: 'getVIPItems', dataType: 'json', success(items){
					//生成列表、显示...
				}, error(err){
					alert('错了');
				}});
		  	}else{
				$.ajax({url: 'getItems', dataType: 'json', success(items){
					//生成列表、显示...
				}, error(err){
					alert('错了');
				}});
		  	}
		}, error(err){
		  	alert('错了');
		}});
```

> 带逻辑-Promise

```
		Promise.all([
		  	$.ajax({url: 'getUserData', dataType: 'json'})
		]).then(results=>{
		  	let userData=results[0];
			
		  	if(userData.type=='VIP'){
				Promise.all([
					$.ajax({url: 'getVIPItems', dataType: 'json'})
				]).then(results=>{
					let items=results[0];
					
					//生成列表、显示...
				}, err=>{
					alert('错了');
				});
		  	}else{
				Promise.all([
					$.ajax({url: 'getItems', dataType: 'json'})
				]).then(results=>{
					let items=results[0];
					
					//生成列表、显示...
				}, err=>{
					alert('错了');
				});
		  	}
		}, err=>{
		  	alert('失败');
		});
```

> 带逻辑-generator

```
		runner(function *(){
		  	let userData=yield $.ajax({url: 'getUserData', dataType: 'json'});
		
		  	if(userData.type=='VIP'){
		  		let items=yield $.ajax({url: 'getVIPItems', dataType: 'json'});
		  	}else{
		  		let items=yield $.ajax({url: 'getItems', dataType: 'json'});
		  	}
		
		  	//生成、显示...
		});
```

> Promise		=>		一次读一堆

> generator		=>		逻辑性
