---
title: 조각 만들기
ms.prod: xamarin
ms.assetid: F2997242-BC29-1440-7F1A-CFC447CD73FA
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/07/2018
ms.openlocfilehash: 0e8d3748c7ddd337cf2f27f5b272b208e79d503a
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027511"
---
# <a name="creating-a-fragment"></a>조각 만들기

조각을 만들려면 클래스가 `Android.App.Fragment`에서 상속한 다음, `OnCreateView` 메서드를 재정의해야 합니다. `OnCreateView`는 화면에 조각을 배치할 때 호스팅 작업을 통해 호출되며, `View`를 반환합니다. 일반적인 `OnCreateView`는 레이아웃 파일을 확장한 후 부모 컨테이너에 연결하여 이 `View`를 만듭니다. Android는 부모의 레이아웃 매개 변수를 조각의 UI에 적용하기 때문에 컨테이너의 특징이 중요합니다. 다음 예제는 이러한 과정을 보여 줍니다.

```csharp
public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
{
    return inflater.Inflate(Resource.Layout.Example_Fragment, container, false);
}
```

위의 코드는 `Resource.Layout.Example_Fragment` 보기를 확장한 후 `ViewGroup` 컨테이너에 자식 보기로 추가합니다.

> [!NOTE]
> 조각 하위 클래스에는 공용 기본 인수 없는 생성자가 있어야 합니다.

## <a name="adding-a-fragment-to-an-activity"></a>작업에 조각 추가

다음 두 가지 방법으로 작업 내부에 조각을 호스팅할 수 있습니다.

- **선언적으로** &ndash; `<Fragment>` 태그를 사용하여 `.axml` 레이아웃 파일 내부에서 선언적으로 조각을 사용할 수 있습니다.

- **프로그래밍 방식으로** &ndash; `FragmentManager` 클래스의 API를 사용하여 조각을 동적으로 인스턴스화할 수도 있습니다.

`FragmentManager` 클래스를 통한 프로그래밍 방식 사용 방법은 이 가이드의 뒷부분에서 설명합니다.

### <a name="using-a-fragment-declaratively"></a>선언적으로 조각 사용

레이아웃 내부에 조각을 추가하려면 `class` 특성 또는 `android:name` 특성을 제공하여 `<fragment>` 태그를 사용한 다음, 조각을 식별해야 합니다. 다음 코드 조각에서는 `class` 특성을 사용하여 `fragment`를 선언하는 방법을 보여줍니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment class="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

다음 코드 조각에서는 조각 클래스를 식별하는 `android:name` 특성을 사용하여 `fragment`를 선언하는 방법을 보여줍니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment android:name="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

작업이 생성되면 Android는 레이아웃 파일에 지정된 각 조각을 인스턴스화하고, `OnCreateView`에서 생성된 보기를 `Fragment` 요소 자리에 삽입합니다.
작업에 선언적으로 추가된 조각은 정적이며 소멸될 때까지 작업에 남아 있습니다. 조각이 연결된 작업의 수명 주기가 끝나기 전에는 이러한 조각을 동적으로 바꾸거나 제거할 수 없습니다.

각 조각에는 고유 식별자가 할당되어야 합니다.

- **android:id** &ndash; 레이아웃 파일의 다른 UI 요소와 마찬가지로 고유한 ID입니다.

- **android:tag** &ndash; 이 특성은 고유한 문자열입니다.

위의 두 메서드 중 어떤 것도 사용하지 않으면 조각은 컨테이너 보기의 ID를 가정합니다. `android:id` 또는 `android:tag`를 하나도 제공하지 않는 다음 예제에서는 Android가 조각에 ID `fragment_container`를 할당합니다.

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

Android는 패키지 이름에 대문자를 사용할 수 없습니다. 패키지 이름에 대문자가 포함된 경우 보기를 확장하려고 하면 예외가 throw됩니다. 그러나 Xamarin.Android는 허용 범위가 더 넓으며, 네임스페이스에 대문자를 허용합니다.

