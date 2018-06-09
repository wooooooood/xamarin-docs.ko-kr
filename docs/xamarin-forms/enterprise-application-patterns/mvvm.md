---
title: 모델-뷰-ViewModel 패턴
description: 이 장에서 eShopOnContainers 모바일 앱 MVVM 패턴을 사용 하 여 해당 사용자 인터페이스에서 앱의 프레젠테이션 및 비즈니스 논리를 명확 하 게 구분 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: dd8c1813-df44-4947-bcee-1a1ff2334b87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: fe2cace6a0fc3a1d901f55556eed09380f8f2006
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245434"
---
# <a name="the-model-view-viewmodel-pattern"></a>모델-뷰-ViewModel 패턴

Xamarin.Forms 개발자 환경에는 일반적으로 XAML 사용자 인터페이스를 만들고 다음 사용자 인터페이스에 작동 하는 코드 숨김을 추가 포함 됩니다. 응용 프로그램은 수정 되 고 크기 및 범위를 처럼 복잡 한 유지 관리 문제가 발생할 수 있습니다. 이러한 문제는 UI 컨트롤 및 UI 수정 및 이러한 코드 단위 테스트의 어려움의 비용을 향상 시키는 비즈니스 논리 간의 긴밀 한 연결을 포함 합니다.

모델-뷰-MVVM () 패턴 사용자 인터페이스 (UI)에서 응용 프로그램의 비즈니스 및 프레젠테이션 논리를 명확 하 게 구분 하는 데 도움이 됩니다. 응용 프로그램 논리 및 UI 명확한 구분을 유지 관리 여러 개발 문제를 해결 하는 데 도움이 및 응용 프로그램 테스트, 유지 관리 및 발전시키를 쉽게 만들 수 있습니다. 코드 다시 사용 하 여 기회를 크게 향상 시킬 수 있게 해 및 더 많은 작업을 디자이너 UI의 해당 부분 응용 프로그램의 개발 하는 경우 쉽게 공동 합니다.

## <a name="the-mvvm-pattern"></a>MVVM 패턴

MVVM 패턴에 세 가지 주요 구성 요소는 가지: 모델, 뷰 및 뷰 모델입니다. 각각 서로 다른 목적을 갖습니다. 그림 2-1은 세 가지 구성 요소 간의 관계를 나타냅니다.

![](mvvm-images/mvvm.png "MVVM 패턴")

**그림 2-1**: The MVVM 패턴

각 구성 요소 책임을 이해 하는 것 외에도 서로 상호 작용 하는 방식을 이해 하는 것이 중요 이기도 합니다. 상위 수준 보기 "에 대해 알고"의 보기 모델 및 뷰 모델 "에 대 한" 모델을 하지만 모델의 보기 모델 인식 하지 않습니다. 알려지고의 보기 모델은 보기 인식 하지 못합니다. 따라서 뷰 모델 모델에서 보기를 격리 하 고 보기와 독립적으로 발전 모델 허용.

MVVM 패턴 사용의 이점은 다음과 같습니다.

-   기존 비즈니스 논리를 캡슐화 하는 기존 모델 구현 이면 하거나 변경 하는 것은 위험할 수 있습니다. 이 시나리오의 보기 모델 역할을 모델 클래스에 대 한 어댑터 하 고 모델 코드에 주요 변경 하지 않을 수 있습니다.
-   개발자는 뷰를 사용 하지 않고 뷰 모델 및 모델에 대 한 단위 테스트를 만들 수 있습니다. 보기 모델에 대 한 단위 테스트 보기에서 사용 된 것과 동일한 기능을 발휘할 수 있습니다.
-   뷰는 XAML에서 완전히 구현 하 앱 UI는 코드를 건드리지 않고 다시 디자인 될 수 있습니다. 따라서 새 버전 보기의 기존 보기 모델을 사용 해야 합니다.
-   디자이너와 개발자가에서 작업할 수 독립적이 고 동시에 해당 구성 요소 개발 프로세스 중. 디자이너는 개발자는 뷰 모델 및 모델 구성 요소에 대해 작업할 수는 보기 집중할 수 있습니다.

MVVM를 효과적으로 사용 하 여 키는 정확한 클래스에 대 한 응용 프로그램 코드를 팩터링 하는 방법을 이해 하 고 클래스 상호 작용 하는 방법을 이해 합니다. 다음 섹션에서는 각 MVVM 패턴에서 클래스의 책임에 설명 합니다.

### <a name="view"></a>보기

보기는 구조, 레이아웃 및 사용자가 화면에서 볼의 모양을 정의 하는 일을 담당 합니다. 이상적으로 각 뷰는 제한 된 코드 숨김 비즈니스 논리를 포함 하지 않는 XAML에서 정의 됩니다. 그러나 경우에 따라 코드 숨김 애니메이션과 같은 XAML에서 표현 하기 어려운 시각적 동작을 구현 하는 UI 논리를 포함할 수 있습니다.

