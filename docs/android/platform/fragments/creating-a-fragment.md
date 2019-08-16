---
title: 조각 만들기
ms.prod: xamarin
ms.assetid: F2997242-BC29-1440-7F1A-CFC447CD73FA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/07/2018
ms.openlocfilehash: 1948c700827f1cc235de5857cde9a2a149af8412
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69524370"
---
# <a name="creating-a-fragment"></a>조각 만들기

조각을 만들려면 클래스가에서 `Android.App.Fragment` 상속 된 다음 메서드를 `OnCreateView` 재정의 해야 합니다. `OnCreateView`는 화면에 조각을 배치할 때 호스팅 활동에 의해 호출 되 고는를 `View`반환 합니다. 일반적 `OnCreateView` 으로 레이아웃 파일을 `View` 않아서 부모 컨테이너에 연결 하 여이를 만듭니다. 컨테이너의 특징은 Android에서 부모의 레이아웃 매개 변수를 조각의 UI에 적용 하기 때문에 중요 합니다. 다음 예제는 이러한 과정을 보여 줍니다.

```csharp
public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
{
    return inflater.Inflate(Resource.Layout.Example_Fragment, container, false);
}
```

위의 코드는 뷰 `Resource.Layout.Example_Fragment`를 확장 하 고 `ViewGroup` 컨테이너에 자식 뷰로 추가 합니다.


> [!NOTE]
> 조각 하위 클래스에는 public default no argument 생성자가 있어야 합니다.

## <a name="adding-a-fragment-to-an-activity"></a>활동에 조각 추가

다음 두 가지 방법으로 작업 내에 조각이 호스팅될 수 있습니다.

- **선언적** 으로 &ndash; 태그 `.axml` 를 사용`<Fragment>` 하 여 레이아웃 파일 내에서 조각을 선언적으로 사용할 수 있습니다.

- **프로그래밍 방식으로** 클래스의 API를 `FragmentManager` 사용 하 여 동적으로 조각을 인스턴스화할 수도 있습니다. &ndash;

클래스를 `FragmentManager` 통한 프로그래밍 방식 사용에 대해서는이 가이드의 뒷부분에서 설명 합니다.

### <a name="using-a-fragment-declaratively"></a>선언적으로 조각 사용

레이아웃 안에 조각을 추가 하려면 태그를 `<fragment>` 사용한 다음 `class` 특성이 나 `android:name` 특성을 제공 하 여 조각을 식별 해야 합니다. 다음 코드 조각에서는 `class` 특성을 사용 하 여를 `fragment`선언 하는 방법을 보여 줍니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment class="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

다음 코드 조각에서는 `fragment` `android:name` 특성을 사용 하 여 Fragment 클래스를 식별 하 여를 선언 하는 방법을 보여 줍니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment android:name="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

활동을 만들 때 Android는 레이아웃 파일에 지정 된 각 조각을 인스턴스화하고 `OnCreateView` `Fragment` 요소 대신에서 만든 뷰를 삽입 합니다.
활동에 선언적으로 추가 되는 조각은 정적 이며 소멸 될 때까지 활동에 유지 됩니다. 연결 된 활동의 수명 동안 이러한 조각을 동적으로 바꾸거나 제거할 수 없습니다.

각 조각에는 고유 식별자가 할당 되어야 합니다.

- **android: id** &ndash; 레이아웃 파일의 다른 UI 요소와 마찬가지로이는 고유 ID입니다.

- **android: 태그** &ndash; 이 특성은 고유 문자열입니다.

이전 두 메서드를 모두 사용 하지 않으면 조각이 컨테이너 뷰의 ID를 가정 합니다. `android:id` `fragment_container` 및 를제공하지않는다음예제에서는Android가해당ID를조각에할당합니다.`android:tag`

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

### <a name="package-name-case"></a>패키지 이름 사례

Android에서는 패키지 이름에 대문자를 사용할 수 없습니다. 패키지 이름에 대문자 문자가 포함 된 경우 뷰를 확장 하려고 하면 예외가 throw 됩니다. 그러나 Xamarin.ios는 더 많은 정보를 제공 하 고 네임 스페이스에 대문자를 허용 합니다.

