# Pracelable 및 Bundle

`Pracelable` 및 `Bundle` 객체는 IPC/바인더 트랜잭션과 같은 프로세스 경계 전반에서 인텐트가 있는 활동간에 사용하고 구성 변경 시 일시적인 상태를 저장하기 위한 것

- `Parcel`은 범용 직렬화 매커니즘이 아니므로 `Parcel`데이터를 디스크에 저장하거나 네트워크를 통해 전송해서는 안된다.



### Activity간 데이터 전송

`startActivity(android.content.Intent)`에 사용할 Intent 객체를 생성 할 때 `putExtra(java.lang.String, java.lang.String)` 메서드를 사용하여 매개변수를 전달 할 수 있다.

```
    Intent intent = new Intent(this, MyActivity.class);
    intent.putExtra("media_id", "a1b2c3");
    // ...
    startActivity(intent);
    
```

- OS는 인텐트의 기본 `Bundle`을 묶습니다. 그런 다음 OS는 새 activity를 생성하고 데이터 번들을 풀며 인텐트에 새 activity를 전달 한다.
- Bundle 클래스를 사용하여 Intent 객체에서 OS에 알려진 프리미티브를 설정하는 것이 좋다.
  - Bundle 클래스는 Parcel을 사용하는 마샬링 및 언마샬링에 고도로 최적화되어있다.(마샬링 및 언마샬링,,,,??)
- activity 간에 합성 또는 복합 객체를 전송하는 메커니즘이 필요한 상황도 있다.
  - 맞춤 클래스는 Pacelable을 구현하고 적절한 `writeToTparcel(android.os.Parcel, int)`메서드를 제공해야 한다.
  - `CREATOR`라는 null이 아닌 필드도 제공해야한다.
  - `CREATOR`는 Parcel을 현재 객체로 다시 변환하는데 `createFromParcel()` 메서드를 사용하는 `Parcelable.Creator` 인터페이스를 구현한다.
- 인텐트를 통해 데이터 전송 시 KB로 제한해야한다.
  - TransactionTooLargeException 예외 발생



### 프로세스간 데이터 전송

- activity 데이터 전송과 비슷
- 맞춤 Parcelable을 사용하지 않는 것이 좋음
  - 동일 한 버전의 맞춤 클래스가 정확히 있어야한다.
  - 공통 라이브러리를 사용할 수 있는데 시스템은 인식하지 못하는 클래스를 언마샬링할 수 없기 때문에 Parcelabe을 시스템에 전송하려고 하면 오류가 발생함
- 바인더 트랜잭션 버퍼는 현재 1MB 고정 크기로 제한되어 있으며 진행중인 모든 트랜잭션에 의해 공유된다.
  - activity별 수준이 아니라 프로세스 수준에 이 제한이 적용되기 때문에 모든 트랜잭션에는 onSaveInstanceState, startActivity, 시스템과의 상호작용 등 앱의 모든 바인더 트랜잭션이 포함된다. 
- saveInstacneState에서는 사용자가 활동으로 다시 이동할 수 있는 한 시스템 프로세스가 제공된 데이터를 계속 보유 행하므로 데이터 양을 적게 유지해야 한다.
  - 저장된 상태를 50k 미만의 데이터로 유지하는 것이 좋다.





//이거 다시공부하기

