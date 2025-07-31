# Backupfor3dgs
3dgs针对安装踩坑记录中的备份以及对应的安装资源
强烈建议全流程不懂的可以参考这个博客，如果遇到其他解决不了的问题可以搭配我的分享食用。[Windows下3D Gaussian Splatting从0开始安装配置环境及训练教程_3d gaussian splatting安装教程-CSDN博客](https://blog.csdn.net/weixin_64588173/article/details/138140240)

本文记录Gaussian Splatting代码部署时候遇到的问题。

1、git clone https://github.com/graphdeco-inria/gaussian-splatting --recursive

针对这一条命令，gs的项目存在子项目，但是在git的时候我这边是不能正常下载 以下两个命令

![image.png](attachment:f85a4052-12e2-4a8a-80f4-12099d935dd6:image.png)

这时候请前往源项目，下载对应的zip解压后放在submodules各自对应的文件夹中，之前存在的内容全部删除后替换。特别是在24年10月份之后的更新，gs的源项目中的diff-gaussian-rasterization是产生变动的，但是在项目git的时候似乎没有变更。如果不加理会，选择较早版本大概率会爆出以下错误
TypeError: GaussianRasterizationSettings.**new**() got an unexpected keyword argument 'antialiasing'

gaussian_render目录下的init文件提升出现错误，点进去一看raster_settings里面的最后一个参数：antialiasing=pipe.antialiasing 显示为未解析（橙色波浪线）（这个参数是有用的，如果注释掉会在后面报错缺少参数）所以这里下载下面的两个（参考原文链接：https://blog.csdn.net/weixin_66220975/article/details/143452937）

diff https://github.com/graphdeco-inria/diff-gaussian-rasterization/tree/9c5c2028f6fbee2be239bc4c9421ff894fe4fbe0

knn [KERBL Bernhard / simple-knn · GitLab](https://gitlab.inria.fr/bkerbl/simple-knn)

我也提供安装包和对应的源码保存在github中。

2、同时这里diff中third_party中存在glm，大概率是没有的，所以也要进行补全，下载替换。

https://github.com/g-truc/glm/tree/5c46b9c07008ae65cb81ab79cd677ecc1934b903

在项目代码没有更新之前，可以直接在我的参考代码中使用，更新后按照上述流程走。
3、过程中报错以下错误：fatal error C1083: 无法打开包括文件: “crtdefs.h”: No such file or directory
请打开x64 Native Tools Command Prompt for VS 2022后激活对应环境之后继续，参考以下博客[fatal error C1083: 无法打开包括文件: “crtdefs.h”: No such file or directory-CSDN博客](https://blog.csdn.net/m0_73958841/article/details/143237706)应该是对应的默认cuda和vs的位数不同。打开对应环境下载即可。

4、过程中有遇到subprocess.CalledProcessError: Command '['ninja', '-v']' returned non-zero exit status 1问题，请C:\Users\XXX\anaconda3\envs\guassian_splatting\Lib\site-packages\torch\utils中的cpp_extension.py中570行将self.use_ninja = kwargs.get('use_ninja', True)这里的True改为False，禁用ninja。到这里应该可以正常下载安装。可以参考这个，应该禁用不只有一种方法[diff-gaussian-rasterization安装踩坑记录-CSDN博客](https://blog.csdn.net/m0_72395250/article/details/148896000)
项目资源汇总：

diff-gaussian-rasterization更新后的版本：

 https://github.com/graphdeco-inria/diff-gaussian-rasterization/tree/9c5c2028f6fbee2be239bc4c9421ff894fe4fbe0
Knn的版本：这个应该不止一个链接的，应该有不少人进行保存了，这个没有了可以在我的git中找
 [KERBL Bernhard / simple-knn · GitLab](https://gitlab.inria.fr/bkerbl/simple-knn)
GLM版本：

https://github.com/g-truc/glm/tree/5c46b9c07008ae65cb81ab79cd677ecc1934b903

参考链接：

[Windows下3D Gaussian Splatting从0开始安装配置环境及训练教程_3d gaussian splatting安装教程-CSDN博客](https://blog.csdn.net/weixin_64588173/article/details/138140240)

[diff-gaussian-rasterization安装踩坑记录-CSDN博客](https://blog.csdn.net/m0_72395250/article/details/148896000)

https://blog.csdn.net/weixin_66220975/article/details/143452937

[fatal error C1083: 无法打开包括文件: “crtdefs.h”: No such file or directory-CSDN博客](https://blog.csdn.net/m0_73958841/article/details/143237706)

[diff-gaussian-rasterization安装踩坑记录-CSDN博客](https://blog.csdn.net/m0_72395250/article/details/148896000)
