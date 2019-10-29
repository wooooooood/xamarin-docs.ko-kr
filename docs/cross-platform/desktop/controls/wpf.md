---
ms.assetid: 1BB412D1-FC3D-4E69-8B01-B976A3DB6328
title: 'WPF 및 Xamarin.ios: 유사성 & 차이점'
description: 이 문서에서는 WPF를 Xamarin.ios와 비교 하 고 대조 합니다. 컨트롤 템플릿, XAML, 바인딩 인프라, 데이터 템플릿, ItemsControl, UserControl, 탐색 및 URL 탐색에 대해 설명 합니다.
author: davidortinau
ms.author: daortin
ms.date: 04/26/2017
ms.openlocfilehash: 798839457a418d457bac83e6e20397722423dbac
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016480"
---
# <a name="wpf-vs-xamarinforms-similarities--differences"></a>WPF 및 Xamarin.ios: 유사성 & 차이점

## <a name="control-templates"></a>컨트롤 템플릿

WPF는 컨트롤에 대 한 시각화 명령 (`Button`, `ListBox` 등)을 제공 하는 *컨트롤 템플릿의* 개념을 지원 합니다. 위에서 설명한 것 처럼 Xamarin.ios는 네이티브 플랫폼 (iOS, Android 등)과 상호 작용 하 여 컨트롤을 시각화 하는 구체적인 _렌더링_ 클래스를 사용 합니다.

그러나 Xamarin.ios에는 `ControlTemplate` _형식이 있습니다._ `Page` 개체에 테마를 사용 하는 데 사용 됩니다. 일관 된 콘텐츠를 제공 하는 `Page`에 대 한 정의를 제공 하지만 페이지 사용자가 색, 글꼴 등을 변경할 수 있도록 하 고, 요소를 추가 하 여 응용 프로그램에 고유 하 게 만들 수도 있습니다.

이에 대 한 일반적인 용도는 인증 대화 상자, 프롬프트 및 표준화 된 기능을 제공 하는 것 이며 앱 내에서 사용자 지정할 수 있는 페이지 모양과 느낌을 제공 합니다. 이 지원의 일부로 익숙한 WPF 이름 컨트롤을 많이 사용 합니다.

1. `ContentPage`
2. `ContentView`
3. `ContentPresenter`
4. `TemplateBinding`

하지만 Xamarin.ios에서 동일한 용도를 제공 _하지_ 않는다는 것을 알아야 합니다. 이 기능에 대 한 자세한 내용은 [설명서 페이지](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md)를 참조 하세요.

## <a name="xaml"></a>XAML

XAML은 WPF 및 Xamarin.ios에 대 한 선언적 태그 언어로 사용 됩니다. 대부분의 경우 구문은 동일 합니다. 주요 차이점은 XAML 그래프에서 정의/만든 개체입니다.

- Xamarin.ios는 [XAML 2009 사양을](/dotnet/framework/xaml-services/xaml-2009-language-features/)지원 합니다. 이렇게 하면 제네릭 형식을 정의 하 고 생성자에 인수를 전달 하는 것 외에도 `string`s, `int`s 등의 데이터를 보다 쉽게 정의할 수 있습니다.

