---
title: Model-view-viewmodel 패턴
description: 이 장에서 eShopOnContainers 모바일 앱은 완전히 사용자 인터페이스에서 앱의 비즈니스 및 프레젠테이션 논리와 분리 MVVM 패턴을 사용 하는 방법에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: dd8c1813-df44-4947-bcee-1a1ff2334b87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 87448c556c66ea086db70699848227e1f671792b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61299625"
---
# <a name="the-model-view-viewmodel-pattern"></a>Model-view-viewmodel 패턴

Xamarin.Forms 개발자 환경에는 일반적으로 XAML에서 사용자 인터페이스를 만들고 사용자 인터페이스에서 작동 하는 코드 숨김을 추가 포함 됩니다. 앱 수정 되 고 크기와 범위 증가 처럼 복잡 한 유지 관리 문제가 발생할 수 있습니다. 이러한 문제는 UI 컨트롤 및 UI 수정 하 고 이러한 코드 유닛 테스트의 어려움 비용이 증가 하는 비즈니스 논리를 간의 긴밀 한 결합을 포함 합니다.

완전히 사용자 인터페이스 (UI)에서 응용 프로그램의 비즈니스 및 프레젠테이션 논리와 분리-Model-view-viewmodel (MVVM) 패턴 수 있습니다. 응용 프로그램 논리 및 UI 명확한 구분을 유지 관리 다양 한 개발 문제를 해결 하는 데 도움이 및 응용 프로그램을 테스트 하 고 유지 관리 하며 진화를 쉽게 만들 수 있습니다. 코드 다시 사용 하 여 기회를 크게 향상 시킬 수 있습니다 하 고 개발자 및 UI 디자이너를 더 쉽게 공동 작업할 수는 앱의 각 해당 부분을 개발 하는 경우.

## <a name="the-mvvm-pattern"></a>MVVM 패턴

MVVM 패턴의 핵심 구성 요소를 세 가지: 모델, 보기 및 보기 모델입니다. 각 고유 용도로 사용 됩니다. 그림 2-1 세 가지 구성 요소 간의 관계를 보여 줍니다.

![](mvvm-images/mvvm.png "MVVM 패턴")

**그림 2-1**: MVVM 패턴

각 구성 요소의 책임을 이해 하는 것 외에도 서로 상호 작용 하는 방법을 이해 해야 이기도 합니다. 높은 수준에서 보기 "knows"에 대 한 뷰 모델 및 뷰 모델 "knows"에 대 한 모델을 모델에는 보기 모델의 인식 되지 있고 뷰 모델 뷰의 인식 되지 합니다. 따라서 뷰 모델 뷰 모델에서 격리 하 고 모델 뷰를 독립적으로 발전할 수 있습니다.

MVVM 패턴을 사용 하는 이점은 아래와 같습니다.

-   기존 비즈니스 논리를 캡슐화 하는 기존 모델 구현이 있으면 어렵거나 위험한 변경할 수 있습니다. 이 시나리오에서는 보기 모델 클래스에 대 한 어댑터로 작동 모델과 모델 코드에 어떤 중요 한 변화를 방지할 수 있습니다.
-   개발자는 뷰를 사용 하지 않고 보기 모델 및 모델에 대 한 단위 테스트를 만들 수 있습니다. 보기 모델에 대 한 단위 테스트를 정확 하 게 보기에서 사용 되는 동일한 기능을 수행 하려면.
-   뷰는 XAML에서 완전히 구현 된 앱 UI 코드를 건드리지 않고 향후 재설계 될 수 있습니다. 따라서 보기의 새 버전이 기존 뷰 모델을 사용 하 여 작동 해야 합니다.
-   디자이너와 개발자가 작동할 수 있습니다 독립적이 고 동시에 해당 구성 요소 개발 프로세스 중. 개발자는 보기 모델 및 모델 구성 요소를 작업할 수 있습니다 하는 동안 디자이너 뷰를 집중할 수 있습니다.

올바른 클래스로 앱 코드를 팩터링 하는 방법에 대 한 이해 및 클래스가 상호 작용 하는 방법에 대 한 이해를 효과적으로 MVVM을 사용 하 여 열쇠입니다. 다음 섹션에서는 각 MVVM 패턴의 클래스의 책임을 설명합니다.

### <a name="view"></a>보기

뷰는 구조, 레이아웃 및 화면에서 사용자에 게 항목의 모양을 정의 하는 일을 담당 합니다. 이상적으로 각 보기는 제한 된 코드 숨김으로 인해 비즈니스 논리를 포함 하지 않는 XAML에 정의 됩니다. 그러나 일부 경우에는 코드 숨김 애니메이션과 같은 XAML을 표현 하기 어려울 정도로 시각적 동작을 구현 하는 UI 논리를 포함할 수 있습니다.

