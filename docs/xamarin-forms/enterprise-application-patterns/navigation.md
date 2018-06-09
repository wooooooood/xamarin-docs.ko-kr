---
title: 엔터프라이즈 응용 프로그램 탐색
description: 이 장에서 eShopOnContainers 모바일 앱 모델 보기에서에서 보기 중심 모델 탐색을 수행 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 4cad57b5-7fe4-4527-a988-d9b60c9620b4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 9ac9f3200440001752c07ad45fdaaf2b1d9ba6a5
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243683"
---
# <a name="enterprise-app-navigation"></a>엔터프라이즈 응용 프로그램 탐색

Xamarin.Forms에는 일반적으로 UI와 사용자의 상호 작용 또는 내부 논리 기반 상태 변경으로 인해 응용 프로그램 자체에서 간격은 페이지 탐색에 대 한 지원이 포함 되어 있습니다. 그러나 탐색 다음 과제를 충족 해야 하는 대로 모델-뷰-MVVM () 패턴을 사용 하는 앱에서 구현 하는 복잡 한 하나일 수 있습니다.

-   밀접 하 게 결합 하 고 뷰 간의 종속성을 발생 하지 않도록 하는 방식을 사용 하 여 보기를 탐색할 수를 식별 하는 방법입니다.
-   기준인 탐색할 수 있으며 보기를 인스턴스화할 초기화 프로세스를 조정 하는 방법입니다. MVVM를 사용할 때는 뷰와 뷰 모델 해야 인스턴스화되고 보기의 바인딩 컨텍스트를 통해 서로 연결 합니다. 응용 프로그램 종속성 주입 컨테이너를 사용 하는 경우 뷰 및 모델 보기의 인스턴스화 특정 생성 메커니즘이 필요할 수 있습니다.
-   보기 중심 탐색을 수행 하거나 모델 중심 탐색을 볼 여부를 나타냅니다. 보기 중심 탐색이 있는 이동할 페이지의 보기 유형 이름으로 참조 합니다. 탐색 하는 동안 지정 된 보기가 인스턴스화될, 해당 되는 뷰 모델 및 다른 종속 된 서비스와 함께 합니다. 다른 접근 방식은 이동할 페이지 보기 모델 형식의 이름으로 참조 하는 위치에 보기 중심 모델 탐색을 사용 하는 것입니다.
-   방법을 명확 하 게 하려면 뷰와 뷰 모델에서 응용 프로그램의 탐색 동작을 구분 합니다. MVVM 패턴은 응용 프로그램의 UI 프레젠테이션 및 비즈니스 논리 간의 분리를 제공 합니다. 그러나 응용 프로그램의 탐색 동작 응용 프로그램의 UI 및 프레젠테이션을 부분 눈금이 경우가 많습니다. 사용자는 보기에서 탐색을 시작 종종 하 하 고 보기의 탐색의 결과로 대체 됩니다. 그러나 탐색 종종도 해야 세션을 시작 하거나 보기 모델 내에서 조정 합니다.
-   초기화 위해 탐색 하는 동안 매개 변수를 전달 하는 방법. 예를 들어 사용자가 주문 세부 정보를 업데이트 하는 뷰를 탐색 하는 경우 주문 데이터가 올바른 데이터를 표시할 수 있도록 뷰에 전달 해야 합니다.
-   특정 비즈니스 규칙 맞는 됩니다 되도록 공동 누진 탐색 하는 방법. 예를 들어 사용자가 제출 하거나 보기 내에서 수행 된 모든 데이터 변경 내용을 취소 하 라는 메시지가 표시 하거나에서 잘못 된 데이터를 수정할 수 있도록 보기에서 다른 페이지로 이동 하기 전에 라는 메시지가 나타날 수 있습니다.

이 장에서 제공 하 여 이러한 문제를 해결할 수는 `NavigationService` 보기 모델 첫 번째 페이지 탐색 하는 데 사용 되는 클래스입니다.

