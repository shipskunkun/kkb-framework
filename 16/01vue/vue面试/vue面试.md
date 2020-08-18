#### 1.v-for  > v-if

在 生成代码的方法中，v-for 是在 v-if 的前面

每次是先遍历，然后再判断，所以为了提高效率，则在外层嵌套template，在这一层进行v-if判断，然后在内部进行v-for循环



问题2，如果数组中对象，存在某个属性，isShow，然后根据这个值去做渲染，如何提高效率？

做计算属性，筛选出需要渲染的，isShow 为true的，不在这个范围的，不管





#### 2.为什么组件的data必须是函数，根实例的data 没必要？

和初始化的代码有关系，init.js  ——> initData()

我的想法：

```
function initData(vm: Comonent) {
	data = vm._data = typeof data === 'function' ? getData(data, vm) : data || {} 
}

```

Vue组件可能存在多个实例，如果使用对象形式定义data，则会导致它们共用一个data对象，那么状态

变更将会影响所有组件实例，这是不合理的；采用函数形式定义，在initData时会将其作为工厂函数返

回全新data对象，有效规避多实例之间状态污染问题。而在Vue根实例创建过程中则不存在该限制，也

是因为根实例只能有一个，不需要担心这种情况。单例。



用自己的话说：组件可能存在多个实例，每次初始化组件，也就是生成一个实例

这个实例的 data 属性 = 传入的 data， 如果这样的话，不同组件的data属性会引用同一个地址，不能很好的进行数据隔离，所以使用方法，返回一个新的data对象，避免多个实例之间的状态污染。



#### 3.key 的作用

\1. key的作用主要是为了高效的更新虚拟DOM，其原理是vue在patch过程中通过key可以精准判断两

个节点是否是同一个，从而避免频繁更新不同元素，使得整个patch过程更加高效，减少DOM操

作量，提高性能。

\2. 另外，若不设置key还可能在列表更新时引发一些隐蔽的bug

\3. vue中在使用相同标签名元素的过渡切换时，也会使用到key属性，其目的也是为了让vue可以区分

它们，否则vue只会替换其内部属性而不会触发过渡效果。



#### 首尾猜测，算法，没搞懂

if 老的开头 = 新的开头

向后移动一位

else if，比较 老的结束 == 新的结束

else if， 老的开始 == 新的结束

else if，老的结束 == 新的开始

else 遍历



答题策略，3w 1h， what where  when how 

同级比较，深度优先

比较两组子节点，4次相同尝试，最后才做通用的方式遍历查找



#### 组件化

全局定义，Vue.component()， extend 方法

单文件组件

还有局部组件，components 中添加一个对象， 通过 createElement 方法



为什么组件必须有个根元素

1. el 定义
2. template包裹，生成 vnode 需要，
3. 比较新旧节点



#### 6种通信方法

props， emit ， 父组件向子组件传值

$emit, $on 事件总线

vuex

$parent $children 

$attr  $listeners

provide inject



父子组件：props

兄弟：

跨层： vuex、事件总线

祖先子孙：以允许一个祖先组件向其所有子孙后代注入一个依赖，不论组件层次有多深，并在起上下游关系成立的时间里始终生效。

provide 和inject绑定并不是可响应的





#### vue 优化

1. 路由懒加载

2. keep-alive 缓存页面

3. v-show 复用 DOM

4. v-for 避免同时使用 v-if

5. 长列表性能优化

   如果列表是纯粹的数据展示，不会有任何改变，就不需要做响应化

   ```
   async created() { const users = await axios.get("/api/users"); this.users = Object.freeze(users); }
   ```

   如果是大数据长列表，可采用虚拟滚动，只渲染少部分区域的内容

   ```
   <recycle-scroller class="items" :items="items" :item-size="24" >
   <template v-slot="{ item }">
   <FetchItemView :item="item" @vote="voteItem(item)" /> </template>
   </recycle-scroller>
   ```

6. 事件的销毁

   ```
   created() { this.timer = setInterval(this.refresh, 2000) },beforeDestroy() { clearInterval(this.timer) }
   ```

   

7. 图片懒加载

   ```
   对于图片过多的页面，为了加速页面加载速度，所以很多时候我们需要将页面内未出现在可视区域
   内的图片先不做加载， 等到滚动到可视区域后再去加载。
   <img v-lazy="/static/img/1.png">
   ```

   

8. 第三方插件按需引入

   像element-ui这样的第三方组件库可以按需引入避免体积太大。

9. 无状态的组件标记为函数式组件
10. 子组件分割
11. 变量本地化
12. ssr



#### vue3

1. 性能优化
2. 响应式系统的改进
3. ts的，可维护化