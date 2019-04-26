---
title: 엔터프라이즈 앱 탐색
description: 이 장에서 eShopOnContainers 모바일 앱 모델 보기에서에서 보기 모델 첫 탐색을 수행 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 4cad57b5-7fe4-4527-a988-d9b60c9620b4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: d306b0c1c0d08129671e27b96911ec771acb658e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61298947"
---
# <a name="enterprise-app-navigation"></a>엔터프라이즈 앱 탐색

Xamarin.Forms에는 일반적으로 UI 사용 하 여 사용자의 상호 작용 또는 내부 논리 기반 상태 변경으로 인해 앱 자체에서 발생 하는 페이지 탐색에 대 한 지원이 포함 됩니다. 그러나 탐색 다음과 같은 과제를 충족 해야 합니다 (MVVM)-Model-view-viewmodel 패턴을 사용 하는 앱에서 구현 하는 복잡 한 수 있습니다.

-   뷰 간의 종속성과 밀접 한 결합을 발생 하지 않도록 하는 방법을 사용 하 여 뷰를 탐색할 수를 식별 하는 방법입니다.
-   뷰를 탐색할 수 인스턴스화되고 초기화 프로세스를 조정 하는 방법. MVVM을 사용 하 여 뷰와 뷰 모델 인스턴스화되고 보기의 바인딩 컨텍스트를 통해 서로 연결에 필요 합니다. 앱 종속성 주입 컨테이너를 사용 하는 뷰와 뷰 모델의 인스턴스화를 특정 생성 메커니즘이 필요할 수도 있습니다.
-   보기 중심 탐색을 수행 하거나 모델 첫 탐색 보려면 여부입니다. 보기 중심 탐색을 사용 하 여 이동할 페이지 뷰 유형의 이름을 가리킵니다. 탐색 하는 동안 지정 된 보기가 인스턴스화될에 해당 하는 보기 모델 및 다른 종속 서비스와 함께 합니다. 대 안으로 이동할 페이지 보기 모델 형식의 이름으로 참조 하는 위치에 보기 모델 첫 탐색을 사용 하는 것입니다.
-   하는 방법을 명확 하 게 보기 및 보기 모델에서 앱의 탐색 동작을 구분 합니다. MVVM 패턴은 앱의 UI 프레젠테이션 및 비즈니스 논리 간의 분리를 제공합니다. 그러나 앱의 탐색 동작은 앱의 UI 및 프레젠테이션 부분에 걸쳐 경우가 많습니다. 사용자는 종종는 뷰에서 탐색 시작한 뷰 탐색의 결과로 바뀝니다. 그러나 탐색 해야 할 수 종종 세션을 시작 하거나 보기 모델 내에서 조정 합니다.
-   초기화를 위해 탐색 하는 동안 매개 변수를 전달 하는 방법입니다. 예를 들어 사용자가 주문 세부 정보를 업데이트 하는 뷰를 탐색 하는 경우 주문 데이터를 올바른 데이터를 표시할 수 있도록 보기로 전달 해야 합니다.
-   특정 비즈니스 규칙 맞는 되도록 좌표 탐색 방법입니다. 예를 들어, 사용자 제출 하거나 보기 내에서 수행 된 모든 데이터 변경 내용을 취소 하 라는 메시지가 표시 될 하거나 잘못 된 데이터를 수정할 수 있도록 보기에서 탐색 하기 전에 라는 메시지가 나타날 수 있습니다.

이 장에서 제공 하 여 이러한 문제를 해결할는 `NavigationService` 보기 모델 첫 번째 페이지 탐색 하는 데 사용 되는 클래스입니다.

> [!NOTE]
> `NavigationService` 사용한만 ContentPage 인스턴스 간의 계층 탐색을 수행 하는 앱은 설계 되었습니다. 다른 페이지 형식 사이 탐색 하는 서비스를 사용 하 여 예기치 않은 동작이 발생할 수 있습니다.

## <a name="navigating-between-pages"></a>페이지 사이 탐색

