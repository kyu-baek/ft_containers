

# STL 학습 전 알아야 하는 C++ 문법

* 연산자 오버로딩 
* 함수 객체 
* 템플릿
* rebind
* friend


	* 연산자 오버로딩
		* 설명

			연산자 오버로딩은 사용자 클래스 타입에 C++ 연산자를 사용할 수 있게 하는 문법.	
			컴파일러는 기본적인 연산 로직이 정의되어 있어서 
			``` 
			int n1 = 1, n2 = 2;
			std::cout << n1 + n2 << std::endl;
			``` 
			을 했을 때 1 + 2 이 잘 연산되어 화면에 3을 만나볼 수 있다.
			
			하지만 사용자가 직접 정의한 클래스의 연산은 불가능하기 때문에,
			해당 커스터마이징 클래스를 만든 사용자가 직접
			단항 연산자(&, +, -, ++, --), 이항 연산자(!=, ==, <, <=)  등등..
			연산이 잘 동작하는 operator 를 정의해서 사용해주어야 한다.
			STL 종류에 따라 필요한 주요 연산자들은 조금씩 다를 수 있다. 
		

	* 함수 객체
		* 설명

			함수 객체는 함수처럼 호출 가능한 클래스 객체이다. 
			함수 객체는 함수처럼 사용할 수 있으면서도 상태를 가질 수 있기 때문에,
			STL은 함수보다 함수 객체를 선호한다. 
			함수 객체는 함수처럼 동작하기 때문에 이름 뒤에 () 가 붙고, 
			그러므로 '()' 연산자를 오버로딩한 객체라고 할 수 있다.
			함수 객체는 함수자, 혹은 Functor 라고 불린다.
			``` 

				void print() 
				{ std::cout << "일반 전역 함수"; }

				struct Functor
				{
					void operator() () 
					{ std::cout << "함수 객체"; }
				};


				int main ()
				{
					Functor functor;

					print();         
					functor();

					return 0;
				}
			``` 
		* 함수 객체를 이점
			1. 함수 객체는 다른 멤버 변수를 가질 수 있고, 일반 함수에서 하지 못하는 것도 지원받는다.
			2. 또한 함수 객체의 서명이 같더라도 객체 타입이 다르면 서로 전혀 다른 타입으로 인식한다.
			3. 속도도 일반 함수보다 함수 객체가 더 빠르고, 
			4. 함수 주소를 전당하여 콜백하는 경우 일반 함수 포인터는 인라인 될 수 없지만 함수 객체는 인라인 될 수 있다.
			5. 최적화 하기 좋다. 


	* 템플릿
		* 설명

			템플릿은 STL 제너릭 프로그램의 핵심이라고 할 수 있다.
			템플릿은 컴파일 타임에 클라이언트가 여러 타입의 함수나 클래스를 쉽게 생성하게 한다.
			template<typename T> 를 위에 적어 놓아야 템플릿이 된다.

			템플릿은 함수 템플릿(Function Template)과 클래스 템플릿(class Template) 으로 나뉜다.
			- 함수 템플릿은 여러 함수를 만들어내는 함수의 틀
			- 클래스 템플릿은 여러 클래스를 만들어내는 클래스의 틀

			템플릿을 호출할 때는, 템플릿에서 자동으로 매개변수 안의 타입으로 함수나 클래스를 만들어주기도 하고,
			또는 사용자가 직접 명시적으로 타입을 지정해서 호출할 수도 있다.
			만약 print(); 라는 템플릿 함수 가 있다면,
			print(1); 은 int 형 정수를 넣어서 템플릿 함수를 호출하는 것은 일반적인 상황이고,
			print<int>(1);  은 명시적인 호출 상황이라고 할 수 있다.



	* struct, class 팁
		* 설명

			우리가 c에서 자주 사용한 구조체인 struct와 
			c++ 객체지향언어로 넘어오면서 만나게되는 class는 본질적으로는 같다고 할 수 있다.
			유일한 차이는 멤버 접슨 한정자를 지정하지 않았을 때 
			struct는 public이 기본 속정이 되고,
			class 는 private이 기본 속성이 된다.

	* typename, class 팁
		* 설명

			템플릿의 타입 매개변수를 선언할 때는 class 와 typename 의 뜻이 완전히 같다.
			하지만 c++ 17 이전의 상황에서 typename, class 두가지를 혼용해서 사용할 때
			주의해야 할 점이 있다. (c++ 20부터는 호환성이 좋아서 두가지를 구분할 필요가 없어짐.)

			1.  dependent type의 경우에는 typename만 사용한다.
					다른 template 파라미터와 관련된 타입을 쓰는 경우, 
					typename 키워드만 쓸 수 있습니다.

			2.  template template을 설정하는 경우 class만 사용한다.
					template template은 꼭 아래와 같이 class키워드를 써야 한다
					template < template < typename, typename > class Container, typename Type >

			3.	template을 인스턴스화할때는 다음과 같이 class만 쓸 수 있습니다. template class Foo<int>;

			4.	에시로 C::const_iterator 와 같이 중첩 의존이 있을 경우 앞에 typename 키워드를 붙인다.

	* rebind
		* 설명
			예를 들어 우리가 
			``` 
			map<int, int> m;
			``` 
			이렇게 두개의 타입을 넣어서 m 이라는 이름의 맵 객체를 하나 만든다고 하자.
			하지만 실제 map의 템플릿에 선언되어 있는 타입은 총 네개이다.
			``` 
			  template <typename _Key, typename _T, typename _Compare = std::less<_Key>, typename _Alloc = std::allocator<ft::pair<const _Key, _T> > >
			``` 
			우리가 메모리에 새로 만들어주는 m은 단순히 int 형 두개가 아니라 다른 여러 정보를 가진 구조체를 저장하는 컨테이너이다. 그래서 과제에서 사용하는 std::allocator 안에는 
			rebind 라는 중첩 구조체 템플릿을 가지고 있고, rebind 이용해서
			해탕 컨테이너에서 요구되는 진정한 할당 타입을 찾게 된다.
			``` 
			template<typename T>
			class StlCustomAllocator : public allocator<T>
			{
				(...)
			
				template<typename _Other>
				struct rebind
				{
					typedef StlCustomAllocator<_Other> other;
				};
			
				(...)
			};
			``` 
			대략적인 allocator 내부의 rebind 구조체는 이렇다. 
			내부에 other 가 있고,  이 other 가 진짜 할당 타입을 저장하게 되는 것이다. 우리가 각 컨테이너에 새로운 원소를 할당할 때 이 rebind other 에 저장된 크기를 불러와 힙공간에 할당하게 된다.


	* friend
		* 설명
			firend 클래스는 friend로 선언된 다른 클래스의 private 및 protected 멤버에 접근할 수 있다. 특정 상황에서 클래스 내에 접근하지 못하도록 private 제한을 두었는데, 필요의 경우 해당 클래스나 함수에서접근 가능하도록 사용하는것이 friend 클래스 및 함수입니다.