Xamarin.Forms 응용 프로그램에서 뷰는 일반적으로 [ `Page` ](xref:Xamarin.Forms.Page)-파생 하거나 [ `ContentView` ](xref:Xamarin.Forms.ContentView)-클래스를 파생 합니다. 그러나 뷰 표시 되는 개체를 시각적으로 나타내는 데 사용할 UI 요소를 지정 하는 데이터 템플릿을으로 표현 될 수 있습니다. 뷰 데이터 템플릿을 모든 코드 숨김 없고는 특정 보기 모델 형식에 바인딩 하도록 설계 되었습니다.

> [!TIP]
> 사용 하도록 설정 하 고 코드 숨김의 UI 요소를 사용 하지 않도록 설정 하지 마세요. 모델 보기 뷰의 표시 되는 명령을 사용할 수 인지 아니면 작업이 보류 중임이 표시 등의 일부 측면에 영향을 주는 논리 상태 변경 내용을 정의 하는 일을 담당 되는지 확인 합니다. 따라서를 사용 하도록 설정 하 고 모델 속성을 대신 사용 하도록 설정 및 코드 숨김에서 해제 보기에 바인딩하여 UI 요소를 사용 하지 않도록 설정 합니다.

여러 가지 방법으로 단추 클릭과 같은 보기에서 상호 작용에 대 한 응답에 뷰 모델에서 코드를 실행 또는 항목을 선택 합니다. 컨트롤에 명령 컨트롤의 지 원하는 경우 `Command` 속성 수 수 데이터 바인딩된은 `ICommand` 보기 모델의 속성입니다. 컨트롤의 명령을 호출 되 면 뷰 모델의 코드가 실행 됩니다. 명령 외에도 동작 뷰에서 개체에 연결 하 고 명령을 호출할 수 또는 발생 하는 이벤트를 대기할 수 있습니다. 응답으로 동작을 호출할 수 있습니다는 `ICommand` 보기 모델 또는 보기 모델의 메서드.

### <a name="viewmodel"></a>ViewModel

뷰 모델 속성과 보기 수, 데이터 바인딩 명령을 구현 하 고 변경 알림 이벤트를 통해 상태 변경에 대해 보기를에 알립니다. 뷰 모델을 제공 하는 명령과 속성 UI를 통해 제공 될 기능을 정의 하지만 보기 기능을 표시 하는 하는 방법을 결정 합니다.

> [!TIP]
> 비동기 작업을 사용 하 여 응답성이 뛰어난 UI를 유지 합니다. 모바일 앱 사용자의 인식 성능을 개선 하기 위해 차단 UI 스레드를 유지 해야 합니다. 따라서 뷰 모델에서 I/O 작업에 대 한 비동기 메서드를 사용 하 고 속성 변경의 뷰를 비동기적으로 알리는 이벤트를 발생 시킵니다.

보기 모델에 대해 필요한 모든 모델 클래스를 사용 하 여 보기의 상호 작용 조정을 담당 이기도 합니다. 뷰 모델에서 모델 클래스 사이 일 대 다 관계는 일반적으로 합니다. 뷰 모델 보기에서 컨트롤에 직접 데이터 바인딩할 수 있도록 보기로 직접 모델 클래스를 노출 하도록 선택할 수 있습니다. 이 경우 모델 클래스 변경 알림 이벤트와 데이터 바인딩을 지원 하도록 디자인 해야 합니다.

각 보기 모델 뷰를 쉽게 사용할 수 있는 형태로 모델에서 데이터를 제공 합니다. 이렇게 하려면 보기 모델은 경우에 따라 데이터 변환을 수행 합니다. 뷰에 바인딩할 수 있는 속성을 제공 하기 때문에 뷰 모델에서이 데이터 변환 배치 것이 좋습니다. 예를 들어, 보기 모델 뷰로 표시 하기 위해 쉽게 수행할 수 있도록 두 속성의 값을 결합할 수 있습니다.

> [!TIP]
> 데이터 변환 변환 계층에 중앙 집중화 합니다. 변환기를 사용 하 여 뷰 모델 및 뷰 사이 별도 데이터 변환 계층으로도 가능 합니다. 예를 들어, 데이터 뷰 모델을 제공 하지 않는 특수 형식이 필요한 경우이 방법을 필요한 수 있습니다.

속성 보기를 사용 하 여 양방향 데이터 바인딩에 참여할는 보기 모델에 대 한 순서 대로 발생 해야 합니다는 `PropertyChanged` 이벤트입니다. 뷰 모델을 구현 하 여이 요구 사항을 충족 합니다 `INotifyPropertyChanged` 인터페이스 및 발생을 `PropertyChanged` 속성이 변경 될 때 이벤트.

