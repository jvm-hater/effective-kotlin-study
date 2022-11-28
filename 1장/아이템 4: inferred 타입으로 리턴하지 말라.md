- 타입 추론을 사용할 때 주의할 점이 있다.
    - inferred 타입은 정확하게 오른쪽에 있는 피연산자에 맞게 설정된다.
    - 절대로 슈퍼 클래스 또는 인터페이스로는 설정되지 않는다.

```kotlin
open class Animal
class Zebra Animal()

fun main() {
    var animal1 = Zebra()
    animal1 = Animal() // 오류: Type mismatch

    var animal2: Animal = Zebra()
    animal2 = Animal()
}
```

- 우리가 코드를 조작할 수 있다면 타입을 명시적으로 지정해서 타입 관련 예외를 해결할 수 있다.
- 하지만 외부 라이브러리를 사용할 때, 이 라이브러리가 inferred 타입을 노출할 경우 타입과 관련된 문제가 발생할 수 있다.

```kotlin
interface CarFactory {
    fun produce(): Car
}

val DEFAULT_CAR: Car = Fiat126P()
```

- 처음에는 위와 같이 코드를 작성하였으나, 나중에 보니 DEFAULT_CAR는 Car로 명시적으로 지정되어 있으므로 따로 필요 없다고 판단하여 함수의 리턴 타입을 제거할 수 있다.

```kotlin
interface CarFactory {
    fun produce() = DEFAULT_CAR
}
```

- 이후에 다른 개발자가 코드를 보다가, DEFAULT_CAR는 타입 추론에 의해 자동으로 타입이 지정될 것이므로 DEFAULT_CAR의 Car 타입을 제거할 수 있다.

```kotlin
val DEFAULT_CAR = Fiat126P() // Car가 아닌 Fiat126 타입으로 추론됨.
```

- 이제 CarFactory에서는 Fiat126P 이외의 자동차를 생산하지 못하게 된다.
- 만약 우리가 인터페이스를 직접 만들었다면, 문제를 찾아서 해결할 수 있지만 외부 API이므로 개발자에게 문의를 하는 수 밖에 없다.
- 따라서 외부 API를 만들 때는 반드시 타입을 지정하고, 이렇게 지정한 타입을 특별한 이유와 확실한 확인 없이는 제거하지 않는 것이 좋다.
- inferred 타입은 프로젝트가 진전될 때, 제한이 많아지거나 예상치 못한 결과를 낼 수 있음을 기억해야 한다.

## 출처

- 이펙티브 자바
