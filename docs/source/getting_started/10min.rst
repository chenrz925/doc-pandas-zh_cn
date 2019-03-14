.. _10min:

====================
10分钟了解pandas
====================

这是一篇主要面向于新用户的关于pandas的简短介绍。
你可以在 :ref:`Cookbook<cookbook>` 中看到更为复杂的指导。

通常地，我们用下面这种方式导入库：

.. ipython:: python

   import numpy as np
   import pandas as pd

----------
创建对象
----------

详见 :ref:`数据结构介绍 <dsintro>` 。

用传一列表值的方式创建一个 :class:`Series` ，使pandas构建一个默认的整数索引。

.. ipython:: python

   s = pd.Series([1, 3, 5, np.nan, 6, 8])
   s

用传NumPy数组的方式构建一个具有列标签与日期时间索引的 :class:`DataFrame` 。

.. ipython:: python

   dates = pd.date_range('20130101', periods=6)
   dates
   df = pd.DataFrame(np.random.randn(6, 4), index=dates, columns=list('ABCD'))
   df

用传可以被转换为序列的对象的列表的方式来构建 ``DataFrame`` 。

.. ipython:: python

   df2 = pd.DataFrame({'A': 1.,
                       'B': pd.Timestamp('20130102'),
                       'C': pd.Series(1, index=list(range(4)), dtype='float32'),
                       'D': np.array([3] * 4, dtype='int32'),
                       'E': pd.Categorical(["test", "train", "test", "train"]),
                       'F': 'foo'})
   df2

得到的 ``DataFrame`` 的每一列具有不同的 :ref:`dtypes <basics.dtypes>` 。

.. ipython:: python

   df2.dtypes

当你在使用IPython时列名（也是对象的公有属性）的自动补全会被自动打开：

.. ipython::

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

如您所见，``A`` 、 ``B`` 、 ``C`` 和 ``D`` 都被收进了自动补全。
而 ``E`` 也同样在其中，简短起见我们把其他的属性截去了。

----------
查看数据
----------

详见 :ref:`基础 <basics>` 。

这里展示如何查看一个数据框最顶端和最底端的几行数据：

.. ipython:: python

   df.head()
   df.tail(3)

展示索引与列：

.. ipython:: python

   df.index
   df.columns

:meth:`DataFrame.to_numpy` 提供了内含数据的NumPy表示。
由于pandas和NumPy的基本区别： **每一个NumPy数组内部只具有一个针对整个数组的dtype，而pandas的DataFrame每一列只有一种dtype** 。
当你调用 :meth:`DataFrame.to_numpy` 时，pandas将会识别出一种NumPy的dtype来适应DataFrame中所有列。
这很有可能是 ``object`` 类型，即把所有值都转换为Python的object类型。

对于 ``df``，一个所有值都是浮点值的 :class:`DataFrame`， :meth:`DataFrame.to_numpy` 很快并且不需要复制数据。

.. ipython:: python

   df.to_numpy()

对于 ``df2``，一个具有多种dtype的 :class:`DataFrame`，相应地 :meth:`DataFrame.to_numpy` 就具有昂贵的性能开销。

.. ipython:: python

   df2.to_numpy()

.. note::

   :meth:`DataFrame.to_numpy` 在输出中 *不* 包括索引或列标签。

:func:`~DataFrame.describe` 显示了数据的快速统计摘要：

.. ipython:: python

   df.describe()

转置你的数据：

.. ipython:: python

   df.T

按轴排序：

.. ipython:: python

   df.sort_index(axis=1, ascending=False)

按值排序：

.. ipython:: python

   df.sort_values(by='B')

----------
选择
----------

.. note::
    虽然在交互式工作中标准Python/NumPy的选择修改表达式直观并且容易上手，
    但在生产代码中，我们仍然推荐使用pandas优化过的数据获取方法，
    ``.at`` 、 ``.iat`` 、 ``.loc`` 和 ``.iloc`` 。

详见关于索引的文档 :ref:`索引与选择数据 <indexing>` and :ref:`多重/高级索引 <advanced>` 。

~~~~~~~~~~
获取
~~~~~~~~~~

