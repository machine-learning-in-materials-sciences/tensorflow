MNIST 手写数据代码补充
---------------------

因为篇幅原因，在介绍MNIST手写数据库时，没有对手写的数字可视化进行详细介绍，这里补充一些代码：

.. code:: bash
  
  $ dataset_url = "https://modelarts-cnnorth1-market-dataset.obs.cn-north-1.myhuaweicloud.com/dataset-market/Mnist-Data-Set/archiver/Mnist-Data-Set.zip"
  $ dataset_local_path = 'dataset/'
  $ wget {dataset_url} -P {dataset_local_path}
  $ unzip -d {dataset_local_path} -o {dataset_local_name}
  $ ls $dataset_local_path
  Mnist-Data-Set.zip	       train-images-idx3-ubyte
  t10k-images-idx3-ubyte	   train-images-idx3-ubyte.gz
  t10k-images-idx3-ubyte.gz  train-labels-idx1-ubyte
  t10k-labels-idx1-ubyte	   train-labels-idx1-ubyte.gz
  t10k-labels-idx1-ubyte.gz
  
  $ train_image = os.path.join(dataset_local_path, 'train-images-idx3-ubyte')
  $ train_lable = os.path.join(dataset_local_path, 'train-labels-idx1-ubyte')
  $ eval_image  = os.path.join(dataset_local_path, 't10k-images-idx3-ubyte')
  $ eval_lable  = os.path.join(dataset_local_path, 't10k-labels-idx1-ubyte')
  
  $ pip install mxnet
  Looking in indexes: http://repo.myhuaweicloud.com/repository/pypi/simple
  Collecting mxnet
  Downloading http://repo.myhuaweicloud.com/repository/pypi/packages/81/f5/d79b5b40735086ff1100c680703e0f3efc830fa455e268e9e96f3c857e93/mxnet-1.6.0-py2.py3-none-any.whl (68.7 MB)
     |████████████████████████████████| 68.7 MB 7.1 MB/s eta 0:00:01MB/s eta 0:00:31
  Requirement already satisfied: numpy<2.0.0,>1.16.0 in /home/ma-user/anaconda3/envs/TensorFlow-2.1.0/lib/python3.6/site-packages (from mxnet) (1.18.4)
  Requirement already satisfied: requests<3,>=2.20.0 in /home/ma-user/anaconda3/envs/TensorFlow-2.1.0/lib/python3.6/site-packages (from mxnet) (2.23.0)
  Requirement already satisfied: graphviz<0.9.0,>=0.8.1 in /home/ma-user/anaconda3/envs/TensorFlow-2.1.0/lib/python3.6/site-packages (from mxnet) (0.8.1)
  Requirement already satisfied: chardet<4,>=3.0.2 in /home/ma-user/anaconda3/envs/TensorFlow-2.1.0/lib/python3.6/site-packages (from requests<3,>=2.20.0->mxnet) (3.0.4)
  Requirement already satisfied: urllib3!=1.25.0,!=1.25.1,<1.26,>=1.21.1 in /home/ma-user/anaconda3/envs/TensorFlow-2.1.0/lib/python3.6/site-packages (from  requests<3,>=2.20.0->mxnet) (1.22)
  Requirement already satisfied: certifi>=2017.4.17 in /home/ma-user/anaconda3/envs/TensorFlow-2.1.0/lib/python3.6/site-packages (from requests<3,>=2.20.0->mxnet) (2018.1.18)
  Requirement already satisfied: idna<3,>=2.5 in /home/ma-user/anaconda3/envs/TensorFlow-2.1.0/lib/python3.6/site-packages (from requests<3,>=2.20.0->mxnet) (2.6)
  Installing collected packages: mxnet
  Successfully installed mxnet-1.6.0
  WARNING: You are using pip version 20.1.1; however, version 20.2.1 is available.
  You should consider upgrading via the '/home/ma-user/anaconda3/envs/TensorFlow-2.1.0/bin/python -m pip install --upgrade pip' command.

