---
title: 测试框架 Mocha
author: 王书硕
type: posts
date: 2018-12-20T06:38:42+00:00
url: /测试框架-mocha.html
categories:
  - 未分类

---
<blockquote class="wp-block-quote">
  <p>
    <a href="http://www.ruanyifeng.com/blog/2015/12/a-mocha-tutorial-of-examples.html">测试框架 Mocha 实例教程</a>
  </p>
  
  <cite>阮一峰</cite>
</blockquote>

发音为“摩卡”

## 1安装

<pre class="wp-block-code"><code>npm install --global mocha</code></pre>

## 2测试脚本的写法

<pre class="wp-block-code"><code>var add = require('./add.js');
var expect = require('chai').expect;

describe('加法函数的测试', function() {
  it('1 加 1 应该等于 2', function() {
    expect(add(1, 1)).to.be.equal(2);
  });
});</code></pre>

这个脚本测试`add.js`文件，所以命名为`add.test.js`（表示测试）或者`add.spec.js`（表示规格）。

测试脚本里面应该包括一个或多个`describe`块，每个`describe`块应该包括一个或多个`it`块。

`describe`块称为&#8221;测试套件&#8221;（test suite），表示一组相关的测试。它是一个函数，第一个参数是测试套件的名称（&#8221;加法函数的测试&#8221;），第二个参数是一个实际执行的函数。

`it`块称为&#8221;测试用例&#8221;（test case），表示一个单独的测试，是测试的最小单位。它也是一个函数，第一个参数是测试用例的名称（&#8221;1 加 1 应该等于 2&#8243;），第二个参数是一个实际执行的函数。

## 3断言库的用法

上面的测试脚本里面，有一句断言。

<pre class="wp-block-code"><code>expect(add(1, 1)).to.be.equal(2);</code></pre>

所谓&#8221;断言&#8221;，就是判断源码的实际执行结果与预期结果是否一致，如果不一致就抛出一个错误。上面这句断言的意思是，调用`add(1, 1)`，结果应该等于2。

所有的测试用例（it块）都应该含有一句或多句的断言。它是编写测试用例的关键。断言功能由断言库来实现，Mocha本身不带断言库，所以必须先引入断言库。

<pre class="wp-block-code"><code>var expect = require('chai').expect;</code></pre>

断言库有很多种，Mocha并不限制使用哪一种。上面代码引入的断言库是<a href="http://chaijs.com/" target="_blank" rel="noreferrer noopener"><code>chai</code></a>，并且指定使用它的<a href="http://chaijs.com/api/bdd/" target="_blank" rel="noreferrer noopener"><code>expect</code></a>断言风格。

`expect`断言的优点是很接近自然语言，下面是一些例子。

<pre class="wp-block-code"><code>// 相等或不相等
expect(4 + 5).to.be.equal(9);
expect(4 + 5).to.be.not.equal(10);
expect(foo).to.be.deep.equal({ bar: 'baz' });

// 布尔值为true
expect('everthing').to.be.ok;
expect(false).to.not.be.ok;

// typeof
expect('test').to.be.a('string');
expect({ foo: 'bar' }).to.be.an('object');
expect(foo).to.be.an.instanceof(Foo);

// include
expect([1,2,3]).to.include(2);
expect('foobar').to.contain('foo');
expect({ foo: 'bar', hello: 'universe' }).to.include.keys('foo');

// empty
expect([]).to.be.empty;
expect('').to.be.empty;
expect({}).to.be.empty;

// match
expect('foobar').to.match(/^foo/);</code></pre>

基本上，`expect`断言的写法都是一样的。头部是`expect`方法，尾部是断言方法，比如`equal`、`a`/`an`、`ok`、`match`等。两者之间使用`to`或`to.be`连接。

如果`expect`断言不成立，就会抛出一个错误。事实上，只要不抛出错误，测试用例就算通过。

## 4Mocha的基本用法

有了测试脚本以后，就可以用Mocha运行它。进入add.test.js目录，执行下面的命令。

<pre class="wp-block-code"><code>$ mocha add.test.js

  加法函数的测试
    ✓ 1 加 1 应该等于 2

  1 passing (8ms)</code></pre>

上面的运行结果表示，测试脚本通过了测试，一共只有1个测试用例，耗时是8毫秒。

`mocha`执行多个测试脚本：

<pre class="wp-block-code"><code>$ mocha file1 file2 file3 
$ mocha</code></pre>

mocha﻿执行目录下所有脚本：

<pre class="wp-block-code"><code>$ mocha

  加法函数的测试
    ✓ 1 加 1 应该等于 2
    ✓ 任何数加0应该等于自身

  2 passing (9ms)</code></pre>

执行当前目录及子目录的脚本：

<pre class="wp-block-code"><code>$ mocha --recursive

  加法函数的测试
    ✓ 1 加 1 应该等于 2
    ✓ 任何数加0应该等于自身

  乘法函数的测试
    ✓ 1 乘 1 应该等于 1

  3 passing (9ms)</code></pre>

监视自动运行：&#8211;watch，-w

<pre class="wp-block-code"><code>$ mocha --watch</code></pre>

有错误立即停止：&#8211;bail, -b

<pre class="wp-block-code"><code>$ mocha --bail</code></pre>

## 5配置文件mocha.opts

测试目录下的<a rel="noreferrer noopener" href="https://github.com/ruanyf/mocha-demos/blob/master/demo03/test/mocha.opts" target="_blank"><code>mocha.opts</code></a>文件：

<pre class="wp-block-code"><code>--reporter tap
--recursive
--growl</code></pre>

它使得执行mocha与执行mocha &#8211;reporter tap &#8211;recursive &#8211;growl等效。

还可以指定目录：

<pre class="wp-block-code"><code>server-tests
--recursive</code></pre>

上面代码指定运行`server-tests`目录及其子目录之中的测试脚本。

## 6ES6测试

<pre class="wp-block-code"><code>import add from '../src/add.js';</code></pre>

ES6转码，需要安装Babel。

<pre class="wp-block-code"><code>$ npm install babel-core babel-preset-es2015 --save-dev</code></pre>

然后，在项目目录下面，新建一个<a rel="noreferrer noopener" href="https://github.com/ruanyf/mocha-demos/blob/master/demo04/.babelrc" target="_blank"><code>.babelrc</code></a>配置文件。

<pre class="wp-block-code"><code>{
  "presets": [ "es2015" ]
}</code></pre>

最后，使用`--compilers`参数指定测试脚本的转码器。

<pre class="wp-block-code"><code>$ ../node_modules/mocha/bin/mocha --compilers js:babel-core/register</code></pre>

<pre class="wp-block-code"><code>$ mocha --require coffee-script/register --watch --watch-extensions js,coffee "test/**/*.{js,coffee}"</code></pre>

## 7异步测试

不用先不抄了