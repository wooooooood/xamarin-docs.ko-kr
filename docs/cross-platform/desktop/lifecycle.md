---
ms.assetid: 7C132A7C-4973-4B2D-98DC-3661C08EA33F
title: WPF 및입니다. Xamarin.Forms 앱 수명 주기
description: 이 문서는 간의 유사점 및 차이점 Xamarin.Forms 및 WPF 응용 프로그램에 대 한 응용 프로그램 수명 주기를 비교합니다. 시각적 트리, 그래픽, 리소스 및 스타일에 대해서도 살펴봅니다.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 5f157f2bbf36076e542a5f96b912cb1788a99052
ms.sourcegitcommit: 64d6da88bb6ba222ab2decd2fdc8e95d377438a6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58175228"
---
# <a name="wpf-vs-xamarinforms-app-lifecycle"></a>WPF 및입니다. Xamarin.Forms 앱 수명 주기

Xamarin.Forms는 여러가지고, 특히 WPF는 XAML 기반 프레임 워크에서 디자인 지침을 사용 합니다. 그러나 다른 방법으로 다른 크게 걸쳐 마이그레이션을 수행 하는 동안 사용자에 대 한 고정 지점 수는 있습니다. 이 문서는 이러한 문제를 파악 하 고 Xamarin.Forms를 WPF 기술 브리지 수 있는 지침을 제공 하려고 합니다.

## <a name="app-lifecycle"></a>앱 수명 주기

WPF 및 Xamarin.Forms 간의 응용 프로그램 수명 주기가 비슷합니다. 외부 (플랫폼) 코드에서 시작 하 고 메서드 호출을 통해 UI를 시작 합니다. Xamarin.Forms 항상 다음 초기화 하 고 앱에 대 한 UI를 만듭니다는 플랫폼별 어셈블리에 시작 하는 것이 다릅니다.

**WPF**

- `Main method > App > MainWindow`

> [!NOTE]
> `Main` 메서드를 기본적으로 자동으로 생성 하 고 코드에 표시 되지 않습니다.

**Xamarin.Forms**

- **iOS** &ndash; `Main method > AppDelegate > App > ContentPage`
- **Android** &ndash; `MainActivity > App > ContentPage`
- **UWP** &ndash; `Main method > App(UWP) > MainPage(UWP) > App > ContentPage`

### <a name="application-class"></a>응용 프로그램 클래스

WPF 및 Xamarin.Forms를 둘 다는 `Application` 단일 항목으로 생성 된 클래스입니다. 대부분의 경우에서 필수는 아니지만이 엄격 하 게 wpf에서 앱은 사용자 지정 응용 프로그램을 제공 하려면이 클래스에서 파생 됩니다. 둘 다 노출는 `Application.Current` 속성 만들어진된 singleton을 찾으려고 합니다.

### <a name="global-properties--persistence"></a>전역 속성 + 지 속성

WPF 및 Xamarin.Forms를 둘 다를 `Application.Properties` 사전을 사용할 수 있는 응용 프로그램에서 어디서 나 액세스할 수 있는 전역 응용 프로그램 수준 개체를 저장할 수 있습니다. 주요 차이점은 해당 Xamarin.Forms 됩니다 _유지_ 앱 일시 중단 하 고 회복 됩니다 하는 경우 다시 로드 하는 경우 컬렉션에 저장 된 모든 기본 형식입니다. WPF는 동작을 자동으로 지원 하지 않습니다-대신 대부분의 개발자가 격리 된 저장소에 의존 하거나 기본 제공 사용 `Settings` 지원 합니다.

## <a name="defining-pages-and-the-visual-tree"></a>페이지 및 시각적 트리를 정의합니다.

WPF를 사용 하는 `Window` 모든 최상위 시각적 요소에 대 한 루트 요소로 합니다. 이 정보를 표시 하는 Windows 환경에서 HWND를 정의 합니다. 수 만들고 만큼 창을 WPF에서 원하는 만큼에 동시에 표시 합니다.