Xamarin.Forms 응용 프로그램에서 뷰는 일반적으로 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)-파생 된 또는 [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)-클래스를 파생 합니다. 그러나 뷰 UI 요소를 시각적으로 표시 되 면 개체를 나타내는 데 사용할 수를 지정 하는 데이터 서식 파일로 표현 될 수 있습니다. 뷰는 데이터 서식 파일에 모든 코드 숨김 없고 특정 보기 모델 형식에 바인딩 하도록 설계 되었습니다.

> [!TIP]
> 코드 숨김의 UI 요소를 설정 하거나 해제 하지 마십시오. 모델 보기를 정의 보기의 표시 되는 명령이를 사용할 수 있는지 여부 또는 작업이 보류 중인 표시 등의 일부 측면에 영향을 주는 논리 상태 변경 되는지 확인 합니다. 따라서, 사용 하도록 설정 하 고 모델 속성을 사용 하도록 설정 하 고 대신 코드 숨김의 사용 하지 않도록 보려는 바인딩을 통해 UI 요소를 사용 하지 않도록 설정 합니다.

단추 클릭과 같은 상호 작용은 보기에 대 한 응답의 보기 모델에 코드를 실행 또는 선택 항목에 대 한 여러 가지가 있습니다. 컨트롤에 명령, 컨트롤의 지 원하는 경우 `Command` 속성 수 수에 데이터 바인딩된은 `ICommand` 의 보기 모델에는 속성입니다. 컨트롤의 명령이 호출 될 때의 보기 모델의 코드가 실행 됩니다. 명령, 외에도 동작 보기에 있는 개체에 연결할 수 및 명령을 호출할 수 또는 이벤트를 발생을 수신할 수 있습니다. 에 대 한 응답 동작 호출할 수는 `ICommand` 뷰 모델 또는 보기 모델에 대 한 메서드.

### <a name="viewmodel"></a>ViewModel

뷰 모델 속성 및 보기 수, 데이터 바인딩 명령을 구현 하 고 변경 알림 이벤트를 통해 상태 변경에 대해 보기를에 알립니다. 속성과 뷰 모델을 제공 하는 명령을 UI에 의해 제공 하는 기능을 정의 하지만 보기 표시할 해당 기능을 하는 방법을 결정 합니다.

> [!TIP]
> UI 응답성이 뛰어난 비동기 작업을 유지 합니다. 모바일 앱에 대 한 사용자의 인식 성능을 개선 하기 위해 차단 해제 UI 스레드를 유지 해야 합니다. 따라서 보기 모델에서 I/O 작업에 대 한 비동기 메서드를 사용 하 고 비동기적으로 속성 변경의 뷰를 알리는 이벤트를 발생 시킵니다.

보기 모델에 대해 필요한 모든 모델 클래스와 상호 작용 보기의 조정을 담당 이기도 합니다. 뷰 모델 및 모델 클래스 간의 일 대 다 관계는 일반적으로 합니다. 뷰 모델 보기에서 컨트롤에 직접 데이터 바인딩할 수 있도록 보기로 직접 모델 클래스를 노출할 수도 있습니다. 이 경우 모델 클래스를 사용 하 여 데이터 바인딩을 지원 하며 변경 알림 이벤트 하도록 디자인 해야 합니다.

각 보기 모델 보기 쉽게 사용할 수 있는 형태로 모델의 데이터를에서 제공 합니다. 뷰 모델은이를 위해 경우에 따라 데이터 변환을 수행 합니다. 이 데이터 변환의 보기 모델에 배치는 것이 좋습니다 보기에 바인딩할 수 있는 속성을 제공 하기 때문에 있습니다. 예를 들어 뷰 모델 보기에서 표시 하기 위해 쉽게 수행할 수 있도록 두 속성의 값을 결합할 수 있습니다.

> [!TIP]
> 변환 계층에서 데이터 변환을 중앙 집중화 합니다. 변환기를 사용 하 여 뷰 모델 및 뷰 사이 있는 별도 데이터 변환 계층으로도 가능 합니다. 예를 들어 데이터의 보기 모델을 제공 하지 않는 한 서식이 해야 하는 경우이 방법을 필요할 수 있습니다.

속성 보기와 양방향 데이터 바인딩을에 참여할 수의 보기 모델에 대 한 순서로 발생 해야는 `PropertyChanged` 이벤트입니다. 모델 보기를 구현 하 여이 요구 사항을 충족는 `INotifyPropertyChanged` , 인터페이스 및 발생 된 `PropertyChanged` 속성이 변경 될 때 이벤트입니다.

컬렉션의 경우 보기 친화적인 `ObservableCollection<T>` 제공 됩니다. 이 컬렉션을 구현 하지 않아도 개발자 하므로 컬렉션 변경 알림을 구현 하는 `INotifyCollectionChanged` 컬렉션에 대 한 인터페이스입니다.

### <a name="model"></a>모델