> [!NOTE]
> `NavigationService` 에서 사용 하는 앱만 ContentPage 인스턴스 간의 계층 탐색을 수행 하는 합니다. 다른 페이지 형식 사이 탐색 하는 서비스를 사용 하 여 예기치 않은 동작이 발생할 수 있습니다.

## <a name="navigating-between-pages"></a>페이지 사이 탐색

탐색 논리 보기의 코드 숨김에 또는 데이터에서 뷰 모델을 바인딩할 수 있습니다. 하지만 보기에서 탐색 논리 배치는 가장 간단한 방법은, 단위 테스트를 통해 쉽게 테스트할 수는 없습니다. 논리 단위 테스트를 통해 연습이 수 있는지 모델 클래스를 의미 보기에서 탐색 논리를 배치 합니다. 또한 뷰 모델 탐색 특정 비즈니스 규칙 적용 되도록을 제어 하는 논리를 구현 다음 수 있습니다. 예를 들어 응용 프로그램 못할 수 없는 첫 번째 페이지를 벗어나려 사용자 입력 한 데이터가 올바른지 확인 합니다.

A `NavigationService` 클래스는 일반적으로 테스트 용이성 승격 하려면 모델 보기에서에서 호출 됩니다. 그러나 모델 보기에서에서 뷰로 이동 참조 뷰와 활성 뷰 모델 권장 되지 않습니다와 연관 되지 않은 뷰에 특히 모델 보기를 필요 합니다. 따라서는 `NavigationService` 제시 여기 보기 모델 형식을 이동할 대상으로 지정 합니다.

EShopOnContainers 모바일 앱 사용 하 여는 `NavigationService` 클래스 보기 중심 모델 탐색을 제공 합니다. 이 클래스가 구현 하는 `INavigationService` 다음 코드 예제에 표시 된 인터페이스:

```csharp
public interface INavigationService  
{  
    ViewModelBase PreviousPageViewModel { get; }  
    Task InitializeAsync();  
    Task NavigateToAsync<TViewModel>() where TViewModel : ViewModelBase;  
    Task NavigateToAsync<TViewModel>(object parameter) where TViewModel : ViewModelBase;  
    Task RemoveLastFromBackStackAsync();  
    Task RemoveBackStackAsync();  
}
```

이 인터페이스를 구현 하는 클래스는 다음 메서드를 제공 해야 지정 합니다.

|메서드|용도|
|--- |--- |
|`InitializeAsync`|앱을 실행 하는 경우 두 페이지 중 하나로 탐색을 수행 합니다.|
|`NavigateToAsync`|지정된 된 페이지에 있는 계층 탐색을 수행합니다.|
|`NavigateToAsync(parameter)`|매개 변수 전달, 특정 페이지로 계층 탐색을 수행 합니다.|
|`RemoveLastFromBackStackAsync`|탐색 스택에서 이전 페이지를 제거합니다.|
|`RemoveBackStackAsync`|탐색 스택에서 이전의 모든 페이지를 제거합니다.|

또한는 `INavigationService` 인터페이스를 구현 하는 클래스를 제공 하는 지정 된 `PreviousPageViewModel` 속성입니다. 이 탐색 스택의 이전 페이지와 관련 된 뷰 모델 유형을 반환 합니다.

> [!NOTE]
> `INavigationService` 인터페이스는 일반적으로 지정할 수도 `GoBackAsync` 메서드를 프로그래밍 방식으로 탐색 스택의 이전 페이지로 반환 하는 데 사용 됩니다. 그러나이 메서드는 필요 하지는 때문에 eShopOnContainers 모바일 앱에 없습니다.

### <a name="creating-the-navigationservice-instance"></a>NavigationService 인스턴스 만들기

`NavigationService` 클래스를 구현 하는 `INavigationService` 인터페이스, 다음 코드 예제에서와 같이 Autofac 종속성 주입 컨테이너와 단일 항목으로 등록 되어 있습니다.

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

`INavigationService` 인터페이스에서 해결 되었습니다는 `ViewModelBase` 클래스 생성자에 다음 코드 예제에서와 같이:

```csharp
NavigationService = ViewModelLocator.Resolve<INavigationService>();
```