.. code:: python
  
  >>> import mxnet as mx
  >>> batch_size = 128
  >>> train_data = mx.io.MNISTIter(image = train_image,
  ...                              label = train_lable,
  ...                              data_shqpe = (1,28,28),
  ...                              batch_size = batch_size,
  ...                              shuffle = True,
  ...                              flat    = False,
  ...                              silent  = False)

  >>> eval_data  = mx.io.MNISTIter(image = eval_image,
  ...                              label = eval_lable,
  ...                              data_shqpe = (1,28,28),
  ...                              batch_size = batch_size,
  ...                              shuffle = False)
  
  >>> import matplotlib.pyplot as plt
  >>> train_data.reset()
  >>> next_batch  =  train_data.next()

  >>> for i in range(128):
  ...   show_image  =  next_batch.data[0][i].asnumpy() * 255      
  ...   show_image  =  show_image.astype('uint8').reshape(28, 28)
  ...   plt.figure(168)
  ...   plt.subplot(16,8,1+i)
  ...   plt.imshow(show_image, cmap = plt.cm.gray)
  
  >>> plt.savefig('Handwriting.png', dpi=1000)
  >>> plt.show()


Ham/Spam Text Dataset(垃圾邮件分类, UCI)
----------------------------

我们会用到加州大学艾文分校机器学习数据库也建立了一个垃圾邮件分类的数据库。我们可以获取.zip 文件， 并获取相应的数据。以下是它的链接 `Ham/Spam Text Dataset <https://archive.ics.uci.edu/ml/datasets/SMS+Spam+Collection>`_ 。顺便提一句，如果一个数据点代表（ :code:`spam` 或者不想要的广告）, 那么另外一个就是
'ham'.

:strong:`Ham/Spam Text Dataset` 是一个从文本输入当中预测二进制结果（spam or ham）一个很好的数据集。 这将对自然语言处理的短文本处理(第七章)和递归神经网络(第九章)
很有用。

.. code:: python

  >>> import requests
  >>> import io
  >>> from zipfile import ZipFile

  # Get/read zip file
  >>> zip_url = 'http://archive.ics.uci.edu/ml/machine-learning-databases/00228/smsspamcollection.zip'
  >>> r = requests.get(zip_url)
  >>> z = ZipFile(io.BytesIO(r.content))
  >>> file = z.read('SMSSpamCollection')
  # Format Data
  >>> text_data = file.decode()
  >>> text_data = text_data.encode('ascii',errors='ignore')
  >>> text_data = text_data.decode().split('\n')
  >>> text_data = [x.split('\t') for x in text_data if len(x)>=1]
  >>> [text_data_target, text_data_train] = [list(x) for x in zip(*text_data)]
  >>> print(len(text_data_train))
  5574
  >>> print(set(text_data_target))
  {'spam', 'ham'}
  >>> print(text_data_train[1])
  Ok lar... Joking wif u oni...
  
  >>> text_data_train[0:10]
  ['Go until jurong point, crazy.. Available only in bugis n great world la e buffet... Cine there got amore wat...',
  'Ok lar... Joking wif u oni...',
  "Free entry in 2 a wkly comp to win FA Cup final tkts 21st May 2005. Text FA to 87121 to receive entry question(std txt rate)T&C's apply 08452810075over18's",
  'U dun say so early hor... U c already then say...',
  "Nah I don't think he goes to usf, he lives around here though",
  "FreeMsg Hey there darling it's been 3 week's now and no word back! I'd like some fun you up for it still? Tb ok! XxX std chgs to send, 1.50 to rcv",
  'Even my brother is not like to speak with me. They treat me like aids patent.',
  "As per your request 'Melle Melle (Oru Minnaminunginte Nurungu Vettam)' has been set as your callertune for all Callers. Press *9 to copy your friends Callertune",
  'WINNER!! As a valued network customer you have been selected to receivea 900 prize reward! To claim call 09061701461. Claim code KL341. Valid 12 hours only.',
  'Had your mobile 11 months or more? U R entitled to Update to the latest colour mobiles with camera for Free! Call The Mobile Update Co FREE on 08002986030']


