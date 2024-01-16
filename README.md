# cubefs_blog
the latest application practice of cubefs

# Blog update process

## Overall directory structure
```ts
.
├── blog // Host English Blogs
│   ├── bestPractices // Blog Directory
│   │   ├── bigo.md
│   │   ├── jd.md
│   │   └── ...
│   ├── communityMeetings
│   ├── news
│   ├── ...
│   └── README.md // English blog menu category structure
├── zh-blog // Host Chinese Blogs
│   ├── bestPractices // Blog Directory
│   │   ├── bigo.md
│   │   └── jd.md
│   ├── communityMeetings
│   ├── news
│   ├── ...
│   └── README.md // Chinese blog menu category structure
└── images // Store blog picture resources, etc
    ├── blog // Blog content resource picture
    └── cover // Blog cover image
```

## Blog directory

### blog、zh-blog
Blog directories are divided by columns, including bestPractices(Best Practices), communityMeetings(Community Meetings), news(News), technicalInsights(Technical Insights), useCases(use cases), etc. Blog md type files are stored in different directories, there is no constraint here, according to the directory distinction is just for better management.

Note that the Chinese and English of the same blog should be placed in the same directory **and with the same name**, **it is best to name the blog by English and underline**, such as jd.md, the Chinese and English names are the same, and both are placed under zh-blog/bestPractices and blog/bestPractices respectively. In this way, the translation of the article can be found accurately at compile time to switch.

### README.md file in the blog and zh-blog directories for blog column menu configuration

The configuration is as follows: there are **two types of blogs**, one is **pdf**  type blog, and the other is **md** type blog.
The configuration for pdf type is a little different from md type. The type field defaults to md, and only when you are using pdf type, you need type and pdfUrl fields.
The unique field for md type blog is filePath, which means the path of the blog, relative directory, relative to the current directory, which is used to generate the url link of the blog.
All Other fields are the same.
```ts
pages: [
  {
    text: 'Technical Insights', // category name
    children: [ // blogs in category
      {
        title: 'Secret of CubeFS Technology | Metadata Subsystem Design',
        // filePath is the blog path, relative directory, relative to the current directory, used to generate the url link of the blog
        filePath: 'technicalInsights/Secret_of_CubeFS_Technology_Metadata_Subsystem_Design',
        date: '2023-08-02',
        author: 'Zhixiang Tang',
        coverUrl: '/images/cover/Group802.png',
        desc: 'xxxxxxx',
      },
      {
        title: 'CubeFS Erasure Code Subsystem Introduction', // blog title
        pdfUrl: 'https://ocs-cn-north1.heytapcs.com/cubefs/website-file/CubeFs纠删码子系统介绍.pdf', // blog pdf url
        type: 'pdf', // Indicates that the blog is a pdf
        date: '2021-12-09', // blog date
        author: 'Zhixiang Tang', // blog author
        coverUrl: '/images/cover/20230427100114704.png', //blog cover image and the blog cover is in the /images/cover directory
        desc: '', // description
      },
    ]
  }
]
```
![image](/images/blog/introduce_blog_en.png)


## Resource placement

blog cover image is placed under the /images/cover directory

blog article resources are placed in the /images/blog directory

## 特别注意

1. The previous blog content has the following content at the beginning, and now the blog can not be needed, the compilation stage will judge whether there is no automatic addition, to maintain the original of the blog
```
---
blog: true
---
```

2. Blogs are sorted by date on the official website, not by profile order.