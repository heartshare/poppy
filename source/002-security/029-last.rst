last
##########

last命令用于查看系统用户登录日志



通过指定日志文件查看最近成功登录的10条记录
===========================================================


.. code-block:: bash

    last -10 -f   /var/log/wtmp


查看最近200条登录失败的记录
==================================================

.. code-block:: bash

     last -200 -f /var/log/btmp