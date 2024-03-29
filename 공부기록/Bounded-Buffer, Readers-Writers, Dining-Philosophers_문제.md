### **Bounded-Buffer Problem(Producer-Consumer Problem)**

생산자는 아이템을 생산하고 버퍼에 넣는 역할을 하고, 소비자는 버퍼에서 아이템을 소비하는 역할을 한다. 문제는 여러 생산자와 소비자가 동시에 실행될 때 데이터 일관성과 동기화 문제가 발생할 수 있다.

일반적으로 생산자가 버퍼가 가득 차있으면 기다리고, 소비자는 버퍼가 비어있으면 기다리는 식으로 동작한다. 이런 상황에서 생산자와 소비자가 서로에게 신호를 보내고 받아야하는데, 이 신호를 효과적으로 주고 받아야 한다. 소비자는 무조건 생산자가 먼저 실행된 적이 있어야하고, 생산자의 실행 횟수가 소비자보다 많아야 한다. 세마포어, 뮤텍스, 조건 변수 등과 같은 동기화 도구를 사용하여 문제를 해결할 수 있다.

### **Readers-Writers Problem**
여러 프로세스나 쓰레드 간 공유 데이터에 대한 동시 접근을 관리해야 한다. 읽기 연산과 쓰기 연산이 동시에 발생할 때 발생하는 동기화 문제이다.

여러 독자는 동시에 데이터를 읽을 수 있지만, 데이터를 쓰기 위해서는 오직 하나의 작가만 접근할 수 있어야한다. 여러 독자가 동시에 읽는 것은 상호 배타적이여야 할 필요가 없기 때문에 동시성을 향상 시킬 수 있다. 하지만 한 작가가 데이터를 쓸 때에는 다른 독자나 작가가 접근하지 못하도록 막아야한다. 이러한 문제를 해결하기 위해 세마포어, 뮤텍스 등과 같은 동기화 메커니즘을 사용할 수 있다. 일반적으로 작가가 데이터를 쓰고있는 동안 다른 사람이 접근하지 못하도록 막는 방식으로 동기화를 구현해야 한다

⇒ 한 프로세스가 DB에 쓰기 작업을 수행하고 있을 때 다른 프로세스가 접근하면 안된다. 읽기는 동시에 여럿이 해도 된다. 해결 방법은 작성자가 DB에 접근 허가를 아직 얻지 못한 상태에서는 모든 대기중인 독자들을 전부 DB에 접근하게 한다. 작가는 대기중인 독자가 하나도 없을 때 DB 접근이 허용된다. 일단 작가가 DB에 접근중이면 독자들은 접근이 금지된다. 작가가 DB에서 빠져나가야 독자의 접근이 허용된다

### **Dining-Philosophers**

각 철학자가 생각을 하다가 배가 고파지면 식사를 하는데, 식사를 하려면 양 옆에 있는 포크를 사용해야한다. 그런데 문제는 모든 철학자가 동시에 자신의 왼쪽과 오른쪽에 있는 포크를 잡고 싶어하는데 이때 데드락(경쟁 상태)이 발생할 수 있다. 만약 모든 철학자가 왼쪽 포크를 잡아서 오른쪽 포크를 잡지 못하면 누구도 식사를 시작할 수 없다. 

해결 방법으로는 세마포어나 뮤텍스를 사용해서 각 포크를 공유 자원으로 간주하고 각 철학자가 식사를 시작하기 전에 뮤텍스나 세마포어를 획득하도록 만들어 동시에 한 철학자만이 특정 포크를 사용할 수 있도록 한다.

두번째로는 각 철학자가 왼쪽과 오른쪽에 있는 포크를 동시에 잡지 못하는 규칙을 만든다. 예를 들어 다섯명 중 네명은 왼쪽 포크를 먼저 잡고 나머지 한명은 오른쪽 포크를 먼저 잡도록 한다

세번째로는 철학자들에게 일정한 순서로 식사를 시작하도록 하여 데드락을 피한다. 첫 번째 철학자부터 차례로 식사를 시작하고 마지막 철학자가 식사를 마치면 다시 첫번째로 돌아간다