모델 클래스는 응용 프로그램의 데이터를 캡슐화 하는 비시각적 클래스입니다. 따라서 모델 일반적으로 비즈니스 및 유효성 검사 논리와 함께 데이터 모델을 포함 하는 응용 프로그램의 도메인 모델을 나타내는 것으로 생각할 수 있습니다. 모델 개체의 예로 데이터 전송 개체 (Dto), 일반 이전 CLR 개체 (POCOs) 및 생성 된 엔터티 및 프록시 개체를 들 수 있습니다.

모델 클래스는 일반적으로 서비스 또는 데이터 액세스 및 캐싱 캡슐화 하는 저장소와 함께에서 사용 됩니다.

## <a name="connecting-view-models-to-views"></a>보기에 연결 하는 모델 보기

모델 보기 Xamarin.Forms의 데이터 바인딩 기능을 사용 하 여 보기에 연결할 수 있습니다. 뷰를 생성 하 고 모델을 볼 하 고 런타임 시 연결에 사용할 수 있는 방법은 여러 가지가 있습니다. 이러한 접근 방식을 보기 첫 번째 구성 및 보기 모델에 대 한 첫 번째 구성으로는 두 가지 범주로 나뉩니다. 보기의 첫 번째 구성 및 보기 모델 첫 번째 컴퍼지션은 기본 설정 및 복잡성 문제 중에서 선택 합니다. 그러나 방법이 모두 동일한 목표를 BindingContext 속성에 할당 된 보기 모델을 갖고자 보기 위한 공유 합니다.

보기와 첫 번째 컴퍼지션 응용 프로그램에 종속 보기 모델에 연결 된 뷰 개념적으로 구성 됩니다. 이 방법의 주요 이점은 수행 하는 것을 쉽게 생성 느슨하게 결합 된 단위 테스트 가능 응용 프로그램 모델 보기 직접 보기에 종속 되지 않음 없으므로입니다. 클래스의 생성 및 연결 방법을 이해 하는 코드 실행을 추적 하는 대신 해당 시각적 구조를 다음으로 응용 프로그램의 구조를 이해 하기 쉬운 이기도 합니다. 또한 보기 첫 번째 생성 탐색이 발생할 때 생성 페이지에 대 한 책임은 복잡 하 고 플랫폼 불일치 뷰 모델 첫 번째 작성 하는 Xamarin.Forms 내비게이션 시스템으로 맞춥니다.

보기와 모델 첫 번째 컴퍼지션 앱 보기 모델에 대 한 뷰를 찾기에 대 한 책임 서비스 있는 모델 보기는 개념적으로 구성 됩니다. 보기 모델에 대 한 첫 번째 구성 보기 생성 추상화할 수 있습니다, 응용 프로그램의 UI가 아닌 논리 구조에 집중할 수 있도록 하므로 일부 개발자 들에 게 더 자연스럽 게 받아들이며 합니다. 다른 모델의 보기를 통해 생성 모델을 보기 수 있습니다. 그러나이 방법은 종종 복잡 하며 앱의 다양 한 부분 생성 및 연결 방법을 이해 하기 어려울 수 있습니다.

> [!TIP]
> 모델 보기 및 보기 독립적으로 유지 합니다. 속성에 데이터 원본 뷰 바인딩을 해당 되는 보기 모델에 대 한 보기의 주 종속성 이어야 합니다. 특히,와 같은 보기 유형을 참조 하지 않는 [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) 및 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/), 모델 보기에서에서. 여기에서 설명 하는 원칙을 따르면 격리 수준 인해 범위를 제한 하 여 소프트웨어 결함의 가능성을 줄이는 보기 모델을 테스트할 수 있습니다.

다음 섹션에서는 모델 보기 보기에 연결 하는 기본 접근 방법을 설명 합니다.

### <a name="creating-a-view-model-declaratively"></a>뷰 모델을 선언적으로 만들기

가장 간단한 방법은 선언적으로 XAML에는 해당 되는 보기 모델을 인스턴스화할 수 보기에 대 한 것입니다. 뷰를 생성할 때 해당 보기 모델 개체도 생성 됩니다. 이 방법은 다음 코드 예제에 설명 된:

```xaml
<ContentPage ... xmlns:local="clr-namespace:eShop">  
    <ContentPage.BindingContext>  
        <local:LoginViewModel />  
    </ContentPage.BindingContext>  
    ...  
</ContentPage>
```

경우는 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 만들어지면의 인스턴스는 `LoginViewModel` 자동으로 구현 되 고를 보기로 설정 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)합니다.

이 선언적 생성 및 할당 뷰에서 뷰 모델의 장점이 것는 간단 하지만 보기 모델에서 기본 (매개 변수가 없는) 생성자 필요 하다는 단점이 있습니다.

### <a name="creating-a-view-model-programmatically"></a>프로그래밍 방식으로 뷰 모델 만들기

보기의 보기 모델에 할당 되는 코드 숨김 파일에 코드를 넣을 수는 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) 속성입니다. 종종 이렇게 보기의 생성자에 다음 코드 예제에 나와 있는 것 처럼:

```csharp
public LoginView()  
{  
    InitializeComponent();  
    BindingContext = new LoginViewModel(navigationService);  
}
```