#	STL 원형 탐색시 주의해야할 4대장

- SFINAE
- 템플릿 메타
- type_traits
- enable_if


	> 이 4대장을 이해할 수 있어야 ft_containers 과제 수행 중 어려움이 덜하다.  특히 각종 traits, enable_if, is_integral 관련 템플릿, 클래스들을 구현하고, 
		이후 vector 에서 InputIterator 타입에 관련한 예외처리를 해줄 때 사용됨으로 개념을 숙지하고 있어야 한다.


	* SFINAE (Substitution Failure Is Not An Error!)
		* 설명

			간단하게는 C++에서 템플릿 매개변수에 자료형이나 값을 넣을 수 없어도 오류가 발생하지 않는 상황을 말한다.
			외우기 어려우면 '스피내' 라고 부르자..? 나루호도..
			함수 템플릿을 사용 시 _T 의 타입이 결정되고 함수를 생성할 때 혹은 호출할 때,
			리턴 타입이나 함수 인자 등에서 치환에 실패하면 컴파일 에러가 아니라 함수 후보에서 제외하는 것으로 넘어간다.
			이후 동일한 이름의 함수가 있다면 다른 함수를 사용하게된다.

			"This feature is used in template metaprogramming."
			위의 문장은 cppreference 의 SFINAE 페이지에 기술되어 있다.
			C++ 에서는 SFINAE 의 이러한 규칙성을 이용해 
			템플릿 메타프로그래밍(?)을 할 수 있는 것으로 보인다.



	* 템플릿 메타 프로그래밍 (Template Meta Programming - TMP)
		* 설명

			타입은 반드시 컴파일 타임에 확정되어야 한다.
			컴파일 타임에 모든 연산이 끝나게 된다.
			이렇게 타입을 가지고 컴파일 타임에 생성되는 
			코드로 프로그래밍을 하는 것을 메타 프로그래밍 이라고 한다.. 
			C++ 의 경우 템플릿을 가지고 이러한 작업을 하기 때문에 템플릿 메타 프로그래밍, 
			줄여서 TMP 라고 부른다.

		
		* TMP 를 사용하는 이유?

			템플릿 메타 프로그래밍으로 작성된 코드는 모두 컴파일 타임에 모든 연산이 끝난다.
			때문에 프로그램 실행 속도를 향상 시킬 수 있다는 장점이 있다.
			하지만 컴파일 시간은 엄청 늘어나게 된다.
			때문에 속도가 매우 중요한 프로그램의 경우 TMP 를 통해서 런타임 속도도 향상 시킬 수 있다.

	* type_traits
		* 설명

			템플릿 클래스나 함수에서 여러 타입에 대해 각기 다르게 처리를 해 줘야 하는 상황이 발생했을 때
			컴파일 타임에 타입들을 구분하기 위해 사용하는 것이 특성정보 클래스이다. 
			type_traits 도 이 특성정보 클래스 중 하나이고, iterator_traits, char_traits 등등 다양하다. 

			과제에서는 is_integral 와 iterators_traits 를 직접 구현하라고 되어 있다.
			is_integral 는 <type_traits> 헤더 안에 있는 여러 클래스 중 한 종류이고,
			이 과제에서 type_traits 안에 있는 전부를 구현할 필요는 없다. 
			직접 구현하지는 않지만 <type_traits> 안에는 정말 다양한 클래스가 있다.

			``` 
			is_void;
			is_null_pointer; 
			is_integral;        <- 우리는 이것만 만든다. 굿~
			is_floating_point;
			is_array;
			is_pointer;
			is_lvalue_reference;
			is_rvalue_reference;
			is_member_object_pointer;
			is_member_function_pointer;
			is_enum;
			is_union;
			is_class;
			등등.....
			``` 

			밑에서 설명할 enable_if 도 <type_traits> 헤더 안에 포함 되어있는 클래스 중 하나.
			왜냐 enable_if를 사용할 때 이 type_traits 이 반환해주는 true 혹은 false 값으로 
			참, 거짓을 판별하는 로직이 흔하게 쓰이기 때문.



	* enable_if
		* 설명

			enable_if 는 <type_traits> 헤더에 포함되어 있고 c++ 11 버전부터 가능하다.
			enable_if 의 원형.
			``` 
				template <bool, typename = void>				
				struct enable_if {};							
																
				template <typename T>							
				struct enable_if<true, T> { typedef T type; };	
			``` 
			첫 번째 인자로 받아오는 bool 형 조건이 false인 경우 아무런 타입을 제공하지 않음으로써, 
			컴파일 타임 에러를 유발할 수도 있고, 필요에 따라서 if 문으로서의 역할을 한다.
			


		* enable_if 를 사용하는 이유?

			그렇다면 그냥 if 문을 사용하지 왜 enable_if 를 사용하는가?
			나도 의문이었다. 하지만 enable_if 쓰임새를 잘 보면, 이것은 컴.파.일 단계에서부터
			어떤 조건을 걸러주는 일을 할 수 있다는 게 뽀인뜨 라고 할 수 있다. 
			``` 
			template<typename T, std::enable_if<std::is_integral<T>::value, int>::type = 0>
			void foo(const T& bar) { isInt(); }
			``` 
			위의 예시에서는 T 타입이 인티저인지 확인하고 거기에서 나오는 true 혹은 false 를 통해
			오버로딩 시 이 함수를 부를지 아님 대체 될지 결정되게 될텐데, 이럴때는 if문처럼 런타임용
			조건식을 사용할 수 없으므로 아주 유용하다고 할 수 있다. 
			C++의 유사한 기능으로 static_assert를 이용하는 방법도 있다.
		
		* 주의 할 점

			1.	타입에 대한 과부하가 있는 경우, 과부하 해상도에서 템플릿보다 비템플릿이 선호되기 때문에 비템플릿이 우선적으로 선택될 때도 있다.
			2.	템플릿의 두번째 인자는 =0 으로 디폴트 되어 있다. 그래서 두 번재 인자 없이도 호출 가능하다.
	


