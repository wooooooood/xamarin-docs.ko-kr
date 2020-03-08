---
title: 모델 뷰-ViewModel 패턴
description: 이 장에서는 eShopOnContainers 모바일 앱이 MVVM 패턴을 사용 하 여 사용자 인터페이스에서 앱의 비즈니스 및 프레젠테이션 논리를 명확 하 게 구분 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: dd8c1813-df44-4947-bcee-1a1ff2334b87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: d6c9b74c9abc1a2c493c31699b52969a7d129429
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/07/2020
ms.locfileid: "78915303"
---
# <a name="the-model-view-viewmodel-pattern"></a>모델 뷰-ViewModel 패턴

일반적으로 Xamarin.ios 개발자 환경에는 XAML에서 사용자 인터페이스를 만든 다음 사용자 인터페이스에서 작동 하는 코드 숨김이 추가 됩니다. 앱이 수정 되 고 크기와 범위가 증가 함에 따라 복잡 한 유지 관리 문제가 발생할 수 있습니다. 이러한 문제에는 ui 컨트롤과 비즈니스 논리를 긴밀 하 게 결합 하 여 UI를 수정 하는 비용과 이러한 코드를 단위 테스트 하는 어려움을 포함 합니다.

MVVM (모델-뷰-ViewModel) 패턴은 응용 프로그램의 비즈니스 및 프레젠테이션 논리를 UI (사용자 인터페이스)와 완전히 분리 하는 데 도움이 됩니다. 응용 프로그램 논리와 UI를 명확 하 게 분리 하면 수많은 개발 문제를 해결 하 고 응용 프로그램을 더 쉽게 테스트 하 고 유지 관리 하 고 진화 시킬 수 있습니다. 또한 코드 다시 사용 기회를 크게 향상 시킬 수 있으며, 개발자와 UI 디자이너는 앱의 각 부분을 개발할 때 더 쉽게 공동 작업을 수행할 수 있습니다.

## <a name="the-mvvm-pattern"></a>MVVM 패턴

MVVM 패턴에는 모델, 뷰 및 뷰 모델의 세 가지 핵심 구성 요소가 있습니다. 각각은 고유한 용도로 사용 됩니다. 그림 2-1에서는 세 가지 구성 요소 간의 관계를 보여 줍니다.

![](mvvm-images/mvvm.png "The MVVM pattern")

**그림 2-1**: MVVM 패턴

각 구성 요소의 책임을 이해 하는 것 외에도 서로 상호 작용 하는 방식을 이해 하는 것도 중요 합니다. 높은 수준에서 뷰 모델은 뷰 모델을 "인식" 하 고 뷰 모델은 모델을 "인식" 하지만 모델은 뷰 모델을 인식 하지 않으며 뷰 모델은 뷰를 인식 하지 못합니다. 따라서 뷰 모델은 모델에서 뷰를 격리 하며 모델을 뷰와 독립적으로 발전 시킬 수 있습니다.

MVVM 패턴을 사용 하는 이점은 다음과 같습니다.

- 기존 비즈니스 논리를 캡슐화 하는 기존 모델 구현이 있는 경우 변경 하는 것이 어렵거나 위험할 수 있습니다. 이 시나리오에서 뷰 모델은 모델 클래스의 어댑터 역할을 하므로 모델 코드를 크게 변경 하지 않아도 됩니다.
- 개발자는 뷰를 사용 하지 않고 뷰 모델 및 모델에 대 한 단위 테스트를 만들 수 있습니다. 뷰 모델에 대 한 단위 테스트는 뷰에서 사용 하는 것과 동일한 기능을 실행할 수 있습니다.
- 뷰가 완전히 XAML로 구현 되는 경우 코드를 건드리지 않고도 앱 UI를 다시 만들 수 있습니다. 따라서 새 버전의 뷰가 기존 뷰 모델에서 작동 해야 합니다.
- 디자이너와 개발자는 개발 프로세스 중에 해당 구성 요소에 대해 독립적으로 또는 동시에 작업할 수 있습니다. 개발자는 뷰 모델 및 모델 구성 요소에서 작업할 수 있는 반면 디자이너는 뷰에 집중할 수 있습니다.

MVVM를 효과적으로 사용 하는 핵심은 응용 프로그램 코드를 올바른 클래스로 요소를 구분 하는 방법과 클래스가 상호 작용 하는 방식을 이해 하는 것입니다. 다음 섹션에서는 MVVM 패턴의 각 클래스에 대 한 책임을 설명 합니다.

### <a name="view"></a>보기

보기는 사용자가 화면에서 볼 수 있는 구조, 레이아웃 및 모양을 정의 하는 작업을 담당 합니다. 각 보기는 비즈니스 논리를 포함 하지 않는 제한 된 코드 숨김으로 XAML로 정의 하는 것이 가장 좋습니다. 그러나 경우에 따라 코드 숨김이 XAML에서 표현 하기 어려운 시각적 동작을 구현 하는 UI 논리 (예: 애니메이션)를 포함할 수 있습니다.

