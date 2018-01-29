---
layout: post
title: "基于TypeScript的react应用"
date: 2018-01-01
categories: [TypeScript]
datetime: 2018-01-01
tags: [TypeScript]

---

* content
{:toc}
最近尝试使用ts开发react，中间碰到不少大坑，在此简单记录一下基于react-create-app+ts的开发环境配置.
<!-- more -->

# 使用TypeScript开发react
1. 初始化项目
```js
    create-react-app app --scripts-version=react-scripts-ts
	cd app 
	yarn start
```
2. 引入redux react-router react-redux
```js
    yarn add --dev redux react-router-dom react-redux @types/redux @types/react-router-dom @types/history
```
3. 传入props前需要先定义接口
example：
```
interface PropsCheck {
    // 传入的property
}
class App extends React.Component<PropsCheck>{
    // code
}
```
4. 使用cssModules 
 需要引入```typings-for-css-modules-loader```
```js
    yarn add --dev typings-for-css-modules-loader
```
之后把css-loader修改为typings-for-css-modules-loader,modules修改为true
5. tsconfig & tsLint（自定义）
```js
	// tsconfig:
	"allowSyntheticDefaultImports": true,
	"typeRoots": [
	    "node_modules/@types"
	],
	"experimentalDecorators": t
	// tsLint
	no-any: false
```
6. 路由懒加载
```js
	<Route path="/v4/appeal/fillname" component={FillName} />
	{* 这里对某些组件做异步加载 *}
	<Route
	    path="/v4/appeal/selectuser"
	    getComponent={selectUser()}
	/>
	
	function selectUser() {
	    return (location, cb) => {
	            require(['../accountselect/container/AccountSelect'], function (component) {
	                cb(null, component);
	            });
	        };
	    }	

```
7. path解析函数(for alias)
```js
function pathResolve(dir) {
	return path.join(__dirname, '..', dir);
}
//  同时需在tsconfig中加上
{
    "compilerOptions": {
        "baseUrl": "./",
        "paths": {
            "aliasName": ["path"]
        }
    }
}

```