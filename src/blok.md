# 事件委托
* 对事件处理程序过多问题的解决方案就是事件委托。事件委托利用了事件冒泡，只指定一个事件处理程序，就可以管理某一类型的所有事件。例如，click事件会一直冒泡到document层次。也就是说，我们可以为整个页面指定一个onclick事件处理程序，而不必给每个可单击的元素分别添加事件处理程序
* 举两个例子
1. 假设给100个按钮添加事件
```
//html代码
<div id="div1">
 <button data-id="1">1</button>
 <button data-id="2">2</button>
 <button data-id="3">3</button>
 <button data-id="4">4</button>
 ****************
</div>

// js代码
div1.addEventListener('click', (e)=>{
    const t = e.target
    if(t.tagName.toLowerCase() === 'button'){
        console.log('button'被点击了)
        console.log('button'内容是 + t.textContext) // 获取被点击元素的文本内容
        console.log('button 的data-id是:'+ t.dataset.id) // 获取被点击元素的dataset.id
    }
})
``` 
2. 监听目前不存在的元素的点击事件，例如下面的例子中1秒钟之后button才出现
```
// html代码
<div id="div1">

</div>

// js代码
setTimeout(()=>{
    const button = document.createElement('button')
    button.textContent= 'click 1'
    div1.appendChild(button)
},1000)

div1.addEventListener('click', (e)=>{
    const t = e.target
    if(t.tagName.toLowerCase() === 'button'){
        console.log('button'被点击了)
    }
})
```
* 接下来我们用一个on函数封装一下事件委托,例如：例如on('click', '#div1','li', fn)，当用户点击div1中的li时，调用fn函数
```
function on(eventType, element, selector, fn) {
   if(!(element instanceOf Element)){
      element = document.querySelector(element)
   }
    element.addEventListener(eventType, e => {
      let el = e.target
      while (!el.matches(selector)) {
        if (element === el) {
          el = null
          break
        }
        el = el.parentNode
      }
      el && fn.call(el, e, el)
    })
    return element
}
```