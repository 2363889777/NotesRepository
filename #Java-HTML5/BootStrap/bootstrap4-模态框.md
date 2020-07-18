# bootstrap4->组件->模态框
## 属性
### 结构属性
类| 描述
:--:|:--:
.modal|模态声明
.modal-dialog|窗口声明
.modal-content|内容声明
.modal-header|模态框头部
.modal-body|模态框主体
.modal-footer|模态框底部
### 选项

### 事件
~~~
//通过 jQuery 方式声明
$('#myModal').modal({
 show : true,		//显示
 backdrop : false,
 keyboard : false,
 remote : 'index.html',
}); 
~~~
Event事件类型|描述
:--:|:--:
show.bs.modal|当调用show实例方法时，会立即触发该事件。如果是由点击引起的，被点击的元素是可用的，成为Event对象的relatedTarget属性。
shown.bs.modal|当模态框对用户来说可见时（需要等待CSS过渡完成），会触发该事件。如果是由点击引起的，被点击的元素是可用的，成为Event对象的relatedTarget属性。
hide.bs.modal|当调用hide实例方法时，会立即触发该事件。
hidden.bs.modal|当模态框对用户来说终于完成隐藏时（需要等待CSS过渡完成），会触发该事件。
~~~
//使用案例
$('#myModal').on('hidden.bs.modal', function (e) {
  // do something...
})
~~~