=========
 pip指南
=========

`pip官方文档`_

以下内容是根据pip的1.5.X版本的官方文档写的笔记。

官方文档有用户指南专门一章节，
我翻译时将之分割到多个章节中去了。

安装pip
-------
可以下载 `get-pip.py`_ ，
运行命令：

.. code:: bash

   $ python get-pip.py

如果setuptools没有安装，该命令同时也会安装它。

如果已经安装了setuptools库，那么可以通过easy_install安装：

.. code:: bash

   $ easy_install pip

升级pip
-------
在Linux系统上：

.. code:: bash

   $ pip insall -U pip

在Windows系统上：

.. code:: bash

   $ python -m pipi install -U pip

常用命令介绍
------------
* 安装包

  .. code:: bash
  
     $ pip install SomePackage

* 升级包

  .. code:: bash
  
     $ pip install --upgrade SomePackage
  
     $ pip install -U SomePackage
  
* 删除包

  .. code:: bash
  
     $ pip uninstall SomePackage
  
* 查看某个包安装了哪些文件

  .. code:: bash
  
     $ pip show --files SomePackage
  
* 列出哪些包过期了，或者说是有新的版本了

  .. code:: bash
  
     $ pip list --outdated
  
配置
----
配置文件
~~~~~~~~
配置文件路径：

    * Unix: ~/.pip/pip.conf
  
    * Windows: %HOMEPATH%\pip\pip.ini

当然，如果你想自定义配置文件的存放路径，
你可以将你自定义的路径保存到环境变量 `PIP_ONFIG_FILE` 。

配置文件的格式类似于INI文件。

当你想添加一个对所有命令都生效的参数时，
可以将该配置放置在 `[global]` 段中。例如：

.. code:: ini
   
   [global]
   timeout = 60
   index-url = http://pypi.douban.com/simple

如果你想针对pip的某个子命令进行配置的话，
可以将该子命令名称当作段名称，该段内的配置与
`[global]` 中的配置冲突则会覆盖之。

* 比如 `freeze` 子命令：

  .. code:: ini

     [freeze]
     timeout = 10

* 比如 `install` 子命令：

  .. code:: ini

     [install]
     ignore-installed = true
     no-dependencies = yes
     find-links =
            http://mirror1.example.com
            http://mirror2.example.com

环境变量
~~~~~~~~
pip的命令行参数也可以通过环境变量来设置。
它的格式为： `PIP_<UPPER_LONG_NAME>` 。

* 比如设置超时：

  .. code:: bash

     $ export PIP_DEFAULT_TIMEOUT=60

  其效果等于：
     
  .. code:: bash

     $ pip --default-timeout=60 [...]

* 比如设置find-links：

  .. code:: bash

     $ export PIP_FIND_LINKS="http://mirror1.example.com http://mirror2.example.com"

  其效果等于：

  .. code:: bash

     $ pip install --find-links=http://mirror1.example.com
     --find-links=http://mirror2.example.com

配置的优先级
~~~~~~~~~~~~
刚才讲到一个参数的三种配置方法优先级顺序是：

1. 命令行
   
2. 环境变量
     
3. 配置文件

命令的自动补全
~~~~~~~~~~~~~~
用过bash/zsh环境的人都知道有自动补全功能。

pip命令想要自动补全功能的话，可以运行以下命令：

.. code:: bash

   $ pip completion --bash >> ~/.profile

   $ pip completion --zsh >> ~/.zprofile

加入到profile文件后，需要重新进入bash/zsh环境才能生效，
你也可以运行以下命令，立即生效自动补全功能：

.. code:: bash

   $ eval "`pip completion --bash`"

pip install
-----------
安装Python包

使用方法
~~~~~~~~
.. code:: bash

   $ pip install [options] <requirement specifier> ...

   $ pip install [options] -r <requirements file> ...

   $ pip install [options] [-e] <vcs project url> ...

   $ pip install [options] [-e] <local project path> ...

   $ pip install [options] <archive url/path> ...

本地快速安装
~~~~~~~~~~~~
1. 下载你所需要的所有的Python包

.. code:: bash
   
   $ pip install --download <DIR> -r requirements.txt

2. 使用以下命令即可安装刚才下载到本地的Python包