- 현재 `XamlReader`에서 WPF와 같은 XAML을 동적으로 로드할 수 있는 방법은 없습니다. 그러나 [NuGet 패키지](https://www.nuget.org/packages/Xamarin.Forms.Dynamic/) 를 사용 하 여 동일한 기본 기능을 얻을 수 있습니다.

### <a name="markup-extensions"></a>태그 확장

Xamarin.ios는 WPF와 매우 유사한 태그 확장을 통해 XAML 확장을 지원 합니다. 기본적으로 기본 구성 요소는 다음과 같습니다.

1. `{x:Array}`
2. `{Binding}`
3. `{DynamicResource}`
4. `{x:Null}`
5. `{x:Static}`
6. `{StaticResource}`
7. `{x:Type}`

또한이 클래스에는 XAML 2009 사양의 `{x:Reference}`와 Xamarin.ios에서 지원 되는 `ControlTemplate`의 특수 버전에 사용 되는 `{TemplateBinding}` 태그 확장이 포함 되어 있습니다.

> [!WARNING]
> 동일한 이름을 가진 `ControlTemplate` 지원도 동일 하지 않습니다.

Xamarin.ios는 사용자 지정 태그 확장도 지원 하지만 구현은 약간 다릅니다. WPF에서는 `MarkupExtension`-추상 기본 클래스에서 파생 해야 합니다. Xamarin.ios에서 인터페이스 `IMarkupExtension` 또는 `IMarkupExtension<T>` 더 유동적입니다.

WPF와 마찬가지로, 단일 필수 메서드는 태그 확장에서 값을 반환 하는 `ProvideValue` 메서드입니다.

## <a name="binding-infrastructure"></a>바인딩 인프라

핵심 개념 중 하나는 .NET 데이터 속성에 시각적 속성을 연결 하는 데이터 바인딩 인프라입니다. 이를 통해 MVVM와 같은 아키텍처 패턴을 사용할 수 있습니다. 기본 디자인은 동일 합니다. 즉, 바인딩할 수 있는 기본 클래스 [Bindableobject](xref:Xamarin.Forms.BindableObject)가 있습니다. WPF에서는 [DependencyObject](xref:System.Windows.DependencyObject) 클래스입니다. 이 기본 클래스는 데이터 바인딩의 대상으로 참여 하는 모든 개체에 대 한 루트 상위 항목으로 사용 됩니다. 그런 다음 파생 된 클래스는 속성 값에 대 한 지원 저장소 역할을 하는 [Bindableproperty](xref:Xamarin.Forms.BindableProperty) 개체를 노출 합니다. 이러한 개체는 WPF에서 [DependencyProperty](xref:System.Windows.DependencyProperty) 개체로 정의 됩니다.

### <a name="defining-bindable-properties"></a>바인딩 가능한 속성 정의

Xamarin.ios의 바인딩 가능한 속성에 대 한 정의는 WPF와 동일 합니다.

1. 개체는 `BindableObject`에서 파생 되어야 합니다.
2. 속성에 대 한 지원 저장소 키를 정의 하려면 선언 된 `BindableProperty` 형식의 공용 정적 필드가 있어야 합니다.
3. `GetValue` 및 `SetValue`를 사용 하 여 속성 값을 검색 하 고 변경 하는 공용 인스턴스 속성 래퍼가 있어야 합니다.

전체 예제는 [xamarin.ios의 바인딩 가능한 속성](~/xamarin-forms/xaml/bindable-properties.md)을 참조 하세요.

### <a name="attached-properties"></a>연결 된 속성

연결 된 속성은 바인딩 가능한 속성의 하위 집합이 며 WPF에서와 동일한 방식으로 작동 합니다. 주요 차이점은 속성 래퍼가이 사례에서 생략 소유 하는 클래스에 대 한 정적 get/set 메서드 집합으로 대체 된다는 것입니다. 자세한 내용은 [xamarin.ios의 연결 된 속성](~/xamarin-forms/xaml/attached-properties.md) 을 참조 하세요.

### <a name="using-the-binding-engine"></a>바인딩 엔진 사용

바인딩 엔진을 사용 하는 프로세스는 WPF의 경우와 동일 합니다. 소스 개체에 연결 된 `Binding` 개체 (.NET 형식) 및 선택적 속성 값 (생략 경우 WPF 처럼 소스 개체를 속성 자체로 처리 하는 경우)을 만들어 코드 숨김으로 활용할 수 있습니다. 그런 다음 `BindableObject`에 대 한 `SetBinding`를 사용 하 여 바인딩을 `BindableProperty`에 연결할 수 있습니다.

또는 `BindingExtension`를 사용 하 여 XAML에서 바인딩 관계를 정의할 수 있습니다. WPF의 확장과 동일한 기본 값을 갖습니다.

바인딩 지원 및 엔진은 WPF 보다 Silverlight 구현과 더 유사 합니다. Xamarin에서 구현 되지 않은 몇 가지 기능이 없습니다.

- 바인딩에는 다음 기능이 지원 되지 않습니다.
  - BindingGroupName
  - BindsDirectlyToSource
  - IsAsync
  - MultiBinding
  - NotifyOnSourceUpdated
  - NotifyOnTargetUpdated
  - NotifyOnValidationError
  - System.windows.data.binding.updatesourcetrigger
  - UpdateSourceExceptionFilter
  - ValidatesOnDataErrors
  - ValidatesOnExceptions
  - ValidationRules 컬렉션
  - XPath입니다.
  - XmlNamespaceManager

#### <a name="relativesource"></a>RelativeSource

`RelativeSource` 바인딩이 지원 되지 않습니다. WPF에서 XAML로 정의 된 다른 시각적 요소에 바인딩할 수 있습니다. Xamarin.ios에서는 `{x:Reference}` 태그 확장을 사용 하 여이 기능을 구현할 수 있습니다. 예를 들어, 텍스트 속성을 포함 하는 "otherControl" 라는 컨트롤이 있는 경우 다음과 같이 바인딩할 수 있습니다.

**WPF**

```xaml
Text={Binding RelativeSource={RelativeSource otherControl}, Path=Text}
```

**Xamarin.Forms**

```xaml
Text={Binding Source={x:Reference otherControl}, Path=Text}
```

동일한 기능을 `{RelativeSource Self}` 기능에 사용할 수 있습니다. 그러나 형식으로 상위 항목을 찾을 수 있는 경우 (`{RelativeSource FindAncestor}`)는 지원 되지 않습니다.

#### <a name="binding-context"></a>바인딩 컨텍스트

WPF에서 기본 바인딩 소스를 다시 지정 하는 `DataContext` 속성 값을 정의할 수 있습니다. 바인딩의 소스가 정의 되어 있지 않으면이 속성 값이 사용 됩니다. 값은 시각적 트리 아래로 상속 되므로 상위 수준에서 정의 된 다음 자식에서 사용할 수 있습니다.

Xamarin.ios에서이 기능은 사용 가능한 이지만 속성 이름은 `BindingContext`입니다.

#### <a name="value-converters"></a>값 변환기

값 변환기는 WPF와 마찬가지로 Xamarin.ios에서 완벽 하 게 지원 됩니다. 동일한 인터페이스 모양이 사용 되지만 Xamarin.ios는 `Xamarin.Forms` 네임 스페이스에 정의 된 인터페이스를 포함 합니다.

### <a name="model-view-viewmodel"></a>모델-뷰-ViewModel

MVVM는 WPF 및 Xamarin.ios에서 완전히 지원 됩니다.

WPF에는 사용 되는 기본 제공 `RoutedCommand`이 포함 되어 있습니다. Xamarin.ios에는 `ICommand` 인터페이스 정의 외에도 기본 제공 되는 명령 지원이 없습니다. 다양 한 MVVM 프레임 워크를 포함 하 여 MVVM를 구현 하는 데 필요한 기본 클래스를 추가할 수 있습니다.

#### <a name="inotifypropertychanged-and-inotifycollectionchanged"></a>INotifyPropertyChanged 및 INotifyCollectionChanged

두 인터페이스 모두 Xamarin. Forms 바인딩에서 완벽 하 게 지원 됩니다. 많은 XAML 기반 프레임 워크와 달리, Xamarin.ios의 백그라운드 스레드에서 속성 변경 알림이 발생할 수 있으며 (예: WPF와 마찬가지로) 바인딩 엔진은 UI 스레드로 적절히 전환 됩니다.

또한 두 환경 모두 `SynchronziationContext` 및 `async` / `await`를 지원 하 여 적절 한 스레드 마샬링을 수행 합니다. WPF는 모든 시각적 요소에 `Dispatcher` 클래스를 포함 하며, Xamarin. Forms에는 사용할 수 있는 정적 메서드 `Device.BeginInvokeOnMainThread` 있습니다 (플랫폼 간 코딩에는 `SynchronizationContext`이 선호 됨).

- Xamarin.ios에는 컬렉션 변경 알림을 지 원하는 `ObservableCollection<T>` 포함 되어 있습니다.
- `BindingBase.EnableCollectionSynchronization`를 사용 하 여 컬렉션에 대 한 크로스 스레드 업데이트를 사용 하도록 설정할 수 있습니다. API는 WPF 변형과 약간 다릅니다. 자세한 내용은 [문서를 참조](xref:Xamarin.Forms.BindingBase.EnableCollectionSynchronization*)하세요.

## <a name="data-templates"></a>데이터 템플릿

데이터 템플릿은 `ListView` 행 (셀)의 렌더링을 사용자 지정 하기 위해 Xamarin.ios에서 지원 됩니다. 콘텐츠 기반 컨트롤에 대 한 `DataTemplate`s를 활용할 수 있는 WPF와는 달리, Xamarin.ios는 현재 `ListView`에만 사용 합니다. 템플릿 정의는 인라인으로 정의 (`ItemTemplate` 속성에 할당 됨) 하거나 `ResourceDictionary`의 리소스로 정의 될 수 있습니다.

또한 WPF에 상응 하는 것 만큼 유연 하지 않습니다.

1. `DataTemplate`의 루트 요소는 _항상_ `ViewCell` 개체 여야 합니다.
2. 데이터 트리거는 데이터 템플릿에서 완전히 지원 되지만 트리거와 연결 된 속성의 유형을 나타내는 `DataType` 속성을 포함 해야 합니다.
3. `DataTemplateSelector`도 지원 되지만 `DataTemplate`에서 파생 되므로 `ItemTemplate` 속성 (vs)에 직접 할당 됩니다.  WPF의 `ItemTemplateSelector`).