# STL 의 개념

* 설명
	STL은 표준 C++ 라이브러리의 일부분이며 Standard Template Library의 약자다.
	STL은 프로그램에 필요한 자료구조와 알고리즘을 템플릿 형식으로 제공한다.
	STL의 자료구조와 알고리즘은 반복자(iterator) 라는 구성 요소를 통해 연결한다.

* STL 구성 요소

	** 한줄요약 **

	- 컨테이너(Container)			 : 객체를 저장하는 자료구조
	- 반복자(iterator)				: 포인터와 비슷한 개념으로 컨테이너의 원소를 가리키고, 가리키는 원소에 접근하여 다음 원소를 가리키게 하는 기능을 한다.
	- 알고리즘(Algorthm)			 : 정렬, 삭제, 검색, 연산 등을 해결하는 함수 템플릿
	- 함수 객체(Function Object) 	 : 위의 설명 참고
	- 어댑터(Adaptor)				: 원래 구성되어 있는 요소의 인터페이스를 변경해 새로운 인터페이스를 갖는 것
	- 메모리 할당기(Allocator)		  : 컨테이너의 메모리 할당기로 malloc, new 연산을 하지 않고도 heap 영역의 메모리 할당 가능하며, 매우 유용하다.
												컨테이너는 자신만의 기본 Allocator를 가진다.