이렇게 반환에 대 한 참조는 `NavigationService` 으로 만들어진 Autofac 종속성 주입 컨테이너에 저장 된 개체는 `InitNavigation` 에서 메서드는 `App` 클래스. 자세한 내용은 참조 [탐색 때의 앱을 실행](#navigating_when_the_app_is_launched)합니다.

`ViewModelBase` 저장소 클래스는 `NavigationService` 인스턴스는 `NavigationService` 형식의 속성이 `INavigationService`합니다. 따라서 모델 클래스에서 파생 되는 모든 볼는 `ViewModelBase` 클래스를 사용할 수는 `NavigationService` 속성으로 지정 된 메서드에 액세스 하는 `INavigationService` 인터페이스입니다. 삽입의 오버 헤드를 피할 수이 고 `NavigationService` 각 보기 모델 클래스로 Autofac 종속성 주입 컨테이너에서 개체입니다.

### <a name="handling-navigation-requests"></a>탐색 요청 처리

Xamarin.Forms는 제공 된 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 사용자가을 앞으로 및 뒤로 필요에 따라 페이지를 탐색할 수 있는 계층적 탐색 환경을 구현 하는 클래스입니다. 계층적 탐색에 대한 자세한 내용은 [계층적 탐색](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)을 참조하세요.

사용 하지 않고는 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) eShopOnContainers 앱 래핑하고 클래스를 직접는 `NavigationPage` 클래스에 `CustomNavigationView` 다음 코드 예제와 같이 클래스:

```csharp
public partial class CustomNavigationView : NavigationPage  
{  
    public CustomNavigationView() : base()  
    {  
        InitializeComponent();  
    }  

    public CustomNavigationView(Page root) : base(root)  
    {  
        InitializeComponent();  
    }  
}
```

이 배치의 목적은 스타일 지정 하기 쉽도록는 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 클래스에 대 한 XAML 파일 안에 인스턴스.

탐색 중 하나를 호출 하 여 뷰 모델 클래스 내에서 수행 되는 `NavigateToAsync` to, 다음 코드 예제에서와 같이 탐색 대상인 페이지에 대 한 보기 모델 유형을 지정 하는 메서드:

```csharp
await NavigationService.NavigateToAsync<MainViewModel>();
```

다음 코드 예제는 `NavigateToAsync` 에서 제공 하는 메서드는 `NavigationService` 클래스:

```csharp
public Task NavigateToAsync<TViewModel>() where TViewModel : ViewModelBase  
{  
    return InternalNavigateToAsync(typeof(TViewModel), null);  
}  

public Task NavigateToAsync<TViewModel>(object parameter) where TViewModel : ViewModelBase  
{  
    return InternalNavigateToAsync(typeof(TViewModel), parameter);  
}
```

