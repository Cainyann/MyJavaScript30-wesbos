## 解决难点

### 如何将键盘按键与页面按钮对应起来？

连接的帮手是 ``keydown`` 事件中的 `keyCode` 属性，`keyCode` 属性的值和 ASCII 编码值相同（对应小写字母）。在[这个网站]( http://keycode.info/ )可以用按键盘来查看对应的键码。

我们能获取到的初始页面中，按钮 `div` 和音频 `audio` 标签中都添加了一个属性 `data-key` 用于存储对应的键码，这样做的目的是，添加键盘事件监听后，触发键盘事件时即可获取事件的 `keyCode` 属性值，以此为线索，操作对应的按钮及音频。

````javascript
const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
const key = document.querySelector(`div[data-key="${e.keyCode}"]`);
````

### 如何保证按键被按住不放时，可以马上响起连续鼓点声？

每次播放音频之前，设置播放时间戳为 0：

````javascript
var audio = document.getElementById("video"); 
audio.currentTime = 0;
audio.play();
````

### 如何使页面按钮恢复原状？

利用一个叫 [`transitionend`](https://developer.mozilla.org/zh-CN/docs/Web/Events/transitionend) 的事件，它在 CSS transition 结束后会被触发。我们就可以利用这个事件，在每次打鼓的效果（尺寸变大、颜色变化）完成之后，去除相应样式。

在这个页面中，发生 `transition` 的样式属性不止一个（`box-shadow`, `transform`, `border-color`），所以需要添加一个判断语句，使每发生一次按键事件时，只去除一次样式。

````javascript
funciton remove(event) {
  if (event.propertyName !== 'border-left-color') return;
  this.classList.remove('playing');
  // event.target.classList.remove('playing');
}
````