탐색 논리 뷰의 코드 숨김에 상주 하거나 데이터에서 뷰 모델을 바인딩할 수 있습니다. 뷰에서 탐색 논리를 배치 가장 간단한 방법일 수 있습니다, 단위 테스트를 통해 쉽게 테스트할 수는 없습니다. 모델 클래스 단위 테스트를 통해 논리를 실행할 수 있습니다 의미 보기에서 탐색 논리를 배치 합니다. 또한 뷰 모델은 특정 비즈니스 규칙 적용 되도록 컨트롤 탐색 논리를 구현한 다음 수 있습니다. 예를 들어, 앱 수 없습니다 없이 첫 번째 페이지를 나 가려고 할 입력 한 데이터가 유효한 지 확인 합니다.

`NavigationService` 클래스 테스트 용이성을 승격 하려면 보기 모델에서 일반적으로 호출 됩니다. 그러나 모델 보기에서에서 뷰로 이동 하 여 참조 뷰 및 활성 뷰 모델을 사용 하 여, 권장 되지 않습니다 연관 되지 않은 뷰 특히 모델 보기를 필요 합니다. 따라서는 `NavigationService` 표시 여기 뷰 모델 형식은 이동할 대상으로 지정 합니다.

EShopOnContainers 모바일 앱 사용을 `NavigationService` 클래스 보기 모델 첫 탐색을 제공 합니다. 이 클래스에서 구현 된 `INavigationService` 다음 코드 예제에 나와 있는 인터페이스:

```csharp
public interface INavigationService  
{  
    ViewModelBase PreviousPageViewModel { get; }  
    Task InitializeAsync();  
    Task NavigateToAsync<TViewModel>() where TViewModel : ViewModelBase;  
    Task NavigateToAsync<TViewModel>(object parameter) where TViewModel : ViewModelBase;  
    Task RemoveLastFromBackStackAsync();  
    Task RemoveBackStackAsync();  
}
```

이 인터페이스는 다음 메서드를 제공 하는 구현 클래스를 지정 합니다.

|메서드|용도|
|--- |--- |
|`InitializeAsync`|앱을 실행 하는 경우 두 페이지 중 하나에 대 한 탐색을 수행 합니다.|
|`NavigateToAsync`|지정된 된 페이지에 대 한 계층적 탐색을 수행합니다.|
|`NavigateToAsync(parameter)`|매개 변수 전달, 지정된 된 페이지에 대 한 계층적 탐색을 수행 합니다.|
|`RemoveLastFromBackStackAsync`|탐색 스택에서 이전 페이지를 제거합니다.|
|`RemoveBackStackAsync`|탐색 스택에서 모든 이전 페이지를 제거합니다.|

또한 합니다 `INavigationService` 인터페이스를 구현 하는 클래스를 제공 하는 지정 된 `PreviousPageViewModel` 속성. 이 탐색 스택의 이전 페이지와 관련 된 뷰 모델 유형을 반환 합니다.

> [!NOTE]
> `INavigationService` 인터페이스는 일반적으로 지정할 수도 `GoBackAsync` 프로그래밍 방식으로 탐색 스택의 이전 페이지로 반환 하는 데 사용 되는 메서드. 그러나이 방법은 eShopOnContainers 모바일 앱에서 누락 된 때문에 필요는 없습니다.

### <a name="creating-the-navigationservice-instance"></a>NavigationService 인스턴스 만들기

합니다 `NavigationService` 클래스를 구현 하는 `INavigationService` 인터페이스, 다음 코드 예제에서 설명한 것 처럼 Autofac 종속성 주입 컨테이너를 사용 하 여 단일 항목으로 등록 하는:

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

합니다 `INavigationService` 인터페이스에서 해결 되었습니다는 `ViewModelBase` 클래스 생성자에 다음 코드 예제에서 설명한 것 처럼:

```csharp
NavigationService = ViewModelLocator.Resolve<INavigationService>();
```

