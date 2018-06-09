---
title: 단위 테스트 엔터프라이즈 앱
description: 이 장에서 eShopOnContainers 모바일 앱에서 단위 테스트를 수행 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 4af82e52-f99b-4cad-b278-1745f190c240
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 06cd89e0b0871eac723e8580340173f77821e4ed
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242533"
---
# <a name="unit-testing-enterprise-apps"></a>단위 테스트 엔터프라이즈 앱

모바일 응용 프로그램 문제가 고유 데스크톱 및 웹 응용 프로그램에 걱정할 필요가 없습니다. 모바일 사용자에 게 사용 하 여 네트워크 연결 서비스의 가용성 및 기타 요인의 범위에서 장치에 따라 다릅니다. 따라서 모바일 앱의 품질, 안정성 및 성능을 개선 하기 위해 실제 상황에서 사용 될 되므로 테스트 되어야 합니다. 단위 테스트, 테스트, 통합 및 테스트, 단위 테스트의 가장 일반적인 형태 되 고 테스트 하는 사용자 인터페이스를 포함 하 여 응용 프로그램에서 수행 해야 하는 여러 종류의 테스트 있습니다.

단위 테스트 메서드 일반적으로 응용 프로그램의 작은 단위를 사용, 코드의 다른 부분과에서 격리 및 예상 대로 작동 하는지 확인 합니다. 목표는 각 기능 단위를 예상 대로 수행 되었음을, 응용 프로그램 전체에서 오류가 전파 하지 않도록 확인 하는입니다. 버그가 발생 한 위치를 검색 하는 것이 더 효율적 해당 보조 오류 발생 시점에서 간접적으로 버그의 효과 관찰 합니다.

단위 테스트는 소프트웨어 개발 워크플로의 핵심 요소로 사용 될 때 코드 품질에 큰 영향을 미칩니다. 메서드를 작성 하는 즉시 표준, 경계, 및 입력된 데이터의 잘못 된 사례에 대 한 응답에서 메서드와 확인의 동작을 코드에 의해 만들어진 모든 명시적 또는 암시적 가정을 확인 하는 단위 테스트를 작성 되어야 합니다. 또는 테스트 구동 방식 개발, 단위 테스트는 코드 보다 먼저 기록 됩니다. 이 시나리오에서는 단위 테스트 기능 사양 및 디자인 설명서로 작동 합니다.

> [!NOTE]
> 단위 테스트는 회귀 – 즉, 있지만 기능을 사용 하는 잘못 된 업데이트에 의해 놨에 매우 효과적입니다.

일반적으로 단위 테스트 정렬 act assert 패턴을 사용합니다.

-   *정렬* 섹션 단위 테스트 메서드의 개체를 초기화 하 고 테스트 중인 메서드에 전달 되는 데이터의 값을 설정 합니다.
-   *역할* 섹션에서 필수 인수와 함께 테스트 중인 메서드를 호출 합니다.
-   *assert* 섹션은 테스트 중인 메서드의 작업이 예상한 대로 작동 하는지 확인 합니다.

다음이 패턴에서는 단위 테스트 읽을 수 있는 고 일관 된 하는지 확인 합니다.

## <a name="dependency-injection-and-unit-testing"></a>종속성 주입 및 단위 테스트

동기 중 하나는 느슨하게 결합 된 아키텍처를 채택 하는 단위 테스트를 용이 하 게 합니다. 등록 된 Autofac 형식 중 하나는 `OrderService` 클래스입니다. 다음 코드 예제에서는이 클래스의 개요를 보여 줍니다.

```csharp
public class OrderDetailViewModel : ViewModelBase  
{  
    private IOrderService _ordersService;  

    public OrderDetailViewModel(IOrderService ordersService)  
    {  
        _ordersService = ordersService;  
    }  
    ...  
}
```

`OrderDetailViewModel` 클래스에 종속 되어는 `IOrderService` 컨테이너 인스턴스화하여 때 확인 되는 입력 한 `OrderDetailViewModel` 개체입니다. 그러나 작성 하기 보다는 `OrderService` 단위 테스트에 대 한 개체는 `OrderDetailViewModel` 클래스 대신 대체는 `OrderService` 테스트 목적으로 모의 개체입니다. 그림 10-1이이 관계를 보여 줍니다.

