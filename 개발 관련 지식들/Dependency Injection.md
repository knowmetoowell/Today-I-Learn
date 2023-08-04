## Dependency injection (의존관계 주입)


### 의존관계란 ?
* 의존관계는 의존 대상 B가 변하면, 그것이 A에 영향을 미칠 때 A는 B와 의존관계라고 한다.
* B가 변경되었을 때 그 영향이 A에 미치는 관계를 말한다. 
```JAVA
//피자 가게의 요리사는 피자 레시피에 의존한다. 만약 피자 레시피가 변경된다면, 요리사는 피자를 새로운 방법으로 만들게 된다. 
//레시피의 변화가 요리사에 미쳤기 때문에 요리사는 레시피에 의존한다라고 할 수 있다.

public class PizzaChef{
	
	private PizzaRecipe pizzaRecipe;
	
	public PizzaChef() {
		this.pizzaRecipe = new PizzaRecipe();
	}
	
}
```
#### 위 코드의 문제점
1. DIP 위반
* 객체 지향 5원칙(SOLID) 중 "추상화(인터페이스)에 의존해야지, 구체화(구현 클래스)에 의존하면 안 된다"라는 DIP 원칙 위반 
* DIP (Dependency Inversion Principle) : 의존 관계 역전 원칙 
* PizzaChef는 PizzaRecipe 클래스를 의존하고 있음
2. 결합성이 높음
* PizzaChef가 새로운 레시피인 CheezePizzaRecipe 클래스를 이용해야 한다면 PizzaChef 클래스의 생성자를 변경해야만 한다.
* 만약 이후 레시피가 계속해서 바뀐다면 매번 생성자를 바꿔줘야 하는 등, 유연성이 떨어지게 된다.
* 근데 사실 2번 내용도 DIP와 OCP를 지키지 못해서 생기는 문제인 듯

&rarr; DI를 이용하면 해결가능~

### 의존 관계 주입(DI)이란?

* DI는 의존 관계를 외부에서 결정(주입)해주는 것을 말한다. 
* 스프링에서는 이러한 DI를 담당하는 DI 컨테이너가 존재한다. 이 DI 컨테이너가 객체들 간의 의존 관계를 주입한다.

```JAVA
public interface PizzaRecipe{ // 다양한 피자 레시피를 추상화하기 위해 PizzaRecipe를 interface를 만들자
	//~~
}

public class CheesePizzaRecipe implements PizzaRecipe{
	//~~~~
}
//----------------------------------------------------
public class PizzaChef{
	
	private PizzaRecipe pizzaRecipe;
	
	public PizzaChef(PizzaRecipe pizzaRecipe) { // DI - 생성자 주입!!
		this.pizzaRecipe = pizzaRecipe;
	}
	
}
//------------------------------------------------------------
// DI 컨테이너에서의 동작
PizzaChef = new PizzaChef(new CheesePizzaRecipe()); 

// 만약 치즈 피자 레시피에서 베이컨 피자 레시피로 바뀐다면?
PizzaChef = new PizzaChef(new BaconPizzaRecipe());

//위 코드처럼 생성자에 new로 받으면 싱글톤은 아니긴 함
```
* 비유를 들자면 피자가게 사장(스프링 DI 컨테이너)이 피자 셰프에게 특정 피자 레시피를 주입해주는 것이다. 
*  피자 셰프는 피자 레시피가 바뀌더라도 생성자를 변경하지 않아도 된다. 
* 그저 레시피가 바뀐다면 외부에서 바뀐 레시피를 주입해주기만 하면 된다.
* 비슷한 예시로 면허증이 있는 사람은 아반떼를 몰던 그랜저를 몰던 몰 수 있다. 만약 차량이 바뀔 때 마다 면허를 갱신해야 한다면 아주 힘들 것임(위 예제에서는 생성자를 계속 변경해주는 것). 자동차 회사는 자동차라는 인터페이스(추상화)에 맞추어서 차종(구현 클래쓰)을 낼 뿐임. 운전자는 추상화에 의존하고 있을 뿐 차종이 바뀌어도 운전은 할 수 있음