* Container 종류

	- 시퀀스 컨테이너 (sequence container)
		vector, list, deque
		@컨테이너 원소가 자신만의 삽입 위치(순서) 를 가지는 선형적 컨테이너.

	- 연관 컨테이너 (associative container)
		set, multiset, map, multimap
		@저장 원소가 삽입 순서와 다르게 특정 정렬 기준에 의해 자동 정렬되는 비선형적 컨테이너.

	- 연속 메모리 기반 (contiquous-memory container)
		vector, string, deque
		@데이터 여러개가 하나의 메모리 단위에 저장되는 배열기반 컨테이너.

	- 노드기 반 컨테이너 (node-based container)
		list, set, multiset, map, multimap
		@데이터 하나를 하나의 메모리 단위에 저장
	
	- 어댑터 컨테이너 (adapter container)
		stack, queue, priority_queue
		@간결함과 명료성을 위해 인터페이스를 제한한 시퀀스나 연관 컨테이너의 변형. 	
		각각의 '기반'이 되는 클래스의 인터페이스를 제한하여, 특정 형태의 동작만들 수행하도록 함.
		iterator을 지원하지 않음 (기본적으로는).


* iterator 종류

	- 입력 반복자(input iterator): 현 위치의 원소를 한 번만 읽을 수 있는 반복자
	- 출력 반복자(output iterator): 현 위치의 원소를 한 번만 쓸 수 있는 반복자
	- 순방향 반복자(forward iterator): 입력, 출력 반복자 기능에 순방향으로 이동(++)이 가능한 재할당될 수 있는 반복자
	- 양방향 반복자(bidirectional iterator): 순방향 반복자 기능에 역방향으로 이동(--)이 가능한 반복자
	- 임의 접근 반복자(random access iterator): 양방향 반복자 기능에 +, -, +=, -=, [] 연산이 가능한 반복자

	* 컨테이너에 따른 iterator
		vector  : 임의 접근 반복자(random access iterator)
		map		: 양방향 반복자(bidirectional iterator)
		set		: 양방향 반복자(bidirectional iterator)
		stack	: 스택은 어댑터 컨테임으로 iterator를 지원하지 않는다 ㅠㅠ 


* Algorthm 종류
	
	STL은 원소를 조회, 관리, 변경, 처리할 목적으로 
	정렬, 삭제, 검색, 연산 대한 알고리즘 구성요소를 제공한다.
	알고리즘은 한 쌍의 반복자(begin, end)를 필요로 하며 알고리즘 대부분은 순방향 반복자를 요구한다.
	하지만 몇몇 알고리즘은 임의 접근 반복자를 요구합니다.
	STL 에서 알고리즘 기능을 수행하는 함수들이 여러 버전으로 오버로딩 되어 100개도 넘는다.
	알고리즘을 분유 하면 7 개의 범주가 나온다.

	- 원소를 수정하지 않는 알고리즘(nonmodifying algorithms)
	- 원소를 수정하는 알고리즘(modifying algorithms)
	- 제거 알고리즘(removing algorithms)
	- 변경 알고리즘(mutating algorithms)
	- 정렬 알고리즘(sorting algorithms)
	- 정렬된 범위 알고리즘(sorted range algorithms)
	- 수치 알고리즘(numeric algorithms)

	!!!!!STL 의 알고리즘은 특정 컨테이너나 원소 타입에 종속적이지 않다.!!!!!!
	예를 들어 find 알고리즘은 순방향 반복자를 요구하기 때문에 순방향 반복자만 지원하는 컨테이너라면 
	어떤 컨테이너가 와도 알고리즘을 수행할 수 있으며 컨테이너 원소가 정수, 실수, 문자열 혹은 사용자 정의 타입이어도 가능하다.


* Adaptor 종류

	어댑터는 구성 요소의 인터페이스를 변경한다.
	대표적인 컨테이너 어댑터가 stack 이다.

	- 컨테이너 어댑터(container adaptor)	: stack, queue, priority queue
	- 반복자 어댑터(iterator adaptor)		: reverse_iterator, back insert iterator, front_insert_iterator, insert iterator
	- 함수 어댑터(function adaptor)			: 바인더(binder), 부정자(negator), 함수 포인터 어댑터 (adaptor for pointers to functions)


