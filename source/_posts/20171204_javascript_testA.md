---
title: JavaScript 練習題
date: 2017-12-04 15:40:00
tags:
    - Javascript
categories: Javascript
---

JavaScript 典型的面試題目
<!-- more -->

整理js的觀念文章後，在做網路上的典型題目
看看自己是否真的有了解

問題一：作用域(Scope)
---
console出來的值是多少?
```js
(function() {
    var a = b = 5;
})();

console.log(b);
```
<div><input id='Q_1' value='解答' type='button' /><div id='A_1' style='display:none'>輸出：5
備註：a是局部變數，b是全域變數
如果要用"嚴格模式"('use strict')的話<figure class="highlight js"><table><tbody><tr><td class="gutter"><pre><div class="line">1</div><div class="line">2</div><div class="line">3</div><div class="line">4</div><div class="line">5</div><div class="line">6</div></pre></td><td class="code"><pre><div class="line">(<span class="function"><span class="keyword">function</span>(<span class="params"></span>) </span>{</div><div class="line"><span class="meta">   'use strict'</span>;</div><div class="line">   <span class="keyword">var</span> a = <span class="built_in">window</span>.b = <span class="number">5</span>;</div><div class="line">})();</div><div class="line"> </div><div class="line"><span class="built_in">console</span>.log(b);</div></pre></td></tr></tbody></table></figure></div></div>

問題二：創建原生方法
---
在String對象上創建一個`repeatify`函數，接收的這個參數，代表要重複幾次。
```js
console.log('hello'.repeatify(3));
```
<div><input id='Q_2' value='解答' type='button' /><div id='A_2' style='display:none'><figure class="highlight js"><table><tbody><tr><td class="gutter"><pre><div class="line">1</div><div class="line">2</div><div class="line">3</div><div class="line">4</div><div class="line">5</div><div class="line">6</div><div class="line">7</div><div class="line">8</div><div class="line">9</div></pre></td><td class="code"><pre><div class="line"><span class="built_in">String</span>.prototype.repeatify = <span class="built_in">String</span>.prototype.repeatify || <span class="function"><span class="keyword">function</span>(<span class="params">times</span>) </span>{</div><div class="line">   <span class="keyword">var</span> str = <span class="string">''</span>;</div><div class="line"> </div><div class="line">   <span class="keyword">for</span> (<span class="keyword">var</span> i = <span class="number">0</span>; i &lt; times; i++) {</div><div class="line">      str += <span class="keyword">this</span>;</div><div class="line">   }</div><div class="line"> </div><div class="line">   <span class="keyword">return</span> str;</div><div class="line">};</div></pre></td></tr></tbody></table></figure></div></div>


問題三：變量提升（Hoisting）
---
執行解過答案會是?
```js
function test() {
   console.log(a);
   console.log(foo());
   
   var a = 1;
   function foo() {
      return 2;
   }
}
 
test();
```
<div><input id='Q_3' value='解答' type='button' /><div id='A_3' style='display:none'>執行結果是 undefined 和 2</div></div>


問題四：在JavaScript中，`this`是如何工作的?
---
console出來的結果?
能的話順便說出原因
```js
var fullname = 'John Doe';
var obj = {
   fullname: 'Colin Ihrig',
   prop: {
      fullname: 'Aurelio De Rosa',
      getFullname: function() {
         return this.fullname;
      }
   }
};
console.log(obj.prop.getFullname());
 
var test = obj.prop.getFullname;
console.log(test());
```
<div><input id='Q_4' value='解答' type='button' /><div id='A_4' style='display:none'>執行結果是 Aurelio De Rosa 和 John Doe</div></div>


問題五：call() 和 apply()
---
承上題，讓最後一個`console.log()`輸出`Aurelio De Rosa`
```js
// todo
```
<div><input id='Q_5' value='解答' type='button' /><div id='A_5' style='display:none'><figure class="highlight js"><table><tbody><tr><td class="gutter"><pre><div class="line">1</div></pre></td><td class="code"><pre><div class="line"><span class="built_in">console</span>.log(test.call(obj.prop));</div></pre></td></tr></tbody></table></figure></div></div>


