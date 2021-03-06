一键优化脚本汇总
############################


解决ssh缓慢问题
==========================

.. code-block:: bash

    bash -c "$(curl -fsSL https://raw.githubusercontent.com/AlvinWanCN/poppy/master/code/common_tools/sshslowly.sh)"


关闭firewalld和selinux
=================================

.. code-block:: bash

    bash -c "$(curl -fsSL https://raw.githubusercontent.com/AlvinWanCN/poppy/master/code/common_tools/disableSeAndFir.sh)"



添加alv.pub本地网络仓库
=========================

.. code-block:: bash

    python -c "$(curl -fsSL https://raw.githubusercontent.com/AlvinWanCN/poppy/master/code/common_tools/pullLocalYum.py)"

将本地光盘作为repositories
===================================

.. code-block:: bash

    curl -s https://raw.githubusercontent.com/AlvinWanCN/poppy/master/code/common_tools/create_local_repo.py|python

加入ipa的alv.pub的ldap系统
================================

描述：加入到ipa.alv.pub ldap系统，并配置autofs将dc.alv.pub的用户数据目录挂载过来，dc.alv.pub是alvin的虚拟机，仅alvin自己可以访问。

.. code-block:: bash

    python -c "$(curl -fsSL https://raw.githubusercontent.com/AlvinWanCN/poppy/master/code/common_tools/joinNatashaLDAP.py)"


常用的系统优化
=================================
包括vim，history,bash-completion

.. code-block:: bash

    curl -fsSL https://raw.githubusercontent.com/AlvinWanCN/poppy/master/code/common_tools/optimize_system.py|python

最大文件打开数优化
==========================

.. code-block:: bash

    curl -fsSL https://raw.githubusercontent.com/AlvinWanCN/poppy/master/code/common_tools/ulimit_optimize.sh|bash


百万并发系统内核优化
==============================

.. code-block:: bash

     curl -fsSL https://raw.githubusercontent.com/AlvinWanCN/poppy/master/code/common_tools/core_optmize.sh|bash