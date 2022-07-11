一.Vue加载keep-alive组件

keep-alive组件作为vue的内置组件,会在Vue进行初始化的时候,通过调用initGlobalAPI,加载到Vue项目当中

通过extend方法,会把builtInComponents所指向的KeepAlive组件,扩展到Vue.options.components,这样就可以在全局上执行了

二.keep-alive和普通组件的区别

首先keep-alive组件是通过render进行渲染的,我们通过模板创建的组件然后再通过Vue的编译,再将模板渲染成render函数

而且keep-alive组件里,不会去渲染具体的实体节点

最后keep-alive组件还是一个抽象组件

抽象组件是什么:

在keep-alive组件设置了abstruct:true,这个属性在初始化的组件的生命周期的时候会用到

在初始化生命周期的时候会更新组件之间的父子关系链,如果设置了abstruct,就不会把当前的组件加到整个父子链中

我们正常的组件也可以加abstruct,然后打印this,看打印结果里$parent和$children,会有明显的变化

加了keep-alive后是不会影响组件间的层级机构

三.keep-alive的运行流程

create(),创建用于保存缓存的对象cache和相应缓存key值的数组

created () {
    this.cache = Object.create(null)
    this.keys = []
},

render()

render () {
    const slot = this.$slots.default
    // 找到当前插槽里第一个VNode
    const vnode: VNode = getFirstComponentChild(slot)
    const componentOptions: ?VNodeComponentOptions = vnode && vnode.componentOptions
    if (componentOptions) {
      // 获取组件的name属性
      const name: ?string = getComponentName(componentOptions)
      const { include, exclude } = this
      // 判断当前是否需要对组件进行缓存,如果不需要直接返回vnode组件本身
      if (
        // not included 不在白名单里
        (include && (!name || !matches(include, name))) ||
        // excluded 在黑名单里
        (exclude && name && matches(exclude, name))
      ) {
        return vnode
      }
      const { cache, keys } = this
      // 获取当前的VNode的key,如果为空的时候,需要生成一个key
      // 这个key也是缓存的缓存的唯一标识,cid+tag 拼接而成
      const key: ?string = vnode.key == null
        // same constructor may get registered as different local components
        // so cid alone is not enough (#3269)
        ? componentOptions.Ctor.cid + (componentOptions.tag ? `::${componentOptions.tag}` : '')
        : vnode.key
      // 如果组件实例已经在缓存里
      if (cache[key]) {
        vnode.componentInstance = cache[key].componentInstance
        // make current key freshest
        // 删除已经存在的组件key
        remove(keys, key)
        // 将当前的组件key,重新保存的数组的最末端
        keys.push(key)
      } else {
        // delay setting the cache until update
        // 直到更新,延迟设置缓存
        this.vnodeToCache = vnode
        this.keyToCache = key
      }
      // 给缓存的vnode打上keepalive标识
      vnode.data.keepAlive = true
    }
    return vnode || (slot && slot[0])
  }
}

删除存在的组件调用的remove函数

export function remove (arr: Array<any>, item: any): Array<any> | void {
  if (arr.length) {
    const index = arr.indexOf(item)
    if (index > -1) {
      return arr.splice(index, 1)
    }
  }
}

mounted()和updated()

mounted () {
    this.cacheVNode()
    this.$watch('include', val => {
      pruneCache(this, name => matches(val, name))
    })
    this.$watch('exclude', val => {
      pruneCache(this, name => !matches(val, name))
    })
},
updated () {
    this.cacheVNode()
}

首先执行 cacheVNode 

cacheVNode() {
    const { cache, keys, vnodeToCache, keyToCache } = this
    // 判断当前是否有需要缓存的VNode
    if (vnodeToCache) {
        const { tag, componentInstance, componentOptions } = vnodeToCache
        // 把缓存的组件保存到cache数组里
        cache[keyToCache] = {
          name: getComponentName(componentOptions),
          tag,
          componentInstance,
        }
        // 保存key到keys
        keys.push(keyToCache)
        // prune oldest entry
        // 2.5版本,新增参数max,通过max,来调整,当前的缓存组件个数,对不常用的缓存做移除
        if (this.max && keys.length > parseInt(this.max)) {
         // 修剪的目标是当前缓存的第一位的VNode
          pruneCacheEntry(cache, keys[0], keys, this._vnode)
        }
        // 置空
        this.vnodeToCache = null
    }
}

pruneCacheEntry作为组件的公共函数,用来对cache和keys数组做管理

function pruneCacheEntry (
  cache: CacheEntryMap,
  key: string,
  keys: Array<string>,
  current?: VNode
) {
  const entry: ?CacheEntry = cache[key]
  // entry存在,并且判断这个组件不是目前正在渲染的组件
  if (entry && (!current || entry.tag !== current.tag)) {
   // 满足条件直接销毁 
    entry.componentInstance.$destroy()
  }
  // 把指定得数据从cache和keys进行移除
  cache[key] = null
  remove(keys, key)
}

在mounted()执行过程中,会为include和exclude添加watch,遍历

matches函数会通过数组/字符串/正则,判断include/exclude是否已经存在

function matches (pattern: string | RegExp | Array<string>, name: string): boolean {
  if (Array.isArray(pattern)) {
    return pattern.indexOf(name) > -1
  } else if (typeof pattern === 'string') {
    return pattern.split(',').indexOf(name) > -1
  } else if (isRegExp(pattern)) {
    return pattern.test(name)
  }
  /* istanbul ignore next */
  return false
}

pruneCache函数用来遍历当前keepAlive实例的cache数组,通过组件的name,调用pruneCacheEntry对缓存进行裁剪

function pruneCache (keepAliveInstance: any, filter: Function) {
  const { cache, keys, _vnode } = keepAliveInstance
  for (const key in cache) {
    const entry: ?CacheEntry = cache[key]
    if (entry) {
      const name: ?string = entry.name
      if (name && !filter(name)) {
        pruneCacheEntry(cache, key, keys, _vnode)
      }
    }
  }
}

四.LRU算法

LRU是Least Recently Used的缩写,即最近最少使用,这实际上是一种页面资源置换的方式

keep-alive组件在新增缓存的时候,keys数组会把新增的key保存在数组的最后,这样活跃的key都保存在数组的后面

keys.push(keyToCache)

在超过max的时候,就会把位置靠前的缓存对象先进行移除

 pruneCacheEntry(cache, keys[0], keys, this._vnode)

这样就保证了,活跃的在后不活跃的在前的数组结构

如果想重复了解LRU

https://km.sankuai.com/page/461775613

LeetCode 146题:  https://leetcode-cn.com/problems/lru-cache/?utm_source=LCUS&utm_medium=ip_redirect&utm_campaign=transfer2china