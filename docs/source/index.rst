.. pandas-zh_cn documentation master file, created by
   sphinx-quickstart on Sun Mar 10 21:55:32 2019.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

.. module:: pandas

==============================
pandas：强力的Python数据分析工具包
==============================

**日期**：2019年2月3日

**版本**：0.24.1

**实用链接**： 
`二进制安装 <https://pypi.org/project/pandas>`__ |
`源代码仓库 <https://github.com/pandas-dev/pandas>`__ |
`问题和想法 <https://github.com/pandas-dev/pandas/issues>`__ |
`Q&A支持 <https://stackoverflow.com/questions/tagged/pandas>`__ |
`邮件列表 <https://groups.google.com/forum/#!forum/pydata>`__

:mod:`pandas` 是一个具有使用简单数据结构的开源高性能 `Python <https://www.python.org/>`__ 数据分析库。

查看 :ref:`overview` 了解关于库的更多细节。

{% if single_doc and single_doc.endswith('.rst') -%}
.. toctree::
    :maxdepth: 2

    {{ single_doc[:-4] }}
{% elif single_doc %}
.. autosummary::
    :toctree: reference/api/

    {{ single_doc }}
{% else -%}
.. toctree::
    :maxdepth: 2
{% endif %}

    {% if not single_doc -%}
    What's New in 0.25.0 <whatsnew/v0.25.0>
    install
    getting_started/index
    user_guide/index
    ecosystem
    {% endif -%}
    {% if include_api -%}
    reference/index
    {% endif -%}
    {% if not single_doc -%}
    development/index
    whatsnew/index
    {% endif -%}


