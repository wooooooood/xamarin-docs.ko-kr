---
제목: "엔터프라이즈 앱 탐색" 설명: "이 장은 eShopOnContainers 모바일 앱이 모델 보기에서 모델을 처음 탐색 하는 방법에 대해 설명 합니다."
assetid: 4cad57b5-7fe4-4527-a988-d9b60c9620b4: xamarin-forms author: davidbritch: dabritch:: 08/07/2017-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="enterprise-app-navigation"></a>엔터프라이즈 앱 탐색

Xamarin.Forms에는 내부 논리 기반 상태 변경으로 인해 일반적으로 사용자가 UI와 상호 작용 하거나 앱 자체에서 발생 하는 페이지 탐색에 대 한 지원이 포함 됩니다. 그러나 다음 문제를 충족 해야 하므로 MVVM (모델-뷰-ViewModel) 패턴을 사용 하는 앱에서 탐색을 구현 하는 것이 복잡할 수 있습니다.

- 보기 간에 밀접 한 결합 및 종속성을 사용 하지 않는 방법을 사용 하 여 탐색할 뷰를 식별 하는 방법입니다.
- 탐색할 뷰가 인스턴스화되어 초기화 되는 프로세스를 조정 하는 방법입니다. MVVM를 사용 하는 경우 뷰 및 뷰 모델을 인스턴스화하고 뷰의 바인딩 컨텍스트를 통해 서로 연결 해야 합니다. 앱에서 종속성 주입 컨테이너를 사용 하는 경우 뷰 및 뷰 모델 인스턴스화에 특정 생성 메커니즘이 필요할 수 있습니다.
- 뷰 우선 탐색을 수행할지 아니면 모델 우선 탐색을 볼 지 여부를 지정 합니다. 뷰 우선 탐색을 사용 하 여 탐색할 페이지는 보기 유형의 이름을 참조 합니다. 탐색 하는 동안 해당 뷰 모델 및 기타 종속 서비스와 함께 지정 된 뷰가 인스턴스화됩니다. 다른 방법으로는 탐색할 페이지가 뷰 모델 형식의 이름을 참조 하는 모델 우선 탐색 보기를 사용 하는 것이 좋습니다.
- 보기 및 뷰 모델에서 앱의 탐색 동작을 명확 하 게 구분 하는 방법입니다. MVVM 패턴은 앱의 UI와 해당 프레젠테이션 및 비즈니스 논리를 구분 합니다. 그러나 앱의 탐색 동작은 종종 응용 프로그램의 UI 및 프레젠테이션 파트를 포괄 합니다. 사용자는 종종 뷰에서 탐색을 시작 하 고 탐색의 결과로 보기가 대체 됩니다. 그러나 탐색은 종종 뷰 모델 내에서 시작 하거나 조정 해야 할 수도 있습니다.
- 초기화를 위해 탐색 하는 동안 매개 변수를 전달 하는 방법입니다. 예를 들어 사용자가 뷰로 이동 하 여 주문 세부 정보를 업데이트 하는 경우 올바른 데이터를 표시할 수 있도록 주문 데이터를 뷰에 전달 해야 합니다.
- 특정 비즈니스 규칙을 맞는 확인 하기 위해 교차 좌표 탐색 하는 방법입니다. 예를 들어 잘못 된 데이터를 수정 하거나 보기 내에서 수행 된 데이터 변경 내용을 제출 하거나 취소 하 라는 메시지가 표시 되도록 보기에서 이동 하기 전에 사용자에 게 메시지가 표시 될 수 있습니다.

이 장에서는 `NavigationService` 모델 우선 페이지 탐색 보기를 수행 하는 데 사용 되는 클래스를 제공 하 여 이러한 문제를 해결 합니다.

> [!NOTE]
> `NavigationService`앱에서 사용 하는은 ContentPage 인스턴스 간에 계층적 탐색을 수행 하는 용도로만 디자인 되었습니다. 서비스를 사용 하 여 다른 페이지 형식 사이를 탐색 하면 예기치 않은 동작이 발생할 수 있습니다.

## <a name="navigating-between-pages"></a>페이지 간 이동

