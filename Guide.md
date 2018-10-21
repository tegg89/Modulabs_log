# SafeAI Lab 
## Prerequisite
앞으로 SafeAI 연구소에서 다같이 프로젝트를 진행하기 위해 알아야 할 내용들의 목록이며,
시간나실 때 틈틈히 공부하시면 됩니다! ^^  

- **Markdown syntax**  
    마크다운은 쉬운 문법과 뛰어난 이식성으로 정말 어디서나 쓰이고 있습니다. 아래 마크다운 튜토리얼들 중 하나를 정독하시거나  
    따로 공부하시면서, 몇가지 중요한 문법들 e.g.) 코드 블록, Headings, 링크 표시.. 등의 몇가지만 숙지하시면 됩니다!
    - [Markdown tutorial (kr)](https://gist.github.com/ihoneymon/652be052a0727ad59601)
    - [Interactive Markdown tutorial](https://www.markdowntutorial.com/)
    - [Markdown cheetsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)  

- **Python module import statement**  
    파이썬 프로젝트의 크기가 커질수록 프로젝트 내부 모듈들의 구성이 중요해집니다.  
    import 구문이 어떻게 내 프로젝트 안에 있는 모듈 또는 그 안에 있는 함수들을 찾아가는지 기본적인 동작 방식을 숙지합니다.  
    아래는 import문의 간단한 예시입니다:
    ```python
    # Directory tree:
    - mymodule.py
    - main.py
    - spackage/
        - __init__.py
        - submodule.py
    
    # In submodule.py
    def sayHi():
        print("HELLO!")
    
    # main.py
    import mymodule
    from spackage import submodule
    submodule.sayHi() 
    ```
    ```python
    # Other examples:
    from package import mymodule
    
    # SafeAI, for example
    from safeai.models import ODIN
    from safeai.models import Lee18
    from safeai.metrics import auroc
    ```
    - [Python module tutorial (kr)](https://wikidocs.net/1074)
    - [Learnpython.org - Modules and packages](https://www.learnpython.org/en/Modules_and_Packages)
    - [Guide to 'import' statements](https://chrisyeh96.github.io/2017/08/08/definitive-guide-python-imports)
    - [Python module-package tutorial (recommended)](https://realpython.com/python-modules-packages/)
    - [Official python docs - The import system](tutoria://docs.python.org/3/reference/import)  

- **tensorflow.contrib package structure**  
    Tensorflow/contrib subpackage에는, tensorflow에서 공식적으로 지원하기 전의 experimental code들을 미리 사용해볼 수 있는 모듈들이 있습니다.
    잘 알려진 패키지로 tf.contrib.{slim, gan, rnn, layers} 등이 있는데요, 이 코드들을 벤치마킹해서 '쉽게 가져다 쓸 수 있는' 코드를 작성하는 방법을
    앞으로도 계속 배울 예정입니다.  
    (미리 조금 봐놓아도 좋겠죠? ^^)  
    - [Github: tensorflow.contrib](https://github.com/tensorflow/tensorflow/tree/master/tensorflow/contrib)  
    - [Docs: tensorflow.contrib](https://www.tensorflow.org/api_docs/python/tf/contrib)  

- **Deploying our package to PyPI, then install with pip**  
    내가 만든 패키지를 배포하는 과정입니다. 자세히 아셔도 좋지만, PyPI라는게 있고, pip으로 설치할 수 있다는  
    정도만 알고 있으면 충분합니다.
    - [Packaging python projects](https://packaging.python.org/tutorials/packaging-projects/)
    - [Python-packaging](https://python-packaging.readthedocs.io/en/latest/)  

- **Test code**  
    딥러닝 모델의 테스트는 어떤 방식으로 진행하는지, 테스팅 코드를 짜는 방법을 contrib 패키지 안에 구현되어 있는  
    테스트 코드들을 보고 공부합니다. 아래는 tensorflow.contrib.slim 패키지 안에 있는 vgg16 구현체의 테스트 코드입니다.
    - [tf.contrib.slim.vgg16: vgg_test.py](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/contrib/slim/python/slim/nets/vgg_test.py)  

- **Python style guide - PEP8, pylint, (flask8)**  
    좋은 코드란 화려한 코드가 아닌 읽기 쉽고, 단순하고, 이해하기 쉬운 코드입니다.  
    변수 네이밍부터 시작해 별 것 아닌것 같아 보였던 공백의 개수까지 하나 하나 신경써주어야 하는데요,  
    이를 위해 잘 알려진 파이썬 스타일 가이드라인을 준수하도록 노력해야 합니다. tensorflow는 파이썬 코드에 대해 [Google Python Style Guide](https://github.com/google/styleguide/blob/gh-pages/pyguide.md)를 따릅니다.
    또 코딩 스타일 가이드 툴 중 하나인 Pylint를 사용하는 방법 또한 배웁니다.
    - [Pylint tutorial](http://pylint.pycqa.org/en/1.9/tutorial.html)  

- **Git basic usage**  
    SafeAI Lab은 [Github.com/EpisysScience](https://github.com/EpisysScience)에서 같이 코드를 작성하게 됩니다. VCS의 개념, Git에 대한 기본적인 사용법(Pull request, commit, push ..)을 
    알고 계시면 협업을 진행하는데 어려움이 없겠죠? ^ㅡ^
    - [Git basic tutorial](https://guides.github.com/activities/hello-world/)  
 
