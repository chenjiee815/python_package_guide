==============
 打包工具对比
==============

我先给出对比结果：
  推荐使用setuptools + pip
  
具体原因看下文。

distutils
---------
* 目前属于Python标准库的一部分。
  
* 只适用于非常简单的应用场景，建议使用setuptools。

setuptools
----------
* setuptools是对distutils的增强, 尤其是引入了包依赖管理。
  
* setuptools可以为Python包创建egg文件。
  
* 包含包目录内的数据文件。
  
* 自动包含包目录内的所有的包，而不用在setup.py中列举。
  
* 自动包含包内和发布有关的所有相关文件，而不用创建一个MANIFEST.in文件。
  
* 自动生成经过包装的脚本或Windows执行文件。
  
* 支持Pyrex，即在可以setup.py中列出.pyx文件，而最终用户无须安装Pyrex。
  
* 支持上传到PyPI。
  
* 可以部署开发模式，使项目在sys.path中。
  
* 用新命令或setup()参数扩展distutils，为多个项目发布/重用扩展。

* 基本满足大型项目的安装和发布。

setuptools的诸多功能下面会有具体讲解。

distribute
----------
* 由于setuptools初期开发进度缓慢, 不支持 Python3, 代码混乱,
  一帮程序员另起炉灶, fork并且重构setuptools代码, 增加功能。
  
* 然后2013年8月，distribute又合并回setuptools 0.7。

easy_install
------------
* setuptools和distribute自带的安装脚本。

* 只支持从 PyPI_ 下载安装Python包。

pip
---
* pip的目标非常明确：取代easy_install。
  
* 支持安装、卸载包。
  
* 支持从任意能够通过VCS或浏览器访问到的地址安装Python包。

pip的功能之强大，可以看下面专门讲解PIP的章节。
  
distutils2
----------
* 它本来应该成为Python3的标准库packaging，
  在其它Python版本里叫做distutils2。
  
* 该项目在2012年已经停止了，官方文档建议使用setuptools + pip。

.. _PyPI: https://pypi.python.org/pypi/