Xamarin Forms 응용 프로그램에서 뷰는 일반적으로 [`Page`](xref:Xamarin.Forms.Page)파생 된 클래스 또는 [`ContentView`](xref:Xamarin.Forms.ContentView)파생 클래스입니다. 그러나 뷰는 표시 될 때 개체를 시각적으로 나타내는 데 사용 되는 UI 요소를 지정 하는 데이터 템플릿으로 나타낼 수도 있습니다. 뷰로 표시 되는 데이터 템플릿에는 코드 숨김이 없으며 특정 뷰 모델 유형에 바인딩되도록 설계 되었습니다.

> [!TIP]
> 코드 숨김으로 UI 요소를 사용 하거나 사용 하지 않도록 설정 합니다. 뷰 모델에서 명령의 사용 가능 여부 또는 작업이 보류 중임 표시와 같은 보기 표시의 일부 측면에 영향을 주는 논리적 상태 변경을 정의 해야 합니다. 따라서 코드 숨김으로 활성화 하거나 비활성화 하는 것이 아니라 모델 속성 보기에 바인딩하여 UI 요소를 사용 하거나 사용 하지 않도록 설정 합니다.

보기의 상호 작용에 대 한 응답으로 보기 모델에서 코드를 실행 하기 위한 몇 가지 옵션이 있습니다 (예: 단추 클릭 또는 항목 선택). 컨트롤이 명령을 지 원하는 경우 컨트롤의 `Command` 속성은 뷰 모델의 `ICommand` 속성에 데이터 바인딩할 수 있습니다. 컨트롤의 명령이 호출 되 면 뷰 모델의 코드가 실행 됩니다. 명령 외에도 동작은 뷰의 개체에 연결 될 수 있으며 호출 되거나 발생 될 이벤트를 수신 대기할 수 있습니다. 이에 대 한 응답으로 동작은 뷰 모델의 `ICommand` 또는 뷰 모델의 메서드를 호출할 수 있습니다.

### <a name="viewmodel"></a>ViewModel

뷰 모델은 뷰가 데이터 바인딩할 수 있는 속성 및 명령을 구현 하 고 변경 알림 이벤트를 통해 상태 변경을 뷰에 알립니다. 뷰 모델에서 제공 하는 속성 및 명령은 UI에서 제공 되는 기능을 정의 하지만 뷰는 해당 기능을 표시 하는 방법을 결정 합니다.

> [!TIP]
> 비동기 작업을 통해 UI의 응답성을 유지 합니다. 모바일 앱은 사용자의 성능 인식 기능을 개선 하기 위해 UI 스레드를 차단 해제 된 상태로 유지 해야 합니다. 따라서 뷰 모델에서 i/o 작업에 비동기 메서드를 사용 하 고 이벤트를 발생 시켜 속성 변경 내용에 대 한 뷰를 비동기적으로 알립니다.

뷰 모델은 필요한 모델 클래스와 뷰의 상호 작용을 조정 해야 합니다. 일반적으로 뷰 모델과 모델 클래스 사이에는 일 대 다 관계가 있습니다. 뷰 모델은 뷰의 컨트롤이 직접 데이터 바인딩할 수 있도록 뷰에 모델 클래스를 직접 노출 하도록 선택할 수 있습니다. 이 경우 데이터 바인딩을 지원 하 고 알림 이벤트를 변경 하려면 모델 클래스를 디자인 해야 합니다.

각 뷰 모델은 뷰에서 쉽게 사용할 수 있는 형태로 모델의 데이터를 제공 합니다. 이를 위해 뷰 모델에서 데이터 변환을 수행 하는 경우가 있습니다. 뷰가 바인딩될 수 있는 속성을 제공 하기 때문에이 데이터 변환을 뷰 모델에 배치 하는 것이 좋습니다. 예를 들어 뷰 모델은 두 속성의 값을 결합 하 여 뷰가 쉽게 표시 되도록 할 수 있습니다.

> [!TIP]
> 변환 계층에서 데이터 변환을 중앙 집중화 합니다. 또한 변환기를 뷰 모델과 뷰 사이에 있는 별도의 데이터 변환 계층으로 사용할 수 있습니다. 예를 들어 뷰 모델에서 제공 하지 않는 특수 형식이 데이터에 필요한 경우이 작업이 필요할 수 있습니다.

뷰 모델에서 뷰를 사용 하 여 양방향 데이터 바인딩에 참여 시키려면 해당 속성은 `PropertyChanged` 이벤트를 발생 시켜야 합니다. 뷰 모델은 `INotifyPropertyChanged` 인터페이스를 구현 하 고 속성이 변경 될 때 `PropertyChanged` 이벤트를 발생 시켜이 요구 사항을 충족 합니다.

컬렉션의 경우 보기 친화적인 `ObservableCollection<T>` 제공 됩니다. 이 컬렉션은 개발자가 컬렉션에 `INotifyCollectionChanged` 인터페이스를 구현 하지 않아도 되는 컬렉션 변경 알림을 구현 하므로.

### <a name="model"></a>모델

모델 클래스는 응용 프로그램의 데이터를 캡슐화 하는 비시각적 클래스입니다. 따라서 모델은 일반적으로 비즈니스 및 유효성 검사 논리와 함께 데이터 모델을 포함 하는 앱의 도메인 모델을 나타내는 것으로 간주할 수 있습니다. 모델 개체의 예로는 Dto (데이터 전송 개체), POCOs (일반 이전 CLR 개체) 및 생성 된 엔터티 및 프록시 개체가 있습니다.

