---
title: requireJs
layout: post
date: 2016-09-14 09:24:23
categories: ['javascript']
tags: ['javascript']
---

一个简化版的 requireJs 实现
```javascript
function irequire(moduleName){
  const fs = require('fs');
  const path = require('path');
  const fileName = path.join(__dirname,moduleName);
  const dirName = path.dirName(fileName);

  let content = fs.readFileSync(fileName,'utf-8');
  let module = { id: fileName,exports: {} };
  let exports = module.exports;

  eval(`
    (function(req,module,exports,__dirname,__filename){
      ${content}
    })(irequire,module,exports,dirName,fileName)
  `);

  return module.exports;
```