탐색 논리는 뷰의 코드 숨김이 나 데이터 바인딩된 뷰 모델에 있을 수 있습니다. 보기에 탐색 논리를 배치 하는 것이 가장 간단한 방법일 수 있지만 단위 테스트를 통해 쉽게 테스트할 수는 없습니다. 뷰 모델 클래스에 탐색 논리를 배치 하면 단위 테스트를 통해 논리를 실행할 수 있음을 의미 합니다. 또한 뷰 모델은 특정 비즈니스 규칙이 적용 되도록 탐색을 제어 하는 논리를 구현할 수 있습니다. 예를 들어 앱에서 사용자가 처음에 입력 한 데이터가 유효한 지 확인 하지 않고 페이지에서 다른 위치로 이동 하는 것을 허용 하지 않을 수 있습니다.

`NavigationService`일반적으로 클래스는 테스트를 촉진 하기 위해 뷰 모델에서 호출 됩니다. 그러나 뷰 모델에서 뷰로 이동 하려면 뷰 모델에서 뷰를 참조 해야 하며, 특히 활성 뷰 모델이 연결 되지 않은 뷰를 사용 하는 것이 좋습니다. 따라서 여기에 `NavigationService` 표시 된는 뷰 모델 유형을 탐색할 대상으로 지정 합니다.

EShopOnContainers 모바일 앱은 클래스를 사용 하 여 `NavigationService` 뷰 모델-첫 번째 탐색을 제공 합니다. 이 클래스는 `INavigationService` 다음 코드 예제에 표시 된 인터페이스를 구현 합니다.

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

이 인터페이스는 구현 하는 클래스에서 다음 메서드를 제공 하도록 지정 합니다.

|방법|목적|
|--- |--- |
|`InitializeAsync`|앱이 시작 될 때 두 페이지 중 하나를 탐색 합니다.|
|`NavigateToAsync`|지정 된 페이지에 대 한 계층적 탐색을 수행 합니다.|
|`NavigateToAsync(parameter)`|지정 된 페이지에 대 한 계층적 탐색을 수행 하 여 매개 변수를 전달 합니다.|
|`RemoveLastFromBackStackAsync`|탐색 스택에서 이전 페이지를 제거 합니다.|
|`RemoveBackStackAsync`|탐색 스택에서 이전 페이지를 모두 제거 합니다.|

또한 `INavigationService` 인터페이스는 구현 하는 클래스에서 속성을 제공 하도록 지정 합니다 `PreviousPageViewModel` . 이 속성은 탐색 스택의 이전 페이지와 연결 된 뷰 모델 유형을 반환 합니다.

> [!NOTE]
> `INavigationService`인터페이스는 일반적으로 메서드를 지정 `GoBackAsync` 합니다 .이 메서드를 사용 하 여 프로그래밍 방식으로 탐색 스택의 이전 페이지로 돌아갈 수 있습니다. 그러나이 메서드는 필요 하지 않으므로 eShopOnContainers 모바일 앱에서 누락 되었습니다.

### <a name="creating-the-navigationservice-instance"></a>NavigationService 인스턴스 만들기

인터페이스를 구현 하는 `NavigationService` 클래스는 `INavigationService` 다음 코드 예제에서 보여 주는 것 처럼 Autofac 종속성 주입 컨테이너를 사용 하 여 singleton으로 등록 됩니다.

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

`INavigationService`인터페이스는 `ViewModelBase` 다음 코드 예제에서 보여 주는 것 처럼 클래스 생성자에서 확인 됩니다.

```csharp
NavigationService = ViewModelLocator.Resolve<INavigationService>();
```

그러면 `NavigationService` 클래스의 메서드에서 만든 Autofac 종속성 주입 컨테이너에 저장 된 개체에 대 한 참조가 반환 됩니다 `InitNavigation` `App` . 자세한 내용은 [앱이 시작 될 때 탐색](#navigating-when-the-app-is-launched)을 참조 하세요.

`ViewModelBase`클래스는 `NavigationService` 형식의 속성에 인스턴스를 저장 합니다 `NavigationService` `INavigationService` . 따라서 클래스에서 파생 되는 모든 뷰 모델 클래스는 `ViewModelBase` 속성을 사용 하 여 `NavigationService` 인터페이스에 지정 된 메서드에 액세스할 수 있습니다 `INavigationService` . 그러면 `NavigationService` Autofac 종속성 주입 컨테이너에서 각 뷰 모델 클래스로 개체를 삽입 하는 오버 헤드가 발생 하지 않습니다.

### <a name="handling-navigation-requests"></a>탐색 요청 처리

Xamarin.Forms사용자가 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 필요에 따라 페이지를 앞뒤로 탐색할 수 있는 계층적 탐색 환경을 구현 하는 클래스를 제공 합니다. 계층적 탐색에 대한 자세한 내용은 [계층적 탐색](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)을 참조하세요.

클래스를 직접 사용 하는 대신 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) eShopOnContainers 앱은 `NavigationPage` `CustomNavigationView` 다음 코드 예제와 같이 클래스를 클래스에 래핑합니다.

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

