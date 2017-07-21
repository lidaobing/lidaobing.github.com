---
layout: post
title: "世俗化的编程语言"
tags: ruby language
---

我在很多场合都表述过一个观点，下一代的工业用语言肯定是一个世俗化的语言，而且 Ruby 语言的优势也部分来自他的世俗化风格。世俗化的语言，最大的好处在于可以让程序员更快地阅读和理解代码，这等价于减少程序代码行数带来的优势。
## 什么是世俗化？

对我来讲，世俗化的含义为用一种更接近自然语言的语序来表达你的思路，比如我打开电视机翻译成代码就应该是 `I.turn_on tv` 而不是 `turn_on(me, tv)` 或者更冗长的 `user_turn_on(me, tv)`。主谓宾这种结构更符合大部分人的思维方式（至少是中文和英文母语人的思维方式），采用这种结构的语言可以让程序员更快地阅读和理解代码，这种的效果与减少代码行数的效果类似。

口语中常用的还有省略主语，比如说 “give me five” 比说 "you give me five" 更自然，Ruby, JS 等语言对这个都有一定的支持，比如 Ruby 的 `factory_girl` 的范例代码, 用 `name` 比用 `user.name` 不仅节省了击键次数，也让表达更加自然。

```ruby
FactoryGirl.define do
  factory :user do
    name 'John Doe'
    date_of_birth { 21.years.ago }
  end
end
```


## 现在的工业级语言在世俗化方面够好了么？

未必，请看下面一个例子，目标是对一个数组的每个数取平方

```java
// Java
List<Integer> src = Arrays.asList(1,2,3);
List<Integer> dst = new ArrayList<Integer>(3);
  for(Integer i: src) {
  dst.add(i*i);
}
```

Java 的显然过于繁琐

```python
# Python
a = [1,2,3]
[x*x for x in a]
map(lambda(x):x*x, a)
```

python 提供了两种方式, 但就自然来讲，都不够。

```ruby
# Ruby
a = [1,2,3]
a.map{|x| x*x}
```

从我的角度来看，Ruby 的表达是最自然的。

要追求自然的表达，那么每个对象就需要支持很多成员函数，Ruby 不仅为常见类型内建了很多成员函数，还提供了开放类的机制

## 开放类

Ruby 自己为每个类定义了很多成员函数(比如数组的成员函数就有167个)，同时还提供了开放类这种机制来让第三方库添加更多的成员函数，比如 ActiveSupport 就加了 `uniq_by`, `sample`, `split`, `in_groups` 等多个成员函数。

开放类的坏处也很明显，就是可能存在名字冲突，但解决方案也很简单，大家可以看看 underscore.js 和  jQuery 的例子

```js
_([1,2,3]).map(function(x){return x*x;})
$(".alert").show()
```

用一个全局函数来引入你的扩展，就可以避免开放类的名字冲突，而且使用/阅读起来也很方便。