컬렉션의 경우 보기에 게 친숙 한 `ObservableCollection<T>` 제공 됩니다. 구현 하지 않아도 개발자 하므로 컬렉션 변경 알림을 구현 하는이 컬렉션은 `INotifyCollectionChanged` 컬렉션의 인터페이스입니다.

### <a name="model"></a>모델

모델 클래스는 앱의 데이터를 캡슐화 하는 비 가시적 클래스입니다. 따라서 모델을 일반적으로 비즈니스 및 유효성 검사 논리와 함께 데이터 모델을 포함 하는 앱의 도메인 모델을 나타내는 것으로 간주 될 수 있습니다. 모델 개체의 예로 데이터 전송 개체 (Dto), Plain Old CLR Objects (Poco) 및 생성 된 엔터티 및 프록시 개체를 들 수 있습니다.

모델 클래스는 일반적으로 서비스 또는 데이터 액세스 및 캐싱 캡슐화 하는 저장소와 함께에서 사용 됩니다.

## <a name="connecting-view-models-to-views"></a>보기 모델 보기에 연결

모델 보기 Xamarin.Forms의 데이터 바인딩 기능을 사용 하 여 보기에 연결할 수 있습니다. 뷰를 생성, 모델 보기 및 런타임에 연결 하는 방법은 여러 가지가 있습니다. 이러한 접근 방식은 첫 번째 컴퍼지션 보기 및 보기 모델에 대 한 첫 번째 컴퍼지션 이라고, 두 가지 범주로 나뉩니다. 첫 번째 컴포지션 뷰 및 모델 첫 번째 컴퍼지션의 기본 설정 및 복잡성 문제 보기 중에서 선택 합니다. 그러나 모든 방법 BindingContext 속성에 할당 하는 보기 모델을 가질 뷰에 대 한 인 동일한 목표를 공유 합니다.

뷰를 사용 하 여 첫 번째 컴퍼지션 앱 종속 보기 모델에 연결 된 뷰의 개념적으로 구성 됩니다. 이 방식의 이점은 경우 수행 하는 것을 쉽게 생성 느슨하게 결합 된 단위 테스트 앱 보기 모델에 직접 보기에 종속 되지 않음 있으므로 클래스 생성 되 고 연결 하는 방법을 이해 하려면 코드 실행을 추적 하는 대신 해당 시각적 구조를 다음으로 앱 구조를 이해 하기 쉬운 이기도 합니다. 또한 Xamarin.Forms 탐색 시스템을 담당 하는 생성 페이지에 대 한 탐색이 발생할 때 뷰 모델 첫 번째 작성 하면 복잡 한 및 플랫폼을 사용 하 여 불일치를 사용 하 여 첫 번째 뷰 생성이 맞춥니다.

뷰를 사용 하 여 모델 첫 번째 컴퍼지션 앱은 개념적 모델 보기의 보기 모델의 뷰를 찾기 위한 책임을 지지 하는 서비스를 사용 하 여 구성 됩니다. 뷰 모델에 대 한 첫 번째 작성 보기 생성을 추상화할 수 있습니다 제거 앱 논리 비 UI 구조에 집중할 수 있게 되므로 일부 개발자에 게 더 자연스럽 게 받아들이며 합니다. 또한 다른 뷰 모델에서 만들어야 하는 보기 모델 수 있습니다. 그러나이 이렇게 복잡 한 경우가 있으며 앱의 여러 부분 생성 되 고 연결 하는 방법을 이해 하기 어렵게 될 수 있습니다.

> [!TIP]
> 모델 보기 및 보기 독립적으로 유지 합니다. 속성에 데이터 원본 뷰의 바인딩에 해당 하는 보기 모델에 대 한 보기의 주 종속성 이어야 합니다. 특히, 같은 보기 유형을 참조 하지 않습니다 [ `Button` ](xref:Xamarin.Forms.Button) 하 고 [ `ListView` ](xref:Xamarin.Forms.ListView), 보기 모델에서. 여기에 설명 된 원칙을 수행 하 여 격리 수준 범위를 제한 하 여 소프트웨어 결함의 가능성을 줄이기 뷰 모델을 테스트할 수 있습니다.

다음 섹션에서는 보기 모델 보기에 연결 하는 기본 방법에 설명 합니다.

### <a name="creating-a-view-model-declaratively"></a>보기 모델을 선언적으로 만들기

가장 간단한 방법은 뷰를 선언적으로 XAML에서 해당 뷰 모델의 인스턴스화입니다. 뷰를 생성할 때 해당 뷰 모델 개체도 생성 됩니다. 다음 코드 예제에서이 방법을 설명 합니다.

```xaml
<ContentPage ... xmlns:local="clr-namespace:eShop">  
    <ContentPage.BindingContext>  
        <local:LoginViewModel />  
    </ContentPage.BindingContext>  
    ...  
</ContentPage>
```

