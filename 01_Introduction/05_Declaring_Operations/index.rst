现在我们必须知道可以加到TensorFlow计算图上的其他计算工具。
除了标准的算式运算外，TensorFlow提供给我们更多需要注意的运算符，我们应当在继续之前知道如何使用它们。同样，我们需要运行一下下面的命令来创建一个 :code:`graph session`:

.. code:: python

  >>> import numpy as np
  >>> import tensorflow as tf
  >>> from tensorflow.python.framework import ops
  >>> ops.reset_default_graph()
  >>> tf.compat.v1.disable_eager_execution()
  >>> sess = tf.compat.v1.Session()

TensorFlow对张量有标准的运算符：:code:`add()` , :code:`sub()` , :code:`mul()` , 和 :code:`div()` . 需要指出的是，这部分所有的运算除了特别说明外，都会输出element-wise式输出结果。

:code:`div()` 函数及其相关的函数
------------------------------

:code:`div()` 返回与输出结果类型相同的结果。这意味着如果输入的是整数的话，它返回 :emphasis:`the floor of the division` (是 :code:`Python 2` 的近亲)。为了产生 :code:`Python 3` 版本的结果，TensorFlow提供了 :code:`truediv()` 函数，如下：

.. code:: python
  
  >>> print(sess.run(tf.compat.v1.div(3,4)))
  0
  >>> print(sess.run(tf.compat.v1.truediv(3,4)))
  0.75
  >>> print(sess.run(tf.compat.v1.div(3.0,4)))
  0.75
  
如果我们浮点数然后希望做一个整数除法，我们可以用 :code:`floordiv()` 函数。 需要注意的是，我们仍然返回一个浮点数，但是已经被近似成最近邻的整数。如下：

.. code:: python
  
  >>> print(sess.run(tf.compat.v1.floordiv(3.0,4.0)))
  0.0


:code:`mod()` 函数
------------------------------

另外一个重要的函数就是 :code:`mod()` . 这个函数返回除法的余数。如下：

.. code:: python
    
   >>> print(sess.run(tf.compat.v1.mod(22,5)))
   2
   >>> print(sess.run(tf.compat.v1.mod(22.0,5)))
   2.0

:code:`cross()` 函数
------------------------------

两个张量的叉乘可以通过调用 :py:func:`tensorflow.compat.v1.cross` 函数来实现。记住，这里的叉乘只定义到俩个三维向量，所以它仅支持俩个三维向量。如下：

.. code:: python
  
   >>> print(sess.run(tf.compat.v1.cross([1.,2.,3.],[4.,5.,6.])))
   [-3.  6. -3.]
   >>> help(tf.compat.v1.cross)

以下是 :py:func:`help` 函数返回的结果：

.. py:function:: cross(a, b, name=None)

   Compute the pairwise cross product.
   
   `a` and `b` must be the same shape; they can either be simple 3-element vectors, or any shape where the innermost dimension is 3. In the latter case, each pair of corresponding 3-element vectors is cross-multiplied independently.

   :param a: Must be one of the following types: `float32`, `float64`, `int32`, `uint8`, `int16`, `int8`, `int64`, `bfloat16`, `uint16`, `half`, `uint32`, `uint64`. A tensor containing 3-element vectors.
   :type a: Tensor
   :param b: Must have the same type as `a`. 
   :type b: Tensor
   :param name: A name for the operation (optional).
   :rtype: Tensor. 
   :returns: Has the same type as `a`.
   
常用的数学函数列表
---------------

.. attention:: 所有这些函数都是element-wise式运行。