* Allocator 부연 설명
	
	Allocator는 컨테이너의 메모리 할당 정보와 정책(메모리 할당 모델)을 캡슐화한 STL 구성 요소로
	여러 종류로 나뉘지는 않지만, 사용자가 직접 정의해서 사용할 수는 있다.
	모든 컨테이너는 템플릿 매개변수에 할당기를 인자로 받습니다. 
	기본 할당기는 allocator<T> 이다.
	컨테이너는 템플릿 매개변수에 디폴트 매개변수 값으로 기본 할당기를 지정합니다.
	과제에서는 std에서 제공된 기본 std::allocator 할당자를 사용하도록 되어 있다.





# STL 컨테이너 자료구조

- vector
- stack
- map
- set

-------------------------------------------------------------------------------

-  vector (c++ 98)


	1. 멤버 형식
	``` 
		typedef  T value_type;
		typedef  T* pointer;
		typedef  T& reference;
		typedef  const T* const_pointer;
		typedef  const T& const_reference;
		typedef typename size_t size_type;
		typedef typename ptrdiff_t difference_type;
		typedef typename random_access_iterator<value_type>	iterator;
		typedef reverse_iterator<iterator>	reverse_iterator;
		typedef  _member class_ rebind<Type>;
	``` 
	2. 생성자

	|제목|내용|
	|---|---|
	|vector v		|		V는 빈 컨테이너이다.|
	|vector v(n)	|			v는 기본값으로 초기화된 n개의 원소를 갖는다.|
	|vector v(n.x)	|		v는 x 값으로 초기화된 n개의 원소를 갖는다.|
	|vector v(v2)	|		v는 V2 컨테이너의 복사본이다(복사 생성자 호출).|
	|vector v(b,e)	|		v는 반복자 구간 [b, e)로 초기화된 원소를 갖는다.|

	3. 멤버 함수

	|제목|내용|
	|---|---|
	|v.assign(n,x)	|	v에 x 값으로 n개의 원소를 할당한다.|
	|v.assign(b,e)	|	V를 반복자 구간 [b, e)로 할당한다.|
	|v.at(i)		|			v의 1번째 원소를 참조한다|
	|v.back()		|		v의 마지막 원소를 참조한다 |
	|p=v.begin()	|		p는 v의 첫 원소를 가리키는 반복자 |
	|x=v.capacity()	|	x는 v에 할당된 공간의 크기|
	|v.clear()		|		v의 모든 원소를 제거한다.|
	|v.empty()	|		v가 비었는지 조사한다.|
	|p=v.end()		|	p 는 v의 끝을 표식하는 반복자다|
	|q=v.erase(p)	|		p가 가리키는 원소를 제거한다. q는 다음 원소를 가리킨다.|
	|q=v.erase(b.e) 	|	반복자 구간 [b. e]의 모든 원소를 제거한다. q는 다음 원소.|
	|v.front()				|	v의 첫 번째 원소를 참조한다|
	|q=v.insert(p.x)		|	p가 가리키는 위치에 x 값을 삽입한다. q는 삽입한 원소를 가리키는 반복자다.|
	|v. insert(p. n. x)	|	p 가 가리키는 위치에 n개의 x 값을 삽입한다|
	|v.insert(p. b. e)  |   p 가 가리키는 위치에 반복자 구간 [b. e]의 원소를 삽입한다.|
	|x=v.max_size()  |   V가 담을 수 있는 최대 원소의 개수다(메모리의 크기)|
	|v.pop_back()		|	v의 마지막 원소를 제거한다.|
	|v.push_back(x)		|	v의 끝에 X를 추가한다.|
	|p=v.rbegin()		|	p는 v의 역 순차열의 첫 원소를 가리키는 반복자|
	|p=v.rend()			|	p는 v의 역 순차열의 끝을 표식하는 반복자|
	|v.reserve(n)	|		n개의 원소를 저장할 공간을 만든다.|
	|v.resize(n)	|		v의 크기를 n으로 변경한다|
	|v.resize(n, x)	|	v의 크기를 n으로 변경하고 확장되는 공간의 값을 기본값으로 초기화한다.|
	|v.size()		|		원소의 개수다.|
	|v.swap(v2)	|		v와 v2를 swap한다.|

	4. 연산자

	|제목|내용|
	|------|---|
	|v1 == v2	|	v1 과 v2의 모든 원소가 같은가?|
	|v1 != v2	|	v1 과 v2의 모든 원소가 다른가?|
	|v1 < v2		|	v1 가 v12보다 작은가?|
	|v1 <= v2	|	v1 가 v2보다 작거나 같은가?|
	|v1 >= v2	|	v1 가 v2보다 크거나 같은가?|
	|v1 > v2		|	v1 가 v2 보다 큰가?|
	|v[i]		|		v의 1번째 원소를 참조|