.. code:: bash

   $ pip install --no-index --find-links=[file://]<DIR> -r requirements.txt

非递归式升级
~~~~~~~~~~~~
其实就是只升级你需要的包，但是该包所依赖的包不安装/升级。

.. code:: bash

   $ pip install --upgrade --no-deps SomePackage

特定用户安装
~~~~~~~~~~~~
从Python2.6开始，Python就支持将Python包安装到指定的用户目录了。

该用户的目录默认值是由 `site.USER_BASE` 所指定的。

如果想覆盖该值，则使用环境变量 `PYTHONUSERBASE` 。

可以通过 `--user` 参数来指定特定用户。

.. code:: bash

   $ export PYTHONUSERBASE=/myappenv
   $ pip install --user SomePackage

pip uninstall
-------------
卸载Python包

使用方法
~~~~~~~~
.. code:: bash

   $ pip uninstall [options] <package> ...

   $ pip uninstall [options] -r <requirements file> ...

已知缺陷
~~~~~~~~
目前已知的两种无法正常删除的情况：

* 完全使用disutils模块制定的Python包，
  且通过 `python setup.py install` 来安装的。
  这种包没有什么元信息能够知道它倒底安装了哪些文件。
  
* 通过 `python setup.py develp` 来安装的脚本。

参数
~~~~
-r, --requirement <file>

  删除requirements.txt文件中的包含的Python包名，可以跟多个该参数

-y, --yes

  在删除时不需要确认

pip freeze
----------
将当前Python环境所有的安装包名输出成requirements格式。

使用方法
~~~~~~~~

.. code:: bash

   $ pip freeze [options]

参数
~~~~
-r, --requirement <file>  

  * 先输出该requirement文件内的Python包名。
  
  * 再输出当前环境安装的Python包，
    requirement文件中有的Python包名则不再显示。
  
  * 当requirement文件中的包名在环境没有，则会给出提示。

-f, --find-links <url>

  从该URL来查找Python包，查找出来的Python包名也会输出出来。
  说实话，我还真不知道这个参数的应用场景是什么。

-l, --local

  如果一个virtualenv环境被配置成能够读取全局的Python包，
  那么在该环境内运行 `pip freeze -l` 时，
  不会显示全局的Python包名。

常见应用场景
~~~~~~~~~~~~
当你需要将一个virtualenv环境中复制到另外一个virtualenv环境时，
你可以先在源virtualenv环境运行命令：

.. code:: bash

   $ pip freeze > requirements.txt

然后再进入目的virtualenv环境运行命令：

.. code:: bash

   $ pip install -r requirements.txt

这样就完成了虚拟环境的复制过程。

这时可能有人会问，virtualenv环境不就是一个目录么，
直接拷贝一下，不就一个跟原来一样的新的virtualenv环境么？

  好吧，我觉得这方法一般情况也可以的。

  但是如果源virtualenv环境和目的virtualenv环境的
  Python版本或者操作系统不一样，
  建议你还是老实地按照上面的说做吧。

pip list
--------
列出当前环境所有已经安装的Python包，包括可编辑的包（including editables）。

好像功能和 `pip freeze` 功能差不多的么，
只是输出的格式不一样。

可编辑的包是啥意思？暂时还不清楚。

使用方法
~~~~~~~~
.. code:: bash

   $ pip list [options]

参数
~~~~
-o, --outdated  列出所有有新版本的Python包名（不包括可编辑包）

-u, --uptodate  列出所有更新到最新版本的Python包名

-e, --editable  列出所有可编辑的包名

-l, --local

  如果一个virtualenv环境被配置成能够读取全局的Python包，
  那么在该环境内运行 `pip list -l` 时，
  不会显示全局的Python包名。

--pre  列出的包中包括预发行或者是开发包，默认只会列出稳定版本的包

-i, --index-url <url>  `Python Package Index` 的URL地址

--extra-index-url <url>  更多的 `Python Package Index` 的URL地址

--no-index  忽略包索引，只与 `--find-links` 参数配合使用

-f, --find-links <url>

  如果为一个URL或者是一个指定HTML页面的路径，PIP会从该地址解析出包地址。
  如果是一个本地目录，PIP就会直接从该目录中查找所需要的包。

--allow-external <package>  

  Allow the installation of externally hosted files

--allow-all-external

  Allow the installation of all externally hosted files

--allow-unverified <package>

  Allow the installation of insecure and unverifiable files

--process-dependency-links

  Enable the processing of dependency links

pip show
--------
列出一个或者多个包的信息

使用方法
~~~~~~~~
.. code:: bash
   
   $ pip show [options] <package> ...

参数
~~~~
-f, --files  列出一个已安装包中的所有文件

pip search
----------
从 `Python Package Index` 上查找
Python包名或者其简要描述中包含关键字（<query>）的Python包。

使用方法
~~~~~~~~
.. code:: bash
   
   $ pip search [options] <query>

参数
~~~~
--index <url>  `Python Package Index` URL地址

pip wheel
---------
创建wheel格式的Python包

使用该参数，需要您的环境安装setuptools >= 0.8，并且安装了wheel。

`pip wheel` 使用setuptools扩展 `bdist_wheel` 来制作wheel格式的Python包。

如果想更多了解wheel，请看 `wheel官方文档`_ 。

使用方法
~~~~~~~~
.. code:: bash
   
   $ pip pip wheel [options] <requirement specifier> ...

   $ pip wheel [options] -r <requirements file> ...

   $ pip wheel [options] <vcs project url> ...

   $ pip wheel [options] <local project path> ...

   $ pip wheel [options] <archive url/path> ...

参数
~~~~
-w, --wheel-dir <dir>  wheel格式Python包的存放目录，默认为 `<cwd>/wheelhouse`

--no-use-wheel  当在 `Python Package Index` 上查找包时不优先查找wheel格式的Python包

--build-option <options>  `setup.py bdist_wheel` 提供的额外参数

-r, --requirement <file>  从requirements文件进行安装，该参数可以跟多个

--download-cache <dir>  从网上下载的原始Python包的临时存放目录

--no-deps  不安装某个包的依赖包

-b, --build <dir>

  build目录，在virtualenv环境中默认值为 `<venv path>/build` ,
  在正常环境默认值为 `<OS temp dir>/pip_build_<username>`

--global-option <options>  `bdsit_wheel` 命令所用到的一些全局参数

--pre  列出的包中包括预发行或者是开发包，默认只会列出稳定版本的包

--no-clean  不清除build目录

-i, --index-url <url>  `Python Package Index` 的URL地址 (默认为 https://pypi.python.org/simple/)

--extra-index-url <url>  更多的 Python Package Index 的URL地址

--no-index  忽略包索引，只与 `--find-links` 参数配合使用

-f, --find-links <url>  

  如果为一个URL或者是一个指定HTML页面的路径，PIP会从该地址解析出包地址。
  如果是一个本地目录，PIP就会直接从该目录中查找所需要的包。

--allow-external <package>  Allow the installation of externally hosted files

--allow-all-external  Allow the installation of all externally hosted files

--allow-unverified <package>  Allow the installation of insecure and unverifiable files

--process-dependency-links  Enable the processing of dependency links.

常见应用场景
~~~~~~~~~~~~
先将需要的所有第三方Python包打包到本地目录，
然后再从本地目录直接安装之前已经打包好的wheel格式的Python包。

.. code:: bash

   $ pip wheel --wheel-dir=/tmp/wheelhouse SomePackage
   $ pip install --no-index --find-links=/tmp/wheelhouse SomePackage


其它
----

国内镜像源
~~~~~~~~~~
* 豆瓣PyPI_
  
* 华中理工大学PyPI_
  
* 山东理工大学PyPI_ ， 这个貌似用不了。
  
* 中国科学技术大学PyPI_

可以用pip的 `-i` 参数来指定镜像源。

.. code:: bash

   $ pip install gevent -i http://pypi.douban.com/simple

如果不想每次手动指定，且永久有效的话，
可以写入到pip的配置文件中。

.. code:: ini
   
   [global]
   index-url = http://pypi.douban.com/simple


.. _`pip官方文档`: https://pip.pypa.io/en/latest/
.. _`wheel官方文档`: http://wheel.readthedocs.org/en/latest/
.. _`get-pip.py`: https://bootstrap.pypa.io/get-pip.py/
.. _`豆瓣PyPI`: http://pypi.douban.com/
.. _`华中理工大学PyPI`: http://pypi.hustunique.com/
.. _`山东理工大学PyPI`: http://pypi.sdutlinux.org/
.. _`中国科学技术大学PyPI`: http://pypi.mirrors.ustc.edu.cn/
