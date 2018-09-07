---
title: wx-mina
date: 2018-04-10 14:22:29
tags: 微信小程序
---

### 微信小程序开发中填坑记录

- 将小程序的API封装成支持Promise的API

  ```
  /**
   * 将小程序的API封装成支持Promise的API
   * @params fn {Function} 小程序原始API，如wx.login
   */
  function wxPromisify(fn, obj) {
      var fun = function (obj = {}) {
          return new Promise((resolve, reject) => {
              obj.success = function (res) {
                  resolve(res)
              }

              obj.fail = function (res) {
                  reject(res)
              }

              fn(obj)
          })
      }
      return fun(obj)
  }
  ```

- 网络请求的封装

  ```
  /**
   * 接口请求基类方法
   * @param method 请求方法 必填
   * @param relativeUrl 相对路径 必填
   * @param param  参数 可选
   * @param header 请求头参数 可选
   * @returns {Promise}
   */
  function request(method, relativeUrl, param, header = { 'Content-Type': 'application/json' }) {
      let response, error;
      return new Promise((resolve, reject) => {
          wx.request({
              url: rootUrlProduce + relativeUrl,
              method: method,
              header: header,
              data: param || {},
              success(res) {
                  response = res.data;
                  resolve(res.data);
              },
              fail(data) {
                  error = data;
                  reject(data);
              },
              complete() {
              }
          });
      });
  }

  /**
   * 发送POST请求
   * @param relativeUrl 相对路径 必填
   * @param param {Object/String/ArrayBuffer} 参数 可选
   * @param header 请求头参数 可选
   * @returns {Promise}
   */
  function post(relativeUrl, param, header) {
      return request("POST", relativeUrl, param, header || { "content-type": "application/x-www-form-urlencoded" });
  }

  /**
   * 发送get 请求
   * @param relativeUrl 相对路径 必填
   * @param param {Object/String/ArrayBuffer} 参数 可选
   * @param header 请求头参数 可选
   * @returns {Promise}
   */
  function get(relativeUrl, param, header) {
     return request("GET", relativeUrl, param, header);
  }
  ```

- 异步的流程控制

  - 小程序的所有网络请求都是异步的，这个很容易出错，在有些逻辑上必须需要又执行先后顺序的，这样如果使用原始的wx.request方法的话或者promise组件的话，会导致嵌套很多层，代码很不美观。这里可以使用co组件

  ```
  // 引入对应的js
  require("../libs/regenerator-runtime");
  const regeneratorRuntime = global.regeneratorRuntime;
  const co = require("../libs/co");
  // 封装基本亲求的文件
  const services = require("../../utils/services");
  // 网络uri
  const API = require("../../utils/api");
  // 使用demo
  co(function *(){
      let demo1Res = yield services.get(API.getUpdatePersonalDataSign, {});
      let demo2Res = yield services.post(API.exmapUrl,{id,param1:param1});
      ...
  })
  ```

- 微信小程序的二维码生成与扫码功能

  - 微信小程序的二维码生成有三种接口，每种接口都有对应的场景和约束，官方api讲的很清楚了。这里我推荐使用接口b，这种二位啊永久有效且目前没有数量限制，限制就是不能在page中传递参数，只能在scene中赋值一个最大32个字符

  - 扫码我想说微信这块有点坑的地方就是测试环境的时候，扫描生成的二维码有两种扫描途径

    - 使用微信的扫一扫功能
    - 使用小程序中的扫一扫功能

  - 以上的两种方法如果没有正式版本在线上的话，都无法获取到正常的值，这就很尴尬了。社区找了很久终于在一个很偏僻的帖子中找到了扫描的值了。具体可以看这个[贴子](https://developers.weixin.qq.com/blogdetail?action=get_post_info&docid=00020e5c1b4418fd1d763f4b856800)

    ​