問題六：閉包(Closures)
---
點擊第一個Button及第四個Button，分別console出來的結果是?
```js
var nodes = document.getElementsByTagName('button');
for (var i = 0; i < nodes.length; i++) {
   nodes[i].addEventListener('click', function() {
      console.log('You clicked element #' + i);
   });
}
```
<div><input id='Q_6' value='解答' type='button' /><div id='A_6' style='display:none'>nodes.length假如是10的話，點擊第一個跟第四個都是10</div></div>


問題七：閉包(Closures)
---
承上題，點擊時第一個要輸出0，第二個要輸出1，依此類推。
```js
// todo
```
<div><input id='Q_7' value='解答' type='button' /><div id='A_7' style='display:none'>第一個解決方法：<figure class="highlight js"><table><tbody><tr><td class="gutter"><pre><div class="line">1</div><div class="line">2</div><div class="line">3</div><div class="line">4</div><div class="line">5</div><div class="line">6</div><div class="line">7</div><div class="line">8</div></pre></td><td class="code"><pre><div class="line"><span class="keyword">var</span> nodes = <span class="built_in">document</span>.getElementsByTagName(<span class="string">'button'</span>);</div><div class="line"><span class="keyword">for</span> (<span class="keyword">var</span> i = <span class="number">0</span>; i &lt; nodes.length; i++) {</div><div class="line">   nodes[i].addEventListener(<span class="string">'click'</span>, (<span class="function"><span class="keyword">function</span>(<span class="params">i</span>) </span>{</div><div class="line">      <span class="keyword">return</span> <span class="function"><span class="keyword">function</span>(<span class="params"></span>) </span>{</div><div class="line">         <span class="built_in">console</span>.log(<span class="string">'You clicked element #'</span> + i);</div><div class="line">      }</div><div class="line">   })(i));</div><div class="line">}</div></pre></td></tr></tbody></table></figure>
第二個解決方法：<figure class="highlight js"><table><tbody><tr><td class="gutter"><pre><div class="line">1</div><div class="line">2</div><div class="line">3</div><div class="line">4</div><div class="line">5</div><div class="line">6</div><div class="line">7</div><div class="line">8</div><div class="line">9</div><div class="line">10</div></pre></td><td class="code"><pre><div class="line"><span class="function"><span class="keyword">function</span> <span class="title">handlerWrapper</span>(<span class="params">i</span>) </span>{</div><div class="line">   <span class="keyword">return</span> <span class="function"><span class="keyword">function</span>(<span class="params"></span>) </span>{</div><div class="line">      <span class="built_in">console</span>.log(<span class="string">'You clicked element #'</span> + i);</div><div class="line">   }</div><div class="line">}</div><div class="line"> </div><div class="line"><span class="keyword">var</span> nodes = <span class="built_in">document</span>.getElementsByTagName(<span class="string">'button'</span>);</div><div class="line"><span class="keyword">for</span> (<span class="keyword">var</span> i = <span class="number">0</span>; i &lt; nodes.length; i++) {</div><div class="line">   nodes[i].addEventListener(<span class="string">'click'</span>, handlerWrapper(i));</div><div class="line">}</div></pre></td></tr></tbody></table></figure>
</div></div>


問題八：數據類型
---
```js
console.log(typeof null);
console.log(typeof {});
console.log(typeof []);
console.log(typeof undefined);
```
<div><input id='Q_8' value='解答' type='button' /><div id='A_8' style='display:none'><figure class="highlight js"><table><tbody><tr><td class="gutter"><pre><div class="line">1</div><div class="line">2</div><div class="line">3</div><div class="line">4</div></pre></td><td class="code"><pre><div class="line">object</div><div class="line">object</div><div class="line">object</div><div class="line"><span class="literal">undefined</span></div></pre></td></tr></tbody></table></figure></div></div>


問題九：事件循環
---
```js
function printing() {
   console.log(1);
   setTimeout(function() { console.log(2); }, 1000);
   setTimeout(function() { console.log(3); }, 0);
   console.log(4);
}
printing();
```
<div><input id='Q_9' value='解答' type='button' /><div id='A_9' style='display:none'><figure class="highlight js"><table><tbody><tr><td class="gutter"><pre><div class="line">1</div><div class="line">2</div><div class="line">3</div><div class="line">4</div></pre></td><td class="code"><pre><div class="line"><span class="number">1</span></div><div class="line"><span class="number">4</span></div><div class="line"><span class="number">3</span></div><div class="line"><span class="number">2</span></div></pre></td></tr></tbody></table></figure></div></div>