경우는 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 만들어지면의 인스턴스를 `LoginViewModel` 자동으로 생성 되 고 뷰의 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)합니다.

이 선언적 생성 및 할당 뷰에서 보기 모델의 이점이 것은 간단 하지만 보기 모델의 기본 (매개 변수가 없는) 생성자 필요 한다는 단점이 있습니다.

### <a name="creating-a-view-model-programmatically"></a>보기 모델을 프로그래밍 방식으로 만들기

보기는 보기 모델에 할당 되는 코드 숨김 파일에 코드가 있을 수 있습니다 해당 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 속성입니다. 종종 이렇게 보기의 생성자에서 다음 코드 예제 에서처럼:

```csharp
public LoginView()  
{  
    InitializeComponent();  
    BindingContext = new LoginViewModel(navigationService);  
}
```

프로그래밍 방식으로 생성 및 보기의 코드 숨김 내의 보기 모델의 할당 간단 하다는 장점이 있습니다. 그러나이 방식의 주요 단점은 뷰에 필요한 종속성을 사용 하 여 뷰 모델을 제공 해야 한다는 됩니다. 종속성 주입 컨테이너를 사용 하 여 뷰와 뷰 모델 간에 느슨한 연결을 유지 관리 하는 데 도움이 됩니다. 자세한 내용은 [종속성 주입](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md)합니다.

### <a name="creating-a-view-defined-as-a-data-template"></a>데이터 서식 파일로 정의 된 뷰 만들기

뷰는 데이터 템플릿으로 정의 하 고 보기 모델 유형과 연결 합니다. 데이터 템플릿 리소스로 정의할 수 있습니다 또는 보기 모델을 표시 하는 컨트롤 내에서 인라인으로 정의 될 수 있습니다. 컨트롤의 내용 보기 모델 인스턴스를 이며 데이터 템플릿을 시각적으로를 나타내는 데 사용 됩니다. 이 방법은 보기 모델 인스턴스화된 먼저 뒤에 뷰 생성 하는 상황의 예입니다.

<a name="automatically_creating_a_view_model_with_a_view_model_locator" />

### <a name="automatically-creating-a-view-model-with-a-view-model-locator"></a>보기 모델 로케이터를 사용 하 여 뷰 모델을 자동으로 만들기

보기 모델 로케이터는 모델 보기 및 보기에 해당 연결의 인스턴스화를 관리 하는 사용자 지정 클래스입니다. EShopOnContainers 모바일 앱에서의 `ViewModelLocator` 클래스에는 연결 된 속성 `AutoWireViewModel`, 보기 모델 뷰를 사용 하 여 연결 하는 데 사용 되는 합니다. 뷰의 XAML이 연결 된 속성은 다음 코드 예제에 나와 있는 것 처럼 보기 모델 보기로 자동으로 연결 합니다을 나타내도록 true로 설정 됩니다.

```xaml
viewModelBase:ViewModelLocator.AutoWireViewModel="true"
```

합니다 `AutoWireViewModel` 속성은 바인딩 가능한 속성을 false로 초기화 된 및 해당 값이 변경 될 때를 `OnAutoWireViewModelChanged` 이벤트 처리기가 호출 됩니다. 이 메서드는 뷰에 대 한 뷰 모델을 확인합니다. 다음 코드 예제에서는 이렇게 하는 방법을 보여 줍니다.

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

`OnAutoWireViewModelChanged` 메서드는 규칙 기반 접근 방식을 사용 하 여 뷰 모델을 확인 하려고 시도 합니다. 이 규칙을 가정합니다.

-   모델 보기 형식 보기와 동일한 어셈블리의 경우
-   보기에는 합니다. 뷰 자식 네임 스페이스입니다.
-   모델 보기에는 합니다. Viewmodel 자식 네임 스페이스입니다.
-   보기 모델 보기 이름으로 해당 이름과 "ViewModel"로 끝나야 합니다.