프로그래밍 방식으로 생성 및 보기의 코드 숨김 내에서 보기 모델의 할당에는 간단 하다는 이점이 있습니다. 그러나이 방법의 주요 단점은 보기 보기 모델에 대 한 모든 필수 종속성 제공 해야 하는 합니다. 종속성 주입 컨테이너를 사용 하 여 낮은 보기와 보기 모델 간에 결합 유지 관리 하는 데 도움이 됩니다. 자세한 내용은 참조 [종속성 주입](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md)합니다.

### <a name="creating-a-view-defined-as-a-data-template"></a>데이터 서식 파일로 정의 된 보기를 만드는 방법

뷰는 데이터 템플릿을으로 정의 하 고 보기 모델 유형에 연결 된 될 수 있습니다. 데이터 템플릿 리소스를 정의할 수 있습니다 또는 보기 모델을 표시 하는 컨트롤 내에서 인라인으로 정의 될 수 있습니다. 컨트롤의 콘텐츠를 보기 모델 인스턴스 및 데이터 템플릿이 것을 시각적으로 나타내는 데 사용 됩니다. 이 방법은는 뷰 모델 인스턴스화될 먼저 보기의 생성 하는 경우의 예에 설명 합니다.

<a name="automatically_creating_a_view_model_with_a_view_model_locator" />

### <a name="automatically-creating-a-view-model-with-a-view-model-locator"></a>자동으로 보기 모델 로케이터에 뷰 모델 만들기

뷰 모델 로케이터는 모델 보기 및 보기에 연결 하 여의 인스턴스화를 관리 하는 사용자 지정 클래스입니다. EShopOnContainers 모바일 앱에서의 `ViewModelLocator` 클래스에 연결된 된 속성 `AutoWireViewModel`, 보기와 보기 모델에 연결 하는 데 사용 되는 합니다. 보기의 XAML에서이 연결 된 속성은 다음 코드 예제에 나와 있는 것 처럼 뷰 모델 보기에 자동으로 연결 해야 나타내도록 true로 설정 됩니다.

```xaml
viewModelBase:ViewModelLocator.AutoWireViewModel="true"
```

`AutoWireViewModel` 속성은 false로 초기화 되 고 해당 값이 변경 될 때 바인딩 가능한 속성은 `OnAutoWireViewModelChanged` 이벤트 처리기가 호출 됩니다. 이 메서드는 뷰에 대 한 뷰 모델을 확인합니다. 다음 코드 예제에서는이 달성 하는 방법을 보여 줍니다.

```csharp
private static void OnAutoWireViewModelChanged(BindableObject bindable, object oldValue, object newValue)  
{  
    var view = bindable as Element;  
    if (view == null)  
    {  
        return;  
    }  

    var viewType = view.GetType();  
    var viewName = viewType.FullName.Replace(".Views.", ".ViewModels.");  
    var viewAssemblyName = viewType.GetTypeInfo().Assembly.FullName;  
    var viewModelName = string.Format(  
        CultureInfo.InvariantCulture, "{0}Model, {1}", viewName, viewAssemblyName);  

    var viewModelType = Type.GetType(viewModelName);  
    if (viewModelType == null)  
    {  
        return;  
    }  
    var viewModel = _container.Resolve(viewModelType);  
    view.BindingContext = viewModel;  
}
```

`OnAutoWireViewModelChanged` 메서드는 규칙 기반 접근 방식을 사용 하 여 뷰 모델을 확인 하려고 시도 합니다. 이 규칙이 있다고 가정 합니다.

-   모델 보기는 보기 형식으로 동일한 어셈블리에 있습니다.
-   뷰는 한 합니다. 뷰 자식 네임 스페이스입니다.
-   모델 보기에 있는 한 합니다. Viewmodel 자식 네임 스페이스입니다.
-   모델 뷰 이름에 해당 이름과 볼 "ViewModel"로 끝나야 합니다.