問題十：算法
---
寫出一個方法 `isPrime()`的函數，當參數是值數時`true`，否則回傳`false`
```js
// todo
```
<div><input id='Q_10' value='解答' type='button' /><div id='A_10' style='display:none'><figure class="highlight js"><table><tbody><tr><td class="gutter"><pre><div class="line">1</div><div class="line">2</div><div class="line">3</div><div class="line">4</div><div class="line">5</div><div class="line">6</div><div class="line">7</div><div class="line">8</div><div class="line">9</div><div class="line">10</div><div class="line">11</div><div class="line">12</div><div class="line">13</div><div class="line">14</div><div class="line">15</div><div class="line">16</div><div class="line">17</div><div class="line">18</div><div class="line">19</div><div class="line">20</div><div class="line">21</div><div class="line">22</div><div class="line">23</div><div class="line">24</div></pre></td><td class="code"><pre><div class="line"><span class="function"><span class="keyword">function</span> <span class="title">isPrime</span>(<span class="params">number</span>) </span>{</div><div class="line">   <span class="comment">// If your browser doesn't support the method Number.isInteger of ECMAScript 6,</span></div><div class="line">   <span class="comment">// you can implement your own pretty easily</span></div><div class="line">   <span class="keyword">if</span> (<span class="keyword">typeof</span> number !== <span class="string">'number'</span> || !<span class="built_in">Number</span>.isInteger(number)) {</div><div class="line">      <span class="comment">// Alternatively you can throw an error.</span></div><div class="line">      <span class="keyword">return</span> <span class="literal">false</span>;</div><div class="line">   }</div><div class="line">   <span class="keyword">if</span> (number &lt; <span class="number">2</span>) {</div><div class="line">      <span class="keyword">return</span> <span class="literal">false</span>;</div><div class="line">   }</div><div class="line"> </div><div class="line">   <span class="keyword">if</span> (number === <span class="number">2</span>) {</div><div class="line">      <span class="keyword">return</span> <span class="literal">true</span>;</div><div class="line">   } <span class="keyword">else</span> <span class="keyword">if</span> (number % <span class="number">2</span> === <span class="number">0</span>) {</div><div class="line">      <span class="keyword">return</span> <span class="literal">false</span>;</div><div class="line">   }</div><div class="line">   <span class="keyword">var</span> squareRoot = <span class="built_in">Math</span>.sqrt(number);</div><div class="line">   <span class="keyword">for</span>(<span class="keyword">var</span> i = <span class="number">3</span>; i &lt;= squareRoot; i += <span class="number">2</span>) {</div><div class="line">      <span class="keyword">if</span> (number % i === <span class="number">0</span>) {</div><div class="line">         <span class="keyword">return</span> <span class="literal">false</span>;</div><div class="line">      }</div><div class="line">   }</div><div class="line">   <span class="keyword">return</span> <span class="literal">true</span>;</div><div class="line">}</div></pre></td></tr></tbody></table></figure></div></div>




<script
  src="https://code.jquery.com/jquery-3.2.1.min.js"
  integrity="sha256-hwg4gsxgFZhOsEEamdOYGBf13FyQuiTwlAQgxVSNgt4="
  crossorigin="anonymous"></script>
<script>
    $().ready(function(){
        $('#Q_1').on('click', function(e) { $('#A_1').toggle() });
        $('#Q_2').on('click', function(e) { $('#A_2').toggle() });
        $('#Q_3').on('click', function(e) { $('#A_3').toggle() });
        $('#Q_4').on('click', function(e) { $('#A_4').toggle() });
        $('#Q_5').on('click', function(e) { $('#A_5').toggle() });
        $('#Q_6').on('click', function(e) { $('#A_6').toggle() });
        $('#Q_7').on('click', function(e) { $('#A_7').toggle() });
        $('#Q_8').on('click', function(e) { $('#A_8').toggle() });
        $('#Q_9').on('click', function(e) { $('#A_9').toggle() });
        $('#Q_10').on('click', function(e) { $('#A_10').toggle() });
    });

</script>