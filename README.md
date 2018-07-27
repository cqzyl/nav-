# 导航栏切换

```
// sass
.page
  display: none
  &:target
    display: list-item
.dib
  display: inline-block
.tac
  text-aligin: center
<nav id="nav" class="nav tac">
  <a class="dib tac" href="#page0">0</a>
  <a class="dib tac" href="#page1">1</a>
  <a class="dib tac" href="#page2">2</a>
  <a class="dib tac" href="#page3">3</a>
</nav>

<ul>
  <li id="page0" class="page">
    page0
  </li>
  <li id="page1" class="page">
    page1
  </li>
  <li id="page2" class="page">
    page2
  </li>
  <li id="page3" class="page">
    page3
  </li>
</ul>

<script>
  // 定义被监听的数据对象
  function dP (obj, key, value, cb, ntcb) {
    var property = Object.getOwnPropertyDescriptor(obj, key);
    if (property && property.configurable === false) {
      return
    }

    // cater for pre-defined getter/setters
    var getter = property && property.get;
    var setter = property && property.set;
    // 判断是否被监听
    if(getter) {
      return ;
    }
    
    !ntcb && cb(value, value);

    Object.defineProperty(obj, key, {
      get : function () {
        return value;
      },
      set : function (newValue) {
        cb(newValue, value);
        value = newValue;
      },
      enumerable : true,
      configurable : true
    });
  }

  var data = {};

  $(function() {
    /* nav.start */
    var nav = document.getElementById('nav').children;
    function getNavIndex () {
      var href = window.location.hash.match(/((\#(\S*)\?)|(\#(\S*)))/)[0];
      return cq_pages.indexOf(href);
    }
    
    // 数据监听
    dP(data, 'nav_active', getNavIndex(), changeTo);

    function changeTo (newval, oldval) {
      oldval !== -1 && nav[oldval].classList.remove('active');
      newval !== -1 && nav[newval].classList.add('active');
    }

    // nav事件绑定
    window.onpopstate = function () {
      data.nav_active = getNavIndex()
    }
    /* nav.end */

  })
</script>
```
