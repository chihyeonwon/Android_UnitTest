# Android_UnitTest
[Android] 테스트 코드 작성해보기
[안드로이드 Test 공식 개발 문서](https://developer.android.com/training/testing/local-tests?hl=ko)
## 테스트 코드를 작성하는 이유
당연한 답변이 될 수도 있겠지만, 구현한 동작이 올바르게 동작하는지 검증하기 위해 테스트 코드를 작성하는 것이다.    
특히, 어떤 코드를 단순히 리팩토링할 때 기존과 다르게 동작할 수 있는 노릇이다.     
따라서 이런 상황을 대비하여 일일히 수동으로 테스트를 해보기 보단, 
다양한 엣지 케이스들을 고려한 테스트 케이스를 코드로써 작성해두면 편리하게 동작의 무결성을 보장할 수 있을 것이다.     

## Andriod 에서의 테스트
안드로이드는 사용자와 인터랙션하는 UI 까지 구현하기 때문에, 단순히 화면에 보여지지 않는 테스트 뿐만 아니라     
UI 인터랙션이 정상적으로 이루어지는지 검증하기 위한 UI 테스트까지 고려하게 된다.     
따라서 아래와 같이 두 가지 종류의 테스트가 존재한다.     

## Unit Test

'테스트 코드'라고 했을 때 가장 일반적으로 떠올리게 되는 테스트이다. 
말 그대로 코드를 유닛 단위로 기능을 검증하는 것이다.     
유닛 단위라 함은 메소드, 클래스 등이 될 수 있다.    

안드로이드에서는 JUnit, Mockito, PowerMock 등을 사용할 수 있다.    

## UI Test

위에서 말한 것처럼 안드로이드에서는 UI 테스트를 고려하게 된다. 
사용자와의 인터랙션을 검증하게 된다. 
사용자 인터랙션이라 함은 버튼 클릭, 테스트 입력, 스크롤 등이 될 수 있다.

Espresso, UIAutomator, Robotium, Calabash, Robolectric 등의 툴을 사용하여 테스트 코드를 작성해볼 수 있다.

## Android 프로젝트에서의 테스트
안드로이드 스튜디오에서 프로젝트를 생성하면 테스트 관련 툴이 자동으로 추가된다.     
app 단위의 build.gradle 파일의 의존성 쪽을 보면 아래와 같은 구문들이 추가되어 있는 것을 볼 수 있다.   
```kotlin
testImplementation 'junit:junit:4.+'
androidTestImplementation 'androidx.test.ext:junit:1.1.2'
androidTestImplementation 'androidx.test.espresso:espresso-core:3.3.0'
```
JUnit, Espresso 등 Unit 테스트 툴과 UI 테스트 툴이 기본적으로 탑재된다.      
      
그리고 프로젝트 디렉토리만 보더라도 테스트 관련 파일을 확인해볼 수 있다.    
     
androidTest, test 등 디렉토리 안에 테스트 파일들이 있다.      
androidTest 디렉토리 안에는 UI 테스트 관련 파일들이 있고, test 디렉토리 안에는 Unit 테스트 관련 파일들이 있다.        

JUnit 을 통한 Unit 테스트를 사용해본다.      

## JUnit 사용방법
ExampleUnitTest 파일을 보면 아래와 같은 모습이다.
```kotlin
import org.junit.Test

import org.junit.Assert.*

/**
 * Example local unit test, which will execute on the development machine (host).
 *
 * See [testing documentation](http://d.android.com/tools/testing).
 */
class ExampleUnitTest {
    @Test
    fun addition_isCorrect() {
        assertEquals(4, 2 + 2)
    }
}
```

@Test 라는 어노테이션과 함께 덧셈 연산을 테스트하는 메소드가 있다. 
이 메소드 안에서는 assertEquals() 라는 메소드를 통해 4와 2 + 2 를 비교하는듯한 모습을 보이고 있는데,
assertEquals() 의 모양을 살펴보자.
```kotlin
/**
 * Asserts that two longs are equal. If they are not, an
 * {@link AssertionError} is thrown.
 *
 * @param expected expected long value.
 * @param actual actual long value
 */
public static void assertEquals(long expected, long actual) {
    assertEquals(null, expected, actual);
}
```
expected 와 actual 파라미터 두 개를 비교하여 같은 값을 갖고 있는 지에 대해 검사하는 동작을 한다.
```
Assert 는 '단언하다, 주장하다' 라는 뜻을 갖고 있다.
실생활에서 '주장'이라 함은 올바르고 틀린 것을 따지기보단 단지 자신의 의견을 표출하는 것 뿐이다.
마찬가지로 테스트 코드에 있어 Assert 도 일단 실행을 해보고, 이것이 올바르게 동작했다면 통과하고
만약 올바르게 동작하지 않았다면 AssertionError 를 쓰로잉하게 된다.
```

아까 살펴봤던 기본 예제 코드에서 '4' 와 '2 + 2' 를 비교했는데, 이 두 값은 같으므로 Unit 테스트를 통과하게 되는 것이다.     

그럼 이 테스트는 어떻게 실행하는 것일까?       

테스트 코드는 이렇게 실행할 수 있다. 테스트 파일을 우클릭해보면 다음과 같은 메뉴가 있는데,       
이를 실행하면 된다. (혹은 적혀있는 단축키를 눌러도 된다)    

![image](https://github.com/chihyeonwon/Android_UnitTest/assets/58906858/646f3f5f-e0e1-4e99-a58e-78bbc60e977d)

그렇게 되면 아래와 같이 테스트 성공이라는 표시가 뜬다.     
  
![image](https://github.com/chihyeonwon/Android_UnitTest/assets/58906858/fde68737-fc69-4768-b864-b79022d7f19f)

만약 테스트 코드를 다음과 같이 작성했다고 해보자.       
이는 기능이 제대로 동작되지 않는 상황을 가정한 것이다. 4 라는 값을 기대하는 유닛 테스트인데, 연산 결과가 5인 상황이다.      
```kotlin
class ExampleUnitTest {
    @Test
    fun addition_isCorrect() {
        assertEquals(4, 2 + 3)
    }
}
```
![image](https://github.com/chihyeonwon/Android_UnitTest/assets/58906858/d5fd38ac-7fe4-44ef-8c5e-7290655afce5)

'Tests Failed' 라고 뜨며 어떤 테스트 케이스가 통과되지 못했는지 (실패했는지) 에 대한 정보를 제공해준다.       
이런 식으로 테스트가 실패한 동작에 대해 디버깅을 수행하면 된다.    

유닛 테스트 코드를 작성하면 (귀찮지만) 다양한 테스트 케이스들을 고려해볼 수 있기 떄문에 기능 추가, 변경, 혹은 코드 리팩토링에 있어 강력한 이점을 지닌다.      
다른 기능에 비해 특히나 동작의 무결성이 보장되어야 하는 부분이라면, 테스트 코드의 중요성은 더욱 높아진다.     

## 직접 Unit 테스트 코드 작성해보기


실제 사례(?)를 바탕으로 연습해봤다.
간단하게 계산기 클래스를 만들어 덧셈, 뺄셈이 정상적으로 이루어지는지에 대해 검증해보았다.

클래스를 아래와 같이 만들어준다.
```kotlin
class Calculator {

    fun add(a: Int, b: Int): Int {
        return a + b
    }
    
    fun sub(a: Int, b: Int): Int {
        return a - b
    }

}
```

그리고 아까 ExampleUnitTest 파일이 있던 경로에 계산기 기능 검증 테스트를 위한 CalculatorTest 라는 테스트 파일을 만들어준다.     
그리고 아래와 같이 코드를 작성해볼 수 있다.     

```kotlin
import org.junit.Test
import org.junit.Assert.assertEquals
import org.junit.Before

class CalculatorTest {
    private var calculator: Calculator? = null

    @Before
    fun setUp() {
        calculator = Calculator()
    }

    @Test
    fun 기본_덧셈() {
        val result = calculator?.add(5, 6)
        assertEquals(11, result)
    }

    @Test
    fun 기본_뺄셈() {
        val result = calculator?.sub(10, 5)
        assertEquals(5, result)
    }

}
```
몇 가지 어노테이션에 대해 설명하자면 다음과 같다.
- @Before    
테스트 시작전 실행되어야 할 동작 (객체 초기화 등)     
- @Test   
@Before 이후 돌아가는 테스트 케이스 코드     
그리고 위 코드에선 그닥 의미가 없지만 이런식으로 엣지 케이스들도 고려해줄 수 있겠다.
```kotlin
import org.junit.Test
import org.junit.Assert.assertEquals
import org.junit.Before

class CalculatorTest {
    private var calculator: Calculator? = null

    @Before
    fun setUp() {
        calculator = Calculator()
    }

    @Test
    fun 기본_덧셈() {
        val result = calculator?.add(5, 6)
        assertEquals(11, result)
    }

    @Test
    fun 음수끼리_덧셈() {
        val result = calculator?.add(-5, -4)
        assertEquals(-9, result)
    }

    @Test
    fun 기본_뺄셈() {
        val result = calculator?.sub(10, 5)
        assertEquals(5, result)
    }

    @Test
    fun 음수끼리_뺄셈() {
        val result = calculator?.sub(-5, -4)
        assertEquals(-1, result)
    }

}
```







