---
title: Redis设置小key过期时间
date: 2022/8/28 22:04
swiper: true # 是否将改文章放入轮播图中
swiperImg: '![](https://zangzang.oss-cn-beijing.aliyuncs.com/img/b081857be95d8ae4c3cf8b1258109f1a.jpg)' # 该文章在轮播图中的图片，可以是本地目录下图片也可以是http://xxx图片
img: '![](https://zangzang.oss-cn-beijing.aliyuncs.com/img/b081857be95d8ae4c3cf8b1258109f1a.jpg)' # 该文章图片，可以是本地目录下图片也可以是http://xxx图片
categories: 技巧 # 分类
tags: ['Redis'] #标签
top: true

---

## 一级缓存命中场景

### 第一种就是执行sql和参数必须相同

![image-20220828134820485](https://zangzang.oss-cn-beijing.aliyuncs.com/img/image-20220828134820485.png)

### 第二种就是相同的statementID

![image-20220828135010630](https://zangzang.oss-cn-beijing.aliyuncs.com/img/image-20220828135010630.png)

我们第一个方法的statementID是com.zang.mybatis.mapper.UsearMapper.queryList

第二个方法是com.zang.mybatis.mapper.UsearMapper.queryListCopy

所以不会命中缓存

### sqlSession必须一样(会话级缓存)

![image-20220828135539728](https://zangzang.oss-cn-beijing.aliyuncs.com/img/image-20220828135539728.png)

### RowBounds返回行范围必须相同