Xamarin.Forms에서 최상위 시각적 개체는 항상 플랫폼이 정의한-iOS에서 예를 들어, 것을 `UIWindow`입니다. Xamarin.Forms 렌더링을 사용 하 여 이러한 네이티브 플랫폼 표현으로 콘텐츠는 것을 `Page` 클래스입니다. 각 `Page` xamarin.forms에서 페이지를 나타내는 고유 "" 응용 프로그램에서 위치 하나를 표시 한 번만 합니다.

두 WPFs `Window` 및 Xamarin.Forms `Page` 포함을 `Title` 에 표시 된 제목 및 둘 다 영향을 주는 속성을 `Icon` 속성 페이지에 대 한 특정 아이콘을 표시 하려면 (**참고** 는 제목 및 아이콘 표시 되지 않습니다 항상 Xamarin.Forms에서). 또한 같은 배경색 또는 이미지를 둘 다에서 공통 시각적 속성을 변경할 수 있습니다.

기술적으로 두 가지 별도 플랫폼 보기에 렌더링할 수 (예: 두 개의 정의 `UIWindow` 외부 디스플레이 또는 AirPlay에 두 번째 렌더링 있고 개체), 그러려면 플랫폼별 코드가 필요 하 고 직접 지원 되는 기능의 아닙니다 자체 Xamarin.Forms입니다.

### <a name="views"></a>보기

두 프레임 워크에 대 한 시각적 계층은 유사 합니다. WPF는 wysiwyg (위지윅) 문서에 대 한 지원으로 인해 조금 더 깊이입니다.

**WPF**

```
DependencyObject - base class for all bindable things
   Visual - rendering mechanics
      UIElement - common events + interactions
         FrameworkElement - adds layout
            Shape - 2D graphics
            Control - interactive controls
```

**Xamarin.Forms**

```
BindableObject - base class for all bindable things
   Element - basic parent/child support + resources + effects
      VisualElement - adds visual rendering properties (color, fonts, transforms, etc.)
         View - layout + gesture support
```

### <a name="view-lifecycle"></a>뷰 수명 주기

Xamarin.Forms는 모바일 시나리오 주로 지향 합니다. 응용 프로그램은 이와 같이 _활성화_를 _일시 중단_, 및 _다시 활성화_ 사용자 상호 작용 하는 대로 합니다. 클릭 비슷합니다는 `Window` WPF 응용 프로그램에서 되며는 일련의 메서드 및 해당 이벤트를 재정의 하거나이 동작을 모니터링 하려면 후크 할 수 있습니다.

| 용도 | WPF 메서드 | Xamarin.Forms Method |
|--- |--- |--- |
|초기 활성화|ctor + Window.OnLoaded|ctor + Page.OnStart|
|표시|Window.IsVisibleChanged|Page.Appearing|
|Hidden|Window.IsVisibleChanged|Page.Disappearing|
|일시 중단/손실 포커스|Window.OnDeactivated|Page.OnSleep|
|활성화/가져왔습니다 포커스|Window.OnActivated|Page.OnResume|
|닫힘|Window.OnClosing + Window.OnClosed|N/A|


지원 숨기기/표시 된 자식 컨트롤 모두 WPF에서는 세 가지 상태 속성 `IsVisible` (표시, 숨김 및 축소) 합니다. Xamarin.Forms에서는 방금 통해 표시 되거나 숨겨지도록는 `IsVisible` 속성입니다.

### <a name="layout"></a>레이아웃

페이지 레이아웃을 동일한 2 패스 (측정값/정렬) WPF에서 발생 하는에서 발생 합니다. Xamarin.Forms의 다음 메서드를 재정의 하 여 페이지 레이아웃에 연결할 수 있습니다 `Page` 클래스:

| 메서드 | 용도 |
|--- |--- |
|OnChildMeasureInvalidated|자식 기본 크기 변경 되었습니다.|
|OnSizeAllocated|페이지 너비/높이 할당 되었습니다.|
|LayoutChanged 이벤트|페이지의 레이아웃/크기가 변경 되었습니다.|

