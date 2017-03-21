
<script type="text/javascript"  src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script> 


# tensor. 
3차원 공간에 있어서 9개의 성분을 가지며, 좌표 변환에 의해 좌표 성분의 곱과 같은 형의 변환을 받는 양. 예를 들면, 물체의 관성 모멘트나 변형은 이것으로 표시됨.

array 

type
rank : 몇 차원 array?  n-dimensional
shape: ex) 3 x 3 


# tensorflow 

1. build computational graph 
  - consist of nodes. 
2. make a session   
3. run (feed data)


- placehoder 

  ```
  a = tf.placeholder(tf.float32)
  b = tf.placeholder(tf.float32)
  c = a + b 
  session.run(c, feed_dict={ a:[1,3], b:[2,4] })

  ```

  `tf.placeholder(tf.float32)`
  sess.run(node, feed_dict={ a:[1,3], b:[2,4] })


![](2017-03-20-10-34-43.png)


---

# Linear Regression. Why linear? 
data  -> training -> regression model 을 만든다. 
model을 통해 학습된 data를 기반해 input에 대한 예측을 할 수 있다. 

|x|y|
|--|--|
|1|1|
|2|2|
|3|3|

Hypothesis 

-> linear하다고 가정 하기 때문에 
매우 많은 현상이 linear 모델로 에측할 수 있다. 

`H(x) = Wx + b` linear한 model의 가설을 세움. 

가설과 실제 data의 차이를 계산하여 어떤 model이 가장 적합한가를 찾을 수 있다. 

cost(loss) function 

`(H(x) - y)^2`

cost function을 최소화 하는것이 목표. 


# cost function은 어떻게 생겼을까?

![](2017-03-20-10-57-33.png)

제곱 항이 있기 때문에 minus가 없다. 따라서 0의 값을 갖는 것이 최소값이다. 


# Gradient Descent Alogrithm
경사도를 판단하여 

learning rate. 

convex function => 밥그릇 모양. 인 경우 GDA를 적용하면 답을 얻을 수 있다. 
![](2017-03-20-11-07-26.png)

단점. 아닌 경우.. 답을 찾을 수 없다. 

![](2017-03-20-11-06-31.png)


* 4. multiple variables. 

matrix를 사용할 것이다. 

* shape

matrix를 아래와 같이 생각 할 수 있다. 

a X b

a -> num of samples. 

b -> num of features / variables. 


* tensorflow matrix raw none => n개. 



# Loading Data from file 

* numpy 
  ```
  xy = np.loadtxt('*.csv', delimiter=',', dtype=np.float32)
  x_data = xy[:, 0:-1] # numpy slicing
  y_data = xy[:, [-1]]
  # need to check its shape! 
  ```

* tensorflow : Queue Runners (매우 큰 파일)

  ```
  # 1
  filename_queue = tf.train.string_input_producer([.....])
  # 2
  reader = tf.TexLineReader()
  key, value = reader.read(filename_queue)
  # 3
  record_defaults = []
  xy = tf.decode_csv(value, record_default = record_default)



  ```


# Classification

* 학습 시간에 따른 시험 합격 여부.

* classfication에서 linear regression.

classfication에서는 0 혹은 1의 값을 가져야 하지만, linear regression의 경우 실수의 값이 나오게 된다. 

* Logistic Hypothesis. 
  
  * sigmoid function, logistic function 
  *  `g(z) = 1 / (1 + e^-z)`


* cost function

  non linear 이기 때문에 local minimum이 존재하게 되어 G.D 로는 찾을 수 없다. 

  따라서 새로운 cost함수가 필요하다. 

  cost(W) 
  y = 1일때, y = 0일 때 구분해서 

  exp term이 존재하기 때문에 log함수를 사용한다. 

  


\\({e}^{i\pi}+1=0\\)