* vector 의 특징
	vector는 임의 접근 반복자를 지원하는 배열 기반 컨테이너이다. 
	vector의 가장 큰 특징 중 하나는 원소가 하나의 메모리 블록에 연속으로 저장된다는 것이다.
	그렇다 보니 원소가 추가되거나 삽입될 때 메모리 재활당이 발생할 수 있고 상당한 비용을 지불하게 된다. 
	그래서 메모리 할당 크기를 알 수 있게 capacity() 멤버 함수를 제공하며, 
	한 번에 메모리를 예약(할당)하고 사용할 수 있는 reserve() 멤버 함수를 제공한다. 
	원소가 연속하게 저장되므로 원소에 접근하는 at()이나 vil 등의 연산은 속도가 빠르지만
	insert(), erase(), push_ back() 등이 빈번하게 호출되어야 하는 
	프로그램이라면 다른 컨테이너의 선택을 고려해야 한다.
	또한, vector는 시퀀스 기반 컨테이너다. 
	시퀀스 기반 컨테이너는 원소가 서로 상대적 인 위치(순서)를 유지하므로 
	가장 앞 요소와 가장 뒤 요소를 참조하는 front(), back() 멤버 함수를 제공하며 
	컨테이너 끝에 추가하고 제거하는 push back(), pop back) 멤버 함수를 제공한다.
	vector가 시퀀스 기반 컨테이너지만 배열 기반 컨테이너이므로 
	push front()와 pop front() 멤버 함수는 제공하지 않는다
	이 두 멤버 함수는 vector에서 매우 비효율적 으로 동작하기 때문이다.
	마지막으로 vector는 연속한 원소를 index 정수로 빠르게 접근하도록 at)과 [] 연산자를 제 공한다.
	at()과 [] 연산자는 같은 기능을 수행하지만, 
	at()은 유효 범위를 점검하여 안전하 게 원소에 접근하도록 하며, 
	[] 연산자는 유효 범위를 점검하지 않아 원소 접근 속도를 조금 더 높인다.



-  stack의 (c++ 98)
	1. 멤버 형식
		``` 
		typedef Container container_type;
		typedef typename container_type::value_type value_type;
		typedef typename container_type::size_type size_type;
		``` 


	2. 생성자
		|제목|내용|
		|------|---|
		|	explicit stack(const Container& = Container( ))	|	컨테이너의 기본 생성자를 호출해 stack을 생성하거나 인자로 받아 stack을 생성한다.	|
	

	3. 멤버 함수

		|제목|내용|
		|------|---|
		|bool empty() const				|			원소가 없는가?|
		|size type size() const				|		원소의 개수|
		|void push(const value_ type& x)	|	원소를 추가한다.|
		|void pop()							|			원소를 제거한다.|
		|value_type& top()				|			Top 원소의 참조한다.|
		|const value _type& top() const	|	const 객체 Top 원소를 참조한다.|



	4. 연산자

		|제목|내용|
		|------|---|
		|	s1 == s2	|	s1 과 s2의 모든 원소가 같은가?|
		|s1 != s2	|	s1 과 s2의 모든 원소가 다른가?|
		|s1 < s2		|	s1 가 s12보다 작은가?|
		|	s1 <= s2	|	s1 가 s2보다 작거나 같은가?|
		|s1 >= s2	|	s1 가 s2보다 크거나 같은가?|
		|	s1 > s2		|	s1 가 2 보다 큰가?|
		|s[i]		|		s의 1번째 원소를 참조|


