---
title: 조각 만들기
ms.prod: xamarin
ms.assetid: F2997242-BC29-1440-7F1A-CFC447CD73FA
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/07/2018
ms.openlocfilehash: eae25dbfaf1125191a83b3cc4326abc19105f22f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30770211"
---
# <a name="creating-a-fragment"></a>조각 만들기

클래스에서 상속 해야 조각을 만들려면, `Android.App.Fragment` 한 후 재정의 `OnCreateView` 메서드. `OnCreateView` 화면에 조각의 보겠습니다 하 고는 반환 하는 경우 호스팅 활동에 의해 호출 될는 `View`합니다. 일반적인 `OnCreateView` 이 만듭니다 `View` 않아서 레이아웃 파일을 한 다음 부모 컨테이너에 연결 하 여 합니다. 컨테이너의 특징은 중요 Android에 조각의 UI에 부모 레이아웃 매개 변수를 적용 됩니다. 다음 예제는 이러한 과정을 보여 줍니다.

```csharp
public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
{
    return inflater.Inflate(Resource.Layout.Example_Fragment, container, false);
}
```

위의 코드에서 보기를 급격 하 게 증가 `Resource.Layout.Example_Fragment`, 자식 뷰를 추가 하 고는 `ViewGroup` 컨테이너입니다.


> [!NOTE]
> 조각 하위 클래스는 공용 기본 인수 없는 생성자를 있어야 합니다.

## <a name="adding-a-fragment-to-an-activity"></a>활동에 조각 추가

두 가지 방법으로 조각 호스팅될 수는 활동 내부:

-   **선언적으로** &ndash; 조각 내에 선언적으로 사용할 수 있습니다 `.axml` 사용 하 여 레이아웃 파일은 `<Fragment>` 태그입니다.

-   **프로그래밍 방식으로** &ndash; 조각도 인스턴스화할 수 동적으로 사용 하 여는 `FragmentManager` 클래스의 API입니다.

통해 프로그래밍 방식으로 사용 된 `FragmentManager` 이 가이드의 뒷부분에 나오는 클래스에 대해서 설명 합니다.

### <a name="using-a-fragment-declaratively"></a>조각을 선언적으로 사용 하 여

사용 하 여 레이아웃 내부 조각을 추가 하려면는 `<fragment>` 태그 식별 한 다음 이러한 조각 중 하나를 제공 하 여는 `class` 특성 또는 `android:name` 특성입니다. 다음 코드 조각에서는 사용 하는 `class` 선언 하는 특성을 `fragment`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment class="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

다음이 코드 조각에서는 선언 하는 방법을 보여 줍니다.는 `fragment` 를 사용 하 여는 `android:name` 조각 클래스를 식별 하는 특성:

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment android:name="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

Android 레이아웃 파일에 지정 된 각 조각을 인스턴스화하는 및에서 생성 된 뷰의 삽입 작업이 만들어질 때 `OnCreateView` 대신는 `Fragment` 요소입니다.
조각 선언적 활동에 추가 되는 정적이 고; 삭제 될 때까지 활동에 유지 됩니다. 동적으로 바꾸거나 연결 되어 있는 활동의 수명 동안 그러한 조각을 제거 하는 것이 불가능 합니다.

각 조각은 고유 식별자를 지정 해야 합니다.

-  **android: id** &ndash; 와 마찬가지로 레이아웃 파일의 다른 UI 요소,이 고유 ID

-  **android: 태그** &ndash; 이 특성은 고유한 문자열입니다.

이전 두 가지 방법 중을 사용 하는 경우 조각 컨테이너 보기의 ID를 갖게 됩니다. 다음 예제에서를 둘 다 `android:id` 나 `android:tag` Android에서의 ID를 할당 하는 제공 된 `fragment_container` 는 조각에:

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

Android는 패키지 이름;에 대 문자가 허용 하지 않습니다. 패키지 이름에 대 문자가 포함 되어 있으면 뷰를 확장할 하려고 할 때 예외가 throw 됩니다. 그러나 Xamarin.Android 보다 관대 이며 네임 스페이스에 대 문자가 허용 됩니다.