마지막으로 `OnAutoWireViewModelChanged` 메서드 집합을 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 해결된 뷰 모델 형식은 뷰 형식의 합니다. 뷰 모델 유형을 해결 하는 방법에 대 한 자세한 내용은 참조 하세요. [해상도](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#resolution)합니다.

이 방법은 앱 모델 보기 및 보기에 대 한 연결의 인스턴스화를 담당 하는 단일 클래스에 장점이 있습니다.

> [!TIP]
> 대체의 편의 위해 보기 모델 로케이터를 사용 합니다. 보기 모델 로케이터를 사용할 수 있습니다도 종속성, 대체 구현에 대 한 대체 지점으로 같은 단위 테스트 또는 디자인 타임 데이터에 대 한.

## <a name="updating-views-in-response-to-changes-in-the-underlying-view-model-or-model"></a>기본 변경 내용에 대 한 응답에서 업데이트 뷰 모델 또는 모델 보기

모든 뷰 모델 및 뷰에 액세스할 수 있는 모델 클래스를 구현 해야 합니다 `INotifyPropertyChanged` 인터페이스입니다. 뷰 모델 또는 모델 클래스에서이 인터페이스를 구현 클래스를 기본 속성 값이 변경 될 때 뷰에서 데이터 바인딩된 컨트롤에 변경 알림을 제공할 수 있습니다.

앱 해야가 직접 설계 올바른 속성 변경 알림 사용에 대 한 다음 요구 사항을 충족 합니다.

-   항상 발생을 `PropertyChanged` 이벤트는 public 속성의 값을 변경 하는 경우. 발생 하는 것을 가정 하지 마십시오는 `PropertyChanged` XAML 바인딩이 발생 하는 방법에 대 한 지식이 때문에 이벤트를 무시할 수 있습니다.
-   항상 발생을 `PropertyChanged` 이벤트에 대 한 계산 속성 값 보기에서 다른 속성에서 사용 하는 모델 또는 모델입니다.
-   항상 발생 합니다 `PropertyChanged` 변경 속성으로 설정 하는 경우 개체는 될 것으로 알려진 안전한 상태로 또는 메서드의 끝에는 이벤트입니다. 이벤트를 발생 시키는 이벤트의 처리기를 동기적으로 호출 하 여 작업을 중단 합니다. 이 작업 중간에 있을 경우에 안전 하지 않은, 부분적으로 업데이트 된 상태에 있을 때 개체 콜백 함수를 노출할 수 있습니다. 또한 있기 연계 변경에 의해 트리거되도록 하는 데 `PropertyChanged` 이벤트입니다. 연계 변경에는 일반적으로 완료 되어야만 연계 변경 내용이 실행 해도 안전한 업데이트 해야 합니다.
-   발생 하지는 `PropertyChanged` 속성이 변경 되지 않는 경우에 이벤트입니다. 이 발생 하기 전에 이전 및 새 값을 비교 해야 하는 것을 의미 합니다 `PropertyChanged` 이벤트입니다.
-   발생 하지는 `PropertyChanged` 속성을 초기화 하는 경우에 보기 모델의 생성자 중 이벤트입니다. 뷰에서 데이터 바인딩된 컨트롤은이 시점에서 변경 알림을 수신 하도록 구독가 없습니다.
-   둘 이상의 발생 하지 않습니다 `PropertyChanged` 클래스의 공용 메서드의 동기 단일 호출 내에서 동일한 속성 이름 인수는 이벤트입니다. 예를 들어를 `NumberOfItems` 백업 저장소가 속성을 `_numberOfItems` 필드, 메서드 증가 하는 경우 `_numberOfItems` 50 번의 루프를 실행 하는 동안만 시켜야 속성 변경 알림에서를 `NumberOfItems` 속성을 한 번 후 모든 작업이 완료 됩니다. 비동기 메서드를 발생 시킵니다.는 `PropertyChanged` 비동기 연속 체인의 동기 각 세그먼트에서 지정 된 속성 이름에 대 한 이벤트입니다.

모바일 앱에서 사용 하는 eShopOnContainers는 `ExtendedBindableObject` 제공 하기 위해 클래스는 다음 코드 예제에 나와 있는 알림 변경:

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

Xamarin.Form의 [ `BindableObject` ](xref:Xamarin.Forms.BindableObject) 클래스가 구현 하는 `INotifyPropertyChanged` 인터페이스를 제공 하 고는 [ `OnPropertyChanged` ](xref:Xamarin.Forms.BindableObject.OnPropertyChanged(System.String)) 메서드. `ExtendedBindableObject` 클래스를 제공 합니다 `RaisePropertyChanged` 속성을 호출 하는 방법 변경 알림 및 과정에서 제공한 기능을 사용 하 여는 `BindableObject` 클래스입니다.

EShopOnContainers 모바일 앱에서 각 보기 모델 클래스에서 파생 되는 `ViewModelBase` 클래스에서 파생 됩니다는 `ExtendedBindableObject` 클래스입니다. 따라서 각 보기 모델 클래스를 사용 합니다 `RaisePropertyChanged` 의 메서드를 `ExtendedBindableObject` 속성 변경 알림을 제공 하는 클래스입니다. 다음 코드 예제에서는 eShopOnContainers 모바일 앱은 람다 식을 사용 하 여 속성 변경 알림을 호출 하는 방법을 보여 줍니다.

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

Note는 이러한 방식으로 람다 식을 사용 하 여 적은 성능 비용 람다 식에는 각 호출에 대해 평가할 때문에 포함 됩니다. 성능은 작은 이며 일반적으로 앱에 영향을 주지는 않지만 경우 여러 변경 알림을 비용 누적 수 있습니다. 그러나이 방식의 장점은 컴파일 타임 형식 안전성 및 속성의 이름을 바꿀 때 리팩터링 지원을 제공 한다는 것입니다.

## <a name="ui-interaction-using-commands-and-behaviors"></a>명령 및 동작을 사용 하 여 UI 상호 작용

모바일 앱에서 작업은 일반적으로 코드 숨김 파일에서 이벤트 처리기를 만들어 구현할 수 있는 단추 클릭과 같은 사용자 작업에 대 한 응답으로 호출 됩니다. 그러나 MVVM 패턴의 동작을 구현 하는 것에 대 한 책임은 뷰 모델 및 코드 숨김에 코드를 배치 하지 않아야 합니다.

명령 UI에서 컨트롤에 바인딩할 수 있는 작업을 나타내는 하는 편리한 방법을 제공 합니다. 작업을 구현 하는 코드를 캡슐화 하 고 보기에서 시각적 표현에서 분리 되도록 하는 데 도움이 됩니다. Xamarin.Forms에는 명령에 선언적으로 연결할 수 있는 컨트롤이 포함 되어 있습니다. 및 컨트롤과 상호 작용할 때 이러한 컨트롤 명령을 호출 합니다.

동작에는 또한 컨트롤을 선언적으로 명령에 연결할 수 있습니다. 그러나 다양 한 컨트롤에서 발생 하는 이벤트와 연결 된 작업을 호출 하려면 동작을 사용할 수 있습니다. 따라서 동작 여러 해결 명령을 활성화 컨트롤로 동일한 시나리오의 뛰어난 유연성과 제어를 제공 하는 동안. 또한 동작 명령을 사용 하 여 상호 작용 하도록 특별히 설계 되지 않았습니다 하는 컨트롤을 사용 하 여 명령 개체 또는 메서드를 연결할 수 있습니다.

### <a name="implementing-commands"></a>명령 구현

보기 모델은 일반적으로 구현 하는 개체 인스턴스는 뷰에서 바인딩에 대 한 명령 속성을 노출 합니다 `ICommand` 인터페이스입니다. 다양 한 Xamarin.Forms 컨트롤이 제공를 `Command` 데이터 수 있는 속성에 바인딩된는 `ICommand` 뷰 모델에서 제공 하는 개체입니다. 합니다 `ICommand` 인터페이스 정의 `Execute` 작업 자체를 캡슐화 하는 메서드를 `CanExecute` 명령을 호출할 수 있는지 여부를 나타내는 메서드를 `CanExecuteChanged` 에 영향을 주는 여부를 변경 하는 경우 발생 하는 이벤트 명령을 실행 해야 합니다. [ `Command` ](xref:Xamarin.Forms.Command) 및 [ `Command<T>` ](xref:Xamarin.Forms.Command) Xamarin.Forms에서 제공 하는 클래스를 구현 합니다 `ICommand` 인터페이스를 여기서 `T` 인수의형식이`Execute`고 `CanExecute`입니다.

보기 모델 내의 없어야 형식의 개체 [ `Command` ](xref:Xamarin.Forms.Command) 하거나 [ `Command<T>` ](xref:Xamarin.Forms.Command) 형식의 보기 모델의 각 공용 속성에 대 한 `ICommand`합니다. 합니다 `Command` 또는 `Command<T>` 생성자를 사용 하려면를 `Action` 를 호출한 경우 콜백 개체는 `ICommand.Execute` 메서드가 실행 됩니다. 합니다 `CanExecute` 메서드는 선택적 생성자 매개 변수는 이며를 `Func` 반환 하는 `bool`합니다.

다음 코드에서는 어떻게를 [ `Command` ](xref:Xamarin.Forms.Command) 인스턴스 등록 명령을 나타내는 대리자를 지정 하 여 생성 되는 `Register` 모델 메서드를 보려면:

```csharp
public ICommand RegisterCommand => new Command(Register);
```

명령에 대 한 참조를 반환 하는 속성을 통해 뷰에 노출 되는 `ICommand`합니다. 경우는 `Execute` 메서드를 호출 합니다 [ `Command` ](xref:Xamarin.Forms.Command) 개체에 지정 된 대리자를 통해 보기 모델의 메서드에 호출을 전달 하기만 `Command` 생성자입니다.

사용 하 여 명령에서 비동기 메서드를 호출할 수 있습니다는 `async` 하 고 `await` 키워드는 명령을 지정 하는 경우 `Execute` 위임 합니다. 콜백 임을 나타냅니다는 `Task` 및 대기 수 해야 합니다. 예를 들어, 다음 코드에서는 어떻게를 [ `Command` ](xref:Xamarin.Forms.Command) 대리자를 지정 하 여 로그인 명령을 나타내는 경우 생성 되는 `SignInAsync` 모델 메서드를 보려면:

```csharp
public ICommand SignInCommand => new Command(async () => await SignInAsync());
```

매개 변수를 전달할 수는 `Execute` 및 `CanExecute` 사용 하 여 작업을 [ `Command<T>` ](xref:Xamarin.Forms.Command) 명령을 인스턴스화할 클래스입니다. 예를 들어, 다음 코드에서는 어떻게를 `Command<T>` 인스턴스를 나타내는 데 사용 되는 `NavigateAsync` 메서드에 필요한 형식의 인수로 `string`:

```csharp
public ICommand NavigateCommand => new Command<string>(NavigateAsync);
```

둘 다에 [ `Command` ](xref:Xamarin.Forms.Command) 및 [ `Command<T>` ](xref:Xamarin.Forms.Command) 클래스, 대리자를 `CanExecute` 각 생성자에 메서드는 선택 사항입니다. 대리자를 지정 하지 않으면 합니다 `Command` 돌아갑니다 `true` 에 대 한 `CanExecute`합니다. 그러나 뷰 모델 명령의 변경을 나타낼 수 있습니다 `CanExecute` 호출 하 여 상태를 `ChangeCanExecute` 메서드를 `Command` 개체입니다. 이 인해는 `CanExecuteChanged` 이벤트가 발생 합니다. 명령에 바인딩되는 UI에서 모든 컨트롤은 데이터 바인딩된 명령의 가용성을 반영 하도록 설정 된 상태를 업데이트 합니다.

#### <a name="invoking-commands-from-a-view"></a>뷰에서 호출 명령

다음 코드 예제에서는 어떻게를 [ `Grid` ](xref:Xamarin.Forms.Grid) 에 `LoginView` 바인딩되는 `RegisterCommand` 에 `LoginViewModel` 클래스를 사용 하 여를 [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) 인스턴스:

```xaml
<Grid Grid.Column="1" HorizontalOptions="Center">  
    <Label Text="REGISTER" TextColor="Gray"/>  
    <Grid.GestureRecognizers>  
        <TapGestureRecognizer Command="{Binding RegisterCommand}" NumberOfTapsRequired="1" />  
    </Grid.GestureRecognizers>  
</Grid>
```

명령 매개 변수를 정의할 수도 있습니다 필요에 따라 사용 하 여 [ `CommandParameter` ](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) 속성입니다. 에 필요한 인수의 형식을 지정 합니다 `Execute` 및 `CanExecute` 메서드를 대상입니다. 합니다 [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) 대상 명령이 연결된 된 컨트롤을 사용 하 여 상호 작용할 때 자동으로 호출 됩니다. 명령 매개 변수를 제공 하는 경우 전달할 인수로 명령의 `Execute` 위임 합니다.

<a name="implementing_behaviors" />

### <a name="implementing-behaviors"></a>동작 구현

동작 기능 해야만 하위 클래스에 UI 컨트롤에 추가할 수 있습니다. 대신 기능은 동작 클래스에서 구현되고 컨트롤 자체의 일부였던 것처럼 컨트롤에 연결됩니다. 동작을 사용 하 여 코드를 간결 하 게 컨트롤에 연결 하 고 수 있습니다 하나 이상의 보기 또는 앱에서 다시 사용할 수 있도록 패키지 하는 방식으로 컨트롤의 API와 직접 상호 작용 하기 때문에 코드 숨김 파일로 작성 해야 일반적으로 구현할 수 있습니다. MVVM 환경에서 동작에는 명령에 컨트롤을 연결 하는 데 유용한 접근 방식입니다.

연결 된 속성을 통해 컨트롤에 연결 된 동작 이라고는 *동작을 연결*합니다. 동작에 해당 컨트롤 또는 보기의 시각적 트리의 다른 컨트롤에 기능을 추가 연결 된 해당 요소의 노출 된 API를 사용할 수 있습니다. EShopOnContainers 모바일 앱에 포함 된 `LineColorBehavior` 클래스는 연결 된 동작입니다. 이 동작에 대 한 자세한 내용은 참조 하세요. [유효성 검사 오류 표시](~/xamarin-forms/enterprise-application-patterns/validation.md#displaying_validation_errors)합니다.

Xamarin.Forms 동작은에서 파생 된 클래스는 [ `Behavior` ](xref:Xamarin.Forms.Behavior) 또는 [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) 클래스는 `T `동작 적용 되어야 하는 컨트롤의 형식입니다. 이러한 클래스를 제공 `OnAttachedTo` 고 `OnDetachingFrom` 메서드 동작에 연결 되어 있고 컨트롤에서 분리 하는 경우 실행 되는 논리를 제공 하도록 재정의 해야 합니다.

EShopOnContainers 모바일 앱에서의 `BindableBehavior<T>` 클래스에서 파생 되는 [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) 클래스입니다. 목적은 `BindableBehavior<T>` 클래스를 필요로 하는 Xamarin.Forms 동작에 대 한 기본 클래스를 제공 하는 합니다 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 연결된 된 컨트롤에 설정 되도록 동작 합니다.

`BindableBehavior<T>` 클래스는 재정의 제공 `OnAttachedTo` 설정 하는 메서드를 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 동작 및는 재정의 가능한 `OnDetachingFrom` 정리 하는 메서드는 `BindingContext`합니다. 또한 클래스는 연결된 컨트롤에 대한 참조를 `AssociatedObject` 속성에 저장합니다.

EShopOnContainers 모바일 앱에 포함 되어는 `EventToCommandBehavior` 클래스 이벤트 발생에 대 한 응답에서 명령을 실행 합니다. 이 클래스에서 파생 되는 `BindableBehavior<T>` 동작을 바인딩할 하 고 실행할 수 있도록 클래스를 `ICommand` 에 지정 된을 `Command` 속성 동작을 사용 하는 경우. 다음 코드 예제는 `EventToCommandBehavior` 클래스를 보여줍니다.

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

`OnAttachedTo` 하 고 `OnDetachingFrom` 메서드는 등록에 정의 된 이벤트에 대 한 이벤트 처리기의 등록을 취소 하는 데 사용 되는 `EventName` 속성입니다. 이벤트가 발생 하는 경우에 다음을 `OnFired` 메서드를 호출 하면 명령을 실행 합니다.

사용 하는 이점은 `EventToCommandBehavior` 이벤트가 발생 하는 경우 명령을 실행 하는 명령을 명령을 사용 하 여 상호 작용 하도록 설계 되지 않은 컨트롤에 연결할 수 있습니다. 또한이 이동 이벤트 처리 코드 모델을 보거나 있는 단위 테스트가 가능 합니다.

#### <a name="invoking-behaviors-from-a-view"></a>보기에서 동작 호출

`EventToCommandBehavior` 명령의 명령을 지원 하지 않는 컨트롤에 연결 하는 데 특히 유용 합니다. 예를 들어,를 `ProfileView` 사용 하 여는 `EventToCommandBehavior` 실행 하는 `OrderDetailCommand` 경우를 [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) 에서 이벤트가 발생를 [ `ListView` ](xref:Xamarin.Forms.ListView) 표시 된 것 처럼 사용자의 주문을 나열 하는 다음 코드에서

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

런타임 시 합니다 `EventToCommandBehavior` 와 상호 작용에 응답할 합니다 [ `ListView` ](xref:Xamarin.Forms.ListView)합니다. 항목을 선택 하는 경우는 `ListView`, [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) 이벤트가 발생 하 고 실행 하는 합니다 `OrderDetailCommand` 에 `ProfileViewModel`. 기본적으로 이벤트에 대 한 이벤트 인수는 명령에 전달 됩니다. 에 지정 된 변환기에서 원본과 대상 간에 전달 될 때이 데이터를 변환 하는 `EventArgsConverter` 반환 하는 속성을 [ `Item` ](xref:Xamarin.Forms.ItemTappedEventArgs.Item) 의 `ListView` 에서 합니다 [ `ItemTappedEventArgs` ](xref:Xamarin.Forms.ItemTappedEventArgs). 따라서 경우 합니다 `OrderDetailCommand` 실행 되 면 선택한 `Order` 등록 된 작업에 매개 변수로 전달 됩니다.

동작에 대 한 자세한 내용은 참조 하세요. [동작](~/xamarin-forms/app-fundamentals/behaviors/index.md)합니다.

## <a name="summary"></a>요약

완전히 사용자 인터페이스 (UI)에서 응용 프로그램의 비즈니스 및 프레젠테이션 논리와 분리-Model-view-viewmodel (MVVM) 패턴 수 있습니다. 응용 프로그램 논리 및 UI 명확한 구분을 유지 관리 다양 한 개발 문제를 해결 하는 데 도움이 및 응용 프로그램을 테스트 하 고 유지 관리 하며 진화를 쉽게 만들 수 있습니다. 코드 다시 사용 하 여 기회를 크게 향상 시킬 수 있습니다 하 고 개발자 및 UI 디자이너를 더 쉽게 공동 작업할 수는 앱의 각 해당 부분을 개발 하는 경우.

패턴을 MVVM을 사용 하 여 앱의 UI 및 기본 프레젠테이션 및 비즈니스 논리는 세 가지 별도 클래스로 구분 됩니다: UI 및 UI를 캡슐화 하는 뷰를 논리 프레젠테이션 논리 및 상태를 캡슐화 하는 뷰 모델 와 응용 프로그램의 비즈니스 논리와 데이터를 캡슐화 하는 모델을 추가 합니다.


## <a name="related-links"></a>관련 링크

- [2mb PDF 전자책 다운로드](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