일반적으로 모델 클래스는 데이터 액세스 및 캐싱을 캡슐화 하는 서비스 또는 리포지토리와 함께 사용 됩니다.

## <a name="connecting-view-models-to-views"></a>뷰에 뷰 모델 연결

뷰 모델은 Xamarin.ios의 데이터 바인딩 기능을 사용 하 여 보기에 연결할 수 있습니다. 뷰를 생성 하 고 모델을 보고 런타임에 연결 하는 데 사용할 수 있는 여러 가지 방법이 있습니다. 이러한 방법은 첫 번째 컴퍼지션 보기 및 모델 첫 번째 컴퍼지션 이라는 두 가지 범주로 나뉩니다. 보기의 첫 번째 컴퍼지션과 뷰 모델 첫 번째 구성 중에서 선택 하는 것은 기본 설정 및 복잡성의 문제입니다. 그러나 모든 접근 방식은 동일한 목표를 공유 합니다. 즉, 뷰의 BindingContext 속성에 뷰 모델이 할당 됩니다.

보기의 첫 번째 구성을 사용 하 여 앱은 개념적으로 종속 된 뷰 모델에 연결 하는 뷰로 구성 됩니다. 이 방법의 주요 장점은 뷰 모델은 뷰 자체에 종속 되지 않기 때문에 느슨하게 연결 된 유닛 테스트 가능 앱을 쉽게 만들 수 있다는 것입니다. 또한 클래스를 만들고 연결 하는 방법을 이해 하기 위해 코드 실행을 추적 하는 대신 시각적 구조를 따라 응용 프로그램 구조를 이해 하는 것이 쉽습니다. 또한 탐색이 발생할 때 페이지를 구성 하는 데 사용 되는 Xamarin.ios 탐색 시스템을 사용 하 여 뷰를 처음 생성 합니다.

모델을 처음 구성 하는 경우 응용 프로그램은 개념적으로 뷰 모델로 구성 되며, 서비스는 뷰 모델에 대 한 뷰를 찾을 책임이 있습니다. 뷰 만들기를 추상화 하 여 앱의 논리적 비 UI 구조에 집중할 수 있으므로 뷰 모델 첫 번째 컴퍼지션은 일부 개발자에 게 더 자연스럽 게 표시 됩니다. 또한 다른 뷰 모델에서 뷰 모델을 만들 수 있습니다. 그러나이 방법은 복잡 하며 앱의 다양 한 부분을 만들고 연결 하는 방법을 이해 하기 어려울 수 있습니다.

> [!TIP]
> 보기 모델과 뷰를 독립적으로 유지 합니다. 뷰를 데이터 원본의 속성에 바인딩하는 것은 해당 뷰 모델에 대 한 뷰의 주 종속성 이어야 합니다. 특히 뷰 모델에서 [`Button`](xref:Xamarin.Forms.Button) 및 [`ListView`](xref:Xamarin.Forms.ListView)같은 뷰 유형을 참조 하지 마세요. 여기에 설명 된 원칙에 따라 뷰 모델을 격리 하 여 테스트할 수 있으므로 범위를 제한 하 여 소프트웨어 결함의 가능성을 줄일 수 있습니다.

다음 섹션에서는 뷰 모델을 뷰에 연결 하는 주요 방법에 대해 설명 합니다.

### <a name="creating-a-view-model-declaratively"></a>선언적으로 뷰 모델 만들기

가장 간단한 방법은 뷰가 XAML에서 해당 뷰 모델을 선언적으로 인스턴스화하는 것입니다. 뷰가 생성 될 때 해당 뷰 모델 개체도 생성 됩니다. 다음 코드 예제에서이 방법을 설명 합니다.

```xaml
<ContentPage ... xmlns:local="clr-namespace:eShop">  
    <ContentPage.BindingContext>  
        <local:LoginViewModel />  
    </ContentPage.BindingContext>  
    ...  
</ContentPage>
```

[`ContentPage`](xref:Xamarin.Forms.ContentPage) 만들어지면 `LoginViewModel` 인스턴스가 자동으로 생성 되 고 뷰의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)로 설정 됩니다.

뷰에서 뷰 모델을 선언적으로 생성 하 고 할당 하는 것은 간단 하지만 뷰 모델에서 기본 (매개 변수 없음) 생성자가 필요 하다는 단점이 있습니다.

### <a name="creating-a-view-model-programmatically"></a>프로그래밍 방식으로 뷰 모델 만들기

뷰 모델은 해당 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 속성에 할당 되는 코드를 코드 파일에 포함할 수 있습니다. 이는 다음 코드 예제와 같이 뷰의 생성자에서 수행 되는 경우가 많습니다.

```csharp
public LoginView()  
{  
    InitializeComponent();  
    BindingContext = new LoginViewModel(navigationService);  
}
```

