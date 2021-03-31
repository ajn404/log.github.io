---
title: 毕业设计技术细节
---

/plaform/viewer/src/index.js

```js
import 'regenerator-runtime/runtime';
import App from './App.js';
import React from 'react';
import ReactDOM from 'react-dom';
import OHIFVTKExtension from '@ohif/extension-vtk';
import OHIFDicomHtmlExtension from '@ohif/extension-dicom-html';
import OHIFDicomSegmentationExtension from '@ohif/extension-dicom-segmentation';
import OHIFDicomRtExtension from '@ohif/extension-dicom-rt';
import OHIFDicomMicroscopyExtension from '@ohif/extension-dicom-microscopy';
import OHIFDicomPDFExtension from '@ohif/extension-dicom-pdf';
import { version } from '../package.json';
let config = {};
if (window) {
  config = window.config || {};
  window.version = version;
}
const appProps = {
  config,
  defaultExtensions: [
    OHIFVTKExtension,
    OHIFDicomHtmlExtension,
    OHIFDicomMicroscopyExtension,
    OHIFDicomPDFExtension,
    OHIFDicomSegmentationExtension,
    OHIFDicomRtExtension,
  ],
};
//创建app
const app = React.createElement(App, appProps, null);
//渲染a
ReactDOM.render(app, document.getElementById('root'));

```

/platform/core/src/redux/index.js

```js
import actions from './actions.js';
import reducers from './reducers';
import localStorage from './localStorage.js';
import sessionStorage from './sessionStorage.js';
const redux = {
  reducers,
  actions,
  localStorage,
  sessionStorage,
};
export default redux;
```

- [关于本地存储的 localStorage 和 sessionStorage](https://zhuanlan.zhihu.com/p/51322888)
- object．key（）返回对象的所有键值
- sessionStorage会在会话结束后清空
- [关于redux状态管理](https://www.redux.org.cn/)

/platform/core/src/redux/action.js

```js
import {
  CLEAR_VIEWPORT,
  SET_ACTIVE_SPECIFIC_DATA,
  SET_SERVERS,
  SET_VIEWPORT,
  SET_VIEWPORT_ACTIVE,
  SET_VIEWPORT_LAYOUT,
  SET_VIEWPORT_LAYOUT_AND_DATA,
  SET_USER_PREFERENCES,
} from './constants/ActionTypes.js';

export const setViewportSpecificData = (
  viewportIndex,
  viewportSpecificData
) => ({
  type: SET_VIEWPORT,
  viewportIndex,
  viewportSpecificData,
});

export const setViewportActive = viewportIndex => ({
  type: SET_VIEWPORT_ACTIVE,
  viewportIndex,
});

export const setLayout = ({ numRows, numColumns, viewports }) => ({
  type: SET_VIEWPORT_LAYOUT,
  numRows,
  numColumns,
  viewports,
});

export const setViewportLayoutAndData = (
  { numRows, numColumns, viewports },
  viewportSpecificData
) => ({
  type: SET_VIEWPORT_LAYOUT_AND_DATA,
  numRows,
  numColumns,
  viewports,
  viewportSpecificData,
});

export const clearViewportSpecificData = viewportIndex => ({
  type: CLEAR_VIEWPORT,
  viewportIndex,
});

export const setActiveViewportSpecificData = viewportSpecificData => ({
  type: SET_ACTIVE_SPECIFIC_DATA,
  viewportSpecificData,
});

export const setStudyLoadingProgress = (progressId, progressData) => ({
  type: 'SET_STUDY_LOADING_PROGRESS',
  progressId,
  progressData,
});

export const clearStudyLoadingProgress = progressId => ({
  type: 'CLEAR_STUDY_LOADING_PROGRESS',
  progressId,
});

export const setUserPreferences = state => ({
  type: SET_USER_PREFERENCES,
  state,
});

export const setExtensionData = (extension, data) => ({
  type: 'SET_EXTENSION_DATA',
  extension,
  data,
});

export const setTimepoints = state => ({
  type: 'SET_TIMEPOINTS',
  state,
});

export const setMeasurements = state => ({
  type: 'SET_MEASUREMENTS',
  state,
});

export const setStudyData = (StudyInstanceUID, data) => ({
  type: 'SET_STUDY_DATA',
  StudyInstanceUID,
  data,
});

export const setServers = servers => ({
  type: SET_SERVERS,
  servers,
});

const actions = {
  setViewportActive,
  setViewportSpecificData,
  setViewportLayoutAndData,
  setLayout,
  clearViewportSpecificData,
  setActiveViewportSpecificData,
  setStudyLoadingProgress,
  clearStudyLoadingProgress,
  setUserPreferences,
  setExtensionData,
  setTimepoints,
  setMeasurements,
  setStudyData,
  setServers,
};

export default actions;

```

- action是store数据的唯一来源，本质是javascript的普通对象
- action表示应用内部的各类动作或者操作，不同的动作会改变相应的state状态，就是**带有type属性的对象**
- 