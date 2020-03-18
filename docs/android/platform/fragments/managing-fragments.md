---
title: 조각 관리
ms.prod: xamarin
ms.assetid: 02C5E8F0-32EF-4FD9-DC8B-04650E20722C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/07/2018
ms.openlocfilehash: 3733ed37abe9604e77529db4864f601d2b473280
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027327"
---
# <a name="managing-fragments"></a>조각 관리

조각 관리에 도움이 되도록 Android는 `FragmentManager` 클래스를 제공합니다. 각 작업에는 해당 조각을 찾거나 동적으로 변경하는 `Android.App.FragmentManager` 인스턴스가 있습니다. 이러한 각 변경 내용의 세트를 *트랜잭션*이라고 하며, `FragmentManager`에서 관리하는 `Android.App.FragmentTransation`클래스에 포함된 API 중 하나를 사용하여 수행됩니다. 작업은 다음과 같은 트랜잭션을 시작할 수 있습니다.

```csharp
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
```

조각에 대한 이러한 변경 내용은 `Add()`, `Remove(),` 및 `Replace().` 등의 메서드를 사용하여 `FragmentTransaction` 인스턴스에서 수행됩니다. 그런 다음, 변경 내용은 `Commit()`를 사용하여 적용됩니다. 트랜잭션의 변경 내용은 즉시 수행되지 않습니다.
대신, 가능하면 빨리 작업의 UI 스레드에서 실행되도록 예약됩니다.

다음 예제에서는 기존 컨테이너에 조각을 추가하는 방법을 보여 줍니다.

```csharp
// Create a new fragment and a transaction.
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
DetailsFragment aDifferentDetailsFrag = new DetailsFragment();

// The fragment will have the ID of Resource.Id.fragment_container.
fragmentTx.Add(Resource.Id.fragment_container, aDifferentDetailsFrag);

// Commit the transaction.
fragmentTx.Commit();
```

`Activity.OnSaveInstanceState()`를 호출한 후 트랜잭션이 커밋되는 경우 예외가 throw됩니다. 이는 작업이 해당 상태를 저장할 때 Android에서 호스팅된 조각의 상태도 저장하기 때문에 발생합니다. 이 시점 이후에 조각 트랜잭션이 커밋되는 경우 작업을 복원할 때 이러한 트랜잭션의 상태가 손실됩니다.

`FragmentTransaction.AddToBackStack()`을 호출하여 조각 트랜잭션을 작업의 [백 스택](https://developer.android.com/guide/topics/fundamentals/tasks-and-back-stack.html)으로 저장할 수 있습니다. 이렇게 하면 **뒤로** 단추를 누를 때 조각 변경을 통해 사용자가 뒤로 이동할 수 있습니다. 이 메서드를 호출하지 않으면 제거되는 조각이 소멸되고 사용자가 작업을 통해 다시 탐색할 경우 사용할 수 없게 됩니다.

다음 예제에서는 `FragmentTransaction`의 `AddToBackStack` 메서드를 사용하여 한 조각을 바꾸고 백 스택의 첫 번째 조각 상태를 유지하는 방법을 보여 줍니다.

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

*FragmentManager*는 작업에 연결된 모든 조각을 인식하고 이러한 조각을 찾는 데 도움이 되는 두 가지 메서드를 제공합니다.

- **FindFragmentById** &ndash; 이 메서드는 조각이 트랜잭션의 일부로 추가될 때 레이아웃 파일 또는 컨테이너 ID에 지정된 ID를 사용하여 조각을 찾습니다.

- **FindFragmentByTag** &ndash; 이 메서드는 레이아웃 파일에 제공되었거나 트랜잭션에 추가된 태그가 있는 조각을 찾는 데 사용됩니다.

조각과 작업은 모두 `FragmentManager`를 참조하므로 동일한 기술을 사용하여 서로 통신하는 데 사용됩니다. 애플리케이션은 이러한 두 메서드 중 하나를 사용하여 참조 조각을 찾고, 적절한 형식에 대해 해당 참조를 캐스팅한 다음, 조각에서 직접 메서드를 호출할 수 있습니다. 다음 코드 조각은 예제를 제공합니다.

또한 작업에서 `FragmentManager`를 사용하여 조각을 찾을 수 있습니다.

```csharp
var emailList = FragmentManager.FindFragmentById<EmailListFragment>(Resource.Id.email_list_fragment);
emailList.SomeCustomMethod(parameter1, parameter2);
```

### <a name="communicating-with-the-activity"></a>작업과 통신

조각이 `Fragment.Activity` 속성을 사용하여 해당 호스트를 참조할 수 있습니다. 작업을 보다 구체적인 형식으로 캐스팅하면 다음 예제와 같이 작업에서 해당 호스트의 메서드와 속성을 호출할 수 있습니다.

```csharp
var myActivity = (MyActivity) this.Activity;
myActivity.SomeCustomMethod();
```
