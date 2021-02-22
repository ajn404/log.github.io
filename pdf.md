**上下页button样式**

样式文件使用的style-components

```js
import styled from 'styled-components';

const PdfViewWrapper = styled.div`
  position: ${props => (props.isFullScreen ? 'static' : 'relative')};
  margin: ${props => (props.isFullScreen ? '32px' : 0)};
`;

const PageButtonCommon = styled.div`
  position: absolute;
  top: 50%;
  width: 24px;
  height: 24px;
  transform: translateY(-50%);
  opacity: ${props => (props.showButton ? 1 : 0)};
  pointer-events: ${props => (props.showButton ? 'auto' : 'none')};
  ${PdfViewWrapper}:hover & {
    opacity: ${props => (props.disabled ? 0.1 : 1)};
    pointer-events: auto;
  }
`;

const PrevButtonWrapper = styled(PageButtonCommon)`
  left: ${props => (props.isFullScreen ? '30px' : '8px')};
`;

const NextButtonWrapper = styled(PageButtonCommon)`
  right: ${props => (props.isFullScreen ? '30px' : '8px')};
`;

const PageIndicatorWrapper = styled.div`
  position: absolute;
  bottom: 5px;
  left: 0;
  width: 100%;
  line-height: 18px;
  text-align: center;
  font-size: 10px;
  color: #999;
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.3 ease-out;
  ${PdfViewWrapper}:hover & {
    opacity: 1;
    pointer-events: auto;
  }
`;
export {
  PdfViewWrapper,
  PrevButtonWrapper,
  NextButtonWrapper,
  PageIndicatorWrapper
};
```

其中

```js
const PageButtonCommon = styled.div`
  position: absolute;
  top: 50%;
  width: 24px;
  height: 24px;
  transform: translateY(-50%);
  opacity: ${props => (props.showButton ? 1 : 0)};
  pointer-events: ${props => (props.showButton ? 'auto' : 'none')};
  ${PdfViewWrapper}:hover & {
    opacity: ${props => (props.disabled ? 0.1 : 1)};
    pointer-events: auto;
  }
`;
```

用于定义切换页数的button样式

在实际环境中classw为`fmgRZS`

![image-20210222180134236](pdf.assets/image-20210222180134236.png)

##### window yarn run build(windows平台不显示sourcemap进行构建的命令)

`set \"GENERATE_SOURCEMAP=false\" && react-app-rewired --max-old-space-size=10240 build     `

### 猜测？

**style.js在生产环境通过webpack没有被打包？**

[styled-components](https://github.com/styled-components/styled-components)

[相关的问题](https://stackoom.com/question/3cQH0/%E4%BB%8E%E7%94%9F%E4%BA%A7%E7%89%88%E6%9C%AC%E4%B8%AD%E5%89%A5%E7%A6%BB%E5%87%BA%E6%9D%A5%E7%9A%84React%E6%A0%B7%E5%BC%8F%E7%BB%84%E4%BB%B6)

生产环境的webpack配置文件中没有加入babel-plugin-styled-components的插件

对比test和本地(localhost环境的pdf上下页样式)：

##### test

![image-20210222175048532](pdf.assets/image-20210222175048532.png)

本地

![image-20210222175213405](pdf.assets/image-20210222175213405.png)

## 分别检查发现test环境的按钮样式丢失！

### 解决方案(不是很确定)：

- 在部署micro的根文件package.json文件中添加styled-components依赖，(yarn add styled-components?)
- 添加beblelrec的插件列表：

以上方案均不对，哈哈好吧



请查看此链接

https://styled-components.com/releases#v4.1.0

**创建一个名为`globals.js`的新文件，其中包含`global.SC_DISABLE_SPEEDY = true`并将其作为`index.js`第一件事导入。**