예를 들어 다음 코드 조각 모두 Xamarin.Android 사용 합니다. 그러나 두 번째 조각은 하면는 `android.view.InflateException` 순수 Java 기반 Android 응용 프로그램에 의해 throw 됩니다.

```xml
<fragment class="com.example.DetailsFragment" android:id="@+id/fragment_content" android:layout_width="match_parent" android:layout_height="match_parent" />
```

또는

```xml
<fragment class="Com.Example.DetailsFragment" android:id="@+id/fragment_content" android:layout_width="match_parent" android:layout_height="match_parent" />
```


## <a name="fragment-lifecycle"></a>조각 수명 주기

조각은 무관 다소 건에 의해 영향을 여전히 자신의 기간이 [호스팅 활동의 수명 주기](~/android/app-fundamentals/activity-lifecycle/index.md)합니다.
예를 들어 활동 일시 중지 될 때 연결 된 해당 조각 모두 일시 중지 됩니다. 다음 다이어그램에 조각의 수명 주기를 간략하게 설명합니다.

[![조각 수명 주기를 보여 주는 흐름 다이어그램](creating-a-fragment-images/fragment-lifecycle.png)](creating-a-fragment-images/fragment-lifecycle.png#lightbox)


### <a name="fragment-creation-lifecycle-methods"></a>조각 생성 수명 주기 메서드

아래 목록 생성 될 때 조각의 수명 주기에서 다양 한 콜백의 흐름을 보여 줍니다.

-   **`OnInflate()`** &ndash; 조각에서이 뷰 레이아웃의 일부로 생성 될 때 호출 됩니다. 이 조각에서 XML 레이아웃 파일에서 선언적으로 생성 된 후 즉시 호출할 수 있습니다. 아직 조각 해당 작업과 연결 되어 있지 않습니다 되지만 **활동**, **번들**, 및 **확인 하기 위해** 보기에서 계층 구조에서 매개 변수로 전달 됩니다. 이 메서드는 구문 분석에 가장 효과적 이며는 **확인 하기 위해** 와 특성을 저장에서 사용할 수 있으므로 나중에 조각의 합니다.

-   **`OnAttach()`** &ndash; 이 부분은 작업과 연결 된 후 호출 됩니다. 이것이 부분은 사용할 수 있게 하는 경우 실행 될 첫 번째 방법입니다. 일반적으로 조각이 해야 하지는 생성자를 구현 하거나 기본 생성자를 재정의 합니다. 이 메서드에서 조각에 필요한 모든 구성 요소를 초기화 합니다.

-   **`OnCreate()`** &ndash; 조각을 만드는 활동에 의해 호출 됩니다. 이 메서드를 호출할 때 호스팅 활동의 계층 구조 보기 인스턴스화할 수 없습니다 완전히, 하므로 조각에서 나중에 조각의 수명 주기 작업의 뷰 계층의 모든 부분에 의존 하지 않아야 합니다. 예를 들어이 메서드는 모든 변경 또는 응용 프로그램의 UI 조정 하는 데 사용 하지 마십시오. 이 가장 빠른 시간 조각에서 필요로 하는 데이터 수집을 시작할 수 있습니다. 이 시점에서 조각 UI 스레드에서 실행 중인, 따라서 시간이 오래 걸리는 처리를 수행한를 방지 하거나 처리 하는 백그라운드 스레드에서 수행 합니다. 경우에이 메서드를 건너뛸 수 있습니다 **SetRetainInstance(true)** 호출 됩니다.
    이 대체 아래에서 자세히 설명 합니다.

-   **`OnCreateView()`** &ndash; 조각에 대 한 뷰를 만듭니다.
    이 메서드는 한 번 활동의 **OnCreate()** 메서드가 완료 되었습니다. 이 시점에서 활동의 계층 구조 보기와 상호 작용할 수 안전 합니다. 이 메서드는 조각에서 사용 될 뷰를 반환 해야 합니다.

-   **`OnActivityCreated()`** &ndash; 다음에 호출 **Activity.OnCreate** 호스팅 작업 완료 합니다.
    최종 단계는 사용자 인터페이스를 지금 수행 되어야 합니다.

-   **`OnStart()`** &ndash; 포함 활동을 다시 시작 된 후 호출 됩니다. 이렇게 하면 조각에서 사용자에 게 표시 합니다. 대부분의 경우에는 조각에 될 수 있는 코드에 포함 됩니다는 **onstart ()** 활동의 메서드가 있습니다.

-   **`OnResume()`** &ndash; 이 조각 상호 작용할 수 전에 호출 마지막 방법. 이 메서드에서 수행 해야 하는 코드의 종류의 예로 사용자 상호 작용할 수, 카메라 같은 장치 기능을 사용 합니다 하는 위치 확인 서비스입니다. 하지만 이러한 서비스 과도 하 게 배터리 소모 발생할 수 있으며 응용 프로그램에 배터리 수명을 보존할 사용을 최소화 해야 합니다.


### <a name="fragment-destruction-lifecycle-methods"></a>조각 소멸 수명 주기 메서드

다음 목록에서는 조각 소멸 될 때 호출 된 수명 주기 메서드를 설명 합니다.

-   **`OnPause()`** &ndash; 사용자가 더 이상 조각에서와 상호 작용할 수 없습니다. 이 조각을 수정 하는 다른 조각 작업 또는 호스팅 활동이 일시 중지 되었습니다.이 경우 존재 합니다. 이 조각 호스팅 활동 표시 될 수 있습니다, 즉, 작업에 포커스가 부분적으로 투명 또는 전체 화면을 차지 하지 않는 것 같습니다. 이 메서드는 활성화 되 면 때 사용자가 조각을 나가는 첫 번째 표시 됩니다. 조각에는 변경 내용을 저장 해야 합니다.

-   **`OnStop()`** &ndash; 조각에서 더 이상 표시 됩니다. 호스트 작업을 중지 될 수 있습니다, 또는 조각 작업 동작에서 수정 됩니다. 이 콜백은 동일한 목적으로 사용 **Activity.OnStop**합니다.

-   **`OnDestroyView()`** &ndash; 이 메서드는 뷰와 연결 된 리소스를 정리 합니다. 이 조각에서와 관련 된 보기 메뉴가 제거 될 때 호출 됩니다.

-   **`OnDestroy()`** &ndash; 이 메서드는 조각에서 더 이상 사용 중입니다. 활동을 여전히 연결 되지만 부분은 더 이상 작동 합니다. 이 메서드는 같은 조각에서 사용 하에서는 모든 리소스를 해제 해야는 [ **SurfaceView** ](https://developer.xamarin.com/api/type/Android.Views.SurfaceView/) 카메라에 사용할 수 있습니다. 경우에이 메서드를 건너뛸 수 있습니다 **SetRetainInstance(true)** 호출 됩니다. 이 대체 아래에서 자세히 설명 합니다.

-   **`OnDetach()`** &ndash; 이 메서드는이 부분은 더 이상 활동과 연결 바로 전에 호출 됩니다. 뷰 계층 구조에 조각의 더 이상 존재 및이 시점에서 조각에서 사용 되는 모든 리소스가 해제 되어야 합니다.


### <a name="using-setretaininstance"></a>SetRetainInstance를 사용 하 여

것은 완전히 소멸 되지 활동이 다시 생성 되는 경우를 지정 하는 조각에 대 한 것 같습니다. `Fragment` 클래스는 메서드를 제공 `SetRetainInstance` 이 목적을 위해 합니다. 경우 `true` 에 조각의 동일한 인스턴스 사용 됩니다 작업이 시작 될 때이 메서드에 전달 됩니다. 이런 경우를 제외 하 고 호출 될 모든 콜백 메서드에 `OnCreate` 및 `OnDestroy` 수명 주기 콜백 합니다. 이 프로세스는 (녹색 점선) 하 여 위에 표시 된 수명 주기 다이어그램에 표시 됩니다.


## <a name="fragment-state-management"></a>조각 상태 관리

년 5 월에 조각의 상태 저장 및 복원의 조각 수명 주기 동안의 인스턴스를 사용 하 여 한 `Bundle`합니다. 번들 키/값 쌍으로 데이터를 저장 하는 조각 수 및 메모리를 많이 필요 하지 않은 간단한 데이터에 유용 합니다. 호출 하 여 해당 상태를 저장할 수는 조각 `OnSaveInstanceState`:

```csharp
public override void OnSaveInstanceState(Bundle outState)
{
    base.OnSaveInstanceState(outState);
    outState.PutInt("current_choice", _currentCheckPosition);
}
```

조각의 새 인스턴스를 만들 때에 저장 된 상태는 `Bundle` 를 통해 새 인스턴스를 사용할 수 있는 `OnCreate`, `OnCreateView`, 및 `OnActivityCreated` 메서드는 새 인스턴스의 합니다.
다음 샘플에서는 값을 검색 하는 방법을 보여 줍니다. `current_choice` 에서 `Bundle`:

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

재정의 `OnSaveInstanceState` 저장 하기 위한 임시 데이터 조각에 모든 방향 변경 내용에서와 같은 적절 한 메커니즘은는 `current_choice` 위의 예에는 값입니다. 그러나의 기본 구현은 `OnSaveInstanceState` 할당 된 ID가 있는 모든 보기에 대 한 UI에서 일시적인 데이터에 저장 하는 담당 합니다. 예를 들어 있는 응용 프로그램을 확인 한 `EditText` 다음과 같이 XML에 정의 된 요소:

```xml
<EditText android:id="@+id/myText"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"/>
```

이후는 `EditText` 컨트롤에는 `id` 할당 조각에서 자동으로 데이터를 저장 위젯에서 때 `OnSaveInstanceState` 라고 합니다.


### <a name="bundle-limitations"></a>번들 제한 사항

사용 하 여 있지만 `OnSaveInstanceState` 에 쉽게이 메서드의 사용 하 여 임시 테이블을 저장 하는 몇 가지 제한이 사용 하면:

-  조각 백 스택에 추가 되지 않습니다 경우 사용자가 누르면 상태로 복원 되지 것입니다는 **다시** 단추입니다.

-  번들을 사용 하 여 데이터 저장을 하는 경우 해당 데이터는 직렬화 됩니다. 처리 지연 시킬 수 있습니다.


## <a name="contributing-to-the-menu"></a>메뉴에 기여합니다.

조각이 해당 호스팅 작업의 메뉴에 항목 현상이 발생할 수 있습니다.
작업 메뉴 항목을 먼저 처리합니다. 활동에 처리기를 찾을 수 없는 경우 다음 이벤트가 전달 됩니다 다음를 처리할는 조각에.

활동의 메뉴 항목을 추가 하려면 조각에는 다음 두 가지 수행 해야 합니다.
첫째, 조각 메서드를 구현 해야 `OnCreateOptionsMenu` 다음 코드에 나와 있는 것 처럼 해당 항목의 메뉴에 배치 합니다.

```csharp
public override void OnCreateOptionsMenu(IMenu menu, MenuInflater menuInflater)
{
    menuInflater.Inflate(Resource.Menu.menu_fragment_vehicle_list, menu);
    base.OnCreateOptionsMenu(menu, menuInflater);
}
```

파일에 있는 다음 XML에서 이전 코드 조각에서 메뉴 확장 `menu_fragment_vehicle_list.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:id="@+id/add_vehicle"
        android:icon="@drawable/ic_menu_add_data"
        android:title="@string/add_vehicle" />
</menu>
```

그런 다음 조각에서 호출 해야 `SetHasOptionsMenu(true)`합니다. 이 메서드를 호출 조각 옵션 메뉴에 기여 하는 메뉴 항목에 Android에 알립니다. 이 메서드를 호출 하지 않는 한 해당 활동의 옵션 메뉴에는 조각에 대 한 메뉴 항목 추가 되지 않습니다. 이 일반적으로 수행 수명 주기 메서드 `OnCreate()`다음 코드 조각에 표시 된 것 처럼:

```csharp
public override void OnCreate(Bundle savedState)
{
    base.OnCreate(savedState);
    SetHasOptionsMenu(true);
}
```

다음 화면이이 메뉴 모양을 보여 줍니다.

[![메뉴 항목을 표시 하는 내와 응용 프로그램의 예제 스크린 샷](creating-a-fragment-images/fragment-menu-example.png)](creating-a-fragment-images/fragment-menu-example.png#lightbox)