`Movie Review Data <http://ai.stanford.edu/~amaas/data/sentiment/aclImdb_v1.tar.gz>`_ (Stanford)
---------------------------
This is a dataset for binary sentiment classification containing substantially more data than previous benchmark datasets. We provide a set of 25,000 highly polar movie reviews for training, and 25,000 for testing. There is additional unlabeled data for use as well. Raw text and already processed bag of words formats are provided. See the README file contained in the release for more details.


You can read more about the dataset and papers using it `here <http://ai.stanford.edu/~amaas/data/sentiment/index.html>`

.. code:: python

  >>> import requests
  >>> import io
  >>> import tarfile

  >>> movie_data_url = 'http://www.cs.cornell.edu/people/pabo/movie-review-data/rt-polaritydata.tar.gz'
  >>> r = requests.get(movie_data_url)
  # Stream data into temp object
  >>> stream_data = io.BytesIO(r.content)
  >>> tmp = io.BytesIO()
  >>> while True:
  ...    s = stream_data.read(16384)
  ...    if not s:  
  ...          break
  ...    tmp.write(s)
  stream_data.close()
  tmp.seek(0)
  # Extract tar file
  tar_file = tarfile.open(fileobj=tmp, mode="r:gz")
  pos = tar_file.extractfile('rt-polaritydata/rt-polarity.pos')
  neg = tar_file.extractfile('rt-polaritydata/rt-polarity.neg')
  # Save pos/neg reviews
  pos_data = []
  for line in pos:
      pos_data.append(line.decode('ISO-8859-1').encode('ascii',errors='ignore').decode())
  neg_data = []
  for line in neg:
      neg_data.append(line.decode('ISO-8859-1').encode('ascii',errors='ignore').decode())
  tar_file.close()
  
  print(len(pos_data))
  print(len(neg_data))
  print(neg_data[0])
  
the output::

  5331
  5331
  simplistic , silly and tedious . 

The Complete Works of William Shakespeare (Gutenberg Project)
-------------------------------------------------------------
For training a TensorFlow Model to create text, we will train it on the complete works
of William Shakespeare. This can be accessed through the good work of the Gutenberg 
Project. The Gutenberg Project frees many non-copyright books by making them accessible
for free from the hard work of volunteers.

You can read more about the Shakespeare works `here <http://www.gutenberg.org/ebooks/100>`_

.. code:: python

  # The Works of Shakespeare Data
  import requests

  shakespeare_url = 'http://www.gutenberg.org/cache/epub/100/pg100.txt'
  # Get Shakespeare text
  response = requests.get(shakespeare_url)
  shakespeare_file = response.content
  # Decode binary into string
  shakespeare_text = shakespeare_file.decode('utf-8')
  # Drop first few descriptive paragraphs.
  shakespeare_text = shakespeare_text[7675:]
  print(len(shakespeare_text))

the output::

  5582212
  
English-German Sentence Translation Database (Manythings/Tatoeba)
-----------------------------------------------------------------

The `Tatoeba Project <http://www.manythings.org/corpus/about.html#info>` is also run 
by volunteers and is set to make the most bilingual sentence translations available 
between many different languages. Manythings.org compiles the data and makes it 
accessible.



`More bilingual sentence pairs <http://www.manythings.org/bilingual/>`_

.. code:: python

  # English-German Sentence Translation Data
  import requests
  import io
  from zipfile import ZipFile
  sentence_url = 'http://www.manythings.org/anki/deu-eng.zip'
  r = requests.get(sentence_url)
  z = ZipFile(io.BytesIO(r.content))
  file = z.read('deu.txt')
  # Format Data
  eng_ger_data = file.decode()
  eng_ger_data = eng_ger_data.encode('ascii',errors='ignore')
  eng_ger_data = eng_ger_data.decode().split('\n')
  eng_ger_data = [x.split('\t') for x in eng_ger_data if len(x)>=1]
  [english_sentence, german_sentence] = [list(x) for x in zip(*eng_ger_data)]
  print(len(english_sentence))
  print(len(german_sentence))
  print(eng_ger_data[10])

the output::

  147788
  147788
  ['I won!', 'Ich hab gewonnen!']
  
CIFAR-10 Data
--------------

