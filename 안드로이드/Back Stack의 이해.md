# Task(작업) 및 백 스택 이해

작업(task)은 사용자가 특정 작업을 할 때 상호작용하는 activity의 컬렉션. activity는 백 스택에 각 activity이 `열린 순서대로` 정렬된다. 

- 새로운 activity는 백 스택에 추가된다.
- 사용자가 **뒤로** 버튼을 누르면 새로운 activity가 완료되고 스택에서 팝된다.



##### 멀티윈도우의 백스택

- 앱이 동시에 실행 될 때 시스템은 각 창의 task를 별도로 관리한다.
- 시스템은 task또는 task그룹을 창 단위로 관리



##### Activity에서의 백 스택

- 현재 activity이 또 다른 activity를 시작하면 새 activity이 스택의 맨 위에 푸시되고 포커스르 갖게된다.
  - 이전 활동은 스택에 남아있지만 중지된다.
- activity이 중지되면 시스템은 activity의 사용자 인터페이서가 갖고 있는 현재 상태를 보존한다
- **뒤로** 버튼을 누르면 현재 화동이 스택의 맨 위에서 팝(제거), 이전 activity가 실행(UI 복원)
- 스택에 팝되고 푸시 될 뿐 >> LIFO구조

![back stack](./img/diagram_backstack.png)

- **뒤로** 버튼을 계속 누르면 홈 화면으로 돌아감 (백 스택이 빈 상태)



##### Task에서의 백 스택

- 사용자가 새 작업을 시작(새로운 앱 시작)하거나 홈 버튼을 통해 홈 화면으로 이동할 떄 작업은 '백그라운드'로 이동 할 수 있는 응집 단위를 말한다.
- 작업의 모든 activity는 백그라운드에 있는 동안 중지되지만 작업의 백 스택은 오전하게 그대로 유지 된다.

![task_multi](./img/diagram_multitasking.png)

- 작업은 다른 작업이 실행 될 동안 포커스를 잃을 뿐!
- 중단된 작업이 포커스를 다시 가지면 중단된 activity들을 계속 이어나간다.
- 여러 작업을 동시에 백그라운드에 유지 가능
  - 하지만 여러개가 백그라운드 작업을 동시에 실행하면 시스템이 메모리 복구하기 위해 백그라운드 활동의 제거를 시작할 수 있으며 이로인해 activity 상태가 손실 될 수 있음



##### 백스택의 활동

- 사용자에게 둘 이상의 activity으로부터 특정 activity을 시작하도록 허용하면 activity의 새 인스턴스가 생성되어 스택으로 푸시됨
  - 이전의 인스턴스가 맨 위로 오는게 아님
  - 한 activity이가 여러번 인스턴스화 될 수 있음

![diagram_multiple_instances](./img/diagram_multiple_instances.png)



### Task 관리

Task 및 백 스택을 관리하는 방식은 매우 효과적. 하지만 정상적인 동작을 중단 시켜야 하거나 activity가 시작될 때 현재 작업 내에 배치 되지 않고 새 작업이 시작되도록 할 수 있다. 또는 activity가 시작 될 때 백 스택의 맨 위에 새 인스턴스가 생성되는 대신 activity의 기존 인스턴스가 앞으로 나오도록 할 수도 있다.



이러한 작업들을 < activity > manifest요소의 속정 및 startActivity()에 전달하는 인텐트 플래그를 사용하여 작업 수행가능



##### < activity > 속성

- taskAffinity
- launchMode
- allowTaskReparenting
- clearTaskOnLaunch
- alwaysRetainTaskState
- finishOntaskLaunch



##### 인텐트 플래그

- FLAG_ACTIVITY_NEW_TASK
- FLAG_ACTIVITY_CLEAR_TOP
- FLAG_ACTIVITY_SINGLE_TOP



대부분 앱에서 activity 및 작업의 기본 동작을 중단해서는 안된다. 개발자는 activity에서 기본 동작을 수정할 필요가 있다고 판단 되면 신중하게 생각하고 활동 실행 중에 **뒤로** 버튼을 통해 다른 activity 및 작업에서 이 activity로 이동할 떄 활동 사용성을 테스트 해야한다. 

- 예상되는 동작과 충돌한 가능성이 있는 탐색 동작을 테스트해야함



##### 실행 모드 정의

실행 모드를 통해 activity의 새 인스턴스가 현재 작업과 연결되는 방식을 정의 할 수 있다.

실행 모드 정의 방법

- manifest 파일 사용
  - activity 선언시 task와 연결되어야하는 방식 지정 가능
- 인텐트 플래그 사용
  - startActivity()를 호출 할 때 새 activity가 현재 task와 연결 되어야 하는 방식을 선언하는 플래그를 Intent에 포함
- 인텐트에 의한 정의가 manifest보다 우선시 됨
  - 인텐트에서만 정의 가능 한 것이 있고 manifest에서만 정의 가능한 것이 있다.



// 플래그, 코드 내용은 추후 추가!



##### 어피니티 처리

Affinity는 activity가 어느 작업에 소속되기를 선호하는지 나타낸다. 동일한 앱의 모든 activity는 서로에게 어피니티를 가지고 있다.

- activity의 기본 affinity를 수정 가능
- 서로 다른 앱에 정의된 activity가 어피니티 공유 가능
- < activity > 요소의 `taskAffinity` 속성을 통해 어피니티 수정

// 플래그, 코드 내용은 추후 추가!





##### 백스택 삭제

사용자가 오랜 시간 동안 task를 떠나 있으면 시스템은 루트 activity을 제외한 모든 activity를 task에서 삭제함

- 사용자가 task로 다시 돌아오면 루트 activity만 복원된다.
- 이런 식으로 동작하는 이유는 사용자가 이전에 하고 있던 일을 그만두고 task로 돌아왔을 때 새로운 일을 시작할 가능성이 크기 때문에
- Activity 속성
  - `alwaysRetainTaskState` : 작업의 루트 activity에서 이 속성을 "true"로 설정하면 기본 동작이 일어나지 않고 오랜 시간이 지난 후에도 스택의 모든 활동이 유지
  - `clearTaskOnLaunch` : 루트 activity에서 이 속성을 "true"로 설정하면 사용자가 작업을 떠났다가 작업으로 다시 돌아올때마다 스택이 루트 activity까지 삭제
    - 항상 작업의 초기 상태로 돌아옴
  - `finishOnTaskLaunch` : 작업 전체가아니라 단일 activity에서 동작
    - 루트 activity를 포함하여 어떤 activity라도 지울 수 있다.
    - 현재 세션에 대해서만 작업을 일부로 유지
    - 사용자가 작업을 떠났다가 다시 도아오면 이 작업은 더이상 존재하지 않음



##### 작업 시작

- `android.intent.action.MAIN`을 작업으로 지정하고 `android.intent.category.LAUNCHER`를 카테고리로 지정하여 인텐트 필터를 제공함으로써 activity를 작업의 진입점으로 설정할 수 있다. 

```
    <activity ... >
        <intent-filter ... >
            <action android:name="android.intent.action.MAIN" />
            <category android:name="android.intent.category.LAUNCHER" />
        </intent-filter>
        ...
    </activity>
    
```