## <a name="itemscontrol"></a>ItemsControl

Xamarin.ios의 `ItemsControl`에는 기본 제공 되는 형식이 없습니다. 하지만 [여기에서 사용할 수 있는 xamarin.ios에 대 한 사용자 지정](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Controls/ItemsControl.cs)이 있습니다.

## <a name="user-controls"></a>사용자 정의 컨트롤

WPF에서 `UserControl`s는 연결 된 동작이 있는 UI 섹션을 제공 하는 데 사용 됩니다. Xamarin.ios에서는 동일한 용도로 `ContentView`를 사용 합니다. 둘 다 XAML에서 바인딩과 포함을 지원 합니다.

## <a name="navigation"></a>탐색

WPF에는 "브라우저와 유사한" 탐색 기능을 제공 하는 데 사용할 수 있는 거의 사용 되지 않는 `NavigationService` 포함 되어 있습니다. 대부분의 앱은이에 대 한 다른 `Window` 요소 또는 데이터를 표시 하는 창의 다른 섹션을 대신 사용 했습니다.

전화 장치에서 다양 한 _화면이_ 종종 솔루션 이므로 Xamarin. 양식에는 여러 가지 형태의 탐색 지원이 포함 됩니다.

| 탐색 스타일 | 페이지 유형 |
|--- |--- |
|스택 기반 (푸시/pop)|NavigationPage|
|마스터/세부 정보|MasterDetailPage|
|탭|TabbedPage|
|왼쪽/오른쪽으로 살짝 밀기|CarouselView|

`NavigationPage`은 가장 일반적인 방법 이며 모든 페이지에는 탐색 스택에서 페이지를 밀거나 끄는 데 사용할 수 있는 `Navigation` 속성이 있습니다. 이는 WPF에서 발견 된 `NavigationService`와 가장 유사한 기능입니다.

### <a name="url-navigation"></a>URL 탐색

WPF는 데스크톱 지향 기술 이며, 명령줄 매개 변수를 허용 하 여 시작 동작을 직접 수행할 수 있습니다. Xamarin.ios는 [전체 URL 링크](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/) 를 사용 하 여 시작 시 페이지로 이동할 수 있습니다.