-  map의 (c++ 98)

	0. 템플릿 형식
	``` 
	template<
	typename Key,
	typename Value, 
	typename Pred=less(Key), 
	typename Allocator=allocator<pair (const Key, Value))
		>
	``` 
	1. 멤버 형식
	``` 
	typedef typename _Rep_type::pointer pointer;
	typedef typename _Rep_type::const_pointer const_pointer;
	typedef typename _Rep_type::reference reference;
	typedef typename _Rep_type::const_reference const_reference;
	typedef typename _Rep_type::iterator iterator;
	typedef typename _Rep_type::const_iterator const_iterator;
	typedef typename _Rep_type::reverse_iterator reverse_iterator;
	typedef typename _Rep_type::const_reverse_iterator const_reverse_iterator;
	typedef typename _Rep_type::size_type size_type;
	typedef typename _Rep_type::difference_type difference_type;
	typedef typename _Rep_type::allocator_type allocator_type;
	``` 

	2. 멤버 함수

	|제목|
	|------|
	|key_compare key_comp() const |
	|value_compare value_comp() const|
	|allocator_type get_allocator() const |
	|iterator begin()|
	|iterator end()|
	|reverse_iterator rbegin()|
	|const_reverse_iterator rbegin()|
	|reverse_iterator rend() |
	|bool empty() const|
	|size_type size() const |
	|size_type max_size() const|
	|_Tp& operator[](const key_type& __k)|
	|void swap(map<_Key,_Tp,_Compare,_Alloc>& __x)|
	|pair<iterator,bool> insert(const value_type& __x) |
	|iterator insert(iterator position, const value_type& __x)|
	|void insert(_InputIterator __first, _InputIterator __last) |
	|void erase(iterator __position) |
	|size_type erase(const key_type& __x)|
	|void erase(iterator __first, iterator __last)|
	|void clear()|
	|iterator find(const key_type& __x) |
	|const_iterator find(const key_type& __x) const |
	|size_type count(const key_type& __x) const |
	|iterator lower_bound(const key_type& __x)|
	|const_iterator lower_bound(const key_type& __x) const|
	|iterator upper_bound(const key_type& __x)|
	|const_iterator upper_bound(const key_type& __x) const |
	|pair<iterator,iterator> equal_range(const key_type& __x) |
	|pair<const_iterator,const_iterator> equal_range(const key_type& __x) const|



	3. 연산자

	|제목|내용|
	|------|---|
	| m[k]=v | m 컨테이너에 원소(K, V)를 추가하거나 key에 해당하는 원소의 value를 v로 갱신한다 |

* Map과 Set 의 특징
	map 컨테이너는 연관 컨테이너 중 자주 사용하는 컨테이너로 원소를 keye value의 쌍으로 저장한다.
	set은 원소로 key 하나만을 저장하지만, map은 원소로 key와 value의 쌍(pair 객체) 로 저장한다.
	Set 처럼 원소의 key는 컨데이너에 중복으로 저장될 수 없으며 중복 key,을  저장해야 한다면 multimap을 사용한다.

* map의 주요 인터페이스
	map도 연관 컨테이너이므로 set과 같은 인터페이스 멤버 함수를 제공하며 템플릿 형식과 내 장 멤버 형식만이 약간의 차이를 보인다. 특히, map은 [] 연산자를 제공하여 key에 해당 하는 원소의 value에 쉽게 접근하거나 변경할 수 있다.
	**과제에서는 set과 map 모두 Red Balck Tree 의 자료구조로 원소들이 저장된다.**




# Red Balck Tree 


> 이진 트리에서는 각각의 자료는 '노드'에 저장이 된다. 자료를 트리 구조로 저장할 때, 노드들 중 최상위에 있는 노드를 루트 (root)라고 부른다. 이진 트리에서 노드는 최대 두 개의 자식 노드를 가질 수 있다. 각각의 자식 노드도 역시 최대 두 개의 자식 노드를 가질 수 있으며, 이런식으로 계속 연결된다. 그러므로, 어떤 트리도 루트 노드로부터 그 트리에 속한 모든 노드에 도달할 수 있다. 어떤 노드에 자식 노드가 없다면, 그 노드를 리프 노드(leaf node)라고 부른다. 말 그대로 트리의 맨 가장자리에 있기 때문이다. 레드 블랙 트리는 이진 트리의 특수한 버전이라고 할 수 있다. 
레드-블랙 트리를 포함한 이진 탐색 트리는, 모든 노드에 대해 '자신이 가진 자료는 자신보다 오른쪽에 위치한 부분트리가 가지고 있는 모든 자료보다 작거나 같고, 자신보다 왼쪽에 위치한 부분트리가 가지고 있는 모든 자료보다 크거나 같다'라는 조건을 만족한다. 이런 특성 때문에 특정 값을 빠르게 찾아 낼 수 있으며, 각 구성원소(elements)간의 효율적인 in-order traversal이 가능하다

>> 레드블랙 트리는  정확하게는 자가 균형 이진 탐색 트리(self-balancing binary search tree)이다. 일반적인 이진 탐색 트리의 비균형적 모양을 보완하는 규칙성으로 원소를 삽입 한 후 균형잡힌 트리형태로 뻗어나가는 것이 큰 특징이다. 
레드-블랙 트리는 복잡한 자료구조지만, 실 사용에서 효율적이고, 최악의 경우에도 상당히 우수한 실행 시간을 보인다: 트리에 n개의 원소가 있을 때 O(log n)의 시간복잡도로 삽입, 삭제, 검색을 할 수 있다. 결국 삽입, 삭제, 검색시 최악의 경우(worst-case)에서의 시간복잡도가 트리의 높이(또는 깊이)에 따라 결정되기 때문에 보통의 이진 탐색 트리에 비해 효율적이라고 할 수 있다. 시뮬레이터를 이용해 먼저 테스트 해보자.
https://www.cs.usfca.edu/~galles/visualization/RedBlack.html

