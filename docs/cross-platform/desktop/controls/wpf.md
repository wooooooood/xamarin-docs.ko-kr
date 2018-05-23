---
ms.assetid: 1BB412D1-FC3D-4E69-8B01-B976A3DB6328
title: 'WPF 및 합니다. Xamarin.Forms: 유사점 및 차이점'
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: ac30a29a2b4982b2f995c9f717cf1893ca5d8b8a
ms.sourcegitcommit: 9f8e7393019791bbd6af4fefaa24a1602adabb4e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/23/2018
---
# <a name="wpf-vs-xamarinforms-similarities--differences"></a>WPF 및 합니다. Xamarin.Forms: 유사점 및 차이점

## <a name="control-templates"></a>컨트롤 템플릿

WPF의 개념을 지 원하는 *컨트롤 템플릿을* 컨트롤에 대 한 시각화 지침을 제공 하는 (`Button`, `ListBox`등.). Xamarin.Forms는 구체적인 위에서 설명 했 듯이 _렌더링_ 이 컨트롤을 시각화 하려면 네이티브 플랫폼 (iOS, Android, 등)와 상호 작용 하는 클래스입니다.

그러나 Xamarin.Forms _않습니다_ 가 `ControlTemplate` 형식-테마 설정에 사용 되 `Page` 개체입니다. 에 대 한 정의 제공는 `Page` 일관성 있는 콘텐츠를 제공 하는 하지만 사용자 페이지의 색, 글꼴 등을 변경 하 고도 응용 프로그램에 고유 하 게 하는 요소를 추가할 수 있습니다.

이 대 한 일반적인 사용에는 앱 내에서 지정할 수 있는 표준화 된 하지만 테마 사용 가능 페이지 모양 및 느낌 제공 및 인증 대화 상자, 표시 되는 메시지와 같은 작업은 같습니다. 이 지원의 일부로 많은 친숙 한 이름이 지정 된 WPF 컨트롤 사용 됩니다.

1. `ContentPage`
2. `ContentView`
3. `ContentPresenter`
4. `TemplateBinding`

이러한 해야 하지만 _하지_ Xamarin.Forms에 같은 목적을 처리 합니다. 이 기능에 대 한 자세한 내용은 체크 아웃 된 [설명서 페이지](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md)합니다.

## <a name="xaml"></a>XAML

XAML은 선언적 태그 언어로 WPF 및 Xamarin.Forms에 사용 됩니다. 대부분의 경우 동일-주요 차이점은 XAML 그래프로 정의/생성 된 개체를 합니다.

- Xamarin.Forms 지원는 [XAML 2009 사양](/dotnet/framework/xaml-services/xaml-2009-language-features/); 이렇게 하면 데이터와 같은 정의를 쉽게 `string`s, `int`s, 등도 같이 정의 된 제네릭 형식 및 인수 생성자에 전달 합니다.