选择单一一列将会产出一个 ``Series`` ，相当于调用 ``df.A`` 。

.. ipython:: python

   df['A']

使用 ``[]`` 来选择，可以按行来对数据进行切片。

.. ipython:: python

   df[0:3]
   df['20130102':'20130104']

~~~~~~~~~~~~~~~
按标签选择
~~~~~~~~~~~~~~~

详见 :ref:`按标签选择 <indexing.label>` 。

用标签获取横截面数据：

.. ipython:: python

   df.loc[dates[0]]

用标签获取多个轴的数据：

.. ipython:: python

   df.loc[:, ['A', 'B']]

使用标签切片，两端的标签 *都被包含* ：

.. ipython:: python

   df.loc['20130102':'20130104', ['A', 'B']]

降低返回对象的维度：

.. ipython:: python

   df.loc['20130102', ['A', 'B']]

获取一个标量值：

.. ipython:: python

   df.loc[dates[0], 'A']

快速获取一个标量值（与先前方法等价）：

.. ipython:: python

   df.at[dates[0], 'A']

~~~~~~~~~~~~~~~
按位置选择
~~~~~~~~~~~~~~~

详见 :ref:`按位置选择 <indexing.integer>` 。

按传入整数的位置选择：

.. ipython:: python

   df.iloc[3]

行为类似于NumPy/Python的按整数切片选择：

.. ipython:: python

   df.iloc[3:5, 0:2]

风格类似于NumPy/Python的按整数位置的列表选择：

.. ipython:: python

   df.iloc[[1, 2, 4], [0, 2]]

显性地按行切片：

.. ipython:: python

   df.iloc[1:3, :]

显性地按列切片：

.. ipython:: python

   df.iloc[:, 1:3]

显性地获得一个值：

.. ipython:: python

   df.iloc[1, 1]

快速获得一个标量值（等价于上个方法）：

.. ipython:: python

   df.iat[1, 1]

~~~~~~~~~~~~~~~
布尔值索引
~~~~~~~~~~~~~~~

用单独一列的值来选择数据。

.. ipython:: python

   df[df.A > 0]

从一个DataFrame中选择符合布尔条件的值。

.. ipython:: python

   df[df > 0]

用 :func:`~Series.isin` 来过滤：

.. ipython:: python

   df2 = df.copy()
   df2['E'] = ['one', 'one', 'two', 'three', 'four', 'three']
   df2
   df2[df2['E'].isin(['two', 'four'])]

~~~~~~~~~~~
设定
~~~~~~~~~~~

设定自动对齐按索引对齐到数据上的新列：

.. ipython:: python

   s1 = pd.Series([1, 2, 3, 4, 5, 6], index=pd.date_range('20130102', periods=6))
   s1
   df['F'] = s1

按标签设定值：

.. ipython:: python

   df.at[dates[0], 'A'] = 0

按位置设定值：

.. ipython:: python

   df.iat[0, 1] = 0

用赋一个NumPy数组的方式设定值：

.. ipython:: python

   df.loc[:, 'D'] = np.array([5] * len(df))

上一个设定操作的结果是：

.. ipython:: python

   df

通过一个 ``where`` 操作来设定值。

.. ipython:: python

   df2 = df.copy()
   df2[df2 > 0] = -df2
   df2

---------------
缺失数据
---------------

pandas主要用 ``np.nan`` 值来代表缺失数据。
这在默认情况下不被包括在运算当中。
详见 :ref:`缺失数据 <missing_data>`。

重新索引可以允许您修改/添加/删除某个特定轴上的索引。这个操作返回源数据的一份拷贝。

.. ipython:: python

   df1 = df.reindex(index=dates[0:4], columns=list(df.columns) + ['E'])
   df1.loc[dates[0]:dates[1], 'E'] = 1
   df1

移除任何带有缺失数据的行。

.. ipython:: python

   df1.dropna(how='any')

填充缺失数据。

.. ipython:: python

   df1.fillna(value=5)

得到值为 `nan` 的值的布尔值掩码。

.. ipython:: python

   pd.isna(df1)

----------
运算符
----------

详见 :ref:`基本二元运算符 <basics.binop>` 。