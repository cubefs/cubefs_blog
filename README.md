# cubefs_blog
the latest application practice of cubefs

# 博客更新流程

## 整体目录结构
```html
.
├── blog //存放英文博客
│   ├── bestPractices // 博客目录
│   │   ├── bigo.md
│   │   ├── jd.md
│   │   └── ...
│   ├── communityMeetings
│   ├── news
│   ├── ...
│   └── README.md 博客英文栏目菜单结构
├── zh-blog // 存放中文博客
│   ├── bestPractices // 博客目录
│   │   ├── bigo.md
│   │   └── jd.md
│   ├── communityMeetings
│   ├── news
│   ├── ...
│   └── README.md 博客中文栏目菜单结构
└── images // 存放博客图片资源等
    ├── blog // 博客内容资源图
    └── cover // 博客封面图
```

## 博客存放目录 blog、zh-blog
博客存放目录按照栏目区分，bestPractices(最佳实践), communityMeetings(社区例会), news(官方动态), technicalInsights(技术揭秘), useCases(用户案例)等等，博客md文件存放于不同的目录下，这里并没有约束，按照目录区分只是为了更好的管理。 

注意同一篇博客中文和英文应该放在**同一目录下且命名相同**，**最好都是通过英文和下划线命名**,例如 jd.md，中英文命名一致, 中英文分别放在zh-blog/bestPractices 和 blog/bestPractices下。这样在编译时就能够准确找到翻译的文章进行切换。

blog和zh-blog目录下的README.md文件，用于博客栏目菜单配置

配置如下，有**两种类型的博客**， 一种是**pdf**类型的博客，一种是**md**博客
pdf类型的配置和md类型的有些许区别，type字段默认是md,只有是pdf类型的时候才需要type和pdfUrl字段
md类型的博客特有字段是filePath，表示博客路径, 相对目录，相对于当前目录，用于生成博客的url链接
其他字段都一致
```ts
pages: [
  {
    text: 'Technical Insights', // 栏目名称
    children: [ // 栏目下的分类，
      {
        title: 'Secret of CubeFS Technology | Metadata Subsystem Design',
        // filePath为博客路径, 相对目录，相对于当前目录，用于生成博客的url链接
        filePath: 'technicalInsights/Secret_of_CubeFS_Technology_Metadata_Subsystem_Design',
        date: '2023-08-02',
        author: 'Zhixiang Tang',
        coverUrl: '/images/cover/Group802.png',
        desc: 'xxxxxxx',
      },
      {
        title: 'CubeFS Erasure Code Subsystem Introduction', // 博客标题
        pdfUrl: 'https://ocs-cn-north1.heytapcs.com/cubefs/website-file/CubeFs纠删码子系统介绍.pdf', // 博客pdf链接
        type: 'pdf', // 表明该博客是pdf类型
        date: '2021-12-09', // 博客日期
        author: 'Zhixiang Tang', // 博客作者
        coverUrl: '/images/cover/20230427100114704.png', // 博客封面图，在/images/cover目录下
        desc: '', 描述
      },
    ]
  }
]
```
![image](/images/blog/introduce_blog.png)


## 资源放置

博客封面图放置在/images/cover目录下
博客文章资源放置在/images/blog目录下

## 特别注意

1. 以往的博客内容开头都有下面内容，现在的博客可以不需要了，编译阶段会判断有没有，没有的自动加上，保持博客的原始性
```
---
blog: true
---
```

2. 博客在官网的排序是按照日期来的，并不是按照配置文件的顺序。