---
title: 关于开源许可证
publishDate: 2017-09-19
tags:
  - license
  - react
  - open source
  - web
---

了解常用的开源许可证，为自己的代码选择一个合适的开源许可证，或者在商业项目中选择具有合适的开源许可证的库或框架。

---

这是[阮一峰](http://www.ruanyifeng.com/home.html)做的一副中文版分析图：

![开源许可证选择策略图](http://image.beekka.com/blog/201105/free_software_licenses.png)

[Github选择开源许可证的指南](https://choosealicense.com/)

其实让我这次注意到License这个问题的严重性是因为知乎上对React License的讨论。
注意[React](https://github.com/facebook/react)所采用的License：
![React License](https://raw.githubusercontent.com/coogleyao/static/master/reactlicense.jpeg)

划红线的部分是[专利许可](https://github.com/facebook/react/blob/master/PATENTS)

简而言之，**用了React，你就不能与其他使用React的公司发生专利纠纷，否则你的React开源许可证就会被撤销**。如果商业项目用了React，这也许会是你需要操心的地方。

[Facebook对React专利许可的解释](https://code.facebook.com/posts/112130496157735/explaining-react-s-license/)

一些公司的反应：
  * [WordPress决定停用React](https://ma.tt/2017/09/on-react-and-wordpress/)
  * [Apache基金会禁止使用React在内的Facebook License软件] (https://react-etc.net/entry/apache-foundation-bans-use-of-facebook-bsd-patents-licensed-libraries-like-react-js)