있습니다 현재 라고 하는 전역 레이아웃 지지 않으며 전역 없는 `CompositionTarget.Rendering` 이벤트와 같은 WPF에서 찾을 수 있습니다.

#### <a name="common-layout-properties"></a>일반적인 레이아웃 속성

WPF 및 Xamarin.Forms 둘 다 지원 `Margin` 요소 주위의 제어 간격을 및 `Padding` 컨트롤 간격 _내에서_ 요소입니다. 또한 대부분의 Xamarin.Forms 레이아웃 보기에 간격 (예: 행 또는 열)을 제어 하는 속성이 있습니다.

또한 대부분의 요소에 영향을 미칠 부모 컨테이너에 배치 되는 방식을 속성이 있습니다.

| WPF | Xamarin.Forms | 용도 |
|--- |--- |--- |
|HorizontalAlignment|HorizontalOptions|왼쪽/Center/오른쪽/Stretch 옵션|
|VerticalAlignment|VerticalOptions|위쪽/Center/아래쪽/Stretch 옵션|

> [!NOTE]
> 이러한 속성의 실제 해석 부모 컨테이너에 따라 달라 집니다.

#### <a name="layout-views"></a>레이아웃 뷰

WPF 및 Xamarin.Forms 레이아웃 컨트롤을 자식 요소의 위치를 사용 합니다. 대부분의 경우에서 이러한에 매우 근접해 있는 서로 다른 기능에 따라 합니다.

| WPF | Xamarin.Forms | 레이아웃 스타일 |
|--- |--- |--- |
|StackPanel|StackLayout|왼쪽에서 오른쪽, 또는 위쪽-아래쪽 무한 스택|
|표|표|테이블 형식으로 (행 및 열)|
|DockPanel|N/A|창의 가장자리에 도킹|
|Canvas|AbsoluteLayout|픽셀/좌표 위치|
|WrapPanel|N/A|래핑 스택|
|N/A|RelativeLayout|상대 규칙 기반 위치|

> [!NOTE]
> Xamarin.Forms를 지원 하지 않습니다는 `GridSplitter`합니다.

두 플랫폼 모두 사용 하 여 _연결 된 속성_ 자식 세밀 하 게 합니다.

### <a name="rendering"></a>렌더링

WPF 및 Xamarin.Forms에 대 한 렌더링 역학은 근본적으로 다릅니다. Wpf에서 직접 만든 컨트롤 화면에서 픽셀에 콘텐츠를 렌더링 합니다. WPF 두 개체 그래프 유지 관리 (_트리_)이-합니다 _논리적 트리_ 코드 또는 XAML에 정의 된 대로 컨트롤을 나타내는 및 _시각적 트리_ 나타냅니다는 실제 렌더링 인 화면에서 발생 하는 (가상 그리기 메서드)를 통해 수행 하거나 직접 시각적 요소 또는 XAML 정의 통해 `ControlTemplate` 대체 하거나 사용자 지정할 수는 있습니다. 일반적으로 시각적 트리는 컨트롤, 암시적 내용에 대 한 레이블 주위에 테두리 등이 포함 되어 있으므로 더 복잡 합니다. WPF Api 집합이 포함 되어 있습니다 (`LogicalTreeHelper` 및 `VisualTreeHelper`) 두 개체 그래프 이러한 오류를 확인 합니다.

Xamarin.Forms 컨트롤 정의에 `Page` 는 매우 단순한 데이터 개체입니다. 논리적 트리 표현을 비슷합니다 있지만 자체적으로 콘텐츠를 렌더링 하지 않습니다. 이들은 합니다 _데이터 모델_ 는 요소의 렌더링에 영향을 줍니다. 실제 렌더링 하면 됩니다는 [집합이 구분 _시각적 렌더러_ 각 컨트롤 형식에 매핑되는](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)합니다. 이 렌더러는 각 플랫폼별 프로젝트에 플랫폼별 Xamarin.Forms 어셈블리에 등록 됩니다. 목록을 볼 수 있습니다 [여기](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)합니다. 바꾸거나 렌더러를 확장 하는 것 외에도 Xamarin.Forms 기능도 지원 [효과](~/xamarin-forms/app-fundamentals/effects/index.md) 플랫폼별 기준 네이티브 렌더링에 영향을 줄에 사용할 수 있는 합니다.