파생 된 모든 보기 모델 클래스를 사용 하는 각 메서드에 `ViewModelBase` 클래스를 호출 하 여 계층 탐색을 수행 하는 `InternalNavigateToAsync` 메서드. 또한 두 번째 `NavigateToAsync` 메서드를 사용 하면 탐색 데이터를 탐색 중인, 일반적으로 사용 된 초기화를 수행 하도록 뷰 모델에 전달 되는 인수로 지정 해야 합니다. 자세한 내용은 참조 [탐색 하는 동안 매개 변수 전달](#passing_parameters_during_navigation)합니다.

`InternalNavigateToAsync` 메서드 탐색 요청을 실행 하 고 다음 코드 예제에 표시 됩니다.

```csharp
private async Task InternalNavigateToAsync(Type viewModelType, object parameter)  
{  
    Page page = CreatePage(viewModelType, parameter);  

    if (page is LoginView)  
    {  
        Application.Current.MainPage = new CustomNavigationView(page);  
    }  
    else  
    {  
        var navigationPage = Application.Current.MainPage as CustomNavigationView;  
        if (navigationPage != null)  
        {  
            await navigationPage.PushAsync(page);  
        }  
        else  
        {  
            Application.Current.MainPage = new CustomNavigationView(page);  
        }  
    }  

    await (page.BindingContext as ViewModelBase).InitializeAsync(parameter);  
}  

private Type GetPageTypeForViewModel(Type viewModelType)  
{  
    var viewName = viewModelType.FullName.Replace("Model", string.Empty);  
    var viewModelAssemblyName = viewModelType.GetTypeInfo().Assembly.FullName;  
    var viewAssemblyName = string.Format(  
                CultureInfo.InvariantCulture, "{0}, {1}", viewName, viewModelAssemblyName);  
    var viewType = Type.GetType(viewAssemblyName);  
    return viewType;  
}  

private Page CreatePage(Type viewModelType, object parameter)  
{  
    Type pageType = GetPageTypeForViewModel(viewModelType);  
    if (pageType == null)  
    {  
        throw new Exception($"Cannot locate page type for {viewModelType}");  
    }  

    Page page = Activator.CreateInstance(pageType) as Page;  
    return page;  
}
```

`InternalNavigateToAsync` 첫 번째 호출 하 여 뷰 모델을 탐색을 수행 하는 메서드는 `CreatePage` 메서드. 이 메서드는 지정 된 뷰 모델 형식에 해당 하 고 만들어이 뷰 형식의 인스턴스를 반환 하는 뷰를 찾습니다. 보기 모델 유형에 해당 하는 보기를 찾는 것으로 가정 하는 규칙 기반 방식을 사용 합니다.

-   뷰는 모델 유형에 보기와 같은 어셈블리에입니다.
-   뷰는 한 합니다. 뷰 자식 네임 스페이스입니다.
-   모델 보기에 있는 한 합니다. Viewmodel 자식 네임 스페이스입니다.
-   뷰 이름은 해당 보기 모델 이름, "모델"을 제거 합니다.

뷰를 인스턴스화할 때 해당 보기 모델과 연결 되어 있습니다. 이 발생 하는 방법에 대 한 자세한 내용은 참조 [보기 모델 로케이터에 뷰 모델을 자동으로 만드는](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator)합니다.

만들려는 보기는는 `LoginView`의 새 인스턴스 내에 래핑됩니다는 `CustomNavigationView` 클래스에 할당 하 고는 [ `Application.Current.MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) 속성입니다. 그렇지 않은 경우는 `CustomNavigationView` 인스턴스가 검색 되 고 null 없음을 제공 되는 [ `PushAsync` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 탐색 스택으로 만들 보기를 적용할 메서드가 호출 됩니다. 그러나 경우 검색 `CustomNavigationView` 인스턴스가 `null`의 새 인스턴스 내에서 만들 뷰는 래핑는 `CustomNavigationView` 클래스 및에 할당 된는 `Application.Current.MainPage` 속성입니다. 이 메커니즘 탐색 하는 동안 되도록, 페이지가 추가 됩니다 올바르게 탐색 스택의 비어 있을 때 및 데이터를 포함 합니다.

> [!TIP]
> 페이지를 캐시 하는 것이 좋습니다. 현재 표시 되지 않은 뷰에 대 한 메모리 사용량의 결과 캐시 하는 페이지입니다. 그러나 페이지 캐싱이 설정 되지 않은 것은 XAML 구문 분석 하 고 페이지의 보기 모델의 구조는 새 페이지를 탐색, 복잡 한 페이지에 대 한 성능 영향을 줄 수 있는 될 때마다 발생 합니다. 잘 디자인 된 페이지의 컨트롤을 너무 많이 사용 하지 않는 경우 성능을 충분 해야 합니다. 그러나 페이지 캐싱을 도움이 느린 페이지 로드 시간이 발생 하는 경우.

뷰를 만들고, 이동한 후의 `InitializeAsync` 보기의 관련된 보기 모델의 메서드를 실행 합니다. 자세한 내용은 참조 [탐색 하는 동안 매개 변수 전달](#passing_parameters_during_navigation)합니다.

<a name="navigating_when_the_app_is_launched" />

### <a name="navigating-when-the-app-is-launched"></a>시작 될 때의 앱 탐색

앱을 실행 하는 경우는 `InitNavigation` 에서 메서드는 `App` 클래스가 호출 됩니다. 다음 코드 예제에서는이 메서드를 보여 줍니다.

```csharp
private Task InitNavigation()  
{  
    var navigationService = ViewModelLocator.Resolve<INavigationService>();  
    return navigationService.InitializeAsync();  
}
```

메서드를 새로 만듭니다. `NavigationService` Autofac 종속성 주입 컨테이너에 있는 개체를 호출 하기 전에,에 대 한 참조를 반환 하 고 해당 `InitializeAsync` 메서드.

> [!NOTE]
> 때는 `INavigationService` 인터페이스 확인는 `ViewModelBase` 클래스, 컨테이너에 대 한 참조를 반환 합니다는 `NavigationService` InitNavigation 메서드를 호출할 때 생성 된 개체입니다.

다음 코드 예제는 `NavigationService` `InitializeAsync` 메서드:

```csharp
public Task InitializeAsync()  
{  
    if (string.IsNullOrEmpty(Settings.AuthAccessToken))  
        return NavigateToAsync<LoginViewModel>();  
    else  
        return NavigateToAsync<MainViewModel>();  
}
```

`MainView` 를 탐색 하는 응용 프로그램에는 캐시 된 액세스 토큰을 인증에 사용 되는 경우. 그렇지 않은 경우는 `LoginView` 을 탐색 합니다.

Autofac 종속성 주입 컨테이너에 대 한 자세한 내용은 참조 [종속성 주입 소개](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection)합니다.

<a name="passing_parameters_during_navigation" />

### <a name="passing-parameters-during-navigation"></a>탐색 하는 동안 매개 변수 전달

중 하나는 `NavigateToAsync` 지정한 메서드는 `INavigationService` 인터페이스를 탐색 데이터를 탐색 중인, 일반적으로 사용 된 초기화를 수행 하도록 뷰 모델에 전달 되는 인수로 지정 해야 합니다.

예를 들어는 `ProfileViewModel` 클래스를 포함 한 `OrderDetailCommand` 에 순서를 선택할 때 실행 되는 `ProfileView` 페이지. 실행 차례로 `OrderDetailAsync` 다음 코드 예제에 표시 된 메서드:

```csharp
private async Task OrderDetailAsync(Order order)  
{  
    await NavigationService.NavigateToAsync<OrderDetailViewModel>(order);  
}
```

이 메서드를 호출 하는 탐색은 `OrderDetailViewModel`전달 하는 `Order` 사용자에서 선택 된 순서를 나타내는 인스턴스는 `ProfileView` 페이지. 때는 `NavigationService` 클래스 만듭니다는 `OrderDetailView`, `OrderDetailViewModel` 클래스는 인스턴스화되고 보기의에 할당 된 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)합니다. 로 이동한 후의 `OrderDetailView`, `InternalNavigateToAsync` 메서드가 실행 되는 `InitializeAsync` 메서드 보기의 보기 모델의 연결 된입니다.

`InitializeAsync` 에 정의 된 메서드는 `ViewModelBase` 재정의할 수 있는 방법으로 클래스입니다. 이 메서드는 지정 된 `object` 탐색 작업 동안 뷰 모델에 전달할 데이터를 표시 하는 인수입니다. 데이터 탐색 작업에서 수신 하려는 뷰 모델 클래스의 자체 구현을 제공 하는 따라서는 `InitializeAsync` 메서드를 필요한 초기화를 수행 하도록 합니다. 다음 코드 예제는 `InitializeAsync` 에서 메서드는 `OrderDetailViewModel` 클래스:

```csharp
public override async Task InitializeAsync(object navigationData)  
{  
    if (navigationData is Order)  
    {  
        ...  
        Order = await _ordersService.GetOrderAsync(  
                        Convert.ToInt32(order.OrderNumber), authToken);  
        ...  
    }  
}
```

이 메서드는 검색에서 `Order` 에서 세부 정보를 사용 하 여 전체 순서를 검색 및 탐색 작업 중의 보기 모델에 전달 된 인스턴스는 `OrderService` 인스턴스.

<a name="invoking_navigation_using_behaviors" />

### <a name="invoking-navigation-using-behaviors"></a>동작을 사용 하 여 호출 탐색

탐색은 일반적으로 사용자 인터페이스에 의해 보기에서 트리거됩니다. 예를 들어는 `LoginView` 인증을 거친 다음 탐색을 수행 합니다. 다음 코드 예제는 동작에 의해 탐색은 호출 하는 방법을 보여 줍니다.

```xaml
<WebView ...>  
    <WebView.Behaviors>  
        <behaviors:EventToCommandBehavior  
            EventName="Navigating"  
            EventArgsConverter="{StaticResource WebNavigatingEventArgsConverter}"  
            Command="{Binding NavigateCommand}" />  
    </WebView.Behaviors>  
</WebView>
```

런타임에 `EventToCommandBehavior` 와 상호 작용에 응답 하는 [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/)합니다. 경우는 `WebView` 웹 페이지로 이동는 [ `Navigating` ](https://developer.xamarin.com/api/event/Xamarin.Forms.WebView.Navigating/) 이벤트는 발생 된 실행 될는 `NavigateCommand` 에 `LoginViewModel`합니다. 기본적으로 이벤트에 대 한 이벤트 인수는 명령에 전달 됩니다. 에 지정 된 변환기에서 원본과 대상 간에 전달 될 때이 데이터를 변환 하는 `EventArgsConverter` 속성을 반환 하는 [ `Url` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebNavigationEventArgs.Url/) 에서 [ `WebNavigatingEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebNavigatingEventArgs/)합니다. 따라서,는 `NavigationCommand` 은 실행 웹 페이지의 Url 매개 변수로 전달 되는 등록 된를 `Action`합니다.

차례로 `NavigationCommand` 실행의 `NavigateAsync` 다음 코드 예제에 표시 된 메서드:

```csharp
private async Task NavigateAsync(string url)  
{  
    ...          
    await NavigationService.NavigateToAsync<MainViewModel>();  
    await NavigationService.RemoveLastFromBackStackAsync();  
    ...  
}
```

이 메서드를 호출 하는 탐색은 `MainViewModel`, 탐색, 다음을 제거 하 고는 `LoginView` 탐색 스택에서 페이지.

### <a name="confirming-or-cancelling-navigation"></a>확인 또는 취소 하는 탐색

응용 프로그램을 확인 하거나 탐색을 취소할 수 있도록 탐색 작업을 사용 하는 동안 사용자 상호 작용 해야 합니다. 예를 들어 사용자가 데이터 입력 페이지를 마치면 완벽 하 게 하기 전에 이동 하려고 할 때이 방법을 필요할 수 있습니다. 이 경우 응용 프로그램 페이지를 나 가려고 합니다 또는 발생 하기 전에 탐색 작업을 취소 하려면 사용자 수 있는 알림이 제공 해야 합니다. 탐색은 호출 여부를 제어 하려면 알림 로부터 응답을 사용 하 여 뷰 모델 클래스에서 수행할 수 있습니다.

## <a name="summary"></a>요약

Xamarin.Forms에는 일반적으로 UI와 사용자의 상호 작용 또는 내부 논리 기반 상태 변경으로 인해 앱 자체에서 간격은 페이지 탐색에 대 한 지원이 포함 되어 있습니다. 그러나 탐색 MVVM 패턴을 사용 하는 앱에서 구현 복잡할 수 있습니다.

제공 된이 장에서 `NavigationService` 모델 보기에서에서 보기 중심 모델 탐색을 수행 하는 데 사용 되는 클래스입니다. 자동화 된 테스트를 통해 논리를 수행 될 수 모델 클래스를 의미 보기에서 탐색 논리를 배치 합니다. 또한 뷰 모델 탐색 특정 비즈니스 규칙 적용 되도록을 제어 하는 논리를 구현 다음 수 있습니다.


## <a name="related-links"></a>관련 링크

- [EBook (2mb PDF)를 다운로드 합니다.](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
