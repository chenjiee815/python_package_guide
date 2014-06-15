==================
 setup.py文件解析
==================
将库添加到 PyPI_

.. code:: bash

   python setup.py register
   python setup.py sdis upload
    
`register` 命令会根据setup.py中的信息，在 PyPI_ 注册，
但不会上传任何内容。

`sdist upload` 命令构造了一个包，并上传到 PyPI_ 。


.. _PyPI: https://pypi.python.org/pypi/