+---------------------------------------------------+---------------------------------------+
| 常用数学函数                                      | 描述                                  |
+===================================================+=======================================+
| :py:function:`tensorflow.compat.v1.abs()`         | 输入张量的绝对值                      |
+---------------------------------------------------+---------------------------------------+
| :py:function:`tensorflow.compat.v1.ceil()`        | 输入张量的向上舍入函数                |
+---------------------------------------------------+---------------------------------------+
| :py:function:`tensorflow.compat.v1.cos()`         | 输入张量的Cosine函数                  |
+---------------------------------------------------+---------------------------------------+
| :py:function:`tensorflow.compat.v1.exp()`         | 输入张量的exp函数                     |
+---------------------------------------------------+---------------------------------------+
| :py:function:`tensorflow.compat.v1.floor()`       | 输入张量的向下舍入函数                |
+---------------------------------------------------+---------------------------------------+
| :py:function:`tensorflow.compat.v1.inv()`         | 输入张量的倒数(用不了)                |
+---------------------------------------------------+---------------------------------------+
| :py:function:`tensorflow.compat.v1.log()`         | 输入张量的自然对数                    |
+---------------------------------------------------+---------------------------------------+
| :py:function:`tensorflow.compat.v1.maximum()`     | 输入张量的最大值                      |
+---------------------------------------------------+---------------------------------------+
| :py:function:`tensorflow.compat.v1.minimum`       | 输入张量的最小值                      |
+---------------------------------------------------+---------------------------------------+
| :py:function:`tensorflow.compat.v1.neg()`         | 输入张量的负值                        |
+---------------------------------------------------+---------------------------------------+
| :py:function:`tensorflow.compat.v1.pow()`         | 第一张量上升到第二张量元素            |
+---------------------------------------------------+---------------------------------------+
| :py:function:`tensorflow.compat.v1.round()`       | 输入张量的近似                        |
+---------------------------------------------------+---------------------------------------+
| :py:function:`tensorflow.compat.v1.rsqrt()`       | 输入张量平方根的倒数                  |
+---------------------------------------------------+---------------------------------------+
| :py:function:`tensorflow.compat.v1.sign()`        | 输出 -1, 0, 或 1 取决输入张量的符号   |
+---------------------------------------------------+---------------------------------------+
| :py:function:`tensorflow.compat.v1.sin()`         | 输入张量的Sine函数                    |
+---------------------------------------------------+---------------------------------------+
| :py:function:`tensorflow.compat.v1.sqrt`          | 输入张量的平方根                      |
+---------------------------------------------------+---------------------------------------+
| :py:function:`tensorflow.compat.v1.square`        | 输入张量的平方                        |
+---------------------------------------------------+---------------------------------------+

Open graph session
------------------

.. code:: python

  sess = tf.Session()
  
Arithmetic Operations
---------------------
TensorFlow has multiple types of arithmetic functions. Here we illustrate the differences
between ``div()``, ``truediv()`` and ``floordiv()``.

``div()`` : integer of division (similar to base python //)

``truediv()`` : will convert integer to floats.

``floordiv()`` : float of div()

.. code:: python

  print(sess.run(tf.div(3,4)))
  print(sess.run(tf.truediv(3,4)))
  print(sess.run(tf.floordiv(3.0,4.0)))

the output::

  0
  0.75
  0.0

Mod function:

.. code:: python

  print(sess.run(tf.mod(22.0,5.0)))

the output::

  2.0

Cross Product:

.. code:: python

  print(sess.run(tf.cross([1.,0.,0.],[0.,1.,0.])))

the output::

  [ 0.  0.  1.]
  
Trig functions
---------------

Sine, Cosine, and Tangent:

.. code:: python

  print(sess.run(tf.sin(3.1416)))
  print(sess.run(tf.cos(3.1416)))
  print(sess.run(tf.div(tf.sin(3.1416/4.), tf.cos(3.1416/4.))))
  
the output::

  -7.23998e-06
  -1.0
   1.0
  
  
Custom operations
------------------

Here we will create a polynomial function:

:math:`f(x) = 3 \ast x^2-x+10`

.. code:: python

  test_nums = range(15)
  
  def custom_polynomial(x_val):
    # Return 3x^2 - x + 10
      return(tf.subtract(3 * tf.square(x_val), x_val) + 10)

  print(sess.run(custom_polynomial(11)))

the output::
  
  362
  
What should we get with list comprehension:

.. code:: python
  
  expected_output = [3*x*x-x+10 for x in test_nums]
  print(expected_output)
  
the output::

  [10, 12, 20, 34, 54, 80, 112, 150, 194, 244, 300, 362, 430, 504, 584]
  
TensorFlow custom function output:

.. code:: python

  for num in test_nums:
      print(sess.run(custom_polynomial(num)))


the output::
  
  10
  12
  20
  34
  54
  80
  112
  150
  194
  244
  300
  362
  430
  504
  584
