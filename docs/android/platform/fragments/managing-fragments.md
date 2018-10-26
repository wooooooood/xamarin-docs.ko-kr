---
title: 조각 관리
ms.prod: xamarin
ms.assetid: 02C5E8F0-32EF-4FD9-DC8B-04650E20722C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/07/2018
ms.openlocfilehash: 107877d0e92d3a46101812b78bc0b414c0fbb320
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105469"
---
# <a name="managing-fragments"></a>조각 관리

Android 조각 관리를 돕기 위해 제공 된 `FragmentManager` 클래스입니다. 각 활동의 인스턴스가 `Android.App.FragmentManager` 는 찾거나 해당 조각에 동적으로 변경 됩니다. 각 이러한 변경 내용 집합 이라고는 *트랜잭션*, 클래스에 포함 된 Api 중 하나를 사용 하 여 수행 됩니다 `Android.App.FragmentTransation`에 의해 관리 되는 `FragmentManager`합니다. 활동 같이 트랜잭션을 시작할 수 있습니다.

```csharp
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
```

조각 이러한 변경 내용을에서 수행 되는 `FragmentTransaction` 같은 메서드를 사용 하 여 인스턴스 `Add()`, `Remove(),` 및 `Replace().` 를 사용 하 여 변경 내용이 적용 한 다음 `Commit()`. 트랜잭션에서 변경 내용은 즉시 수행 되지 않습니다.
대신 가능한 한 빨리 활동의 UI 스레드에서 실행을 예약 합니다.

다음 예제에서는 기존 컨테이너에 조각을 추가 하는 방법을 보여 줍니다.

```csharp
// Create a new fragment and a transaction.
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
DetailsFragment aDifferentDetailsFrag = new DetailsFragment();

// The fragment will have the ID of Resource.Id.fragment_container.
fragmentTx.Add(Resource.Id.fragment_container, aDifferentDetailsFrag);

// Commit the transaction.
fragmentTx.Commit();
```

트랜잭션이 커밋된 후 하면 `Activity.OnSaveInstanceState()` 는 호출 예외가 throw 됩니다. Android 모든 호스 티 드 조각의 상태를 저장 하 작업 상태를 저장 하기 때문에 발생 합니다. 이 시점 이후에 모든 조각을 트랜잭션이 커밋되는, 이러한 트랜잭션의 상태 활동 복원 되 면이 변경 내용이 손실 됩니다.

작업의 조각 트랜잭션을 저장 하는 것이 불가능 [백 스택에](http://developer.android.com/guide/topics/fundamentals/tasks-and-back-stack.html) 를 호출 하 여 `FragmentTransaction.AddToBackStack()`입니다. 이렇게 하면 사용자를 통해 뒤로 탐색할 수 있습니다. 조각을 변경 하는 경우는 **다시** 단추가 눌러져 있습니다. 이 메서드를 호출 하지 않고 조각 제거 되는 소멸 됩니다 및 사용자 활동을 통해 다시 이동 하는 경우에 사용할 수 없게 됩니다.

다음 예제에서는 사용 하는 방법을 보여 줍니다 합니다 `AddToBackStack` 메서드는 `FragmentTransaction` 백 스택에 첫 번째 조각의 상태를 유지 하는 동안 하나의 조각을 바꾸려면:

```csharp
// Create a new fragment and a transaction.
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
DetailsFragment aDifferentDetailsFrag = new DetailsFragment();

// Replace the fragment that is in the View fragment_container (if applicable).
fragmentTx.Replace(Resource.Id.fragment_container, aDifferentDetailsFrag);

// Add the transaction to the back stack.
fragmentTx.AddToBackStack(null);

// Commit the transaction.
fragmentTx.Commit();
```


## <a name="communicating-with-fragments"></a>조각을 사용 하 여 통신

합니다 *FragmentManager* 모든 활동에 연결 된 조각에서 인식 하 고 이러한 조각을 찾을 수 있도록 두 가지 방법을 제공 합니다.

-   **FindFragmentById** &ndash; 이 메서드는 트랜잭션의 일부로 추가 될 때 레이아웃 파일에 지정 된 ID 또는 컨테이너 ID를 사용 하 여 조각을 찾을 수 있습니다.

-   **FindFragmentByTag** &ndash; 이 메서드는 레이아웃 파일에서 제공한 또는 트랜잭션에서 추가 된 태그가 있는 조각을 찾는 데 사용 됩니다.

조각 및 활동에 대 한 참조를 `FragmentManager`이므로 동일한 기술을 서로 앞뒤로 알리는 데 사용 됩니다. 응용 프로그램 두 가지 방법 중 하나를 사용 하 여 조각 참조를 찾을 하 고 해당 참조를 적절 한 형식으로 캐스팅 하 고 조각에서 메서드를 직접 호출할 수 있습니다. 다음 코드 조각은 예제를 제공 합니다.

사용 하는 작업에 대 한도 가능 합니다 `FragmentManager` 조각을 찾으려면:

```csharp
var emailList = FragmentManager.FindFragmentById<EmailListFragment>(Resource.Id.email_list_fragment);
emailList.SomeCustomMethod(parameter1, parameter2);
```


### <a name="communicating-with-the-activity"></a>활동을 사용 하 여 통신

사용 하는 조각 수는 `Fragment.Activity` 해당 호스트를 참조 하는 속성입니다. 보다 구체적인 형식으로 작업을 캐스팅 하 여 가능성이 해당 호스트의 속성과 메서드를 호출 하기 위한 작업에 대 한 다음 예와에서 같이:

```csharp
var myActivity = (MyActivity) this.Activity;
myActivity.SomeCustomMethod();
```