마지막으로 `OnAutoWireViewModelChanged` 메서드 집합은 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) 보기 형식이 해결된 보기 모델 형식으로. 보기 모델 유형을 확인 하는 방법에 대 한 자세한 내용은 참조 [해상도](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#resolution)합니다.

이 방법은 응용 프로그램 모델 보기 및 보기에 연결의 인스턴스화를 담당 하는 단일 클래스에 이점이 있습니다.

> [!TIP]
> 대체 하기 쉽도록 보기 모델 표시기를 사용 합니다. 뷰 모델 로케이터를 사용 하는 데도 사용할 수 있습니다 종속 항목의 대체 구현 위한 대체 지점으로와 같은 단위 테스트 또는 디자인 타임 데이터에 대 한 합니다.

## <a name="updating-views-in-response-to-changes-in-the-underlying-view-model-or-model"></a>기본 변경 사항에 따라 업데이트 보기 모델 또는 모델 보기

모든 모델 보기 및 보기에 액세스할 수 있는 모델 클래스를 구현 해야는 `INotifyPropertyChanged` 인터페이스입니다. 기본 속성 값이 변경 될 때 보기에서 데이터 바인딩된 컨트롤에 대 한 변경 알림을 제공 하기 위해 클래스를 허용 보기 모델 또는 모델 클래스에서이 인터페이스를 구현 합니다.

다음 요구 사항을 충족 하 여 앱 속성 변경 알림의 올바른 사용에 대 한 설계 해야 합니다.

-   항상 발생 한 `PropertyChanged` 이벤트 공용 속성의 값을 변경 합니다. 해당를 발생 시키는 가정 하지 않습니다는 `PropertyChanged` XAML 바인딩이 발생 하는 방법에 대 한 지식이 때문에 이벤트를 무시할 수 있습니다.
-   항상 발생 한 `PropertyChanged` 이벤트에 대 한 해당 값은 보기에서 다른 속성에 의해 사용 되는 속성 계산 모델 또는 모델입니다.
-   항상 발생는 `PropertyChanged` 하는 속성을 변경 하 고, 전화를 걸거나 개체 안전 상태인 것으로 알려진 경우 메서드의 끝에서 이벤트입니다. 이벤트를 발생 시키는 이벤트의 처리기를 동기적으로 호출 하 여 작업을 중단 합니다. 작업 중간에 이런 경우 부분적으로 업데이트 된, 안전 하지 않은 상태에 있을 때 개체 콜백 함수를 노출할 수 있습니다. 또한에 의해 트리거될 연계 변경 내용은 `PropertyChanged` 이벤트입니다. 연계 변경에는 일반적으로 연계 변경 실행 해도 안전한 전에 먼저 완료 되어야 하는 업데이트 해야 합니다.
-   발생 하지는 `PropertyChanged` 속성이 변경 되지 않는 경우에 이벤트입니다. 즉, 발생 하기 전에 기존 패턴과 새 값을 비교 해야 합니다는 `PropertyChanged` 이벤트입니다.
-   발생 하지는 `PropertyChanged` 속성을 초기화 하는 경우 뷰 모델의 생성자 중 이벤트입니다. 보기에서 데이터 바인딩된 컨트롤 변경 알림을이 시점에서 수신 하도록 등록 되지 않습니다.
-   둘 이상의 발생 시키는 하지 `PropertyChanged` 단일 클래스의 public 메서드의 동기 호출 내에서 동일한 속성 이름 인수를 사용 하 여 이벤트입니다. 예를 들어는 `NumberOfItems` 해당 백업 저장소가 속성은 `_numberOfItems` 메서드 씩 증가 하는 경우 필드 `_numberOfItems` 50 번 루프가 실행 될 때만 발생 시켜야 속성 변경 알림을에 `NumberOfItems` 속성을 한 번 후 모든 작업이 완료 됩니다. 비동기 메서드를 발생 시킵니다.는 `PropertyChanged` 비동기 연속 체인의 각 동기 세그먼트에 지정 된 속성 이름에 대 한 이벤트입니다.

EShopOnContainers 모바일 앱 사용 하 여는 `ExtendedBindableObject` 제공 하는 다음 코드 예제에 나와 있는 알림, 변경:

```csharp
public abstract class ExtendedBindableObject : BindableObject  
{  
    public void RaisePropertyChanged<T>(Expression<Func<T>> property)  
    {  
        var name = GetMemberInfo(property).Name;  
        OnPropertyChanged(name);  
    }  

    private MemberInfo GetMemberInfo(Expression expression)  
    {  
        ...  
    }  
}
```

Xamarin.Form의 [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) 클래스가 구현 하는 `INotifyPropertyChanged` 인터페이스를 제공 하 고는 [ `OnPropertyChanged` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.OnPropertyChanged/p/System.String/) 메서드. `ExtendedBindableObject` 클래스를 제공는 `RaisePropertyChanged` 메서드를 호출 하는 속성 변경 알림 및이 작업에 의해 제공 되는 기능을 사용 하 여는 `BindableObject` 클래스입니다.

EShopOnContainers 모바일 앱에서 각 보기 모델 클래스에서 파생 되는 `ViewModelBase` 클래스에서 파생 된 `ExtendedBindableObject` 클래스. 따라서 각 보기 모델 클래스를 사용 하 여는 `RaisePropertyChanged` 에서 메서드는 `ExtendedBindableObject` 속성 변경 알림을 제공 하는 클래스입니다. 다음 코드 예제에서는 람다 식을 사용 하 여 eShopOnContainers 모바일 앱 속성 변경 알림을 호출 하는 방법을 보여 줍니다.

```csharp
public bool IsLogin  
{  
    get  
    {  
        return _isLogin;  
    }  
    set  
    {  
        _isLogin = value;  
        RaisePropertyChanged(() => IsLogin);  
    }  
}
```

Note는 이러한 방식으로 람다 식을 사용 하 여 작은 성능 비용이 각 호출에 대해 계산 되는 람다 식에 있기 때문에 포함 됩니다. 성능 비용이 작습니다. 일반적으로 응용 프로그램에 영향을 주지는 있지만 경우 많은 변경 알림을 비용 계산 수 있습니다. 그러나이 방법의 장점은 컴파일 타임 형식 안전성 및 속성의 이름을 바꿀 때 리팩터링 지원은 제공 한다는 것입니다.

## <a name="ui-interaction-using-commands-and-behaviors"></a>명령 및 동작을 사용 하 여 UI 상호 작용

모바일 앱에서 작업은 일반적으로 코드 숨김 파일에 이벤트 처리기를 만들어 구현할 수 있는 단추 클릭과 같은 사용자 동작에 대 한 응답으로 호출 됩니다. 그러나 MVVM 패턴 동작 구현에 대 한 책임은 뷰 모델 및 코드 숨김의 코드를 배치 하지 말아야 합니다.

명령은 ui에서 컨트롤에 바인딩할 수 있는 동작을 나타냅니다 하는 편리한 방법을 제공 합니다. 작업을 구현 하는 코드를 캡슐화 하 고 보기에서의 시각적 표시에서 분리 하는 데 도움이 됩니다. Xamarin.Forms는 명령에 선언적으로 연결할 수 있는 컨트롤을 포함 하 고 컨트롤과 상호 작용할 때 이러한 컨트롤 명령을 호출 합니다.

동작에는 또한 컨트롤에는 명령에 선언적으로 연결할 수 있습니다. 그러나 동작을 사용 하는 컨트롤에 의해 발생 한 이벤트의 범위와 관련 된 작업을 호출 하 수 있습니다. 따라서 동작 주소 많은 명령을 사용할 수 컨트롤로 동일한 시나리오는 더 높은 수준의 유연성 및 제어를 제공 합니다. 또한 이러한 동작 명령 개체 또는 메서드 명령을와 상호 작용 하도록 특별히 디자인 되지 않은 컨트롤을 연결 하려면 데도 수 있습니다.

### <a name="implementing-commands"></a>명령 구현

모델 보기는 일반적으로 보기에서 바인딩을 인스턴스인 개체를 구현 하는 명령 속성을 노출 된 `ICommand` 인터페이스입니다. 다양 한 Xamarin.Forms 컨트롤이 제공는 `Command` 데이터 될 수 있는 속성에 바인딩된는 `ICommand` 보기 모델에서 제공 하는 개체입니다. `ICommand` 인터페이스 정의 `Execute` 작업 자체를 캡슐화 하는 메서드는 `CanExecute` 명령을 호출할 수 있는지 여부를 나타내는 메서드를 `CanExecuteChanged` 변경 여부에 영향을 주는 될 때 발생 하는 이벤트 명령을 실행 해야 합니다. [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) 및 [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) Xamarin.Forms에에서 제공 하는 클래스를 구현에서 `ICommand` 인터페이스, 여기서 `T` 에대한인수의형식이`Execute`및 `CanExecute`합니다.

뷰 모델 내에서 있어야 형식의 개체로 [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) 또는 [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) 유형의 보기 모델의 각 public 속성에 대 한 `ICommand`합니다. `Command` 또는 `Command<T>` 필요한 생성자는 `Action` 가 호출 하는 콜백 개체는 `ICommand.Execute` 메서드가 호출 됩니다. `CanExecute` 메서드는 선택적 생성자 매개 변수 이며,이 `Func` 반환 하는 `bool`합니다.

다음 코드 방법을 보여 줍니다는 [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) 레지스터 명령을 나타내는 인스턴스가 대리자를 지정 하 여 생성 되는 `Register` 모델 메서드를 보려면:

```csharp
public ICommand RegisterCommand => new Command(Register);
```

명령에 대 한 참조를 반환 하는 속성을 통해 뷰에 노출 되는 `ICommand`합니다. 경우는 `Execute` 메서드가 호출 되는 [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) 개체에 지정 된 대리자를 통해 뷰 모델에서 메서드를 호출 하기만 하면 전달는 `Command` 생성자입니다.

사용 하 여 명령에서 비동기 메서드를 호출할 수 있습니다는 `async` 및 `await` 키워드는 명령을 지정 하는 경우 `Execute` 위임 합니다. 이 콜백은 된다는 의미는 `Task` 대기 해야 하 고 합니다. 예를 들어 다음 코드 방법을 [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) 로그인 명령을 나타내는 인스턴스가 대리자를 지정 하 여 생성 되는 `SignInAsync` 모델 메서드를 보려면:

```csharp
public ICommand SignInCommand => new Command(async () => await SignInAsync());
```

매개 변수를 전달할 수는 `Execute` 및 `CanExecute` 사용 하 여 동작은 [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) 명령을 인스턴스화할 클래스입니다. 예를 들어 다음 코드 방법을 `Command<T>` 인스턴스를 나타내는 데 사용 되는 `NavigateAsync` 메서드에 형식 인수를 필요한 `string`:

```csharp
public ICommand NavigateCommand => new Command<string>(NavigateAsync);
```

모두에 [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) 및 [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) 클래스, 대리자는 `CanExecute` 각 생성자에 대 한 메서드는 선택 사항입니다. 대리자를 지정 하지 않으면는 `Command` 돌아갑니다 `true` 에 대 한 `CanExecute`합니다. 그러나 뷰 모델 명령의 변경을 나타낼 수 있습니다 `CanExecute` 호출 하 여 상태는 `ChangeCanExecute` 에서 메서드는 `Command` 개체입니다. 이 인해는 `CanExecuteChanged` 이벤트를 발생 합니다. UI에 명령에 바인딩되는 컨트롤은 데이터 바인딩된 명령의 가용성을 반영 하기 위해 활성화 된 상태를 업데이트 합니다.

#### <a name="invoking-commands-from-a-view"></a>보기에서 명령을 호출

다음 코드 예제와 어떻게는 [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) 에 `LoginView` 바인딩되는 `RegisterCommand` 에 `LoginViewModel` 클래스를 사용 하 여는 [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) 인스턴스:

```xaml
<Grid Grid.Column="1" HorizontalOptions="Center">  
    <Label Text="REGISTER" TextColor="Gray"/>  
    <Grid.GestureRecognizers>  
        <TapGestureRecognizer Command="{Binding RegisterCommand}" NumberOfTapsRequired="1" />  
    </Grid.GestureRecognizers>  
</Grid>
```

명령 매개 변수는 필요에 따라를 정의할 수도 있습니다를 사용 하 여 [ `CommandParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TapGestureRecognizer.CommandParameter/) 속성입니다. 예상 된 인수의 형식에 지정 된는 `Execute` 및 `CanExecute` 메서드 대상으로 합니다. [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) 대상 명령이 연결된 된 컨트롤와 상호 작용할 때 자동으로 호출 됩니다. 명령에 인수로 전달 명령 매개 변수를 제공 하면 됩니다 `Execute` 위임 합니다.

<a name="implementing_behaviors" />

### <a name="implementing-behaviors"></a>동작 구현

동작 기능 하위 클래스에 필요 없이 UI 컨트롤에 추가할 수 있습니다. 대신 기능 동작 클래스에서 구현 되 고 컨트롤 자체의 일부 하는 경우 해당 컨트롤에 연결. 동작을 사용 하는 간단 하 게 해당 컨트롤에 연결 하 고 수 있습니다 하나 이상의 보기 또는 응용 프로그램 간에 다시 사용할 수 있도록 패키지 하는 방식으로 컨트롤의 API와 직접 상호 작용 하기 때문에 코드 숨김 파일로 작성 해야 일반적으로 코드를 구현할 수 있습니다. MVVM의 컨텍스트에서 동작은 명령에 컨트롤을 연결 하는 데 유용한 접근 방법은.

연결 된 속성을 통해 컨트롤에 연결 하는 동작 라고는 *동작의 첨부 된*합니다. 동작 기능을 추가 하려면 해당 컨트롤 또는 기타 컨트롤을 보기의 시각적 트리에 연결 되는 요소의 노출 된 API를 유도할 수 있습니다. EShopOnContainers 모바일 앱에 포함 되어는 `LineColorBehavior` 클래스는 연결 된 동작입니다. 이 문제에 대 한 자세한 내용은 참조 [유효성 검사 오류 표시](~/xamarin-forms/enterprise-application-patterns/validation.md#displaying_validation_errors)합니다.

Xamarin.Forms 동작은에서 파생 되는 클래스는 [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/) 또는 [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) 클래스, 여기서 `T `동작 적용 되어야 하는 컨트롤의 형식입니다. 이 클래스는 제공 `OnAttachedTo` 및 `OnDetachingFrom` 메서드 동작에 연결 되어 있고 컨트롤에서 분리 하는 경우 실행 되는 논리를 제공 하도록 재정의 해야 합니다.

EShopOnContainers 모바일 앱에서의 `BindableBehavior<T>` 클래스에서 파생 되는 [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) 클래스입니다. 용도 `BindableBehavior<T>` 필요로 하는 Xamarin.Forms 동작에 대 한 기본 클래스를 제공 하는 클래스는 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) 연결된 된 컨트롤에 설정할 동작의 합니다.

`BindableBehavior<T>` 클래스는 재정의할 수 있는 제공 `OnAttachedTo` 설정 하는 메서드는 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) 는 동작 및는 overridable `OnDetachingFrom` 를 정리 하는 메서드는 `BindingContext`합니다. 이 클래스에 연결된 된 컨트롤에 대 한 참조를 저장 하는 또한는 `AssociatedObject` 속성입니다.

EShopOnContainers 모바일 앱에 포함 되어는 `EventToCommandBehavior` 이벤트 발생에 대 한 응답의 명령을 실행 하는 클래스입니다. 이 클래스에서 파생 되는 `BindableBehavior<T>` 동작 바인딩할 하 고 실행할 수 있도록 클래스는 `ICommand` 에 지정 된는 `Command` 동작 사용 되는 경우이 속성입니다. 다음 코드 예제는 `EventToCommandBehavior` 클래스를 보여줍니다.

```csharp
public class EventToCommandBehavior : BindableBehavior<View>  
{  
    ...  
    protected override void OnAttachedTo(View visualElement)  
    {  
        base.OnAttachedTo(visualElement);  

        var events = AssociatedObject.GetType().GetRuntimeEvents().ToArray();  
        if (events.Any())  
        {  
            _eventInfo = events.FirstOrDefault(e => e.Name == EventName);  
            if (_eventInfo == null)  
                throw new ArgumentException(string.Format(  
                        "EventToCommand: Can't find any event named '{0}' on attached type",   
                        EventName));  

            AddEventHandler(_eventInfo, AssociatedObject, OnFired);  
        }  
    }  

    protected override void OnDetachingFrom(View view)  
    {  
        if (_handler != null)  
            _eventInfo.RemoveEventHandler(AssociatedObject, _handler);  

        base.OnDetachingFrom(view);  
    }  

    private void AddEventHandler(  
            EventInfo eventInfo, object item, Action<object, EventArgs> action)  
    {  
        ...  
    }  

    private void OnFired(object sender, EventArgs eventArgs)  
    {  
        ...  
    }  
}
```

`OnAttachedTo` 및 `OnDetachingFrom` 메서드 등록 하 고에 정의 된 이벤트에 대 한 이벤트 처리기의 등록을 취소를 사용 하는 `EventName` 속성입니다. 그런 다음 이벤트가 발생할 때의 `OnFired` 명령을 실행 하는 메서드가 호출 됩니다.

사용 시의 이점은 `EventToCommandBehavior` 는 이벤트가 발생할 때 명령을 실행 하는 명령을 명령을 사용 하 여 상호 작용 하도록 설계 되지 않은 컨트롤에 연결할 수 있습니다. 또한이 이벤트 처리 코드를 이동 모델 보기 수 있는 단위 테스트 합니다.

#### <a name="invoking-behaviors-from-a-view"></a>보기에서 동작 호출

`EventToCommandBehavior` 명령의 명령을 지원 하지 않는 컨트롤에 연결 하는 데 특히 유용 합니다. 예를 들어는 `ProfileView` 사용 하 여는 `EventToCommandBehavior` 실행 하는 `OrderDetailCommand` 때는 [ `ItemTapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemTapped/) 에 이벤트가 발생는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 표시 된 것 처럼 사용자의 주문을 나열 하는 다음 코드:

```xaml
<ListView>  
    <ListView.Behaviors>  
        <behaviors:EventToCommandBehavior             
            EventName="ItemTapped"  
            Command="{Binding OrderDetailCommand}"  
            EventArgsConverter="{StaticResource ItemTappedEventArgsConverter}" />  
    </ListView.Behaviors>  
    ...  
</ListView>
```

런타임에 `EventToCommandBehavior` 와 상호 작용에 응답 하는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)합니다. 항목을 선택 하는 경우는 `ListView`, [ `ItemTapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemTapped/) 이벤트는 발생 된 실행 될는 `OrderDetailCommand` 에 `ProfileViewModel`합니다. 기본적으로 이벤트에 대 한 이벤트 인수는 명령에 전달 됩니다. 에 지정 된 변환기에서 원본과 대상 간에 전달 될 때이 데이터를 변환 하는 `EventArgsConverter` 속성을 반환 하는 [ `Item` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemTappedEventArgs.Item/) 의 `ListView` 에서 [ `ItemTappedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemTappedEventArgs/). 따라서,는 `OrderDetailCommand` 실행 되는 선택한 `Order` 등록 된 작업 매개 변수로 전달 됩니다.

