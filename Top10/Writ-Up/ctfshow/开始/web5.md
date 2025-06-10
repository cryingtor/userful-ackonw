# web5
这题是两个函数的绕过MD5和ctype_alpha
查一下ctype_alpha函数
![](vx_images/241735951563319.png =982x)

这里的MD5的比较示弱比较,这里得用加密后0e开头绕过,构造?v1=QNKCDZO&v2=240610708,更多绕过可见大佬文章[130119145?spm=1001.2101.3001.6650.2&utm_medium=distribute.pc_relevant.none-task-blog-2~default~BlogCommendFromBaidu~Rate-2-130119145-blog-138400908.235%5Ev43%5Epc_blog_bottom_relevance_base1&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~BlogCommendFromBaidu~Rate-2-130119145-blog-138400908.235%5Ev43%5Epc_blog_bottom_relevance_base1&utm_relevant_index=5](https://blog.csdn.net/weixin_54438700/article/details/130119145?spm=1001.2101.3001.6650.2&utm_medium=distribute.pc_relevant.none-task-blog-2~default~BlogCommendFromBaidu~Rate-2-130119145-blog-138400908.235%5Ev43%5Epc_blog_bottom_relevance_base1&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~BlogCommendFromBaidu~Rate-2-130119145-blog-138400908.235%5Ev43%5Epc_blog_bottom_relevance_base1&utm_relevant_index=5)