예를 들어 다음 두 코드 조각은 모두 Xamarin.Android에서 작동합니다. 그러나 두 번째 코드 조각은 순수 Java 기반 Android 애플리케이션에서 `android.view.InflateException`을 throw합니다.

```xml
<fragment class="com.example.DetailsFragment" android:id="@+id/fragment_content" android:layout_width="match_parent" android:layout_height="match_parent" />
```

또는

```xml
<fragment class="Com.Example.DetailsFragment" android:id="@+id/fragment_content" android:layout_width="match_parent" android:layout_height="match_parent" />
```

## <a name="fragment-lifecycle"></a>조각 수명 주기

조각마다 자체 수명 주기가 있는데, 이 수명 주기는 약간은 독립적이지만 여전히 [호스팅 작업 수명 주기](~/android/app-fundamentals/activity-lifecycle/index.md)의 영향을 받습니다.
예를 들어 작업이 일시 중지되면 작업과 연결된 모든 조각이 일시 중지됩니다. 다음 다이어그램에서는 조각의 수명 주기를 간략하게 설명합니다.

[![조각 수명 주기를 보여주는 흐름 다이어그램](creating-a-fragment-images/fragment-lifecycle.png)](creating-a-fragment-images/fragment-lifecycle.png#lightbox)

### <a name="fragment-creation-lifecycle-methods"></a>조각 생성 수명 주기 메서드

아래 목록은 조각이 만들어질 때 조각 수명 주기의 다양한 콜백 흐름을 보여줍니다.

- **`OnInflate()`** &ndash; 보기 레이아웃의 일부로 조각이 만들어질 때 호출됩니다. XML 레이아웃 파일에서 선언적으로 조각이 만들어진 직후에 호출될 수 있습니다. 조각이 아직 해당 작업에 연결되지 않았지만, 보기 계층 구조의 **Activity**, **Bundle**및 **AttributeSet**는 매개 변수로 전달됩니다. 이 메서드는 **AttributeSet**를 구문 분석하고 나중에 조각에서 사용할 수 있는 특성을 저장하는 데 가장 적합합니다.

- **`OnAttach()`** &ndash; 조각이 작업과 연결된 후 호출됩니다. 조각을 사용할 준비가 되었을 때 첫 번째로 실행되는 메서드입니다. 일반적으로 조각에서 생성자를 구현하거나 기본 생성자를 재정의하면 안 됩니다. 조각에 필요한 구성 요소를 이 메서드에서 초기화해야 합니다.

- **`OnCreate()`** &ndash; 조각을 만드는 작업에서 호출됩니다. 이 메서드가 호출되면 호스팅 작업의 보기 계층 구조가 완전히 인스턴스화되지 않을 수 있으므로, 조각의 수명 주기가 후반부를 넘어가기 전에는 작업의 보기 계층 구조를 조각에서 사용하면 안 됩니다. 예를 들어 애플리케이션의 UI를 조정할 때 이 메서드를 사용하지 마세요. 이 시기는 조각이 필요한 데이터 수집을 시작할 수 있는 초기 단계입니다. 현재는 조각이 UI 스레드에서 실행되므로, 시간이 오래 걸리는 프로세스를 수행하거나 이러한 프로세스를 백그라운드 스레드에서 수행하지 마세요. **SetRetainInstance(true)** 가 호출되면 이 메서드를 건너뛸 수 있습니다.
    이 대체 메서드에 대해서는 아래에서 자세히 설명하겠습니다.

- **`OnCreateView()`** &ndash; 조각의 보기를 만듭니다.
    이 메서드는 작업의 **OnCreate()** 메서드가 완료되면 호출됩니다. 현재는 작업의 보기 계층 구조와 상호 작용하는 것이 안전합니다. 이 메서드는 조각에서 사용될 보기를 반환해야 합니다.

- **`OnActivityCreated()`** &ndash; **Activity.OnCreate**가 완료된 후 호스팅 작업에서 호출됩니다.
    이 시점에 사용자 인터페이스의 최종 조정 작업을 수행해야 합니다.

- **`OnStart()`** &ndash; 이 메서드를 포함하고 있는 작업이 계속될 때 호출됩니다. 그러면 조각이 사용자에게 표시됩니다. 대부분의 경우 조각에 코드가 포함되며, 그렇지 않으면 작업의 **OnStart()** 메서드에 코드가 포함됩니다.

- **`OnResume()`** &ndash; 마지막으로 이 메서드가 호출되면 사용자가 조각과 상호 작용할 수 있습니다. 이 메서드에서 수행해야 하는 코드 종류의 예로는 위치에서 서비스를 제공하는 카메라처럼 사용자가 상호 작용할 수 있는 디바이스의 기능을 활성화는 것을 들 수 있습니다. 이러한 서비스는 배터리를 과도하게 소모할 수 있으므로, 배터리 수명을 유지할 수 있도록 애플리케이션에서 이러한 서비스 사용을 최소화해야 합니다.

### <a name="fragment-destruction-lifecycle-methods"></a>조각 소멸 수명 주기 메서드

다음 목록에서는 조각이 소멸될 때 호출되는 수명 주기 메서드에 대해 설명합니다.

- **`OnPause()`** &ndash; 사용자가 더 이상 조각과 상호 작용할 수 없습니다. 다른 조각 작업에서 이 조각을 수정하거나 호스팅 작업이 일시 중지될 때 이 상황이 발생합니다. 이 조각을 호스팅하는 작업이 계속 표시될 수 있습니다. 즉, 현재 포커스가 있는 작업이 부분적으로 투명하거나 전체 화면을 차지하지 않을 수 있습니다. 사용자가 조각을 벗어날 때 발생하는 첫 번째 조짐은 이 메서드가 활성화되는 것입니다. 조각에서 모든 변경 내용을 저장해야 합니다.

- **`OnStop()`** &ndash; 조각이 더 이상 표시되지 않습니다. 호스트 작업이 중지되었거나, 조각 연산에서 작업을 수정하고 있는 것입니다. 이 콜백은 **Activity.OnStop**과 동일한 용도로 사용됩니다.

- **`OnDestroyView()`** &ndash; 이 메서드는 보기와 연결된 리소스를 정리하기 위해 호출됩니다. 조각과 연결된 보기가 제거될 때 호출됩니다.

- **`OnDestroy()`** &ndash; 이 메서드는 조각이 더 이상 사용되지 않을 때 호출됩니다. 여전히 작업과 연결되어 있지만, 조각이 더 이상 작동하지 않습니다. 이 메서드는 카메라에 사용할 수 있는 [**SurfaceView**](xref:Android.Views.SurfaceView)처럼 조각에서 사용 중인 리소스를 해제해야 합니다. **SetRetainInstance(true)** 가 호출되면 이 메서드를 건너뛸 수 있습니다. 이 대체 메서드에 대해서는 아래에서 자세히 설명하겠습니다.

- **`OnDetach()`** &ndash; 이 메서드는 조각과 작업의 연결이 해제되기 직전에 호출됩니다. 조각의 보기 계층 구조가 더 이상 존재하지 않으며, 조각에서 사용하는 모든 리소스를 이 시점에서 해제해야 합니다.

### <a name="using-setretaininstance"></a>SetRetainInstance 사용

작업을 다시 만들 때 조각이 완전히 제거되지 않도록 지정할 수 있습니다. `Fragment` 클래스는 이 용도로 `SetRetainInstance` 메서드를 제공합니다. 이 메서드에 `true`가 전달되면 작업이 다시 시작될 때 조각의 동일한 인스턴스가 사용됩니다. 이 경우 `OnCreate` 및 `OnDestroy` 수명 주기 콜백을 제외한 모든 콜백 메서드가 호출됩니다. 이 프로세스는 위의 수명 주기 다이어그램에 녹색 점선으로 표시되어 있습니다.

## <a name="fragment-state-management"></a>조각 상태 관리

조각은 조각 수명 주기 중에 `Bundle` 인스턴스를 사용하여 상태를 저장하고 복원할 수 있습니다. Bundle을 사용하면 조각이 데이터를 키/값 쌍으로 저장할 수 있으며, 많은 메모리가 필요하지 않은 간단한 데이터에 유용합니다. 조각은 `OnSaveInstanceState`를 호출하여 상태를 저장할 수 있습니다.

```csharp
public override void OnSaveInstanceState(Bundle outState)
{
    base.OnSaveInstanceState(outState);
    outState.PutInt("current_choice", _currentCheckPosition);
}
```

조각의 새 인스턴스가 생성되면 `Bundle`에 저장된 상태를 새 인스턴스의 `OnCreate`, `OnCreateView` 및 `OnActivityCreated` 메서드를 통해 새 인스턴스에 사용할 수 있게 됩니다.
다음 샘플은 `Bundle`에서 `current_choice` 값을 검색하는 방법을 보여줍니다.

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

`OnSaveInstanceState`를 재정의하는 것은 위 예제의 `current_choice` 값처럼 방향 변경에 따라 임시 데이터를 조각에 저장하는 데 적합한 메커니즘입니다. 그러나 `OnSaveInstanceState`의 기본 구현에서는 ID가 할당된 모든 보기의 UI에 임시 데이터를 저장합니다. 예를 들어 다음과 같이 `EditText` 요소가 XML로 정의된 애플리케이션을 살펴보겠습니다.

```xml
<EditText android:id="@+id/myText"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"/>
```

`EditText` 컨트롤에 `id`가 할당되었기 때문에 `OnSaveInstanceState`가 호출될 때 조각이 자동으로 위젯에 데이터를 저장합니다.

### <a name="bundle-limitations"></a>Bundle 제한 사항

`OnSaveInstanceState`를 사용하면 임시 데이터를 쉽게 저장할 수 있지만, 이 메서드를 사용할 때 다음과 같은 제한 사항이 있습니다.

- 조각이 백 스택에 추가되지 않으면 사용자가 **뒤로** 단추를 누를 때 상태가 복원되지 않습니다.

- Bundle을 사용하여 데이터를 저장할 때 데이터가 직렬화됩니다. 이로 인해 처리가 지연될 수 있습니다.

## <a name="contributing-to-the-menu"></a>메뉴에 기여

조각은 호스팅 작업의 메뉴에 항목을 제공할 수 있습니다.
작업은 메뉴 항목을 가장 먼저 처리합니다. 작업에 처리기가 없는 경우 조각에 이벤트가 전달되고, 이 이벤트가 조각을 처리합니다.

작업의 메뉴에 항목을 추가하려면 조각에서 다음 두 가지를 수행해야 합니다.
첫 번째로, 다음 코드처럼 조각이 `OnCreateOptionsMenu` 메서드를 구현하고 해당 항목을 메뉴에 배치해야 합니다.

```csharp
public override void OnCreateOptionsMenu(IMenu menu, MenuInflater menuInflater)
{
    menuInflater.Inflate(Resource.Menu.menu_fragment_vehicle_list, menu);
    base.OnCreateOptionsMenu(menu, menuInflater);
}
```

이전 코드 조각의 메뉴는 `menu_fragment_vehicle_list.xml` 파일에 있는 다음 XML에서 확장됩니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:id="@+id/add_vehicle"
        android:icon="@drawable/ic_menu_add_data"
        android:title="@string/add_vehicle" />
</menu>
```

다음으로, 조각에서 `SetHasOptionsMenu(true)`를 호출해야 합니다. 이 메서드를 호출하면 옵션 메뉴에 추가할 메뉴 항목이 조각에 있음을 Android가 알게 됩니다. 이 메서드를 호출하지 않으면 조각의 메뉴 항목이 작업의 옵션 메뉴에 추가되지 않습니다. 이 작업은 일반적으로 다음 코드 조각처럼 `OnCreate()` 수명 주기 메서드에서 수행됩니다.

```csharp
public override void OnCreate(Bundle savedState)
{
    base.OnCreate(savedState);
    SetHasOptionsMenu(true);
}
```

다음 화면은 이 메뉴의 모양을 보여줍니다.

[![메뉴 항목을 표시하는 내 여행 앱의 예제 스크린샷](creating-a-fragment-images/fragment-menu-example.png)](creating-a-fragment-images/fragment-menu-example.png#lightbox)