동작에 대 한 자세한 내용은 참조 [동작](~/xamarin-forms/app-fundamentals/behaviors/index.md)합니다.

## <a name="summary"></a>요약

모델-뷰-MVVM () 패턴 사용자 인터페이스 (UI)에서 응용 프로그램의 비즈니스 및 프레젠테이션 논리를 명확 하 게 구분 하는 데 도움이 됩니다. 응용 프로그램 논리 및 UI 명확한 구분을 유지 관리 여러 개발 문제를 해결 하는 데 도움이 및 응용 프로그램 테스트, 유지 관리 및 발전시키를 쉽게 만들 수 있습니다. 코드 다시 사용 하 여 기회를 크게 향상 시킬 수 있게 해 및 더 많은 작업을 디자이너 UI의 해당 부분 응용 프로그램의 개발 하는 경우 쉽게 공동 합니다.

패턴은 MVVM를 사용 하 여 응용 프로그램의 UI 및 기본 프레젠테이션 및 비즈니스 논리는 세 가지 별도 클래스도 분리 되어: 보기 UI와 UI를 캡슐화 하는 논리; 프레젠테이션 논리 및 상태; 캡슐화 하는 뷰 모델 및 응용 프로그램의 비즈니스 논리와 데이터를 캡슐화 하는 모델을 추가 합니다.


## <a name="related-links"></a>관련 링크

- [EBook (2mb PDF)를 다운로드 합니다.](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