![](unit-testing-images/unittesting.png "IOrderService 인터페이스를 구현 하는 클래스")

**그림 10-1:** IOrderService 인터페이스를 구현 하는 클래스

이 방법을 사용 하면는 `OrderService` 에 전달 될 개체는 `OrderDetailViewModel` 클래스 런타임에 및 테스트 용이성의 관심 사항이에서 허용는 `OrderMockService` 전달 되어야 하는 클래스는 `OrderDetailViewModel` 테스트 시 클래스입니다. 이 방법의 주요 장점은 단위 테스트를 웹 서비스 또는 데이터베이스와 같은 리소스를 다루기 필요로 하지 않고 실행 하는 것입니다.

## <a name="testing-mvvm-applications"></a>MVVM 응용 프로그램 테스트

모델 및 MVVM 응용 프로그램에서 뷰 모델을 테스트 다른 클래스를 테스트 동일 하며 동일한 도구 및-단위 테스트 및 모의 같은 기술을 사용할 수 있습니다. 그러나 몇 가지는 일반적으로 모델에 있는 패턴 및 특정 단위 테스트 기법을 활용할 수 있는 뷰 모델 클래스입니다.

> [!TIP]
> 각 단위 테스트와 함께 한 가지를 테스트 합니다. 단위 테스트 연습 장치의 동작의 여러 측면을 확인 하려고 하지 마십시오. 이렇게 하면 읽고 업데이트할 수 없는 테스트. 오류를 해석할 때 혼란이 발생할 수도 있습니다.