뷰의 코드 숨김으로 보기 모델을 프로그래밍 방식으로 생성 하 고 할당 하는 것은 간단 합니다. 그러나이 방법의 주요 단점은 뷰가 뷰 모델에 필요한 종속성을 제공 해야 한다는 것입니다. 종속성 주입 컨테이너를 사용 하면 뷰와 뷰 모델 간의 느슨한 결합을 유지 하는 데 도움이 됩니다. 자세한 내용은 [종속성 주입](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md)을 참조 하세요.

### <a name="creating-a-view-defined-as-a-data-template"></a>데이터 템플릿으로 정의 된 뷰 만들기

뷰를 데이터 템플릿으로 정의 하 고 뷰 모델 유형과 연결할 수 있습니다. 데이터 템플릿을 리소스로 정의 하거나 뷰 모델을 표시 하는 컨트롤 내에서 인라인으로 정의할 수 있습니다. 컨트롤의 내용은 뷰 모델 인스턴스이고 데이터 템플릿이 시각적으로 표시 하는 데 사용 됩니다. 이 기법은 뷰 모델을 먼저 인스턴스화한 후 뷰를 만든 상황의 예입니다.

<a name="automatically_creating_a_view_model_with_a_view_model_locator" />

### <a name="automatically-creating-a-view-model-with-a-view-model-locator"></a>뷰 모델 로케이터를 사용 하 여 자동으로 뷰 모델 만들기

뷰 모델 로케이터는 뷰 모델의 인스턴스화 및 뷰에 대 한 연결을 관리 하는 사용자 지정 클래스입니다. EShopOnContainers 모바일 앱에서 `ViewModelLocator` 클래스에는 뷰 모델을 뷰와 연결 하는 데 사용 되는 연결 된 속성인 `AutoWireViewModel`있습니다. 뷰의 XAML에서이 연결 된 속성은 다음 코드 예제와 같이 뷰 모델이 뷰에 자동으로 연결 되어야 함을 나타내기 위해 true로 설정 됩니다.

```xaml
viewModelBase:ViewModelLocator.AutoWireViewModel="true"
```

`AutoWireViewModel` 속성은 false로 초기화 되는 바인딩 가능한 속성입니다. 값이 변경 되 면 `OnAutoWireViewModelChanged` 이벤트 처리기가 호출 됩니다. 이 메서드는 뷰에 대 한 뷰 모델을 확인 합니다. 다음 코드 예제에서는이 작업을 수행 하는 방법을 보여 줍니다.

```csharp
private static void OnAutoWireViewModelChanged(BindableObject bindable, object oldValue, object newValue)  
{  
    var view = bindable as Element;  
    if (view == null)  
    {  
        return;  
    }  

    var viewType = view.GetType();  
    var viewName = viewType.FullName.Replace(".Views.", ".ViewModels.");  
    var viewAssemblyName = viewType.GetTypeInfo().Assembly.FullName;  
    var viewModelName = string.Format(  
        CultureInfo.InvariantCulture, "{0}Model, {1}", viewName, viewAssemblyName);  

    var viewModelType = Type.GetType(viewModelName);  
    if (viewModelType == null)  
    {  
        return;  
    }  
    var viewModel = _container.Resolve(viewModelType);  
    view.BindingContext = viewModel;  
}
```

`OnAutoWireViewModelChanged` 메서드는 규칙 기반 접근 방법을 사용 하 여 뷰 모델을 확인 하려고 합니다. 이 규칙은 다음을 가정 합니다.

- 뷰 모델은 뷰 유형과 동일한 어셈블리에 있습니다.
- 보기는에 있습니다. 뷰 하위 네임 스페이스입니다.
- 뷰 모델은에 있습니다. ViewModels 자식 네임 스페이스입니다.
- 뷰 모델 이름은 뷰 이름에 해당 하며 "ViewModel"으로 끝납니다.

