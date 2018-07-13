---
ms.assetid: 1BB412D1-FC3D-4E69-8B01-B976A3DB6328
title: 'WPF 및입니다. Xamarin.Forms: 유사성 및 차이점'
description: 이 문서는 비교 하 고 WPF Xamarin.Forms에 대조 합니다. 컨트롤 템플릿, XAML, 바인딩 인프라, 데이터 템플릿, ItemsControl, UserControl, 탐색 및 URL 탐색에 설명 합니다.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 4d6585715b2fc118bb350c242abccbc68791ec0b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998520"
---
# <a name="wpf-vs-xamarinforms-similarities--differences"></a>WPF 및입니다. Xamarin.Forms: 유사성 및 차이점

## <a name="control-templates"></a>컨트롤 템플릿

WPF는 개념을 지원 *컨트롤 템플릿을* 컨트롤에 대 한 시각화 지침을 제공 하는 (`Button`, `ListBox`등.). Xamarin.Forms 구체적인 사용 하 여 위에서 설명 했 듯이 _렌더링_ 이 컨트롤을 시각화 하려면 네이티브 플랫폼 (iOS, Android 등)와 상호 작용 하는 클래스입니다.

그러나 Xamarin.Forms _않습니다_ 가 `ControlTemplate` 유형-테마에 사용 됩니다 `Page` 개체입니다. 에 대 한 정의 제공 하는 `Page` 는 일관성 있는 콘텐츠를 제공 하지만 사용자 페이지의 색, 글꼴 등을 변경 하 고도 응용 프로그램에 고유 하 게 요소를 추가할 수 있습니다.

이 대 한 일반적인 사용은 인증 대화 상자, 표시 되는 메시지와 같은 앱 내에서 사용자 지정할 수 있는 표준화 되지만 테마 사용 가능 페이지 모양 및 느낌을 제공 하는 사항입니다. 이 지원의 일환으로, 많은 친숙 한 이름의 WPF 컨트롤 사용 됩니다.

1. `ContentPage`
2. `ContentView`
3. `ContentPresenter`
4. `TemplateBinding`

이 알아야 이지만 _하지_ Xamarin.Forms에 동일한 목적을 제공 합니다. 이 기능에 대 한 자세한 내용은 체크 아웃 합니다 [설명서 페이지](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md)합니다.

## <a name="xaml"></a>XAML

XAML은 WPF 및 Xamarin.Forms에 대 한 선언적 태그 언어로 사용 됩니다. 대부분의 경우 구문은 동일-주요 차이점은 XAML 그래프로 정의/생성 하는 개체입니다.

- Xamarin.Forms 지원 합니다 [XAML 2009 사양](/dotnet/framework/xaml-services/xaml-2009-language-features/);이 쉽게와 같은 데이터 정의 `string`s, `int`s, 등도으로 정의 된 제네릭 형식 및 인수 생성자에 전달 합니다.

