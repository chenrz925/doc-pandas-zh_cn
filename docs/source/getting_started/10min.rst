.. _10min:

====================
10分钟了解pandas
====================

这是一篇主要面向于新用户的关于pandas的简短介绍。
你可以在 :ref:`Cookbook<cookbook>` 中看到更为复杂的指导。

通常地，我们用下面这种方式导入库：

.. code-block:: python
    :linenos:

    import numpy as np
    import pandas as pd

----------
创建对象
----------

查看 :ref:`数据结构介绍 <dsintro>` 。

用传一列表值的方式创建一个 :class:`Series` ，使pandas构建一个默认的整数索引。

.. code-block:: python
    :linenos:

    s = pd.Series([1, 3, 5, np.nan, 6, 8])
    print(s)

用传NumPy数组的方式构建一个具有列标签与日期时间索引的 :class:`DataFrame` 。

.. code-block:: python
    :linenos:

    dates = pd.date_range('20130101', periods=6)
    print(dates)
    df = pd.DataFrame(np.random.randn(6, 4), index=dates, columns=list('ABCD'))
    print(df)
用传可以被转换为序列的对象的列表的方式来构建 ``DataFrame`` 。

.. code-block:: python
    :linenos:

    df2 = pd.DataFrame({'A': 1.,
                       'B': pd.Timestamp('20130102'),
                       'C': pd.Series(1, index=list(range(4)), dtype='float32'),
                       'D': np.array([3] * 4, dtype='int32'),
                       'E': pd.Categorical(["test", "train", "test", "train"]),
                       'F': 'foo'})
    print(df2)

得到的 ``DataFrame`` 的每一列具有不同的 :ref:`dtypes <basics.dtypes>` 。

.. code-block:: python
    :linenos:

    print(df2.dtypes)

当你在使用IPython时列名（也是对象的公有属性）的自动补全会被自动打开：

.. code-block:: python

   @verbatim
   In [1]: df2.<TAB>  # noqa: E225, E999
   df2.A                  df2.bool
   df2.abs                df2.boxplot
   df2.add                df2.C
   df2.add_prefix         df2.clip
   df2.add_suffix         df2.clip_lower
   df2.align              df2.clip_upper
   df2.all                df2.columns
   df2.any                df2.combine
   df2.append             df2.combine_first
   df2.apply              df2.compound
   df2.applymap           df2.consolidate
   df2.D