마지막으로 `OnAutoWireViewModelChanged` 메서드는 뷰 유형의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 를 확인 된 뷰 모델 유형으로 설정 합니다. 뷰 모델 유형을 확인 하는 방법에 대 한 자세한 내용은 [해결](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#resolution)을 참조 하세요.

이 방법은 응용 프로그램에 뷰 모델 인스턴스화 및 뷰에 대 한 연결을 담당 하는 단일 클래스가 있다는 장점이 있습니다.

> [!TIP]
> 대체 하기 쉽도록 보기 모델 로케이터를 사용 합니다. 모델 보기 로케이터는 단위 테스트 또는 디자인 타임 데이터와 같은 종속성의 대체 구현에 대 한 대체 지점으로도 사용할 수 있습니다.

## <a name="updating-views-in-response-to-changes-in-the-underlying-view-model-or-model"></a>기본 뷰 모델 또는 모델의 변경 내용에 대 한 응답으로 뷰를 업데이트 합니다.

뷰에 액세스할 수 있는 모든 뷰 모델 및 모델 클래스는 `INotifyPropertyChanged` 인터페이스를 구현 해야 합니다. 뷰 모델 또는 모델 클래스에서이 인터페이스를 구현 하면 내부 속성 값이 변경 될 때 클래스에서 뷰의 데이터 바인딩된 컨트롤에 변경 알림을 제공할 수 있습니다.

다음 요구 사항을 충족 하 여 적절 한 속성 변경 알림을 사용 하도록 앱을 설계 해야 합니다.

- 공용 속성의 값이 변경 되는 경우 항상 `PropertyChanged` 이벤트를 발생 시킵니다. XAML 바인딩이 발생 하는 방법에 대 한 정보로 인해 `PropertyChanged` 이벤트의 발생을 무시할 수 있다고 가정 하지 마십시오.
- 뷰 모델 또는 모델의 다른 속성에서 값을 사용 하는 계산 된 속성에 대해서는 항상 `PropertyChanged` 이벤트를 발생 시킵니다.
- 속성을 변경 하는 메서드의 끝에서 항상 `PropertyChanged` 이벤트를 발생 시키고, 개체가 안전 상태에 있는 것으로 알려진 경우에는 항상이 이벤트를 발생 시킵니다. 이벤트를 발생 시키면 이벤트의 처리기를 동기적으로 호출 하 여 작업이 중단 됩니다. 작업 중간에 발생 하는 경우 개체를 노출 하 여 안전 하지 않은 부분적으로 업데이트 된 상태에 있을 때 함수를 콜백 할 수 있습니다. 또한 `PropertyChanged` 이벤트를 통해 연계 변경을 트리거할 수 있습니다. 일반적으로 연계 변경을 수행 하려면 먼저 업데이트를 완료 해야 합니다.
- 속성이 변경 되지 않는 경우에는 `PropertyChanged` 이벤트를 발생 시 키 지 않습니다. 즉, `PropertyChanged` 이벤트를 발생 시키기 전에 이전 값과 새 값을 비교 해야 합니다.
- 속성을 초기화 하는 경우 뷰 모델의 생성자 중에 `PropertyChanged` 이벤트를 발생 시 키 지 않습니다. 뷰의 데이터 바인딩된 컨트롤은이 시점에서 변경 알림을 받도록 구독 하지 않습니다.
- 클래스의 공용 메서드에 대 한 단일 동기 호출 내에서 동일한 속성 이름 인수를 사용 하 여 둘 이상의 `PropertyChanged` 이벤트를 발생 시 키 지 않습니다. 예를 들어 백업 저장소가 `_numberOfItems` 필드인 `NumberOfItems` 속성이 지정 된 경우 루프를 실행 하는 동안 메서드가 50 시간 `_numberOfItems` 증가 하면 모든 작업이 완료 된 후에 `NumberOfItems` 속성에 대해 속성 변경 알림만 발생 해야 합니다. 비동기 메서드의 경우 비동기 연속 체인의 각 동기 세그먼트에서 지정 된 속성 이름에 대 한 `PropertyChanged` 이벤트를 발생 시킵니다.

EShopOnContainers 모바일 앱은 다음 코드 예제에 표시 된 대로 `ExtendedBindableObject` 클래스를 사용 하 여 변경 알림을 제공 합니다.

```csharp
public abstract class ExtendedBindableObject : BindableObject  
{  
    public void RaisePropertyChanged<T>(Expression<Func<T>> property)  
    {  
        var name = GetMemberInfo(property).Name;  
        OnPropertyChanged(name);  
    }  

    private MemberInfo GetMemberInfo(Expression expression)  
    {  
        ...  
    }  
}
```

Xamarin.ios의 [`BindableObject`](xref:Xamarin.Forms.BindableObject) 클래스는 `INotifyPropertyChanged` 인터페이스를 구현 하 고 [`OnPropertyChanged`](xref:Xamarin.Forms.BindableObject.OnPropertyChanged(System.String)) 메서드를 제공 합니다. `ExtendedBindableObject` 클래스는 속성 변경 알림을 호출 하는 `RaisePropertyChanged` 메서드를 제공 하며, 이렇게 하면 `BindableObject` 클래스에서 제공 하는 기능을 사용 합니다.

EShopOnContainers 모바일 앱의 각 뷰 모델 클래스는 `ViewModelBase` 클래스에서 파생 되 고이 클래스는 `ExtendedBindableObject` 클래스에서 파생 됩니다. 따라서 각 뷰 모델 클래스는 `ExtendedBindableObject` 클래스의 `RaisePropertyChanged` 메서드를 사용 하 여 속성 변경 알림을 제공 합니다. 다음 코드 예제에서는 eShopOnContainers mobile 앱에서 람다 식을 사용 하 여 속성 변경 알림을 호출 하는 방법을 보여 줍니다.

```csharp
public bool IsLogin  
{  
    get  
    {  
        return _isLogin;  
    }  
    set  
    {  
        _isLogin = value;  
        RaisePropertyChanged(() => IsLogin);  
    }  
}
```

이러한 방식으로 람다 식을 사용 하면 각 호출에 대해 람다 식을 평가 해야 하기 때문에 성능이 약간 저하 됩니다. 성능 비용이 작으며 일반적으로 앱에 영향을 주지 않지만 많은 변경 알림이 있는 경우 비용이 발생할 수 있습니다. 그러나이 방법의 혜택은 속성 이름을 바꿀 때 컴파일 시간 형식 안전성 및 리팩터링 지원을 제공 한다는 것입니다.

## <a name="ui-interaction-using-commands-and-behaviors"></a>명령 및 동작을 사용 하 여 UI 상호 작용

모바일 앱에서 작업은 일반적으로 단추 클릭과 같은 사용자 작업에 대 한 응답으로 호출 됩니다 .이 작업은 코드 숨김으로 이벤트 처리기를 만들어 구현할 수 있습니다. 그러나 MVVM 패턴에서 작업을 구현 하는 책임은 뷰 모델을 사용 하는 것 이며 코드 숨김으로 코드를 배치 하는 것은 피해 야 합니다.

명령은 UI의 컨트롤에 바인딩할 수 있는 작업을 표시 하는 편리한 방법을 제공 합니다. 작업을 구현 하는 코드를 캡슐화 하 고 뷰의 시각적 표현에서 분리 된 상태로 유지 하는 데 도움을 줍니다. Xamarin.ios는 명령에 선언적으로 연결 될 수 있는 컨트롤을 포함 하며, 이러한 컨트롤은 사용자가 컨트롤과 상호 작용할 때 명령을 호출 합니다.

동작을 통해 컨트롤을 명령에 선언적으로 연결할 수도 있습니다. 그러나 동작은 컨트롤에서 발생 한 이벤트 범위와 연결 된 작업을 호출 하는 데 사용할 수 있습니다. 따라서 동작은 더 많은 유연성과 제어를 제공 하는 동시에 명령 사용 컨트롤과 동일한 많은 시나리오를 처리 합니다. 또한 동작을 사용 하 여 명령 개체 또는 메서드를 명령과 상호 작용 하도록 특별히 설계 되지 않은 컨트롤과 연결할 수도 있습니다.

### <a name="implementing-commands"></a>명령 구현

뷰 모델은 일반적으로 뷰의 바인딩에 대해 `ICommand` 인터페이스를 구현 하는 개체 인스턴스인 명령 속성을 노출 합니다. 많은 Xamarin Forms 컨트롤은 뷰 모델에서 제공 하는 `ICommand` 개체에 데이터 바인딩될 수 있는 `Command` 속성을 제공 합니다. `ICommand` 인터페이스는 작업 자체를 캡슐화 하는 `Execute` 메서드, 명령이 호출 될 수 있는지 여부를 나타내는 `CanExecute` 메서드 및 명령이 실행 되어야 하는지 여부에 영향을 주는 변경이 발생할 때 발생 하는 `CanExecuteChanged` 이벤트를 정의 합니다. Xamarin.ios에서 제공 하는 [`Command`](xref:Xamarin.Forms.Command) 및 [`Command<T>`](xref:Xamarin.Forms.Command) 클래스는 `ICommand` 인터페이스를 구현 합니다. 여기서 `T`은 `Execute` 및 `CanExecute`에 대 한 인수의 형식입니다.

뷰 모델 내에서 `ICommand`유형의 뷰 모델에 있는 각 공용 속성에 대해 [`Command`](xref:Xamarin.Forms.Command) 또는 [`Command<T>`](xref:Xamarin.Forms.Command) 유형의 개체가 있어야 합니다. `Command` 또는 `Command<T>` 생성자에는 `ICommand.Execute` 메서드를 호출할 때 호출 되는 `Action` 콜백 개체가 필요 합니다. `CanExecute` 메서드는 선택적 생성자 매개 변수 이며 `bool`를 반환 하는 `Func`입니다.

다음 코드는 `Register` 뷰 모델 메서드에 대리자를 지정 하 여 register 명령을 나타내는 [`Command`](xref:Xamarin.Forms.Command) 인스턴스가 생성 되는 방법을 보여 줍니다.

```csharp
public ICommand RegisterCommand => new Command(Register);
```

명령은 `ICommand`에 대 한 참조를 반환 하는 속성을 통해 뷰에 노출 됩니다. [`Command`](xref:Xamarin.Forms.Command) 개체에서 `Execute` 메서드가 호출 되 면 `Command` 생성자에 지정 된 대리자를 통해 뷰 모델의 메서드에 대 한 호출을 전달 하기만 하면 됩니다.

명령의 `Execute` 대리자를 지정할 때 `async` 및 `await` 키워드를 사용 하 여 명령에 의해 비동기 메서드를 호출할 수 있습니다. 이는 콜백이 `Task` 이며 대기 되어야 함을 나타냅니다. 예를 들어 다음 코드는 `SignInAsync` 뷰 모델 메서드에 대리자를 지정 하 여 로그인 명령을 나타내는 [`Command`](xref:Xamarin.Forms.Command) 인스턴스가 생성 되는 방법을 보여 줍니다.

```csharp
public ICommand SignInCommand => new Command(async () => await SignInAsync());
```

[`Command<T>`](xref:Xamarin.Forms.Command) 클래스를 사용 하 여 명령을 인스턴스화하면 매개 변수를 `Execute`에 전달 하 고 작업을 `CanExecute` 수 있습니다. 예를 들어 다음 코드는 `Command<T>` 인스턴스를 사용 하 여 `NavigateAsync` 메서드가 `string`형식의 인수를 요구 하도록 지정 하는 방법을 보여 줍니다.

```csharp
public ICommand NavigateCommand => new Command<string>(NavigateAsync);
```

[`Command`](xref:Xamarin.Forms.Command) 및 [`Command<T>`](xref:Xamarin.Forms.Command) 클래스 모두에서 각 생성자의 `CanExecute` 메서드에 대 한 대리자는 선택 사항입니다. 대리자가 지정 되지 않은 경우 `Command` `CanExecute`에 대 한 `true` 반환 됩니다. 그러나 뷰 모델은 `Command` 개체의 `ChangeCanExecute` 메서드를 호출 하 여 명령의 `CanExecute` 상태 변경을 나타낼 수 있습니다. 이로 인해 `CanExecuteChanged` 이벤트가 발생 합니다. 명령에 바인딩된 UI의 모든 컨트롤은 해당 사용 상태를 업데이트 하 여 데이터 바인딩된 명령의 가용성을 반영 합니다.

#### <a name="invoking-commands-from-a-view"></a>뷰에서 명령 호출

다음 코드 예제에서는 [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) 인스턴스를 사용 하 여 `LoginView`의 [`Grid`](xref:Xamarin.Forms.Grid) `LoginViewModel` 클래스의 `RegisterCommand` 바인딩하는 방법을 보여 줍니다.

```xaml
<Grid Grid.Column="1" HorizontalOptions="Center">  
    <Label Text="REGISTER" TextColor="Gray"/>  
    <Grid.GestureRecognizers>  
        <TapGestureRecognizer Command="{Binding RegisterCommand}" NumberOfTapsRequired="1" />  
    </Grid.GestureRecognizers>  
</Grid>
```

[`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) 속성을 사용 하 여 명령 매개 변수를 선택적으로 정의할 수도 있습니다. 필요한 인수의 형식은 `Execute` 및 `CanExecute` 대상 메서드에서 지정 합니다. 사용자가 연결 된 컨트롤과 상호 작용할 때 [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) 가 대상 명령을 자동으로 호출 합니다. 명령 매개 변수 (제공 된 경우)는 명령 `Execute` 대리자에 인수로 전달 됩니다.

<a name="implementing_behaviors" />

### <a name="implementing-behaviors"></a>동작 구현

동작을 사용 하면 기능을 서브 클래스 하지 않고도 UI 컨트롤에 추가할 수 있습니다. 대신 기능은 동작 클래스에서 구현되고 컨트롤 자체의 일부였던 것처럼 컨트롤에 연결됩니다. 동작을 사용 하면 컨트롤의 API와 직접 상호 작용 하 고, 컨트롤에 간단 하 게 연결 하 고, 둘 이상의 뷰나 앱에서 다시 사용 하기 위해 패키지할 수 있으므로 일반적으로 코드 숨김으로 작성 해야 하는 코드를 구현할 수 있습니다. MVVM 컨텍스트에서 동작은 컨트롤을 명령에 연결 하는 데 유용한 방법입니다.

연결 된 속성을 통해 컨트롤에 연결 된 동작을 *연결 된 동작*이라고 합니다. 그러면 동작에서 해당 컨트롤이 연결 된 요소의 노출 된 API를 사용 하 여 뷰의 시각적 트리에서 해당 컨트롤이 나 기타 컨트롤에 기능을 추가할 수 있습니다. EShopOnContainers 모바일 앱에는 연결 된 동작인 `LineColorBehavior` 클래스가 포함 되어 있습니다. 이 동작에 대 한 자세한 내용은 [유효성 검사 오류 표시](~/xamarin-forms/enterprise-application-patterns/validation.md#displaying_validation_errors)를 참조 하세요.

Xamarin.ios 동작은 [`Behavior`](xref:Xamarin.Forms.Behavior) 또는 [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1) 클래스에서 파생 되는 클래스입니다. 여기서 `T` 동작은 동작을 적용할 컨트롤의 형식입니다. 이러한 클래스는 `OnAttachedTo` 및 `OnDetachingFrom` 메서드를 제공 합니다 .이 메서드는 동작을 컨트롤에 연결 하 고 분리할 때 실행 되는 논리를 제공 하도록 재정의 해야 합니다.

EShopOnContainers 모바일 앱에서 `BindableBehavior<T>` 클래스는 [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1) 클래스에서 파생 됩니다. `BindableBehavior<T>` 클래스의 목적은 연결 된 컨트롤에 동작의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 를 설정 해야 하는 xamarin.ios 동작에 대 한 기본 클래스를 제공 하는 것입니다.

`BindableBehavior<T>` 클래스는 동작의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 를 설정 하는 재정의 가능한 `OnAttachedTo` 메서드와 `BindingContext`를 정리 하는 재정의 가능한 `OnDetachingFrom` 메서드를 제공 합니다. 또한 클래스는 연결된 컨트롤에 대한 참조를 `AssociatedObject` 속성에 저장합니다.

EShopOnContainers 모바일 앱에는 발생 하는 이벤트에 대 한 응답으로 명령을 실행 하는 `EventToCommandBehavior` 클래스가 포함 됩니다. 이 클래스는 동작이 사용 될 때 동작에서 `Command` 속성으로 지정 된 `ICommand`를 바인딩하고 실행할 수 있도록 `BindableBehavior<T>` 클래스에서 파생 됩니다. 다음 코드 예제는 `EventToCommandBehavior` 클래스를 보여줍니다.

```csharp
public class EventToCommandBehavior : BindableBehavior<View>  
{  
    ...  
    protected override void OnAttachedTo(View visualElement)  
    {  
        base.OnAttachedTo(visualElement);  

        var events = AssociatedObject.GetType().GetRuntimeEvents().ToArray();  
        if (events.Any())  
        {  
            _eventInfo = events.FirstOrDefault(e => e.Name == EventName);  
            if (_eventInfo == null)  
                throw new ArgumentException(string.Format(  
                        "EventToCommand: Can't find any event named '{0}' on attached type",   
                        EventName));  

            AddEventHandler(_eventInfo, AssociatedObject, OnFired);  
        }  
    }  

    protected override void OnDetachingFrom(View view)  
    {  
        if (_handler != null)  
            _eventInfo.RemoveEventHandler(AssociatedObject, _handler);  

        base.OnDetachingFrom(view);  
    }  

    private void AddEventHandler(  
            EventInfo eventInfo, object item, Action<object, EventArgs> action)  
    {  
        ...  
    }  

    private void OnFired(object sender, EventArgs eventArgs)  
    {  
        ...  
    }  
}
```

`OnAttachedTo` 및 `OnDetachingFrom` 메서드는 `EventName` 속성에 정의 된 이벤트에 대 한 이벤트 처리기를 등록 및 등록 취소 하는 데 사용 됩니다. 그런 다음 이벤트가 발생 하면 명령을 실행 하는 `OnFired` 메서드가 호출 됩니다.

이벤트가 발생할 때 `EventToCommandBehavior`를 사용 하 여 명령을 실행 하는 장점은 명령과 상호 작용 하도록 설계 되지 않은 컨트롤과 명령을 연결할 수 있다는 것입니다. 또한 모델을 볼 수 있도록 이벤트 처리 코드를 이동 하 여 단위 테스트를 수행할 수 있습니다.

#### <a name="invoking-behaviors-from-a-view"></a>뷰에서 동작 호출

`EventToCommandBehavior` 명령은 명령을 지원 하지 않는 컨트롤에 명령을 연결 하는 데 특히 유용 합니다. 예를 들어 다음 코드에 표시 된 것 처럼 `ProfileView`는 `EventToCommandBehavior`를 사용 하 여 사용자의 주문이 나열 된 [`ListView`](xref:Xamarin.Forms.ListView) 에서 [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) 이벤트가 발생 하는 경우 `OrderDetailCommand`를 실행 합니다.

```xaml
<ListView>  
    <ListView.Behaviors>  
        <behaviors:EventToCommandBehavior             
            EventName="ItemTapped"  
            Command="{Binding OrderDetailCommand}"  
            EventArgsConverter="{StaticResource ItemTappedEventArgsConverter}" />  
    </ListView.Behaviors>  
    ...  
</ListView>
```

런타임에 `EventToCommandBehavior` [`ListView`](xref:Xamarin.Forms.ListView)와의 상호 작용에 응답 합니다. `ListView`에서 항목을 선택 하면 [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) 이벤트가 발생 하며,이는 `ProfileViewModel`에서 `OrderDetailCommand`를 실행 합니다. 기본적으로 이벤트에 대 한 이벤트 인수가 명령에 전달 됩니다. 이 데이터는 `EventArgsConverter` 속성에 지정 된 변환기에서 소스와 대상 사이에 전달 될 때 변환 되며 [`ItemTappedEventArgs`](xref:Xamarin.Forms.ItemTappedEventArgs)에서 `ListView`의 [`Item`](xref:Xamarin.Forms.ItemTappedEventArgs.Item) 반환 합니다. 따라서 `OrderDetailCommand`를 실행 하면 선택한 `Order` 등록 된 작업에 매개 변수로 전달 됩니다.

동작에 대 한 자세한 내용은 [동작](~/xamarin-forms/app-fundamentals/behaviors/index.md)을 참조 하세요.

## <a name="summary"></a>요약

MVVM (모델-뷰-ViewModel) 패턴은 응용 프로그램의 비즈니스 및 프레젠테이션 논리를 UI (사용자 인터페이스)와 완전히 분리 하는 데 도움이 됩니다. 응용 프로그램 논리와 UI를 명확 하 게 분리 하면 수많은 개발 문제를 해결 하 고 응용 프로그램을 더 쉽게 테스트 하 고 유지 관리 하 고 진화 시킬 수 있습니다. 또한 코드 다시 사용 기회를 크게 향상 시킬 수 있으며, 개발자와 UI 디자이너는 앱의 각 부분을 개발할 때 더 쉽게 공동 작업을 수행할 수 있습니다.

MVVM 패턴을 사용 하 여 앱의 UI와 기본 프레젠테이션 및 비즈니스 논리는 세 개의 개별 클래스로 구분 됩니다. 뷰는 UI와 UI 논리를 캡슐화 합니다. 프레젠테이션 논리 및 상태를 캡슐화 하는 뷰 모델 그리고 모델은 앱의 비즈니스 논리 및 데이터를 캡슐화 합니다.

## <a name="related-links"></a>관련 링크

- [다운로드 전자책 (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
