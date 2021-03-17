---
title： flask server
tags:
 - flask
 - server
 - keras
---

### HelloWorld

```python
from flask import Flask
from flask import Request

app = Flask(__name__)

@app.route('/hello')
def hello():
    return 'Hello World'

if __name__=='__main__':
    app.run(host='0.0.0.0',port=1234)
```

### 简单函数回显传入的参数

```python
import flask as falsk

app =falsk.Flask(__name__)

@app.route("/predict",methods=["GET","POST"])
def predict():
    data = {"success":False}
    # 获取请求参数
    params=flask.request.json
    if(params==None):
        params=flask.request.json
    if(params==None):
        params=flask.request.args
    if(params!=None):
        data["response"]=params.get("msg")
        data["success"]=True
    return flask.jsonify(data)

app.run(host='0.0.0.0')

```

**需要ec2,我用windows本地运行，本地打开localhost:5000，报Internal Server Error**