예를 들어 다음 코드 조각은 모두 Xamarin Android에서 작동 합니다. 그러나 두 번째 코드 조각은 순수한 Java 기반 `android.view.InflateException` Android 응용 프로그램에서을 throw 하는 것입니다.

```xml
<fragment class="com.example.DetailsFragment" android:id="@+id/fragment_content" android:layout_width="match_parent" android:layout_height="match_parent" />
```

또는

```xml
<fragment class="Com.Example.DetailsFragment" android:id="@+id/fragment_content" android:layout_width="match_parent" android:layout_height="match_parent" />
```


## <a name="fragment-lifecycle"></a>조각 수명 주기

조각에는 [호스팅 활동의 수명 주기와](~/android/app-fundamentals/activity-lifecycle/index.md)약간 독립적 이지만 여전히 영향을 받는 자체 수명 주기가 있습니다.
예를 들어 작업이 일시 중지 되 면 연결 된 모든 조각이 일시 중지 됩니다. 다음 다이어그램에서는 조각의 수명 주기를 간략하게 설명 합니다.

[![조각 수명 주기를 보여 주는 흐름 다이어그램](creating-a-fragment-images/fragment-lifecycle.png)](creating-a-fragment-images/fragment-lifecycle.png#lightbox)


### <a name="fragment-creation-lifecycle-methods"></a>조각 생성 수명 주기 메서드

아래 목록에는 생성 되는 조각의 수명 주기에 있는 다양 한 콜백의 흐름이 나와 있습니다.

- **`OnInflate()`** &ndash; 조각이 보기 레이아웃의 일부로 생성 될 때 호출 됩니다. 이는 조각이 XML 레이아웃 파일에서 선언적으로 만들어진 직후에 호출 될 수 있습니다. 조각은 아직 해당 작업에 연결 되어 있지 않지만 뷰 계층 구조에서 **활동**, **번들**및 **attributeset** 은 매개 변수로 전달 됩니다. 이 메서드는 **Attributeset** 를 구문 분석 하 고 나중에 조각에서 사용할 수 있는 특성을 저장 하는 데 가장 적합 합니다.

- **`OnAttach()`** &ndash; 조각이 활동과 연결 된 후에 호출 됩니다. 조각을 사용할 준비가 되었을 때 실행 되는 첫 번째 메서드입니다. 일반적으로 조각에서는 생성자를 구현 하거나 기본 생성자를 재정의 하면 안 됩니다. 이 메서드에서는 조각에 필요한 구성 요소를 초기화 해야 합니다.

- **`OnCreate()`** &ndash; 조각을 만들기 위해 활동에 의해 호출 됩니다. 이 메서드가 호출 되 면 호스팅 활동의 뷰 계층 구조가 완전히 인스턴스화되지 않을 수 있으므로 조각이 조각의 수명 주기에서 나중에까지 활동의 뷰 계층 구조를 사용 하지 않아야 합니다. 예를 들어이 메서드를 사용 하 여 응용 프로그램의 UI에 대 한 조정 또는 조정을 수행 하지 마세요. 조각에서 필요한 데이터를 수집 하기 시작할 수 있는 가장 빠른 시간입니다. 이 시점에서 조각이 UI 스레드에서 실행 되므로 시간이 오래 걸리는 처리를 수행 하지 않거나 백그라운드 스레드에서 해당 처리를 수행 합니다. **SetRetainInstance (true)** 를 호출 하면이 메서드를 건너뛸 수 있습니다.
    이 대안에 대해서는 아래에서 자세히 설명 합니다.

- **`OnCreateView()`** &ndash; 조각에 대 한 뷰를 만듭니다.
    이 메서드는 활동의 **OnCreate ()** 메서드가 완료 되 면 호출 됩니다. 이 시점에서 활동의 뷰 계층 구조와 상호 작용 하는 것이 안전 합니다. 이 메서드는 조각에서 사용 되는 뷰를 반환 해야 합니다.

- **`OnActivityCreated()`** 호스팅 활동에서 OnCreate를 완료 한 후에 호출 됩니다. &ndash;
    사용자 인터페이스에 대 한 최종 조정 작업이 현재 수행 되어야 합니다.

- **`OnStart()`** &ndash; 포함 하는 활동이 다시 시작 된 후에 호출 됩니다. 이렇게 하면 조각이 사용자에 게 표시 됩니다. 대부분의 경우 조각은 활동의 **OnStart ()** 메서드에 있는 코드를 포함 합니다.

- **`OnResume()`** &ndash; 사용자가 조각과 상호 작용할 수 있기 전에 호출 되는 마지막 메서드입니다. 이 방법에서 수행 해야 하는 코드 종류의 예로는 위치 서비스에 해당 하는 카메라와 같이 사용자가 조작할 수 있는 장치의 기능을 사용 하도록 설정 해야 합니다. 이러한 서비스는 과도 한 배터리 소모를 일으킬 수 있으며, 응용 프로그램은 배터리 수명을 보존 하는 데 사용을 최소화 해야 합니다.


### <a name="fragment-destruction-lifecycle-methods"></a>조각 소멸 수명 주기 메서드

다음 목록에서는 조각이 소멸 될 때 호출 되는 수명 주기 메서드에 대해 설명 합니다.

- **`OnPause()`** &ndash; 사용자는 더 이상 조각과 상호 작용할 수 없습니다. 이 상황은 다른 조각 작업에서이 조각을 수정 하거나 호스팅 작업이 일시 중지 되었기 때문에 발생 합니다. 이 조각을 호스팅하는 활동은 계속 표시 될 수 있습니다. 즉, 포커스의 활동이 부분적으로 투명 하거나 전체 화면을 차지 하지 않을 수 있습니다. 이 메서드가 활성화 되 면 사용자가 조각을 벗어나기 때문입니다. 조각에서 모든 변경 내용을 저장 해야 합니다.

- **`OnStop()`** &ndash; 조각이 더 이상 표시 되지 않습니다. 호스트 활동이 중지 되었거나 조각 작업이 활동에서이 작업을 수정 하 고 있습니다. 이 콜백은 **Activity**와 동일한 용도로 사용 됩니다.

- **`OnDestroyView()`** &ndash; 이 메서드는 뷰와 연결 된 리소스를 정리 하기 위해 호출 됩니다. 조각과 연결 된 뷰가 제거 될 때 호출 됩니다.

- **`OnDestroy()`** &ndash; 이 메서드는 조각이 더 이상 사용 되지 않을 때 호출 됩니다. 활동에 계속 연결 되어 있지만 조각이 더 이상 작동 하지 않습니다. 이 메서드는 카메라에 사용 될 수 있는 [**SurfaceView**](xref:Android.Views.SurfaceView) 같이 조각에서 사용 중인 모든 리소스를 해제 해야 합니다. **SetRetainInstance (true)** 를 호출 하면이 메서드를 건너뛸 수 있습니다. 이 대안에 대해서는 아래에서 자세히 설명 합니다.

- **`OnDetach()`** &ndash; 이 메서드는 조각이 더 이상 활동과 연결 되지 않기 바로 전에 호출 됩니다. 조각의 뷰 계층 구조는 더 이상 존재 하지 않으며 조각에서 사용 하는 모든 리소스를이 시점에서 해제 해야 합니다.


### <a name="using-setretaininstance"></a>SetRetainInstance 사용

작업을 다시 만들 때 조각이 완전히 제거 되지 않도록 지정할 수 있습니다. 클래스 `Fragment` 는이 용도로 메서드 `SetRetainInstance` 를 제공 합니다. 이 `true` 메서드에 전달 된 경우 활동이 다시 시작 되 면 조각의 동일한 인스턴스가 사용 됩니다. 이 경우 `OnCreate` 및 `OnDestroy` 수명 주기 콜백을 제외 하 고 모든 콜백 메서드가 호출 됩니다. 이 프로세스는 위에 표시 된 수명 주기 다이어그램 (녹색 점선으로 표시)에 나와 있습니다.


## <a name="fragment-state-management"></a>조각 상태 관리

조각은 인스턴스 `Bundle`를 사용 하 여 조각 수명 주기 중에 상태를 저장 하 고 복원할 수 있습니다. 번들을 사용 하면 조각이 키/값 쌍으로 데이터를 저장할 수 있으며 많은 메모리가 필요 하지 않은 간단한 데이터에 유용 합니다. 조각은을 호출 하 여 해당 상태를 `OnSaveInstanceState`저장할 수 있습니다.

```csharp
public override void OnSaveInstanceState(Bundle outState)
{
    base.OnSaveInstanceState(outState);
    outState.PutInt("current_choice", _currentCheckPosition);
}
```

조각의 새 `Bundle` 인스턴스를 만들면에 저장 된 상태를 새 인스턴스의 `OnCreate`, `OnCreateView`및 `OnActivityCreated` 메서드를 통해 새 인스턴스에 사용할 수 있게 됩니다.
다음 샘플에서는 `current_choice` `Bundle`에서 값을 검색 하는 방법을 보여 줍니다.

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

재정의 `OnSaveInstanceState` 는 위의 예에 있는 `current_choice` 값과 같은 방향 변경 내용에 따라 조각에 임시 데이터를 저장 하는 데 적합 한 메커니즘입니다. 그러나의 `OnSaveInstanceState` 기본 구현은 ID가 할당 된 모든 뷰에 대해 UI에 임시 데이터를 저장 하는 일을 담당 합니다. 예를 들어 다음과 같이 XML에 정의 된 `EditText` 요소가 있는 응용 프로그램을 살펴봅니다.

```xml
<EditText android:id="@+id/myText"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"/>
```

컨트롤 `EditText` `OnSaveInstanceState` 에가 할당 되어 있으므로이 호출 되 면 조각이 자동으로 위젯에 데이터를 저장 합니다. `id`


### <a name="bundle-limitations"></a>번들 제한 사항

를 사용 `OnSaveInstanceState` 하면 임시 데이터를 쉽게 저장할 수 있지만이 메서드를 사용 하면 몇 가지 제한 사항이 있습니다.

- 조각이 백 스택에 추가 되지 않은 경우 사용자가 **뒤로** 단추를 누르면 상태가 복원 되지 않습니다.

- 번들을 사용 하 여 데이터를 저장 하는 경우 해당 데이터는 serialize 됩니다. 이로 인해 처리가 지연 될 수 있습니다.


## <a name="contributing-to-the-menu"></a>메뉴에 기여

조각은 호스팅 활동의 메뉴에 항목을 제공할 수 있습니다.
활동은 메뉴 항목을 먼저 처리 합니다. 활동에 처리기가 없는 경우 해당 이벤트는 조각으로 전달 된 후 처리 됩니다.

활동의 메뉴에 항목을 추가 하려면 조각에서 두 가지를 수행 해야 합니다.
첫째, 조각은 다음 코드와 같이 메서드 `OnCreateOptionsMenu` 를 구현 하 고 해당 항목을 메뉴에 추가 해야 합니다.

```csharp
public override void OnCreateOptionsMenu(IMenu menu, MenuInflater menuInflater)
{
    menuInflater.Inflate(Resource.Menu.menu_fragment_vehicle_list, menu);
    base.OnCreateOptionsMenu(menu, menuInflater);
}
```

이전 코드 조각의 메뉴는 파일 `menu_fragment_vehicle_list.xml`에 있는 다음 XML에서 팽창 됩니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:id="@+id/add_vehicle"
        android:icon="@drawable/ic_menu_add_data"
        android:title="@string/add_vehicle" />
</menu>
```

그런 다음 조각이를 호출 `SetHasOptionsMenu(true)`해야 합니다. 이 메서드에 대 한 호출은 옵션 메뉴에 기여할 메뉴 항목이 조각에 있음을 Android에 알립니다. 이 메서드에 대 한 호출을 수행 하지 않으면 조각의 메뉴 항목이 활동의 옵션 메뉴에 추가 되지 않습니다. 이는 다음 코드 조각과 같이 일반적으로 `OnCreate()`수명 주기 메서드에서 수행 됩니다.

```csharp
public override void OnCreate(Bundle savedState)
{
    base.OnCreate(savedState);
    SetHasOptionsMenu(true);
}
```

다음 화면에서는이 메뉴가 어떻게 표시 되는지 보여 줍니다.

[![메뉴 항목을 표시 하는 내 여행 앱의 예제 스크린샷](creating-a-fragment-images/fragment-menu-example.png)](creating-a-fragment-images/fragment-menu-example.png#lightbox)
