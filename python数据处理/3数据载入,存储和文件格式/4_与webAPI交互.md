我们可以使用request库进行http请求

```python
import requests
url = 'https://api.github.com/repos/pandas-dev/pandas/issues'
resp = requests.get(url)
resp
Out[18]: <Response [200]>
```

Response对象的json方法将返回一个包含解析为本地python对象的josn字典

```python
Out[18]: <Response [200]>
data = resp.json()
data[0]['title']
Out[19]: 'DOC: Fix svg images for dark mode'
```

data中每个元素都是一个包含Github问题页面上的所有数据的字典，我们可以将data直接传给DataFrame并提取一个感兴趣的字段

```python
issues = pd.DataFrame(data, columns=['number', 'title', 'labels', 'state'])
issues
Out[21]: 
    number  ... state
0    48527  ...  open
1    48526  ...  open
2    48525  ...  open
3
```

