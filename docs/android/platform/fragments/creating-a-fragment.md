---
title: 조각 만들기
ms.prod: xamarin
ms.assetid: F2997242-BC29-1440-7F1A-CFC447CD73FA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/07/2018
ms.openlocfilehash: 339de4930242e35c40b034af2ce6ba47fe1543af
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61023675"
---
# <a name="creating-a-fragment"></a>조각 만들기

조각을 만들려면 클래스에서 상속 해야 합니다 `Android.App.Fragment` 한 후 재정의 `OnCreateView` 메서드. `OnCreateView` 시간 조각 화면에서 적용할 되 고 반환 하는 경우 호스팅 활동에 의해 호출 될는 `View`합니다. 일반적인 `OnCreateView` 이 만들어집니다 `View` 값 레이아웃 파일을 해당 부모 컨테이너에 연결 하 여 합니다. 컨테이너의 특성에는 Android 조각의 UI에는 부모의 레이아웃 매개 변수를 적용할 중요 합니다. 다음 예제는 이러한 과정을 보여 줍니다.

```csharp
public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
{
    return inflater.Inflate(Resource.Layout.Example_Fragment, container, false);
}
```

위의 코드에서 뷰를 급격 하 게 증가 `Resource.Layout.Example_Fragment`, 자식 뷰를 추가 하 고는 `ViewGroup` 컨테이너입니다.


> [!NOTE]
> 조각 하위 클래스를 공용 기본 없는 인수 생성자를 있어야 합니다.

## <a name="adding-a-fragment-to-an-activity"></a>활동에 조각 추가

두 가지 방법으로 조각이 호스팅될 수 작업 내에서:

-   **선언적** &ndash; 조각 내에서 선언적으로 사용할 수 있습니다 `.axml` 사용 하 여 레이아웃 파일을 `<Fragment>` 태그입니다.

-   **프로그래밍 방식으로** &ndash; 조각을 인스턴스화할 수 있습니다 동적으로 사용 하 여는 `FragmentManager` 클래스의 API입니다.

통해 프로그래밍 방식으로 사용 된 `FragmentManager` 클래스는이 가이드의 뒷부분에서 설명 합니다.

### <a name="using-a-fragment-declaratively"></a>선언적으로 조각을 사용 하 여

레이아웃 내부 조각을 추가 하려면 사용 해야 합니다 `<fragment>` 태그 식별 한 다음 이러한 조각 중 하나를 제공 하 여는 `class` 특성 또는 `android:name` 특성입니다. 다음 코드 조각을 사용 하는 방법을 보여 줍니다 합니다 `class` 선언 하는 특성을 `fragment`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment class="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

이 다음 조각에서는 선언 하는 방법을 보여 줍니다.는 `fragment` 를 사용 하 여는 `android:name` 조각을 클래스를 식별 하려면 특성:

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment android:name="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

Android 레이아웃 파일에 지정 된 각 조각을 인스턴스화하는 및에서 생성 된 뷰의 삽입 작업이 만들어질 때 `OnCreateView` 대신는 `Fragment` 요소입니다.
활동에 선언적으로 추가 되는 조각은 정적 및; 삭제 될 때까지 활동에 유지 됩니다. 동적으로 바꾸거나 연결 되는 활동의 수명 동안 이러한 조각을 제거 하는 것이 불가능 합니다.

각 조각에는 고유 식별자를 할당 되어야 합니다.

-  **android: id** &ndash; 그대로 레이아웃 파일의 다른 UI 요소를 사용 하 여이 고유한 id입니다.

-  **android: tag** &ndash; 이 특성은 고유한 문자열입니다.

앞의 두 메서드 모두를 사용 하는 경우 조각을 컨테이너 보기의 ID를 갖게 됩니다. 다음 예제에서 위치 모두 `android:id` 나 `android:tag` Android는 ID를 할당 하는 제공 `fragment_container` 조각으로:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
                android:id="+@id/fragment_container"
                android:orientation="horizontal"
                android:layout_width="match_parent"
                android:layout_height="match_parent">

        <fragment class="com.example.android.apis.app.TitlesFragment"
                android:layout_width="match_parent"
                android:layout_height="match_parent" />
