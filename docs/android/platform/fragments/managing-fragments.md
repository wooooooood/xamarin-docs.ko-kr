---
title: 조각 관리
ms.prod: xamarin
ms.assetid: 02C5E8F0-32EF-4FD9-DC8B-04650E20722C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/07/2018
ms.openlocfilehash: 5b829b1c38007bbb070b643a355d70bffaef01a5
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69524306"
---
# <a name="managing-fragments"></a>조각 관리

조각 관리에 도움이 되도록 Android는 `FragmentManager` 클래스를 제공 합니다. 각 활동에는 해당 조각을 `Android.App.FragmentManager` 찾거나 동적으로 변경 하는의 인스턴스가 있습니다. 이러한 각 변경 내용 집합을 *트랜잭션*이라고 하며,이는에서 관리 `Android.App.FragmentTransation` `FragmentManager`되는 클래스에 포함 된 api 중 하나를 사용 하 여 수행 됩니다. 활동은 다음과 같은 트랜잭션을 시작할 수 있습니다.

```csharp
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
```

조각에 대 한 이러한 변경 `FragmentTransaction` 내용은와 `Add()`같은 메서드를 사용 하 여 인스턴스에서 수행 `Remove(),` 되며 `Replace().` 변경 내용은를 사용 `Commit()`하 여 적용 됩니다. 트랜잭션의 변경 내용은 즉시 수행 되지 않습니다.
대신, 가능한 한 빨리 활동의 UI 스레드에서 실행 되도록 예약 됩니다.

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

가 호출 된 후 `Activity.OnSaveInstanceState()` 트랜잭션이 커밋되면 예외가 throw 됩니다. 이는 활동이 상태를 저장할 때 Android에서 호스팅된 조각의 상태도 저장 하기 때문에 발생 합니다. 이 시점 이후에 커밋되는 조각 트랜잭션이 있으면 작업을 복원할 때 이러한 트랜잭션의 상태가 손실 됩니다.

을`FragmentTransaction.AddToBackStack()`호출 하 여 조각 트랜잭션을 활동의 [백 스택으로](https://developer.android.com/guide/topics/fundamentals/tasks-and-back-stack.html) 저장할 수 있습니다. 이렇게 하면 사용자가 **뒤로** 단추를 누를 때 조각 변경 내용을 뒤로 이동할 수 있습니다. 이 메서드를 호출 하지 않으면 제거 되는 조각이 소멸 되 고 사용자가 활동을 통해 다시 탐색할 경우 사용할 수 없게 됩니다.

다음 예제에서는 `AddToBackStack` `FragmentTransaction` 의 메서드를 사용 하 여 백 스택의 첫 번째 조각 상태를 유지 하면서 한 조각을 바꾸는 방법을 보여 줍니다.

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


## <a name="communicating-with-fragments"></a>조각과 통신

*FragmentManager* 는 활동에 연결 된 모든 조각을 인식 하 고 이러한 조각을 찾는 데 도움이 되는 두 가지 메서드를 제공 합니다.

- **FindFragmentById** &ndash; 이 메서드는 조각이 트랜잭션의 일부로 추가 될 때 레이아웃 파일 또는 컨테이너 ID에 지정 된 ID를 사용 하 여 조각을 찾습니다.

- **FindFragmentByTag** &ndash; 이 메서드는 레이아웃 파일에 제공 되었거나 트랜잭션에 추가 된 태그가 있는 조각을 찾는 데 사용 됩니다.

조각과 활동은 모두를 참조 `FragmentManager`하므로 동일한 기술을 사용 하 여 서로 통신 하는 데 사용 됩니다. 응용 프로그램은 이러한 두 메서드 중 하나를 사용 하 고 적절 한 형식에 대 한 참조를 캐스팅 한 다음 조각에서 직접 메서드를 호출 하 여 참조 조각을 찾을 수 있습니다. 다음 코드 조각은 예제를 제공 합니다.

활동에서를 `FragmentManager` 사용 하 여 조각을 찾을 수도 있습니다.

```csharp
var emailList = FragmentManager.FindFragmentById<EmailListFragment>(Resource.Id.email_list_fragment);
emailList.SomeCustomMethod(parameter1, parameter2);
```


### <a name="communicating-with-the-activity"></a>활동과 통신

조각에서 `Fragment.Activity` 속성을 사용 하 여 해당 호스트를 참조할 수 있습니다. 활동을 보다 구체적인 형식으로 캐스팅 하면 다음 예제와 같이 작업에서 호스트의 메서드와 속성을 호출할 수 있습니다.

```csharp
var myActivity = (MyActivity) this.Activity;
myActivity.SomeCustomMethod();
```