이 래핑의 목적은 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 클래스의 XAML 파일 내에서 인스턴스의 스타일을 쉽게 지정 하기 위한 것입니다.

탐색은 `NavigateToAsync` 다음 코드 예제와 같이 탐색 중인 페이지의 뷰 모델 형식을 지정 하는 메서드 중 하나를 호출 하 여 뷰 모델 클래스 내에서 수행 됩니다.

```csharp
await NavigationService.NavigateToAsync<MainViewModel>();
```

다음 코드 예제에서는 `NavigateToAsync` 클래스에서 제공 하는 메서드를 보여 줍니다 `NavigationService` .

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

각 메서드는 클래스에서 파생 되는 모든 뷰 모델 클래스에서 `ViewModelBase` 메서드를 호출 하 여 계층적 탐색을 수행할 수 있도록 `InternalNavigateToAsync` 합니다. 또한 두 번째 방법에서는 `NavigateToAsync` 탐색 데이터를 탐색 중인 뷰 모델에 전달 되는 인수로 지정할 수 있습니다. 여기서는 일반적으로 초기화를 수행 하는 데 사용 됩니다. 자세한 내용은 [탐색 하는 동안 매개 변수 전달](#passing-parameters-during-navigation)을 참조 하세요.

`InternalNavigateToAsync`메서드는 탐색 요청을 실행 하 고 다음 코드 예제에 표시 됩니다.

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

`InternalNavigateToAsync`메서드는 먼저 메서드를 호출 하 여 뷰 모델에 대 한 탐색을 수행 합니다 `CreatePage` . 이 메서드는 지정 된 뷰 모델 유형에 해당 하는 뷰를 찾고이 뷰 유형의 인스턴스를 만들고 반환 합니다. 뷰 모델 유형에 해당 하는 뷰를 찾는 것은 다음을 전제로 하는 규칙 기반 방법을 사용 합니다.

- 뷰는 뷰 모델 유형과 동일한 어셈블리에 있습니다.
- 보기는에 있습니다. 뷰 하위 네임 스페이스입니다.
- 뷰 모델은에 있습니다. ViewModels 자식 네임 스페이스입니다.
- 뷰 이름은 "모델"이 제거 된 모델 이름 보기에 해당 합니다.

뷰가 인스턴스화된 경우 해당 뷰 모델에 연결 됩니다. 이를 수행 하는 방법에 대 한 자세한 내용은 [뷰 모델 로케이터를 사용 하 여 자동으로 뷰 모델 만들기](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically-creating-a-view-model-with-a-view-model-locator)를 참조 하세요.

만드는 뷰가 인 경우 `LoginView` 클래스의 새 인스턴스 내에 래핑되어 `CustomNavigationView` 속성에 할당 됩니다 [`Application.Current.MainPage`](xref:Xamarin.Forms.Application.MainPage) . 그렇지 않으면 `CustomNavigationView` 인스턴스가 검색 되 고 null이 아닌 경우 [`PushAsync`](xref:Xamarin.Forms.NavigationPage) 메서드를 호출 하 여 생성 되는 뷰를 탐색 스택으로 푸시합니다. 그러나 검색 된 인스턴스가 인 경우에는 `CustomNavigationView` `null` 만들어지는 뷰가 클래스의 새 인스턴스 내에 래핑되어 `CustomNavigationView` 속성에 할당 됩니다 `Application.Current.MainPage` . 이 메커니즘을 사용 하면 탐색 하는 동안 페이지가 비어 있을 때와 데이터를 포함 하는 경우 탐색 스택에 올바르게 추가 됩니다.

> [!TIP]
> 페이지 캐싱을 고려 합니다. 페이지 캐싱은 현재 표시 되지 않은 뷰에 대해 메모리 소비가 발생 합니다. 그러나 페이지 캐싱이 없으면 페이지 및 해당 뷰 모델의 XAML 구문 분석과 생성이 새 페이지를 탐색할 때마다 발생 하며,이는 복잡 한 페이지의 성능에 영향을 줄 수 있습니다. 컨트롤을 많이 사용 하지 않는 잘 디자인 된 페이지의 경우 성능이 충분 해야 합니다. 그러나 페이지 캐싱은 저속 페이지 로드 시간이 발생 하는 경우에 유용할 수 있습니다.

뷰가 생성 되 고 탐색 되 면 `InitializeAsync` 뷰의 연결 된 뷰 모델의 메서드가 실행 됩니다. 자세한 내용은 [탐색 하는 동안 매개 변수 전달](#passing-parameters-during-navigation)을 참조 하세요.

### <a name="navigating-when-the-app-is-launched"></a>앱이 시작 될 때 탐색

앱이 시작 되 면 `InitNavigation` 클래스의 메서드가 `App` 호출 됩니다. 다음 코드 예제에서는 이 메서드를 보여줍니다.

```csharp
private Task InitNavigation()  
{  
    var navigationService = ViewModelLocator.Resolve<INavigationService>();  
    return navigationService.InitializeAsync();  
}
```

메서드는 `NavigationService` 메서드를 호출 하기 전에 Autofac 종속성 주입 컨테이너에 새 개체를 만들고 해당 개체에 대 한 참조를 반환 합니다 `InitializeAsync` .

> [!NOTE]
> 인터페이스를 `INavigationService` 클래스에서 확인 하면 `ViewModelBase` 해당 컨테이너는 `NavigationService` initnavigation 메서드가 호출 될 때 생성 된 개체에 대 한 참조를 반환 합니다.

다음 코드 예제는 `NavigationService` `InitializeAsync` 메서드를 보여줍니다.

```csharp
public Task InitializeAsync()  
{  
    if (string.IsNullOrEmpty(Settings.AuthAccessToken))  
        return NavigateToAsync<LoginViewModel>();  
    else  
        return NavigateToAsync<MainViewModel>();  
}
```

`MainView`앱에 인증에 사용 되는 캐시 된 액세스 토큰이 있는 경우를 탐색 합니다. 그렇지 않으면를 `LoginView` 탐색 합니다.

Autofac 종속성 주입 컨테이너에 대 한 자세한 내용은 [종속성 주입 소개](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction-to-dependency-injection)를 참조 하세요.

### <a name="passing-parameters-during-navigation"></a>탐색 하는 동안 매개 변수 전달

인터페이스에서 지정 하는 메서드 중 하나를 사용 하 여 `NavigateToAsync` `INavigationService` 탐색 데이터를 탐색 중인 뷰 모델에 전달 되는 인수로 지정할 수 있습니다. 여기서는 일반적으로 초기화를 수행 하는 데 사용 됩니다.

예를 들어, `ProfileViewModel` 클래스에는 `OrderDetailCommand` 사용자가 페이지에서 순서를 선택할 때 실행 되는가 포함 됩니다 `ProfileView` . 그러면 `OrderDetailAsync` 다음 코드 예제에 표시 된 메서드를 실행 합니다.

```csharp
private async Task OrderDetailAsync(Order order)  
{  
    await NavigationService.NavigateToAsync<OrderDetailViewModel>(order);  
}
```

이 메서드는에 대 한 탐색을 호출 하 여 `OrderDetailViewModel` `Order` 페이지에서 사용자가 선택한 순서를 나타내는 인스턴스를 전달 합니다 `ProfileView` . `NavigationService`클래스가를 만들면 `OrderDetailView` `OrderDetailViewModel` 클래스가 인스턴스화되고 뷰의에 할당 됩니다 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) . 로 이동한 후 `OrderDetailView` `InternalNavigateToAsync` 메서드는 `InitializeAsync` 뷰의 연결 된 뷰 모델의 메서드를 실행 합니다.

`InitializeAsync`메서드는 재정의 가능한 메서드로 클래스에서 정의 됩니다 `ViewModelBase` . 이 메서드는 `object` 탐색 작업을 수행 하는 동안 뷰 모델에 전달할 데이터를 나타내는 인수를 지정 합니다. 따라서 탐색 작업에서 데이터를 수신 하려는 뷰 모델 클래스는 `InitializeAsync` 필요한 초기화를 수행 하기 위해 메서드의 고유한 구현을 제공 합니다. 다음 코드 예제에서는 클래스의 메서드를 보여 줍니다 `InitializeAsync` `OrderDetailViewModel` .

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

이 메서드는 `Order` 탐색 작업 중에 뷰 모델로 전달 된 인스턴스를 검색 하 고이 인스턴스를 사용 하 여 인스턴스에서 전체 주문 세부 정보를 검색 합니다 `OrderService` .

### <a name="invoking-navigation-using-behaviors"></a>동작을 사용 하 여 탐색 호출

탐색은 일반적으로 사용자 조작을 통해 뷰에서 트리거됩니다. 예를 들어는 `LoginView` 성공적인 인증 후에 탐색을 수행 합니다. 다음 코드 예제에서는 동작에 따라 탐색을 호출 하는 방법을 보여 줍니다.

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

런타임에는와의 `EventToCommandBehavior` 상호 작용에 응답 합니다 [`WebView`](xref:Xamarin.Forms.WebView) . 에서 `WebView` 웹 페이지로 이동 하면 이벤트가 발생 하 고 [`Navigating`](xref:Xamarin.Forms.WebView.Navigating) 에서이 실행 됩니다 `NavigateCommand` `LoginViewModel` . 기본적으로 이벤트에 대 한 이벤트 인수가 명령에 전달 됩니다. 이 데이터는 속성에 지정 된 변환기에서 소스와 대상 사이에 전달 될 때 변환 되며 `EventArgsConverter` 에서을 반환 합니다 [`Url`](xref:Xamarin.Forms.WebNavigationEventArgs.Url) [`WebNavigatingEventArgs`](xref:Xamarin.Forms.WebNavigatingEventArgs) . 따라서을 실행 하면 `NavigationCommand` 웹 페이지의 Url이 등록 된에 매개 변수로 전달 됩니다 `Action` .

그러면는 `NavigationCommand` `NavigateAsync` 다음 코드 예제에 표시 된 메서드를 실행 합니다.

```csharp
private async Task NavigateAsync(string url)  
{  
    ...          
    await NavigationService.NavigateToAsync<MainViewModel>();  
    await NavigationService.RemoveLastFromBackStackAsync();  
    ...  
}
```

이 메서드는에 대 한 탐색을 호출 하 고, 탐색 다음에 탐색 `MainViewModel` 스택에서 페이지를 제거 합니다 `LoginView` .

### <a name="confirming-or-cancelling-navigation"></a>탐색 확인 또는 취소

사용자가 탐색을 확인 하거나 취소할 수 있도록 앱이 탐색 작업 중에 사용자와 상호 작용 해야 할 수 있습니다. 예를 들어 사용자가 데이터 입력 페이지를 완전히 완료 하기 전에 탐색 하려고 할 때이 작업이 필요할 수 있습니다. 이 경우 앱은 사용자가 페이지에서 다른 곳으로 이동 하거나 이동 하기 전에 탐색 작업을 취소할 수 있는 알림을 제공 해야 합니다. 이는 탐색이 호출 되는지 여부를 제어 하기 위해 알림의 응답을 사용 하 여 뷰 모델 클래스에서 수행할 수 있습니다.

## <a name="summary"></a>요약

Xamarin.Forms에는 내부 논리 기반 상태 변경으로 인해 일반적으로 사용자가 UI와 상호 작용 하거나 앱 자체에서 발생 하는 페이지 탐색에 대 한 지원이 포함 되어 있습니다. 그러나 MVVM 패턴을 사용 하는 앱에서 탐색을 구현 하는 것은 복잡할 수 있습니다.

이 장에서는 `NavigationService` 뷰 모델에서 뷰 모델을 처음 탐색 하는 데 사용 되는 클래스를 제공 했습니다. 뷰 모델 클래스에 탐색 논리를 배치 하면 자동화 된 테스트를 통해 논리를 수행할 수 있습니다. 또한 뷰 모델은 특정 비즈니스 규칙이 적용 되도록 탐색을 제어 하는 논리를 구현할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [다운로드 전자책 (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