</LinearLayout>
```

### <a name="package-name-case"></a>패키지 이름 대/소문자

Android는 패키지 이름에 대문자를 허용 하지 않습니다. 패키지 이름이 대문자를 포함 하는 경우 뷰를 확장 하려고 할 때 예외가 throw 됩니다 것입니다. 그러나 Xamarin.Android 좀 더 관대, 및 네임 스페이스에 대 문자가 허용 됩니다.

예를 들어 Xamarin.Android를 사용 하 여는 모두 다음 코드 조각은 작동 합니다. 그러나 두 번째 조각은 하면는 `android.view.InflateException` 순수 Java 기반 Android 응용 프로그램에 의해 throw 됩니다.

```xml
<fragment class="com.example.DetailsFragment" android:id="@+id/fragment_content" android:layout_width="match_parent" android:layout_height="match_parent" />
```

또는

```xml
<fragment class="Com.Example.DetailsFragment" android:id="@+id/fragment_content" android:layout_width="match_parent" android:layout_height="match_parent" />
```


## <a name="fragment-lifecycle"></a>조각 수명 주기

조각으로 어느 정도 독립적 이지만 여전히에서 영향을 받는 자체 수명 주기를 갖는 합니다 [호스팅 작업의 수명 주기](~/android/app-fundamentals/activity-lifecycle/index.md)합니다.
예를 들어 작업 일시 중지 하는 경우 연결 된 해당 조각 모두 일시 중지 됩니다. 다음 다이어그램에서는 조각의 수명 주기를 설명합니다.

[![조각 수명 주기를 보여 주는 흐름 다이어그램](creating-a-fragment-images/fragment-lifecycle.png)](creating-a-fragment-images/fragment-lifecycle.png#lightbox)


### <a name="fragment-creation-lifecycle-methods"></a>조각 생성 수명 주기 메서드

아래 목록 생성 되는 조각 수명 주기에서 다양 한 콜백의 흐름을 보여 줍니다.

-   **`OnInflate()`** &ndash; 조각 보기 레이아웃의 일부로 만들어질 때 호출 됩니다. 이 조각 XML 레이아웃 파일에서 선언적으로 만들어진 후에 즉시 호출할 수 있습니다. 아직 조각이 해당 활동와 연결 되지 않은 하지만 **활동**를 **번들**, 및 **확인 하기 위해** 뷰에서 계층 구조에 매개 변수로 전달 됩니다. 이 메서드 구문 분석에 가장 적합 합니다 **확인 하기 위해** 및 특성을 저장 하는에서 사용할 수 있습니다 나중 조각에 대 한 합니다.

-   **`OnAttach()`** &ndash; 이 부분은 작업과 연결 된 후 호출 됩니다. 조각을 사용할 수 있을 때 실행할 수는 첫 번째 메서드입니다. 일반적으로 조각 해야 하지 생성자를 구현 하거나 기본 생성자를 재정의 합니다. 이 메서드에서 조각에 필요한 모든 구성 요소를 초기화 합니다.

-   **`OnCreate()`** &ndash; 조각을 만들려면 활동에 의해 호출 됩니다. 이 메서드를 호출 하는 경우 호스팅 작업의 계층 구조 보기 인스턴스화할 수 없음을 완전히, 조각에서 나중에 조각의 수명 주기 작업의 뷰 계층 구조의 모든 부분에 의존 하지 않아야 하므로 합니다. 예를 들어이 메서드는 모든 조정 또는 응용 프로그램의 UI 조정 하는 데 사용 하지 마십시오. 이 가장 이른 시간 조각 해야 하는 데이터 수집을 시작할 수 있습니다. 조각의 UI 스레드에서 시점에서 실행 중인, 따라서 시간이 오래 걸리는 모든 처리를 방지 또는 백그라운드 스레드에서 처리를 수행 합니다. 경우에이 메서드를 건너뛸 수 있음 **SetRetainInstance(true)** 라고 합니다.
    이 대체 아래에서 자세히 설명 합니다.

-   **`OnCreateView()`** &ndash; 조각에 대 한 뷰를 만듭니다.
    이 메서드는 한 번 활동의 **OnCreate()** 메서드는 완료 합니다. 이 시점에서 활동의 뷰 계층 구조와 상호 작용 하는 안전 합니다. 이 메서드는 조각에서 사용 될 뷰를 반환 해야 합니다.

-   **`OnActivityCreated()`** &ndash; 다음에 호출 **Activity.OnCreate** 호스팅 작업에서 완료 되었습니다.
    최종 단계는 사용자 인터페이스를 지금 수행 되어야 합니다.

-   **`OnStart()`** &ndash; 포함 활동을 다시 시작 된 후 호출 됩니다. 따라서 조각을 사용자에 게 표시 합니다. 대부분의 경우에서 조각은 되는 코드가 포함 됩니다는 **onstart ()** 활동의 메서드.

-   **`OnResume()`** &ndash; 이 방법은 마지막 조각을 사용 하 여 사용자가 조작할 수 되기 전에 호출 됩니다. 이 메서드에서 수행 해야 하는 코드 종류의 예로 사용자 조작할 수 있는, 카메라와 같은 장치의 기능을 사용는 하는 위치 확인 서비스입니다. 그러나 이러한 서비스에 과도 한 배터리가 발생할 수 있습니다 및 배터리를 유지 하기 위해 사용을 최소화 해야 하는 응용 프로그램입니다.


### <a name="fragment-destruction-lifecycle-methods"></a>조각 소멸 수명 주기 메서드

다음 목록에서는 조각을 소멸 될 때 호출 되는 수명 주기 메서드를 설명 합니다.

-   **`OnPause()`** &ndash; 사용자가 더 이상 조각을 사용 하 여 상호 작용할 수 없습니다. 이 코드 조각을 수정 하는 다른 조각 작업 또는 호스팅 작업 일시 중지 된이 상황이 존재 합니다. 이 조각 호스팅 작업 표시 될 수 있습니다, 포커스가 작업은 부분적으로 투명 또는 전체 화면을 차지 하지 않습니다, 가능성이 있습니다. 이 메서드는 활성화 되 면 때 사용자가 조각 나가는 첫 번째 표시 합니다. 조각에는 변경 내용을 저장 해야 합니다.

-   **`OnStop()`** &ndash; 조각 더 표시 되지 않습니다. 호스트 작업을 중지 될 수 있습니다, 또는 조각 작업을 작업에서 수정 됩니다. 이 콜백은 동일한 목적으로 사용 **Activity.OnStop**합니다.

-   **`OnDestroyView()`** &ndash; 이 메서드는 뷰와 연결 된 리소스를 정리 합니다. 이 조각과 사용 하 여 연결 된 뷰를 제거 될 때 호출 됩니다.

-   **`OnDestroy()`** &ndash; 이 메서드는이 부분은 더 이상 사용에서 하는 경우 호출 됩니다. 활동을 사용 하 여 여전히 연결 되지만 부분은 더 이상 작동 합니다. 이 메서드는 사용 중인 조각에서와 같은 모든 리소스를 해제 해야는 [ **SurfaceView** ](https://developer.xamarin.com/api/type/Android.Views.SurfaceView/) 는 카메라를 사용할 수 있습니다. 경우에이 메서드를 건너뛸 수 있음 **SetRetainInstance(true)** 라고 합니다. 이 대체 아래에서 자세히 설명 합니다.

-   **`OnDetach()`** &ndash; 이 메서드는 조각 작업과 연결 되어 있지 않습니다. 직전에 호출 됩니다. 조각의 뷰 계층 구조는 더 이상 존재 하 고이 시점에서 조각에서 사용 되는 모든 리소스가 해제 되어야 합니다.


### <a name="using-setretaininstance"></a>SetRetainInstance를 사용 하 여

해당 되지 완전히 제거 해야 작업을 다시 생성 되 면 지정 된 조각에 대 한 것 같습니다. 합니다 `Fragment` 클래스는 메서드를 제공 `SetRetainInstance` 이 목적을 위해. 경우 `true` 단편의 동일한 인스턴스에 사용할 작업을 다시 시작 되 면 다음이 메서드에 전달 됩니다. 이런 경우를 제외한 모든 콜백 메서드를 호출할 합니다 `OnCreate` 및 `OnDestroy` 수명 주기 콜백 합니다. 이 프로세스는 녹색 점선) (으로 위에 표시 된 수명 주기 다이어그램에 나와 있습니다.


## <a name="fragment-state-management"></a>조각 상태 관리

월 조각을 저장 하 고 인스턴스를 사용 하 여 조각 수명 주기 동안 해당 상태를 복원 된 `Bundle`합니다. 번들 키/값 쌍으로 데이터를 저장 하는 조각 수 및 메모리를 많이 필요 하지 않은 간단한 데이터에 유용 합니다. 조각에 대 한 호출을 사용 하 여 해당 상태를 저장할 수 `OnSaveInstanceState`:

```csharp
public override void OnSaveInstanceState(Bundle outState)
{
    base.OnSaveInstanceState(outState);
    outState.PutInt("current_choice", _currentCheckPosition);
}
```

조각의 새 인스턴스는 만들 때에 저장 된 상태를 `Bundle` 를 통해 새 인스턴스를 사용할 수 있게 됩니다 합니다 `OnCreate`, `OnCreateView`, 및 `OnActivityCreated` 새 인스턴스의 메서드.
다음 샘플에는 값을 검색 하는 방법을 보여 줍니다 `current_choice` 에서 `Bundle`:

```csharp
public override void OnActivityCreated(Bundle savedInstanceState)
{
    base.OnActivityCreated(savedInstanceState);
    if (savedInstanceState != null)
    {
        _currentCheckPosition = savedInstanceState.GetInt("current_choice", 0);
    }
}
```

재정의 `OnSaveInstanceState` 는 저장 하기 위한 임시 데이터 조각에 방향 변경에서와 같은 적절 한 메커니즘을 `current_choice` 위 예의 값. 그러나 기본 구현은 `OnSaveInstanceState` 할당 ID가 있는 모든 보기에 대 한 UI에서 임시 데이터를 저장 하는 처리 합니다. 예를 들어 있는 응용 프로그램을 살펴보고는 `EditText` 다음과 같이 XML에 정의 된 요소:

```xml
<EditText android:id="@+id/myText"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"/>
```

있으므로 합니다 `EditText` 컨트롤에는 `id` 에 할당 하는 조각 위젯에서 데이터 자동으로 저장 때 `OnSaveInstanceState` 라고 합니다.


### <a name="bundle-limitations"></a>번들 제한 사항

사용 하 여 있지만 `OnSaveInstanceState` 는 몇 가지 제한 사항이 일시적인 데이터를 저장 한 다음이 메서드의 사용 하기가 쉽습니다.

-  조각 백 스택에 추가 되지 않습니다 경우 사용자가 누르면 상태가 복원 되지 것입니다 합니다 **다시** 단추입니다.

-  번들을 사용 하 여 데이터 저장을 하는 경우 해당 데이터 serialize 됩니다. 처리 지연 시킬 수 있습니다.


## <a name="contributing-to-the-menu"></a>메뉴에 영향을 주는

조각에는 해당 호스팅 활동의 메뉴 항목 원인이 될 수 있습니다.
작업 메뉴 항목을 먼저 처리합니다. 활동 처리기에 없는 경우 다음 이벤트를 전달할 수는 처리 해야 하는 조각.

활동의 메뉴 항목에 추가 하려면 조각에는 두 가지 수행 해야 합니다.
조각에서 메서드를 구현 해야는 먼저 `OnCreateOptionsMenu` 다음 코드 에서처럼 메뉴에 해당 항목을 배치 합니다.

```csharp
public override void OnCreateOptionsMenu(IMenu menu, MenuInflater menuInflater)
{
    menuInflater.Inflate(Resource.Menu.menu_fragment_vehicle_list, menu);
    base.OnCreateOptionsMenu(menu, menuInflater);
}
```

이전 코드 조각에 있는 메뉴에서 파일에 있는 다음 XML을 더하게 됩니다 `menu_fragment_vehicle_list.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:id="@+id/add_vehicle"
        android:icon="@drawable/ic_menu_add_data"
        android:title="@string/add_vehicle" />
</menu>
```

다음으로 조각이 호출 해야 `SetHasOptionsMenu(true)`합니다. 이 메서드에 대 한 호출 조각 [옵션] 메뉴에 영향을 주는 메뉴 항목에 Android에 알립니다. 이 메서드를 호출 하지 않는 한 조각에 대 한 메뉴 항목은 활동의 옵션 메뉴에 추가 되지 않습니다. 이 일반적으로 수행 수명 주기 메서드 `OnCreate()`다음 코드 조각에 나와 있는 것 처럼:

```csharp
public override void OnCreate(Bundle savedState)
{
    base.OnCreate(savedState);
    SetHasOptionsMenu(true);
}
```

다음 화면은이 메뉴는 표시 되는 방식을 보여 줍니다.

[![메뉴 항목을 표시 하는 내 여정 앱의 스크린샷 예제](creating-a-fragment-images/fragment-menu-example.png)](creating-a-fragment-images/fragment-menu-example.png#lightbox)