EShopOnContainers 모바일 앱 사용 하 여 [xUnit](https://xunit.github.io/) 단위 테스트는 두 가지 유형의 단위 테스트를 수행 하려면:

-   팩트는 테스트는 항상 true를 고정 조건 테스트 합니다.
-   이론은 테스트 에게만 제공 되는 특정 데이터 집합에도 마찬가지입니다.

EShopOnContainers 모바일 앱에 포함 된 단위 테스트는 팩트 테스트 하 고 각 단위 테스트 메서드가으로 데코레이팅되 어 있으므로 `[Fact]` 특성입니다.

> [!NOTE]
> test runner를 의해 xUnit 테스트를 실행 합니다. Test runner를 실행 하려면 필요한 플랫폼에 대 한 eShopOnContainers.TestRunner 프로젝트를 실행 합니다.

### <a name="testing-asynchronous-functionality"></a>비동기 기능을 테스트

MVVM 패턴을 구현할 때는 모델 보기 일반적으로 서비스에 대 한 작업 종종 비동기적으로 호출 합니다. 일반적으로 이러한 작업을 호출 하는 코드에 대 한 테스트는 실제 서비스에 대 한 대체 항목으로 모의 개체를 사용 합니다. 다음 코드 예제에서는 모의 서비스 보기 모델에 전달 하 여 비동기 기능을 테스트 하는 방법을 보여 줍니다.

```csharp
[Fact]  
public async Task OrderPropertyIsNotNullAfterViewModelInitializationTest()  
{  
    var orderService = new OrderMockService();  
    var orderViewModel = new OrderDetailViewModel(orderService);  

    var order = await orderService.GetOrderAsync(1, GlobalSetting.Instance.AuthToken);  
    await orderViewModel.InitializeAsync(order);  

    Assert.NotNull(orderViewModel.Order);  
}
```

단위 테스트를 검사 하는 `Order` 의 속성은 `OrderDetailViewModel` 인스턴스는 다음 값을 갖게 됩니다는 `InitializeAsync` 메서드를 호출 했습니다. `InitializeAsync` 뷰 모델의 해당 뷰를 탐색 하는 메서드가 호출 됩니다. 탐색에 대 한 자세한 내용은 참조 [탐색](~/xamarin-forms/enterprise-application-patterns/navigation.md)합니다.

경우는 `OrderDetailViewModel` 가정 인스턴스를 만든는 `OrderService` 인스턴스를 인수로 지정 해야 합니다. 그러나는 `OrderService` 웹 서비스에서 데이터를 검색 합니다. 따라서는 `OrderMockService` 인스턴스 즉 모의 버전의는 `OrderService` 클래스에 대 한 인수로 지정 합니다.는 `OrderDetailViewModel` 생성자입니다. 다음 경우의 보기 모델 `InitializeAsync` 메서드를 호출 하 `IOrderService` 작업 모의 데이터는 검색 된 것이 아니라 웹 서비스와 통신 합니다.

### <a name="testing-inotifypropertychanged-implementations"></a>테스트 INotifyPropertyChanged 구현

구현 된 `INotifyPropertyChanged` 인터페이스 보기에서 생성 되는 변경에 대응 하는 뷰를 사용 하면 모델 및 모델입니다. 이러한 변경 내용은 컨트롤에 표시 된 데이터에 제한 되지는 않습니다. – 애니메이션을 시작할 수 또는 사용 하지 않도록 설정할 컨트롤 발생 하는 뷰 모델 상태와 같은 보기를 제어할도 사용 됩니다.

단위 테스트에서 직접 업데이트할 수 있는 이벤트 처리기를 연결 하 여 테스트할 수 있습니다는 `PropertyChanged` 이벤트 및 속성에 대 한 새 값을 설정한 후 이벤트를 발생시킬지 여부를 확인 합니다. 다음 코드 예제에서는 이러한 테스트를 보여 줍니다.

```csharp
[Fact]  
public async Task SettingOrderPropertyShouldRaisePropertyChanged()  
{  
    bool invoked = false;  
    var orderService = new OrderMockService();  
    var orderViewModel = new OrderDetailViewModel(orderService);  

    orderViewModel.PropertyChanged += (sender, e) =>  
    {  
        if (e.PropertyName.Equals("Order"))  
            invoked = true;  
    };  
    var order = await orderService.GetOrderAsync(1, GlobalSetting.Instance.AuthToken);  
    await orderViewModel.InitializeAsync(order);  

    Assert.True(invoked);  
}
```

이 단위 테스트를 호출는 `InitializeAsync` 의 메서드는 `OrderViewModel` 클래스를 초래 하는 해당 `Order` 업데이트할 속성입니다. 단위 테스트는 성공 하는 `PropertyChanged` 에 대 한 이벤트는 `Order` 속성입니다.

### <a name="testing-message-based-communication"></a>테스트 메시지 기반 통신

보기를 사용 하는 모델링는 [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) 느슨하게 결합 된 클래스 간의 수 단위 테스트에서 다음 코드 예제와 같이, 테스트 중인 코드에서 전송 되는 메시지를 구독 하 여 통신 하는 클래스:

```csharp
[Fact]  
public void AddCatalogItemCommandSendsAddProductMessageTest()  
{  
    bool messageReceived = false;  
    var catalogService = new CatalogMockService();  
    var catalogViewModel = new CatalogViewModel(catalogService);  

    Xamarin.Forms.MessagingCenter.Subscribe<CatalogViewModel, CatalogItem>(  
        this, MessageKeys.AddProduct, (sender, arg) =>  
    {  
        messageReceived = true;  
    });  
    catalogViewModel.AddCatalogItemCommand.Execute(null);  

    Assert.True(messageReceived);  
}
```

단위 테스트를 검사 하는 `CatalogViewModel` 게시는 `AddProduct` 대 한 응답으로 메시지 해당 `AddCatalogItemCommand` 실행 되 고 있습니다. 때문에 [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) 클래스 멀티 캐스트 메시지 등록 지원, 단위 테스트를 구독할 수는 `AddProduct` 메시지 및 응답 받은으로 콜백 대리자를 실행 합니다. 설정 하는 람다 식으로 지정 된이 콜백 대리자는 `boolean` 에서 사용 되는 필드는 `Assert` 테스트의 동작을 확인 하는 문입니다.

### <a name="testing-exception-handling"></a>예외 처리를 테스트합니다.

단위 테스트 기록할 수도 잘못 된 동작 또는 입력에 대 한 특정 예외 throw 되는 확인 다음 코드 예제에서와 같이:

```csharp
[Fact]  
public void InvalidEventNameShouldThrowArgumentExceptionText()  
{  
    var behavior = new MockEventToCommandBehavior  
    {  
        EventName = "OnItemTapped"  
    };  
    var listView = new ListView();  

    Assert.Throws<ArgumentException>(() => listView.Behaviors.Add(behavior));  
}
```

이 단위 테스트 하기 때문에 예외를 throw 합니다는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 컨트롤에는 라는 이벤트 `OnItemTapped`합니다. `Assert.Throws<T>` 메서드는 제네릭 메서드가 있는 `T` 예상된 된 예외의 형식입니다. 에 전달 되는 인수는 `Assert.Throws<T>` 메서드는 예외를 throw 하는 람다 식입니다. 람다 식은 throw 하는 단위 테스트 따라서 통과 하는지는 `ArgumentException`합니다.

>💡 **팁**: 예외 메시지 문자열을 검사 하는 단위 테스트를 작성 하지 마세요. 예외 메시지 문자열은 시간이 지남에 따라 변경 될 수 있습니다 및 단위 테스트의 현재 상태에 의존 하는 불안정 한으로 간주 되는 하므로 합니다.

### <a name="testing-validation"></a>유효성 검사 테스트

다음 두 가지 테스트 유효성 검사 구현: 모든 유효성 검사 규칙 올바르게 구현 될 테스트 및 테스트 하는 `ValidatableObject<T>` 클래스 예상 대로 수행 합니다.

유효성 검사 논리를 테스트 하려면 일반적으로 간단한 이므로 일반적으로 자체 포함 된 프로세스에서 출력 입력에 따라 달라 집니다. 호출의 결과에 테스트 해야 합니다.는 `Validate` 다음 코드 예제에서와 같이 관련된 유효성 검사 규칙을 하나 이상 포함 된 각 속성에 대 한 메서드:

```csharp
[Fact]  
public void CheckValidationPassesWhenBothPropertiesHaveDataTest()  
{  
    var mockViewModel = new MockViewModel();  
    mockViewModel.Forename.Value = "John";  
    mockViewModel.Surname.Value = "Smith";  

    bool isValid = mockViewModel.Validate();  

    Assert.True(isValid);  
}
```

이 단위 테스트 유효성 검사 될 때 성공 했는지 확인 두 `ValidatableObject<T>` 속성에는 `MockViewModel` 인스턴스는 모두 데이터를 포함 합니다.

유효성 검사에 성공 하면 검사 및 단위 테스트 유효성 검사도 확인 해야의 값은 `Value`, `IsValid`, 및 `Errors` 각 속성 `ValidatableObject<T>` 클래스 예상 대로 수행 되었음을 확인 하려면 인스턴스. 다음 코드 예제에서는이 작업을 수행 하는 단위 테스트를 보여 줍니다.

```csharp
[Fact]  
public void CheckValidationFailsWhenOnlyForenameHasDataTest()  
{  
    var mockViewModel = new MockViewModel();  
    mockViewModel.Forename.Value = "John";  

    bool isValid = mockViewModel.Validate();  

    Assert.False(isValid);  
    Assert.NotNull(mockViewModel.Forename.Value);  
    Assert.Null(mockViewModel.Surname.Value);  
    Assert.True(mockViewModel.Forename.IsValid);  
    Assert.False(mockViewModel.Surname.IsValid);  
    Assert.Empty(mockViewModel.Forename.Errors);  
    Assert.NotEmpty(mockViewModel.Surname.Errors);  
}
```

단위 테스트 유효성 검사 때 실패를 검사는 `Surname` 속성은 `MockViewModel` 없는 경우 모든 데이터 및 `Value`, `IsValid`, 및 `Errors` 각 속성 `ValidatableObject<T>` 인스턴스 정확 하 게 설정 합니다.

## <a name="summary"></a>요약

단위 테스트 메서드 일반적으로 응용 프로그램의 작은 단위를 사용, 코드의 다른 부분과에서 격리 및 예상 대로 작동 하는지 확인 합니다. 목표는 각 기능 단위를 예상 대로 수행 되었음을, 응용 프로그램 전체에서 오류가 전파 하지 않도록 확인 하는입니다.

종속 개체의 동작을 시뮬레이션 하는 모의 개체와 종속 개체를 대체 하 여 테스트 중인 개체의 동작을 격리 될 수 있습니다. 이렇게 하면 단위 테스트를 웹 서비스 또는 데이터베이스와 같은 리소스를 다루기 필요로 하지 않고 실행할 수 있습니다.

모델 및 MVVM 응용 프로그램에서 뷰 모델을 테스트 다른 클래스를 테스트 동일 하며 동일한 도구 및 기법을 사용할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [EBook (2mb PDF)를 다운로드 합니다.](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
