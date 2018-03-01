---
title: "조각 관리"
ms.topic: article
ms.prod: xamarin
ms.assetid: 02C5E8F0-32EF-4FD9-DC8B-04650E20722C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/07/2018
ms.openlocfilehash: 33f8b44a1213df4a8a1c75e2df66cc0b007a5749
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="managing-fragments"></a>조각 관리

Android 조각 관리에 도움이 되도록 제공 된 `FragmentManager` 클래스입니다. 각 활동의 인스턴스가 있는 `Android.App.FragmentManager` 있는 찾거나는 조각의 동적으로 변경 합니다. 이러한 변경의 각 집합 이라고는 *트랜잭션*, 클래스에 포함 된 Api 중 하나를 사용 하 여 수행 되 고 `Android.App.FragmentTransation`, 의해 관리 되는 `FragmentManager`합니다. 활동 다음과 같이 트랜잭션을 시작할 수 있습니다.

```csharp
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
```

조각에 변경 내용을이 적용에서 수행 되는 `FragmentTransaction` 같은 메서드를 사용 하 여 인스턴스 `Add()`, `Remove(),` 및 `Replace().` 를 사용 하 여 변경 내용이 적용 될 다음 `Commit()`합니다. 트랜잭션에서 변경 내용은 즉시 수행 되지 않습니다.
대신, 가능한 한 빨리 활동의 UI 스레드에서 실행 예약 합니다.

다음 예제에서는 조각을 기존 컨테이너에 추가 하는 방법을 보여 줍니다.

```csharp
// Create a new fragment and a transaction.
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
DetailsFragment aDifferentDetailsFrag = new DetailsFragment();

// The fragment will have the ID of Resource.Id.fragment_container.
fragmentTx.Add(Resource.Id.fragment_container, aDifferentDetailsFrag);

// Commit the transaction.
fragmentTx.Commit();
```

후 커밋된 경우 `Activity.OnSaveInstanceState()` 는 호출에서 예외가 throw 됩니다. 이 Android 모든 호스팅된 조각의 상태를 저장 하는 활동의 상태를 저장 하기 때문에 발생 합니다. 이 시점 이후에 조각 트랜잭션이 커밋됩니다, 이러한 트랜잭션의 상태 활동 복원 되 면이 변경 내용이 손실 됩니다.

활동의 조각 트랜잭션을 저장할 수는 [백 스택에](http://developer.android.com/guide/topics/fundamentals/tasks-and-back-stack.html) 호출 하 여 `FragmentTransaction.AddToBackStack()`합니다. 이렇게 하면 사용자를 통해 뒤로 탐색할 때 조각 변경는 **다시** 단추가 눌려질 합니다. 이 메서드를 호출 하지 않고 제거 된 소멸 됩니다 조각과 사용자가 활동을 통해 다시 탐색 하는 경우에 사용할 수 없게 됩니다.

사용 하는 방법을 보여 주는 다음 예제는 `AddToBackStack` 의 메서드는 `FragmentTransaction` 백 스택에 첫 번째에 조각의 상태를 유지 하면서 한 조각의 바꾸려면:

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

<a name="Communicating_with_Fragments" />

## <a name="communicating-with-fragments"></a>조각을 사용 하 여 통신합니다.

*FragmentManager* 모든 활동에 연결 되어 있는 조각에서 인식 하 고 이러한 조각을 찾을 수 있도록 두 가지 방법을 제공 합니다.

-   **FindFragmentById** &ndash; 이 메서드는 트랜잭션의 일부로 조각 추가 될 때 레이아웃 파일에 지정 된 ID 또는 컨테이너 ID를 사용 하 여 조각 경우가 있습니다.

-   **FindFragmentByTag** &ndash; 이 메서드는 레이아웃 파일에 제공한 또는 트랜잭션에 추가 된 태그가 있는 조각을 찾는 데 사용 됩니다.

조각 및 활동을 모두 참조는 `FragmentManager`이므로 서로 간에 통신에 동일한 기술을 사용 합니다. 응용 프로그램 참조 조각 두 가지 방법 중 하나를 사용 하 여, 참조 하는 적절 한 형식으로 캐스팅 찾아서 다음 조각에서 메서드를 직접 호출 합니다. 다음 코드 조각 예제를 제공 합니다.

또한 활동을 사용 하는 `FragmentManager` 조각을 찾을 수:

```csharp
var emailList = FragmentManager.FindFragmentById<EmailListFragment>(Resource.Id.email_list_fragment);
emailList.SomeCustomMethod(parameter1, parameter2);
```

<a name="Communicating_with_the_Activity" />

### <a name="communicating-with-the-activity"></a>활동와의 통신

사용 하는 조각 수는 `Fragment.Activity` 해당 호스트를 참조 하도록 합니다. 더 구체적인 형식으로 활동을 캐스팅 하 여 있기 호출 메서드 및 속성에 해당 호스트에 활동에 대 한 다음 예제와 같이:

```csharp
var myActivity = (MyActivity) this.Activity;
myActivity.SomeCustomMethod();
```