- 현재 사용 하 여 WPF 수와 같은 XAML을 동적으로 로드할 수 없으므로 `XamlReader`합니다. 동일한 기본 기능을 가져올 수 있습니다는 [NuGet 패키지](https://www.nuget.org/packages/Xamarin.Forms.Dynamic/) 있지만.

### <a name="markup-extensions"></a>태그 확장

Xamarin.Forms는 XAML을 WPF 마찬가지로 태그 확장을 통해 확장을 지원 합니다. 기본적으로 동일한 기본 빌딩 블록에 있습니다.

1. `{x:Array}`
2. `{Binding}`
3. `{DynamicResource}`
4. `{x:Null}`
5. `{x:Static}`
6. `{StaticResource}`
7. `{x:Type}`

여기 뿐만 `{x:Reference}` XAML 2009 사양에서 및 `{TemplateBinding}` 의 특수 버전에 사용 되는 태그 확장 `ControlTemplate` Xamarin.Forms에서 지 원하는 합니다.

> [!WARNING]
> `ControlTemplate` 지원 하지 않습니다 동일한 이름이 동일한 경우에 합니다.

Xamarin.Forms에는 사용자 지정 태그 확장으로도 지원 하지만 구현 약간 다릅니다. 파생 되어야 합니다 wpf에서 `MarkupExtension` -추상 기본 클래스입니다. Xamarin.forms에 바뀝니다 인터페이스 `IMarkupExtension` 또는 `IMarkupExtension<T>` 하는 방법이 유연성이 뛰어납니다.

WPF에서와 마찬가지로 단일 필수 메서드는을 `ProvideValue` 태그 확장에서 값을 반환 하는 방법입니다.

## <a name="binding-infrastructure"></a>바인딩 인프라

전달 되는 핵심 개념 중 하나에.NET 데이터 속성에 시각적 속성을 연결 하는 데이터 바인딩 인프라입니다. 이 통해 MVVM과 같은 아키텍처 패턴. 기본 디자인 동일-바인딩할 수 있는 기본 클래스 [BindableObject](xref:Xamarin.Forms.BindableObject),이 WPF에는 [DependencyObject](https://msdn.microsoft.com/en-us/library/system.windows.dependencyobject(v=vs.110).aspx) 클래스. 이 기본 클래스는 데이터 바인딩의 대상으로 참여 하는 모든 개체에 대 한 루트 상위 요소로 사용 됩니다. 파생된 클래스를 노출 합니다 [BindableProperty](xref:Xamarin.Forms.BindableProperty) 속성 값에 대 한 백업 저장소로 작동 하는 개체 (으로 정의 됩니다 [DependencyProperty](https://msdn.microsoft.com/library/system.windows.dependencyproperty(v=vs.110).aspx) wpf에서 개체).

### <a name="defining-bindable-properties"></a>바인딩 가능한 속성 정의

Xamarin.Forms의 바인딩 가능한 속성에 대 한 정의 WPF와 같습니다.
1. 개체에서 파생 되어야 합니다 `BindableObject`합니다.
2. 형식의 공용 정적 필드 여야 `BindableProperty` 속성에 대 한 백업 저장소 키를 정의 하도록 선언 합니다.
3. 사용 하는 공용 인스턴스 속성 래퍼 없어야 `GetValue` 및 `SetValue` 검색 하 고 속성 값을 변경 합니다.

전체 예제를 참조 하세요 [xamarin.forms에서 바인딩 가능한 속성](~/xamarin-forms/xaml/bindable-properties.md)합니다.

### <a name="attached-properties"></a>연결 된 속성

연결 된 속성은 바인딩 가능한 속성의 하위 집합 및 WPF에서 동일한 방식으로 작동 합니다. 주요 차이점은 속성 래퍼가 경우 매개 변수가 생략 되는 것은 소유 하는 클래스에서 정적 get/set 메서드 집합이 바뀝니다. 참조 [Xamarin.Forms에서 연결 된 속성](~/xamarin-forms/xaml/attached-properties.md) 자세한 내용은 합니다.

### <a name="using-the-binding-engine"></a>바인딩 엔진을 사용 하 여

바인딩 엔진을 사용 하기 위한 프로세스 WPF에 있는 것과 같습니다. 만들어 코드 숨김에 활용할 수 있습니다는 `Binding` 개체가 소스 개체 (모든.NET 유형) 및 선택적 속성 값을 연결 (하는 경우 매개 변수가 생략 되, 처리 소스 개체 자체 속성과 마찬가지로 WPF). 사용할 수 있습니다 `SetBinding` 하나 `BindableObject` 에 바인딩을 연결을 `BindableProperty`입니다.

또는 다음을 사용 하 여 XAML에서 바인딩 관계를 정의할 수 있습니다는 `BindingExtension`합니다. WPF의 확장으로 동일한 기본 값을 포함 합니다.

바인딩 지원 및 엔진 비슷합니다 더 Silverlight 구현과 WPF 보다. Xamarin.Forms 구현 되지 않았던 몇 가지 누락 된 기능을 가지 있습니다.

- 바인딩에 다음과 같은 기능이 지원 되지 않습니다.
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
- `Binding.Mode` 지원 하지 않습니다 `OneTime`를 대신 사용 `OneWay`합니다.

#### <a name="relativesource"></a>RelativeSource

에 대 한 지원은 `RelativeSource` 바인딩. Wpf에서 수 XAML에 정의 된 기타 시각적 요소를 바인딩할 수 있습니다. Xamarin.Forms이 동일한 기능을 수행할 수 있습니다 사용 하 여 `{x:Reference}` 태그 확장 합니다. 예를 들어, 가정 하 고 텍스트 속성을 가진 이름 "otherControl"를 사용 하 여 컨트롤을 바인딩할 수 있습니다를 다음과 같이 합니다.

**WPF**

```xaml
Text={Binding RelativeSource={RelativeSource otherControl}, Path=Text}
```

**Xamarin.Forms**

```xaml
Text={Binding Source={x:Reference otherControl}, Path=Text}
```

에 대 한 동일한 기능을 사용할 수는 `{RelativeSource Self}` 기능입니다. 그러나 유형별 상위 항목을 찾기 위한 지원 되지 않습니다 (`{RelativeSource FindAncestor}`).

#### <a name="binding-context"></a>바인딩 컨텍스트

Wpf에서 정의할 수 있습니다는 `DataContext` 속성 값는 reprents 기본 바인딩 소스입니다. 바인딩에 대 한 소스를 정의 하지 않은 경우이 속성 값이 사용 됩니다. 값을 높은 수준에서 정의할 수 있도록 하 고 다음 자식 요소에서 사용 되는 시각적 트리 아래로 상속 됩니다.

Xamarin.Forms에서이 동일한 기능은 사용할 수 있는, 있지만 속성 이름이 `BindingContext`합니다.

#### <a name="value-converters"></a>값 변환기

값 변환기는 WPF와 마찬가지로-Xamarin.Forms에서 완전히 지원 됩니다. 동일한 인터페이스 셰이프는 사용 되지 않지만 Xamarin.Forms에 정의 된 인터페이스가 `Xamarin.Forms` 네임 스페이스입니다.

### <a name="model-view-viewmodel"></a>모델-뷰-ViewModel

MVVM은 WPF 및 Xamarin.Forms를 둘 다에서 완전히 지원 됩니다.

WPF에 기본 제공 `RoutedCommand` 하는 경우에 따라 사용 됩니다. Xamarin.Forms에 이외의 기본 제공 명령 지원 되지 않습니다는 `ICommand` 인터페이스 정의 합니다. 다양 한 MVVM을 구현 하는 데 필요한 기본 클래스를 추가 하려면 MVVM 프레임 워크를 포함할 수 있습니다.

#### <a name="inotifypropertychanged-and-inotifycollectionchanged"></a>INotifyCollectionChanged와 INotifyPropertyChanged

두 인터페이스 모두 Xamarin.Forms 바인딩에서 완전히 지원 됩니다. 많은 XAML 기반 프레임 워크와 달리 속성 변경 알림 (WPF) 처럼 Xamarin.Forms의 백그라운드 스레드에서 발생할 수 있습니다 및 바인딩 엔진을 UI 스레드로 전환 제대로 됩니다.

또한 두 환경 모두 지원 `SynchronziationContext` 하 고 `async` / `await` 적절 한 스레드 마샬링을 수행 합니다. WPF에서는 합니다 `Dispatcher` 클래스 모든 시각적 요소를 Xamarin.Forms에는 정적 메서드 `Device.BeginInvokeOnMainThread` 하는 데도 사용할 수 있습니다 (하지만 `SynchronizationContext` 는 플랫폼 간 코딩에 대 한 것이 좋습니다).

- Xamarin.Forms에는 `ObservableCollection<T>` 컬렉션 변경 알림을 지원입니다.
- 사용할 수 있습니다 `BindingBase.EnableCollectionSynchronization` 컬렉션에 대 한 크로스 스레드 업데이트를 사용 하도록 설정 합니다. API는 약간 다른 WPF variation [사용량 세부 정보에 대 한 문서를 확인](xref:Xamarin.Forms.BindingBase.EnableCollectionSynchronization*)합니다.

## <a name="data-templates"></a>데이터 템플릿

Xamarin.forms의 렌더링을 사용자 지정 데이터 템플릿은 지원 되지만 `ListView` 행 (셀). 활용할 수 있는 WPF와 달리 `DataTemplate`Xamarin.Forms 현재 모든 콘텐츠 기반 컨트롤에 대 한 s만 사용 하 여에 대 한 `ListView`합니다. 템플릿 정의 인라인으로 정의할된 수 있습니다 (에 할당 합니다 `ItemTemplate` 속성), 또는 리소스에는 `ResourceDictionary`합니다.

또한 WPF 상응으로 매우 유연 하지 않습니다.

1. 루트 요소는 `DataTemplate` 해야 합니다 _항상_ 수는 `ViewCell` 개체입니다.
2. 데이터 트리거 데이터 템플릿을 완벽 하 게 지원 되지만 포함 해야 합니다는 `DataType` 트리거 연관 된 속성의 형식을 나타내는 속성입니다.
3. `DataTemplateSelector` 도 지원 되지만에서 파생 `DataTemplate` 를 직접 할당만 합니다 `ItemTemplate` 속성 (및 `ItemTemplateSelector` WPF에서).

## <a name="itemscontrol"></a>ItemsControl

에 없는 기본 제공 equivelent는는 `ItemsControl` xamarin.forms; 이지만 [사용 가능한 여기에 Xamarin.Forms에 대 한 사용자 지정 항목](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Controls/ItemsControl.cs)합니다.

## <a name="user-controls"></a>사용자 정의 컨트롤

Wpf에서 `UserControl`동작 연결 UI의 섹션 수 있도록 사용 됩니다. Xamarin.Forms를 사용 하 여는 `ContentView` 같은 목적을 위해. XAML에서 바인딩 및 포함 지원합니다.

## <a name="navigation"></a>탐색

WPF는 드물게 사용 되는 `NavigationService` "브라우저와 유사한" 탐색 기능을 제공 하는 데 사용할 수 없습니다. 대부분의 앱이 있지만 및 대신 사용 하지 않은 것 다른 `Window` 요소 또는 다른 부분 데이터를 표시할 창입니다.

전화 장치, 다른 _화면_ 솔루션 많습니다 하며 따라서 Xamarin.Forms 여러 형태의 탐색에 대 한 지원:

| 탐색 스타일 | 페이지 유형 |
|--- |--- |
|스택 기반 (푸시/표시)|NavigationPage|
|마스터/세부 정보|MasterDetailPage|
|탭|TabbedPage|
|안쪽으로 살짝 밀어 왼쪽/오른쪽|CarouselView|

합니다 `NavigationPage` 가장 일반적인 방법은 이며 모든 페이지에는 `Navigation` 푸시 또는 페이지에는 탐색 스택에서 팝을 사용할 수 있는 속성입니다. 가장 가까운 equivelent 이것이 `NavigationService` WPF에서 찾을 수 있습니다.

### <a name="url-navigation"></a>URL 탐색

WPF는 데스크톱 지향 기술인 이며 시작 동작을 직접는 명령줄 매개 변수를 받아들일 수 있습니다. Xamarin.Forms를 사용 하 여 수 [딥 링크 설정 URL](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/) 시작 페이지로 이동 합니다.