The `CIFAR-10 data <https://www.cs.toronto.edu/~kriz/cifar.html>`_ contains 60,000 
32x32 color images of 10 classes collected by Alex Krizhevsky, Vinod Nair, and 
Geoffrey Hinton. Alex Krizhevsky maintains the page referenced here. This is such a
common dataset, that there are built in functions in TensorFlow to access this data 
(the keras wrapper has these commands). Note that the keras wrapper for these functions
automatically splits the images into a 50,000 training set and a 10,000 test set.

.. code:: python

  from PIL import Image
  # Running this command requires an internet connection and a few minutes to download all the images.
  (X_train, y_train), (X_test, y_test) = tf.contrib.keras.datasets.cifar10.load_data()

the output:: 

  Downloading data from http://www.cs.toronto.edu/~kriz/cifar-10-python.tar.gz
  The ten categories are (in order):
  
  Airplane
  Automobile
  Bird
  Car
  Deer
  Dog
  Frog
  Horse
  Ship
  Truck

.. code:: python
  
  X_train.shape
  y_train.shape
  y_train[0,] # this is a frog
  # Plot the 0-th image (a frog)
  %matplotlib inline
  img = Image.fromarray(X_train[0,:,:,:])
  plt.imshow(img)

the output::

  (50000, 32, 32, 3)
  (50000, 1)
  array([6], dtype=uint8)
  <matplotlib.image.AxesImage at 0x7ffb48a47400>
  
  
.. raw:: html

  <table>
    <tr>
        <td class="cifar-class-name">飞机</td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/airplane1.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/airplane2.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/airplane3.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/airplane4.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/airplane5.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/airplane6.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/airplane7.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/airplane8.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/airplane9.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/airplane10.png" class="cifar-sample" /></td>
    </tr>
    <tr>
        <td class="cifar-class-name">汽车</td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/automobile1.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/automobile2.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/automobile3.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/automobile4.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/automobile5.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/automobile6.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/automobile7.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/automobile8.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/automobile9.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/automobile10.png" class="cifar-sample" /></td>
    </tr>
    <tr>
        <td class="cifar-class-name">小鸟</td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/bird1.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/bird2.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/bird3.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/bird4.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/bird5.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/bird6.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/bird7.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/bird8.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/bird9.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/bird10.png" class="cifar-sample" /></td>
    </tr>
    <tr>
        <td class="cifar-class-name">猫</td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/cat1.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/cat2.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/cat3.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/cat4.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/cat5.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/cat6.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/cat7.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/cat8.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/cat9.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/cat10.png" class="cifar-sample" /></td>
    </tr>
    <tr>
        <td class="cifar-class-name">鹿</td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/deer1.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/deer2.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/deer3.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/deer4.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/deer5.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/deer6.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/deer7.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/deer8.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/deer9.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/deer10.png" class="cifar-sample" /></td>
    </tr>
    <tr>
        <td class="cifar-class-name">狗</td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/dog1.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/dog2.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/dog3.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/dog4.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/dog5.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/dog6.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/dog7.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/dog8.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/dog9.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/dog10.png" class="cifar-sample" /></td>
    </tr>
    <tr>
        <td class="cifar-class-name">青蛙</td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/frog1.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/frog2.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/frog3.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/frog4.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/frog5.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/frog6.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/frog7.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/frog8.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/frog9.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/frog10.png" class="cifar-sample" /></td>
    </tr>
    <tr>
        <td class="cifar-class-name">马</td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/horse1.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/horse2.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/horse3.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/horse4.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/horse5.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/horse6.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/horse7.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/horse8.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/horse9.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/horse10.png" class="cifar-sample" /></td>
    </tr>
    <tr>
        <td class="cifar-class-name">船</td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/ship1.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/ship2.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/ship3.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/ship4.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/ship5.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/ship6.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/ship7.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/ship8.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/ship9.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/ship10.png" class="cifar-sample" /></td>
    </tr>
    <tr>
        <td class="cifar-class-name">卡车</td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/truck1.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/truck2.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/truck3.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/truck4.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/truck5.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/truck6.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/truck7.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/truck8.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/truck9.png" class="cifar-sample" /></td>
        <td><img src="http://www.cs.toronto.edu/~kriz/cifar-10-sample/truck10.png" class="cifar-sample" /></td>
    </tr>
  </table>
