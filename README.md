## Python decorator (데코레이터)

 Python 으로 작성된 Opensource 의 코드들을 보다 보면, 아래와 같이 @ 로 시작하는 구문 들을 볼 수 있다. 
<pre><code>
@decorator_
def function():
    print "what is decorator?"

</code></pre>
 decorator를 한마디로 얘기하자면, 대상 함수를 wrapping 하고, 이 wrapping 된 함수의 앞뒤에 추가적으로 꾸며질 구문 들을 정의해서 손쉽게 재사용 가능하게 해주는 것이다. (무슨 말이야?)
 


 Decorator는 어떤 경우에 쓰는건가?


코딩을 하다 보면 종종 이런 경우가 있다. 

 메인 구문이 있고, 여기에  부가적인 구문을 추가하고 싶을때 말이다. 그리고 이 부가적인 구문을 반복해서 사용하고 싶은 경우도 있다. 이때 부가적인(그리고 반복적인) 작업을 decorator 로 선언해서 자유롭게 사용이 가능하다는 것이다. 
 
처음엔 이해가 잘 안가지만, 막상 사용하다 보면 굉장히 쉽다는 것을 느낄 수 있다. 


예를 들어 아래와 같은 함수가 있다고 생각해보자. 

<pre><code>
def main_function():
     print "MAIN FUNCTION START"
</code></pre>
 "MAIN FUNCTION START" 라는 문장을 출력하는, 매우 간단한 함수이다. 

 만약, 이 함수에 추가적인 작업들을 더 넣고 싶다면?
 예를 들어, 해당 문장을 출력하기 전과 후에 날짜와 시간을 출력하고 싶다면?

 간단히 생각하면 아래와 같이 하면 될것이다. 
<pre><code>
import datetime

def main_function():
     print datetime.datetime.now()
     print "MAIN FUNCTION START"
     print datetime.datetime.now()
</code></pre>
이 예제만 놓고 보면, 매우 간단한 함수이고 특별히 복잡하게 추가할게 없는 작업이기 때문에, 이렇게 해도 큰 무리는 없다. 
그런데 만약 이와 같은 패턴의 함수가 여러번 있다면 어떨까?
<pre><code>
def main_function_1():
     print "MAIN FUNCTION 1 START"

def main_function_2():
     print "MAIN FUNCTION 2 START"

def main_function_3():
     print "MAIN FUNCTION 3 START"
</code></pre>
..... X 100번..

그리고 여기에도 각 함수의 문장이 출력되기 전과 후에 시간을 출력하고 싶다면?
<pre><code>
import datetime

def main_function_1():
     print datetime.datetime.now()
     print "MAIN FUNCTION 1 START"
     print datetime.datetime.now()

def main_function_2():
     print datetime.datetime.now()
     print "MAIN FUNCTION 2 START"
     print datetime.datetime.now()

def main_function_3():
     print datetime.datetime.now()
     print "MAIN FUNCTION 3 START"
     print datetime.datetime.now()
</code></pre>
.... X 100번


 반복되는 구문이 많아지다 보니 점점 소스가 지저분해지며, 원래 main 함수의 가독성도 떨어진다. (코드가 변태적으로 변함을 볼 수 있다.) 지금 예문은 메인과 추가 구문이 한 줄밖에 안되기 때문에 매우 간단하지만, 실제 코딩할때의 상황을 생각해보면 문제는 더욱 심각해 진다. 

이럴 경우에 decorator 구문을 사용해 보자. 



decorator를 사용해 보자

아래의 예제는 위의 변태 코드에 decorator를 적용한 것이다.
<pre><code>
import datetime

def datetime_decorator(func):
        def decorated():
                print datetime.datetime.now()
                func()
                print datetime.datetime.now()
        return decorated

@datetime_decorator
def main_function_1():
        print "MAIN FUNCTION 1 START"

@datetime_decorator
def main_function_2():
        print "MAIN FUNCTION 2 START"

@datetime_decorator
def main_function_3():
        print "MAIN FUNCTION 3 START"
        
</code></pre>
..... X 100번

decorator 함수를 재사용 함으로써, main 함수에 대한 가독성과 직관성이 훨씬 좋아진 것을 볼 수 있다. 그리고 같은 패턴을 여러번 사용하더라고 간단히 @를 붙이면 끝이므로 사용도 간편하다. 

decorator 선언된 부분을 자세히 설명하면,

* 먼저 decorator 역할을 하는 함수를 정의하고, 이 함수에서 decorator가 적용될 함수를 인자로 받는다. python 은 함수의 인자로 다른 함수를 받을 수 있다는 특징을 이용하는 것이다. 
* decorator 역할을 하는 함수 내부에 또 한번 함수를 선언(nested function)하여 여기에 추가적인 작업(시간 출력) 을 선언해 주는 것이다. 
* nested 함수를 return 해주면 된다. 

 마지막으로, main 함수들의 앞에 @를 붙여 decorator 역할을 하는 함수를 호출해 준다. 그러면 끝-
 그렇다. 생각보다 어렵지 않다. 

 노파심에 이야기하면, decorator가 꾸며주는 기능이라고 해서 대상 함수의 수행 중간에 끼어드는 구문은 할 수 없다.  decorator는 원래 작업의 앞 뒤에 추가적인 작업을 손쉽게 사용 가능하도록 도와주는 역할이라는 것이다.  

 위 예제는 함수를 이용한 decorator를 구현한 것이고, class 형태로도 구현이 가능하다. 




class 형태로 decorator를 사용해 보자.


 decorator를 class로 사용하고 싶다면 아래와 같이 __call__ 함수로 decorator 형식을 정의해 주면된다. 
 class의 __call__ 함수로 정의해주는게 nested 함수 형식으로 정의한 것 보다 더 깔끔해 보인다.
 
<pre><code>
import datetime

class DatetimeDecorator:
        def __init__(self, f):
                self.func = f

        def __call__(self, *args, **kwargs):
                print datetime.datetime.now()
                self.func(*args, **kwargs)
                print datetime.datetime.now()

class MainClass:
        @DatetimeDecorator
        def main_function_1():
                print "MAIN FUNCTION 1 START"

        @DatetimeDecorator
        def main_function_2():
                print "MAIN FUNCTION 2 START"

        @DatetimeDecorator
        def main_function_3():
                print "MAIN FUNCTION 3 START"


my = MainClass()
my.main_function_1()
my.main_function_2()
my.main_function_3()
</code></pre>