>>> 왜 일반 이진트리가 아닌 레드블랙 트리를 사용하는가? 
이진트리의 경우 최악의 경우 노드들이 한쪽으로 몰려서 대각선 처럼 줄줄이 이어지게 되는데,
이 경우 시간 복잡도가  빅오엔 O(n) 이 되는데 만약 내가 탐색하려는 원소가 맨 마지막에 있다면,
루트부터 맨 마지막까지 모든 원소를 한 번씩 전부 탐색해야 하기 때문에 효율이 떨어진다.


* Red Balck Tree 큰 규칙
``` 
1. 모든 노드는 Red or Black. (회색은 없다!)
2. Root 노드는 Black.
3.  nil 노드는 Black (rbtree 에서 nill 노드는 leaf 노드)
4. Red 의 양쪽 자식은 Black. 
5. 각 노드에서 nil 노드까지의 Black 수가 같다 (자기 자신은 제외).
``` 
Red Balck Tree 가 일반 이진 트리보다 균형잡힌 모양을 유지할 수 있는건,
이 자료구조 내부에 삽입, 삭제 로직이 모두 저 다섯 가지 대전제를 기반으로
설계되어 있기 때문이다. 어떤 로직이든 저 대전제에 어긋난다면 곧바로 
수정되어 저 규칙성을 기어코 기어코 맞춘 뒤에 끝난다. 
	

* Red Balck Tree 동작
	* 설명

		레드블랙 트리의 읽기 동작(탐색 등)은 이진 탐색 트리의 읽기 동작의 구현과 동일하다, 왜냐하면 레드 블랙 트리가 이진 탐색 트리의 특수한 한 형태이기 때문이다. 그러나 삽입과 삭제의 경우 이진 탐색 트리의 구현에 따른 동작만으로는 레드 블랙 트리의 특성을 만족하지 못한다. 다시 레드 블랙 트리의 특성을 만족하게 만들기 위해서는 색 변환과 최대 3회의 트리 회전(tree rotation)이 필요하다(삽입의 경우 2회). 삽입과 삭제는 복잡한 동작이지만, 그 복잡도는 여전히 O(log n)이다. 
		삽입과 삭제시에는 주로 #4, #5번 속성을 위반하게 되는데 이를 해결하기 위래 구조를 바꾸다보면 자연스럽게 트리의 균형이 잡힌다.

	* 삽입

		삽입 전에는 모든 레드블랙 트리 속성을 만족한 상태일 것이다.
		처음 삽입을 할 때는 일반적인 이진트리와 동일하다. 하지만 삽입 후에 레드블랙 트리 속성을 위반 했는지를 체크 한 뒤 재조정 한다.
		그러고 나면 레드 블랙 트리의 속성을 다시 만족한 상태로 삽입 로직은 끝!

		새로운 원소를 삽입할 때는 항상 red 이다. red로 넣는 이유는 노드를 새로 삽입한 후에도 #5번 속성을 만족하기 위해서이다.
	
		삽입 후에는 nill 노드가 자동으로 양쪽 자식에 함께 삽입된다. 

		case 1. 새로 삽입 되는 원소가 root 일때 (최초의 삽입)
			새로 삽입 될 때는 red 이지만 이후 체크 단계에서 #2번 속성을 어긴 것으로 판별나면, 그냥 red 에서 black으로 바꿔주고 끝. 
		case 3. 삽입된 red 노드가 부모의 왼쪽 자녀이면서, 부모도 red이고, 부모의 형제가 black 일때. ->부모와 할아버지의 색을 바꾸고 할아버지 기준으로 오른쪽 회전.




	* 삭제
	
		삭제 후 속성 위반 여부 확인은 
		삭제되는 색을 통해 한다.
		만약 삭제하려는 노드의 자녀가 없거나 하나라면,
		삭제 되는 색은 삭제되는 노드의 색이고,
		삭제하려는 노드의 자녀가 둘이라면 삭제되는 색은,
		삭제 되는 색은 삭제되는 노드의 successor의 색이 된다.
		successor 는 오른쪽 자녀의 왼쪽 끝.

		만약 삭제되는 색이 red 라면 어떠한 속성도 위반하지 않게 된다.
		하지만 블랙이라면 이때는 #2 번 #4번 #5번 속성을 위반할 수 있다.

		결국 레드 블랙 트리에서 삭제 시 재조정은
		삭제되는 색이 black 일 경우에만 해당된다.

		만약 black 이 삭제 된 후에 #2번 속성을 위반했다면,
		루트 노드를 black 으로 바꾸면 끝난다.

		