- WPF와 처럼 XAML을 동적으로 로드할 수 없으므로 현재는 `XamlReader`합니다. 와 같은 기본 기능을 얻을 수는 [NuGet 패키지](https://www.nuget.org/packages/Xamarin.Forms.Dynamic/) 있지만 합니다.

### <a name="markup-extensions"></a>태그 확장

Xamarin.Forms는 XAML을 WPF 매우 유사 하 게 태그 확장을 통해 확장을 지원 합니다. 기본적으로 동일한 기본 빌딩 블록에 있습니다.

1. `{x:Array}`
2. `{Binding}`
3. `{DynamicResource}`
4. `{x:Null}`
5. `{x:Static}`
6. `{StaticResource}`
7. `{x:Type}`

또한 포함 `{x:Reference}` XAML 2009 사양에서 및 `{TemplateBinding}` 의 특수 버전에 사용 되는 태그 확장 `ControlTemplate` Xamarin.Forms에서 지원 합니다.

> [!WARNING]
> `ControlTemplate` 지원 동일 하지 않습니다-동일한 이름을 갖는 경우에 합니다.

Xamarin.Forms에는 사용자 지정 태그 확장에도 지원 하지만 구현은 약간 다릅니다. 파생 해야 WPF에서 `MarkupExtension` -추상 기본 클래스입니다. Xamarin.forms에 아래 템플릿으로 바뀝니다 인터페이스 `IMarkupExtension` 또는 `IMarkupExtension<T>` 있는 보다 유연 합니다.

WPF에서와 마찬가지로 단일 필요한 메서드는 한 `ProvideValue` 메서드를 태그 확장에서 값을 반환 합니다.

## <a name="binding-infrastructure"></a>바인딩 인프라

통해 적용 되는 핵심 개념 중 하나에.NET 데이터 속성에 시각적 속성을 연결 하는 데이터 바인딩 인프라입니다. 이 통해 MVVM 같은 아키텍처 패턴. 기본 디자인은 동일-는 바인딩할 수 있는 기본 클래스가 있는 [BindableObject](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/),이 wpf에서는 [DependencyObject](https://msdn.microsoft.com/en-us/library/system.windows.dependencyobject(v=vs.110).aspx) 클래스입니다. 이 기본 클래스는 데이터 바인딩에서 대상으로 참여 하는 모든 개체에 대 한 루트 상위 항목으로 사용 됩니다. 파생된 클래스를 노출 합니다 [BindableProperty](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) 속성 값에 대 한 지원 저장소로 역할을 하는 개체 (정의 되며 [DependencyProperty](https://msdn.microsoft.com/library/system.windows.dependencyproperty(v=vs.110).aspx) wpf에서 개체).

### <a name="defining-bindable-properties"></a>바인딩 가능한 속성 정의

Xamarin.Forms에 바인딩 가능한 속성에 대 한 정의 WPF와 같습니다.
1. 개체에서 파생 되어야 `BindableObject`합니다.
2. 형식의 공용 정적 필드 있어야 `BindableProperty` 선언 된 속성에 대 한 지원 저장소 키를 정의 합니다.
3. 사용 하는 public 인스턴스 속성 래퍼가 있어야 `GetValue` 및 `SetValue` 검색 하 고 속성 값을 변경 합니다.

전체 예제를 참조 하십시오. [Xamarin.Forms에 바인딩 가능한 속성](~/xamarin-forms/xaml/bindable-properties.md)합니다.

### <a name="attached-properties"></a>연결 된 속성

연결 된 속성은 바인딩 가능한 속성의 하위 집합 및 WPF에는 동일한 방식으로 작동 합니다. 주요 차이점은 속성 래퍼가 경우 매개 변수가 생략 되는 및 소유 하는 클래스에 정적 get/set 메서드 집합이 아래 템플릿으로 바뀝니다. 참조 [Xamarin.Forms에 연결 된 속성](~/xamarin-forms/xaml/attached-properties.md) 자세한 정보에 대 한 합니다.

### <a name="using-the-binding-engine"></a>바인딩 엔진을 사용 하 여

바인딩 엔진을 사용 하기 위한 프로세스 WPF에 있는 것과 같습니다. 만들어서 코드 숨김에서 활용할 수 있습니다는 `Binding` 소스 개체 (모든.NET 유형)와 선택적 속성 값에 연결 된 개체 (경우 생략 하면 소스 개체도 처리 속성-자체와 마찬가지로 WPF). 사용할 수 있습니다 `SetBinding` 에 `BindableObject` 에 바인딩을 연결 하는 `BindableProperty`합니다.

또는 사용 하 여 XAML에서 바인딩 관계를 정의할 수 있습니다는 `BindingExtension`합니다. WPF의 확장 프로그램으로 동일한 기본 값이 있습니다.

바인딩 지원 및 엔진은 WPF 보다 Silverlight 구현 더 유사 합니다. 여러 가지 누락 된 기능이 있는 Xamarin.Forms에서는 구현 되지 않았습니다.

- 바인딩에 다음과 같은 기능에 대 한 지원 되지 않습니다.
    - BindingGroupName
    - BindsDirectlyToSource
    - IsAsync
    - MultiBinding
    - NotifyOnSourceUpdated
    - NotifyOnTargetUpdated
    - NotifyOnValidationError
    - UpdateSourceTrigger
    - UpdateSourceExceptionFilter
    - ValidatesOnDataErrors
    - ValidatesOnExceptions
    - Validationrule 컬렉션
    - XPath
    - XmlNamespaceManager
- `Binding.Mode` 지원 하지 않습니다 `OneTime`, 사용할 `OneWay`합니다.

#### <a name="relativesource"></a>RelativeSource

에 대 한 지원은 `RelativeSource` 바인딩. WPF에서 수 XAML에서 정의 된 기타 시각적 요소를 바인딩할 수 있습니다. Xamarin.forms에이 동일한 기능을 통해서도 얻을 수 있으므로 `{x:Reference}` 태그 확장 합니다. 예를 들어 있다고 가정해 봅시다 텍스트 속성을 가진 이름 "otherControl" 인 컨트롤을 바인딩할 수 있습니다에 다음과 같이 합니다.

**WPF**

```xaml
Text={Binding RelativeSource={RelativeSource otherControl}, Path=Text}
```

**Xamarin.Forms**

```xaml
Text={Binding Source={x:Reference otherControl}, Path=Text}
```

에 대 한 것과 동일한 기능을 사용할 수는 `{RelativeSource Self}` 기능입니다. 그러나 유형별로 상위 항목 찾기에 대 한 지원 되지 않습니다 (`{RelativeSource FindAncestor}`).

#### <a name="binding-context"></a>바인딩 컨텍스트

WPF에서 정의할 수 있습니다는 `DataContext` 속성 값이 바인딩 소스 기본 어떤 reprents 합니다. 바인딩에 대 한 소스를 정의 하지 않은 경우이 속성 값이 사용 됩니다. 값이 하므로 더 높은 수준에서 정의 하 고 다음 자식에 의해 사용 되는 시각적 트리 아래로 상속 됩니다.

Xamarin.Forms에이 동일한 기능을 사용할 수 있는, 있지만 속성 이름이 `BindingContext`합니다.

#### <a name="value-converters"></a>값 변환기

값 변환기는 WPF 마찬가지로 Xamarin.Forms-에서 완벽 하 게 지원 됩니다. 동일한 인터페이스 셰이프는 사용 되지 않지만 Xamarin.Forms에 정의 된 인터페이스에는 `Xamarin.Forms` 네임 스페이스입니다.

### <a name="model-view-viewmodel"></a>모델-뷰-ViewModel

MVVM은 완전히 WPF 및 Xamarin.Forms를 모두 지원 합니다.

WPF에는 기본적으로 포함 `RoutedCommand` 를 사용 하는 경우도 있습니다. Xamarin.Forms은 이외의 기본 제공 명령 지원 하지 않습니다는 `ICommand` 인터페이스 정의 합니다. 다양 한 MVVM를 구현 하는 데 필요한 기본 클래스를 추가 하려면 MVVM 프레임 워크를 포함할 수 있습니다.

#### <a name="inotifypropertychanged-and-inotifycollectionchanged"></a>INotifyPropertyChanged 및 INotifyCollectionChanged

두 인터페이스 모두 Xamarin.Forms 바인딩에서 완전히 지원 됩니다. 많은 XAML 기반 프레임 워크, 속성 변경 알림을 (WPF) 마찬가지로 xamarin.forms 백그라운드 스레드에서 발생할 수 있는 달리 바인딩 엔진을 UI 스레드에 제대로 전환 됩니다.

또한 두 환경 모두 지원 `SynchronziationContext` 및 `async` / `await` 올바른 스레드 마샬링 작업을 수행 하 합니다. WPF 포함는 `Dispatcher` 클래스 Xamarin.Forms에는 정적 메서드가 모든 시각적 요소에 `Device.BeginInvokeOnMainThread` 사용할 수도 있습니다 (하지만 `SynchronizationContext` 는 플랫폼 간 코딩 하기 위한 것이 좋습니다).

- Xamarin.Forms에 포함 되어는 `ObservableCollection<T>` 컬렉션 변경 알림도 지원입니다.
- 사용할 수 있습니다 `BindingBase.EnableCollectionSynchronization` 컬렉션에 대 한 크로스 스레드 업데이트할 수 있도록 합니다. API는 약간 다른 WPF 변형 [사용 정보에 대 한 문서를 확인](https://developer.xamarin.com/api/member/Xamarin.Forms.BindingBase.EnableCollectionSynchronization/)합니다.

## <a name="data-templates"></a>데이터 템플릿

데이터 서식 파일의 렌더링을 사용자 지정 하는 Xamarin.Forms에서는 사용할 수는 `ListView` 행 (셀). 활용할 수 있는 WPF 달리 `DataTemplate`Xamarin.Forms 현재 모든 콘텐츠 기반 컨트롤에 대 한 s만 사용 하 여 자신에 게 한 `ListView`합니다. 템플릿 정의는 인라인으로 정의 될 수 있습니다 (에 할당 된는 `ItemTemplate` 속성), 또는에 리소스로 `ResourceDictionary`합니다.

또한 매우 유연 하 게 해당 WPF 대응 하지 않습니다.

1. 루트 요소는 `DataTemplate` 해야 _항상_ 수는 `ViewCell` 개체입니다.
2. 데이터 트리거는 데이터 서식 파일에서 완벽 하 게 지원 되지만 포함 되어야 합니다는 `DataType` 트리거 연결 된 속성의 유형을 나타내는 속성입니다.
3. `DataTemplateSelector` 도 지원 하지만에서 파생 `DataTemplate` 를에 직접 할당만 `ItemTemplate` 속성 (및 `ItemTemplateSelector` WPF에서).

## <a name="itemscontrol"></a>ItemsControl

에 기본 제공 equivelent 없습니다는 `ItemsControl` xamarin.forms; 이지만 [Xamarin.Forms 사용 가능한 여기에 대 한 사용자 지정 1](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Controls/ItemsControl.cs)합니다.

## <a name="user-controls"></a>사용자 정의 컨트롤

WPF에서 `UserControl`s는 동작에 연결 되어 있는 UI의 한 섹션을 제공 하는 데 사용 됩니다. Xamarin.Forms를 사용 하 여는 `ContentView` 같은 목적을 위해 합니다. XAML의 바인딩과 포함 지원합니다.

## <a name="navigation"></a>탐색

WPF는 거의 사용 되지 않는 포함 `NavigationService` "브라우저와 유사한" 탐색 기능을 제공 하 사용할 수 있는 합니다. 그러나 대부분의 응용 프로그램를 대신 사용 되는이 대해서는 신경 쓰지 하지 않은 다른 `Window` 요소 또는 다양 한 데이터를 표시 하기 위해 창의 섹션입니다.

다른 phone 장치에서 _화면_ 는 주로 솔루션 고 따라서 Xamarin.Forms 나와 여러 형태의 탐색에 대 한 지원:

| 탐색 스타일 | 페이지 유형 |
|--- |--- |
|스택 기반 (푸시/표시)|NavigationPage|
|마스터/세부 정보|MasterDetailPage|
|탭|TabbedPage|
|왼쪽/오른쪽으로 살짝|CarouselView|

`NavigationPage` 가장 일반적인 방법은 하며 모든 페이지에는 `Navigation` 페이지에 및 탐색 스택에서 팝 하거나 사용할 수 있는 속성입니다. 이것은 가장 가까운 equivelent에는 `NavigationService` WPF에서 찾을 수 있습니다.

### <a name="url-navigation"></a>URL 탐색

WPF는 데스크톱 중심으로 하는 기술 및 시작 동작을 직접는 명령줄 매개 변수를 허용할 수 있습니다. Xamarin.Forms צ ְ ײ [직접 URL 링크](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/) 시작 시에는 페이지로 이동할 수 있습니다.