에 대 한 참조를 반환 하는이 `NavigationService` 하 여 만든 Autofac 종속성 주입 컨테이너에 저장 된 개체를 `InitNavigation` 에서 메서드를 `App` 클래스. 자세한 내용은 [탐색할 때 앱이 시작 될](#navigating_when_the_app_is_launched)합니다.

`ViewModelBase` 저장소 클래스를 `NavigationService` 의 인스턴스를 `NavigationService` 형식의 속성 `INavigationService`합니다. 따라서 모든 보기 모델 클래스에서 파생 되는 합니다 `ViewModelBase` 클래스를 사용할 수는 `NavigationService` 속성으로 지정 된 메서드에 액세스할 수는 `INavigationService` 인터페이스입니다. 삽입의 오버 헤드를 방지 하는이 `NavigationService` 보기 모델 클래스 각 Autofac 종속성 주입 컨테이너에서 개체입니다.

### <a name="handling-navigation-requests"></a>탐색 요청 처리

Xamarin.Forms는 제공 된 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 사용자가 앞으로 및 뒤로 필요에 따라 페이지를 통해 이동할 수 있는 계층적 탐색 환경을 구현 하는 클래스입니다. 계층적 탐색에 대한 자세한 내용은 [계층적 탐색](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)을 참조하세요.

사용 하지 않고는 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 클래스를 직접 eShopOnContainers 앱 래핑하는 `NavigationPage` 클래스는 `CustomNavigationView` 클래스에 다음 코드 예제에 표시 된 대로:

```csharp
public partial class CustomNavigationView : NavigationPage  
{  
    public CustomNavigationView() : base()  
    {  
        InitializeComponent();  
    }  

    public CustomNavigationView(Page root) : base(root)  
    {  
        InitializeComponent();  
    }  
}
```

스타일의 편의 위해이 래핑 목적은 합니다 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 클래스에 대 한 XAML 파일 내에서 인스턴스.

탐색 중 하나를 호출 하 여 뷰 모델 클래스 내에서 수행 되는 `NavigateToAsync` 에 다음 코드 예제에서 설명한 것 처럼 탐색 중인 페이지에 대 한 뷰 모델 유형을 지정 하는 메서드:

```csharp
await NavigationService.NavigateToAsync<MainViewModel>();
```

다음 코드 예제는 `NavigateToAsync` 에서 제공 하는 메서드는 `NavigationService` 클래스:

```csharp
public Task NavigateToAsync<TViewModel>() where TViewModel : ViewModelBase  
{  
    return InternalNavigateToAsync(typeof(TViewModel), null);  
}  

public Task NavigateToAsync<TViewModel>(object parameter) where TViewModel : ViewModelBase  
{  
    return InternalNavigateToAsync(typeof(TViewModel), parameter);  
}
```

파생 되는 모든 보기 모델 클래스를 사용 하는 각 메서드는 `ViewModelBase` 를 호출 하 여 계층적 탐색을 수행 하는 클래스는 `InternalNavigateToAsync` 메서드. 또한 두 번째 `NavigateToAsync` 메서드를 사용 하면 탐색 데이터를 탐색 중인, 일반적으로 사용 된 초기화를 수행 하는 보기 모델에 전달 되는 인수로 지정 해야 합니다. 자세한 내용은 [매개 변수 중 탐색 전달](#passing_parameters_during_navigation)합니다.

`InternalNavigateToAsync` 메서드 탐색 요청을 실행 하 고 다음 코드 예제에 표시 됩니다.

```csharp
private async Task InternalNavigateToAsync(Type viewModelType, object parameter)  
{  
    Page page = CreatePage(viewModelType, parameter);  

    if (page is LoginView)  
    {  
        Application.Current.MainPage = new CustomNavigationView(page);  
    }  
    else  
    {  
        var navigationPage = Application.Current.MainPage as CustomNavigationView;  
        if (navigationPage != null)  
        {  
            await navigationPage.PushAsync(page);  
        }  
        else  
        {  
            Application.Current.MainPage = new CustomNavigationView(page);  
        }  
    }  

    await (page.BindingContext as ViewModelBase).InitializeAsync(parameter);  
}  

private Type GetPageTypeForViewModel(Type viewModelType)  
{  
    var viewName = viewModelType.FullName.Replace("Model", string.Empty);  
    var viewModelAssemblyName = viewModelType.GetTypeInfo().Assembly.FullName;  
    var viewAssemblyName = string.Format(  
                CultureInfo.InvariantCulture, "{0}, {1}", viewName, viewModelAssemblyName);  
    var viewType = Type.GetType(viewAssemblyName);  
    return viewType;  
}  

private Page CreatePage(Type viewModelType, object parameter)  
{  
    Type pageType = GetPageTypeForViewModel(viewModelType);  
    if (pageType == null)  
    {  
        throw new Exception($"Cannot locate page type for {viewModelType}");  
    }  

    Page page = Activator.CreateInstance(pageType) as Page;  
    return page;  
}
```

합니다 `InternalNavigateToAsync` 첫 번째 호출 하 여 보기 모델에 대 한 탐색을 수행 하는 메서드를 `CreatePage` 메서드. 이 메서드는 지정 된 뷰 모델 형식에 해당 하 고 만들고이 뷰 형식의 인스턴스를 반환 하는 뷰를 찾습니다. 보기 모델 형식에 해당 하는 뷰를 찾기는 가정 하는 규칙 기반 접근 방식을 사용 합니다.

-   뷰는 뷰 모델 형식으로 동일한 어셈블리에 있습니다.
-   보기에는 합니다. 뷰 자식 네임 스페이스입니다.
-   모델 보기에는 합니다. Viewmodel 자식 네임 스페이스입니다.
-   뷰 이름은 "Model" 제거를 사용 하 여 모델 이름을 보려면 해당 합니다.

뷰를 인스턴스화할 때 정책이 해당 하는 보기 모델에 연결 합니다. 이 발생 하는 방법에 대 한 자세한 내용은 참조 하세요. [자동으로 보기 모델 로케이터를 사용 하 여 뷰 모델을 만들기](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator)합니다.

뷰 생성 되는 경우는 `LoginView`의 새 인스턴스 내에서 래핑된를 `CustomNavigationView` 클래스 및 할당 합니다 [ `Application.Current.MainPage` ](xref:Xamarin.Forms.Application.MainPage) 속성. 이 고, 그렇지를 `CustomNavigationView` 인스턴스는 검색 하 고는 null이 아닌, 제공 합니다 [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage) 탐색 스택으로 만들어지는 보기를 적용할 메서드가 실행 됩니다. 그러나 경우 검색 `CustomNavigationView` 인스턴스가 `null`, 만들어지는 보기의 새 인스턴스 내에서 래핑된 합니다 `CustomNavigationView` 클래스 및 할당할는 `Application.Current.MainPage` 속성입니다. 이 메커니즘을 이용 하면 탐색 도중 되도록, 페이지에 추가 됩니다 올바르게 탐색 스택에서 비어 있는 경우와 데이터를 포함 하는 경우.

> [!TIP]
> 페이지를 캐시 하는 것이 좋습니다. 현재 표시 되지 않는 뷰에 대 한 메모리 사용 결과 캐시 하는 페이지입니다. 그러나 페이지 캐싱을 사용 하지 않고 XAML 구문 분석 하 고 페이지와 해당 하는 보기 모델의 생성 될 때마다 새 페이지를 탐색, 복잡 한 페이지에 대 한 성능 영향을 줄 수 있는 발생할 수 있는 것이 의미가 있습니다. 잘 디자인 된 페이지의 컨트롤을 너무 많이 사용 하지 않는 경우 성능 충분 해야 합니다. 그러나 페이지 캐싱 도움이 느린 페이지 로드 시간이 발생 하는 경우.

뷰를 만들고 탐색 한 후의 `InitializeAsync` 보기의 관련된 보기 모델의 메서드를 실행 합니다. 자세한 내용은 [매개 변수 중 탐색 전달](#passing_parameters_during_navigation)합니다.

<a name="navigating_when_the_app_is_launched" />

### <a name="navigating-when-the-app-is-launched"></a>시작 될 때 앱 탐색

앱을 실행 하는 경우는 `InitNavigation` 의 메서드는 `App` 클래스에서 호출 됩니다. 다음 코드 예제에서는 이 메서드를 보여줍니다.

```csharp
private Task InitNavigation()  
{  
    var navigationService = ViewModelLocator.Resolve<INavigationService>();  
    return navigationService.InitializeAsync();  
}
```

메서드를 만듭니다 `NavigationService` Autofac 종속성 주입 컨테이너에서 개체를 호출 하기 전에 대 한 참조를 반환 하 고 해당 `InitializeAsync` 메서드.

> [!NOTE]
> 때를 `INavigationService` 하 여 인터페이스를 해결 합니다 `ViewModelBase` 클래스 컨테이너 참조를 반환 합니다.를 `NavigationService` InitNavigation 메서드가 호출 될 때 생성 된 개체.

다음 코드 예제는 `NavigationService` `InitializeAsync` 메서드:

```csharp
public Task InitializeAsync()  
{  
    if (string.IsNullOrEmpty(Settings.AuthAccessToken))  
        return NavigateToAsync<LoginViewModel>();  
    else  
        return NavigateToAsync<MainViewModel>();  
}
```

`MainView` 앱에 인증에 사용 되는 캐시 된 액세스 토큰을 사용 하는 경우 탐색 합니다. 이 고, 그렇지는 `LoginView` 가 탐색 됩니다.

Autofac 종속성 주입 컨테이너에 대 한 자세한 내용은 참조 하세요. [종속성 주입 소개](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection)합니다.

<a name="passing_parameters_during_navigation" />

### <a name="passing-parameters-during-navigation"></a>탐색 하는 동안 매개 변수 전달

중 하나를 `NavigateToAsync` 지정한 메서드는 `INavigationService` 인터페이스를 탐색 데이터를 탐색 중인, 일반적으로 사용 된 초기화를 수행 하는 보기 모델에 전달 되는 인수로 서 지정 합니다.

예를 들어 합니다 `ProfileViewModel` 클래스를 포함를 `OrderDetailCommand` 에서 순서를 선택할 때 실행 되는 `ProfileView` 페이지. 실행이 차례로 `OrderDetailAsync` 메서드를 다음 코드 예제에 나와 있는:

```csharp
private async Task OrderDetailAsync(Order order)  
{  
    await NavigationService.NavigateToAsync<OrderDetailViewModel>(order);  
}
```

이 메서드 호출에 대 한 탐색 합니다 `OrderDetailViewModel`전달를 `Order` 순서에서 선택한 사용자를 나타내는 인스턴스는 `ProfileView` 페이지입니다. 경우는 `NavigationService` 클래스를 만듭니다를 `OrderDetailView`, `OrderDetailViewModel` 클래스를 인스턴스화하고 뷰의 할당할 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)합니다. 로 이동한 후는 `OrderDetailView`, `InternalNavigateToAsync` 메서드가 실행 되는 `InitializeAsync` 뷰의 메서드 보기 모델의 연결 된.

합니다 `InitializeAsync` 에 정의 된 메서드는 `ViewModelBase` 재정의할 수 있는 메서드로 클래스입니다. 이 메서드는 `object` 탐색 작업을 하는 동안 보기 모델에 전달할 데이터를 나타내는 인수입니다. 탐색 작업에서 데이터를 수신 하는 보기 모델 클래스의 자체 구현을 제공 하는 따라서는 `InitializeAsync` 필수 초기화를 수행 하는 방법입니다. 다음 코드 예제는 `InitializeAsync` 메서드에서 `OrderDetailViewModel` 클래스:

```csharp
public override async Task InitializeAsync(object navigationData)  
{  
    if (navigationData is Order)  
    {  
        ...  
        Order = await _ordersService.GetOrderAsync(  
                        Convert.ToInt32(order.OrderNumber), authToken);  
        ...  
    }  
}
```

이 메서드를 검색 합니다 `Order` 에서 탐색 작업을 하는 동안 보기 모델에 전달 된 전체 순서를 검색 하는 데 사용 하는 인스턴스 세부 정보는 `OrderService` 인스턴스.

<a name="invoking_navigation_using_behaviors" />

### <a name="invoking-navigation-using-behaviors"></a>동작을 사용 하 여 호출 탐색

탐색은 일반적으로 사용자 상호 작용 하 여 보기에서 트리거됩니다. 예를 들어를 `LoginView` 성공적으로 인증을 수행 하는 탐색을 수행 합니다. 다음 코드 예제에서는 탐색 동작에 의해 호출 하는 방법을 보여 줍니다.

```xaml
<WebView ...>  
    <WebView.Behaviors>  
        <behaviors:EventToCommandBehavior  
            EventName="Navigating"  
            EventArgsConverter="{StaticResource WebNavigatingEventArgsConverter}"  
            Command="{Binding NavigateCommand}" />  
    </WebView.Behaviors>  
</WebView>
```

런타임 시 합니다 `EventToCommandBehavior` 와 상호 작용에 응답할 합니다 [ `WebView` ](xref:Xamarin.Forms.WebView)합니다. 경우는 `WebView` 웹 페이지로 이동 합니다 [ `Navigating` ](xref:Xamarin.Forms.WebView.Navigating) 이벤트가 발생 하 고 실행 하는 `NavigateCommand` 에서 `LoginViewModel`. 기본적으로 이벤트에 대 한 이벤트 인수는 명령에 전달 됩니다. 에 지정 된 변환기에서 원본과 대상 간에 전달 될 때이 데이터를 변환 하는 `EventArgsConverter` 반환 하는 속성을 [ `Url` ](xref:Xamarin.Forms.WebNavigationEventArgs.Url) 에서 [ `WebNavigatingEventArgs` ](xref:Xamarin.Forms.WebNavigatingEventArgs)합니다. 따라서 경우 합니다 `NavigationCommand` 는 실행, 웹 페이지의 Url 매개 변수로 전달 되는 등록 된 `Action`합니다.

차례로 합니다 `NavigationCommand` 실행을 `NavigateAsync` 메서드를 다음 코드 예제에 나와 있는:

```csharp
private async Task NavigateAsync(string url)  
{  
    ...          
    await NavigationService.NavigateToAsync<MainViewModel>();  
    await NavigationService.RemoveLastFromBackStackAsync();  
    ...  
}
```

이 메서드 호출에 대 한 탐색 합니다 `MainViewModel`, 탐색, 다음 제거를 `LoginView` 페이지 탐색 스택에서.

### <a name="confirming-or-cancelling-navigation"></a>확인 또는 탐색을 취소 합니다.

앱은 사용자를 확인 하거나 탐색을 취소할 수 있도록 탐색 작업을 하는 동안 사용자 상호 작용 해야 합니다. 예를 들어 사용자가 데이터 입력 페이지를 완벽 하 게 완료 하기 전에 이동 하려고 할 때이 방법을 필요한 수 있습니다. 이 경우 앱 사용자가 페이지를 나 가려고 하거나 발생 하기 전에 탐색 작업을 취소할 수 있도록 알림을 제공 해야 합니다. 이 탐색은 호출 여부를 제어 하는 알림 응답을 사용 하 여 보기 모델 클래스에서 구현할 수 있습니다.

## <a name="summary"></a>요약

Xamarin.Forms에는 일반적으로 UI 사용 하 여 사용자의 상호 작용 또는 내부 논리 기반 상태 변경으로 인해 앱 자체에서 발생 하는 페이지 탐색에 대 한 지원이 포함 됩니다. 그러나 탐색 MVVM 패턴을 사용 하는 앱에서 구현 복잡할 수 있습니다.

제공이 장에서 `NavigationService` 모델 보기에서에서 보기 모델 첫 탐색 하는 데 사용 되는 클래스입니다. 보기에서 탐색 논리를 배치 하면 모델 클래스를 통해 자동화 된 테스트 논리를 실행할 수 있습니다 의미 합니다. 또한 뷰 모델은 특정 비즈니스 규칙 적용 되도록 컨트롤 탐색 논리를 구현한 다음 수 있습니다.


## <a name="related-links"></a>관련 링크

- [2mb PDF 전자책 다운로드](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
