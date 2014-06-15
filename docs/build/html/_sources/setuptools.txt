================
 setuptools教程
================

`setuptools官方文档`_

推荐用法
--------
在项目根目录添加 `ez_setup.py`_ 文件，
然后在setup.py文件的开头添加如下代码：

.. code:: python

    from ez_setup import use_setuptools
    use_setuptools()

这样写的话，其他人安装你的Python包时，
如果没有安装setuptools工具，该脚本就会自动下载并安装setuptools。

当然也可以通过这个脚本来直接安装、更新setuptools：

.. code:: bash

   $ sudo python ez_setup.py -U setuptools

基本用法
--------
下面是一个最基本的setup.py。

.. code:: python

   from setuptools import setup, find_packages
   setup(
          name = "HelloWorld",
          version = "0.1",
          packages = find_packages(),
   )

就是这里的几行代码，就可以自动查找需要打包的文件，
将当前的项目打包成eggs，支持上传到 PyPI_ 。

当然如果需要上传到 PyPI_ ，最好再添加一些额外信息。

版本号格式
----------
setuptools能够支持大部分的版本号格式。

如果您不是很确定您定义的版本号是否被setuptools支持，
您可以使用pkg_resources.parse_version()来判断。

.. code:: python

   >>> from pkg_resources import parse_version
   >>> parse_version('1.9.a.dev') == parse_version('1.9a0dev')
   True
   >>> parse_version('2.1-rc2') < parse_version('2.1')
   True
   >>> parse_version('0.6a9dev-r41475') < parse_version('0.6a9')
   True

可选的setup()函数参数
---------------------
include_package_data

   如果设置为True，setuptools就会在项目目录里自动查找所有的数据文件

exclude_package_data

   

package_data
   
   

zip_safe


install_requires

entry_points

extras_require

setup_requires

dependency_links

namespace_packages


easy_install
------------


pkg_resources
-------------
该模块的全称为： `Package Discovery and Resource Access`

这个模块主要有以下几个功能：

* 提供API给Python库来让它们能够访问它们自己的资源文件
  
* 能让扩展的应用和框架自动发现插件
  
* 给zip格式的eggs包内的C扩展提示运行时支持
  
* 能够合并各种独立分发的Python模块，就是下文的命名空间包
  
* 提供API来管理当前“工作集”中的Python包
  
命名空间包
~~~~~~~~~~
命名空间包本质算是一个虚拟包，
这个包主要将多个独立分发的包集合成一个，方便安装。
比如： `peak` 是一个命名空间包，它包含了一系列不同用途的子包。

那么如何创建一个命名空间包呢？

    在 `setup()` 函数中添加 `namespace_package` 参数。
    该参数为一个列表，然后将需要集合的包的名称写进去。

    你也必须在命名空间包的 `__init__.py` 文件中添加
    `__declare_namespace()` 函数的调用。

工作集对象
~~~~~~~~~~


.. _`ez_setup.py`: https://bootstrap.pypa.io/ez_setup.py/
.. _`setuptools官方文档`: http://pythonhosted.org/setuptools/
.. _PyPI: https://pypi.python.org/pypi/
