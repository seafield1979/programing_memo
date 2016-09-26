#POST GET
htmlのフォームから送信されたGET/POSTメッセージを取得する  

**送信側 html**

```html
<form action="test/php" method="post">
	<input type="submit" placeholder="どう思う？">
	<input type="hidden" name = "hidden1" value="１２３４">
	<div class="btn-large-area">
		<a id="button03" class="btn-def">button3</a>
	</div>
</form>	
```

**受信側 php**

```php
$_GET['param1']
$_POST['hidden1']		// "１２３４"
```