#### <a name="the-logicalvisual-tree"></a>논리/시각적 트리

Xamarin.forms-논리 트리를 이동 하려면 노출 된 API 이지만 리플렉션을 사용 하 여 동일한 정보를 얻을 수 있습니다. 예를 들어 [논리적 자식을 열거할 수 하는 방법을 다음과 같습니다](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Extensions/ElementExtensions.cs#L108) 리플렉션을 사용 하 여 합니다.

## <a name="graphics"></a>그래픽

Xamarin.Forms는 간단한 사각형 초과 기본 형식에 대해 그래픽 시스템을 다루지 않습니다 (`BoxView`). 와 같은 타사 라이브러리를 포함할 수 있습니다 [SkiaSharp](~/graphics-games/skiasharp/index.md) 플랫폼 간 2D 드로잉을 가져오려고 하거나 [UrhoSharp](~/graphics-games/urhosharp/index.md) 3D에 대 한 합니다.

## <a name="resources"></a>자료

WPF 및 Xamarin.Forms 둘 다 리소스 및 리소스 사전 개념을 가집니다. 임의의 개체 형식으로 배치할 수 있습니다는 `ResourceDictionary` 키로 사용 하 여를 조회 `{StaticResource}` 변경 되지 것입니다는 항목에 대 한 또는 `{DynamicResource}` 런타임 시 사전에 변경 될 수 있는 항목에 대 한 합니다. 사용량 및 mechanics 차이점 중 하나를 사용 하 여 동일합니다. Xamarin.Forms에서는 정의 해야 합니다 `ResourceDictionary` 할당할는 `Resources` 속성 WPF 미리 만들고를 할당 하는 반면 합니다.

예를 들어, 아래 정의 참조 하세요.

**WPF**

```xaml
<Window.Resources>
   <Color x:Key="redColor">#ff0000</Color>
   ...
</Window.Resources>
```

**Xamarin.Forms**

```xaml
<ContentPage.Resources>
   <ResourceDictionary>
      <Color x:Key="redColor">#ff0000</Color>
      ...
   </ResourceDictionary>
</ContentPage.Resources>
```

정의 하지 않는 경우는 `ResourceDictionary`, 런타임 오류가 생성 됩니다.

## <a name="styles"></a>스타일

스타일은 Xamarin.Forms에서도 완전히 지원 및 수 UI를 구성 하는 Xamarin.Forms 요소 테마를 사용 합니다. 트리거 (속성, 이벤트 및 데이터), 상속을 통해 지 `BasedOn`, 및 값에 대 한 리소스를 조회 합니다. 스타일은 요소에 적용 하거나 통해 명시적으로 `Style` 속성 또는 암시적으로 WPF와 마찬가지로 리소스 키-제공 안 함.

### <a name="device-styles"></a>장치 스타일

WPF에는 미리 정의 된 속성 집합이 있습니다. (같은 정적 클래스의 집합에 정적 값으로 저장 `SystemColors`) 시스템 색, 글꼴 및 메트릭 값과 리소스 키의 형태로 지정입니다. Xamarin.Forms는 유사 하지만 집합을 정의 [장치 스타일](~/xamarin-forms/user-interface/styles/device.md) 동일한 항목을 나타냅니다. 이러한 스타일 프레임 워크에서 제공 되며 런타임 환경 (예: 내게 필요한 옵션)에 따라 값으로 설정 됩니다.

**WPF**

```xaml
<Label Text="Title" Foreground="{DynamicResource {x:Static SystemColors.DesktopBrushKey}}" />
```

**Xamarin.Forms**

```xaml
<Label Text="Title" Style="{DynamicResource TitleStyle}" />
```
