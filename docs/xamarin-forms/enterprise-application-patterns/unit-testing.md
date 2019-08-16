---
title: 엔터프라이즈 앱 유닛 테스트
description: 이 장에서는 eShopOnContainers 모바일 앱에서 단위 테스트를 수행 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 4af82e52-f99b-4cad-b278-1745f190c240
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: c631ca73d69ea630592920a32804512f89d5baaf
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69529077"
---
# <a name="unit-testing-enterprise-apps"></a>엔터프라이즈 앱 유닛 테스트

모바일 앱에는 데스크톱 및 웹 기반 응용 프로그램에서 걱정 하지 않아도 되는 고유한 문제가 있습니다. 모바일 사용자는 사용 하는 장치, 네트워크 연결, 서비스 가용성, 기타 다양 한 요인에 따라 다릅니다. 따라서 모바일 앱은 품질, 안정성 및 성능을 개선 하기 위해 실제 세계에서 사용 되는 것으로 테스트 해야 합니다. 단위 테스트, 통합 테스트, 사용자 인터페이스 테스트를 비롯 하 여 앱에서 수행 해야 하는 다양 한 테스트 유형과 단위 테스트를 가장 일반적인 테스트 형식으로 사용할 수 있습니다.

단위 테스트는 응용 프로그램의 작은 단위 (일반적으로 메서드)를 사용 하 여 코드의 나머지 부분에서 분리 하 고 예상 대로 작동 하는지 확인 합니다. 이러한 목표는 각 기능 단위가 예상 대로 수행 되는지 확인 하 여 응용 프로그램 전체에서 오류가 전파 되지 않도록 하는 것입니다. 문제가 발생 하는 버그를 검색 하는 것은 보조 실패 지점에서 간접적으로 버그의 효과를 관찰 하는 것이 더 효율적입니다.

유닛 테스트는 소프트웨어 개발 워크플로의 필수적인 부분인 코드 품질에 가장 큰 영향을 미칠 수 있습니다. 메서드를 작성 하는 즉시 표준, 경계, 잘못 된 입력 데이터 사례에 대 한 응답으로 메서드의 동작을 확인 하는 단위 테스트를 작성 하 고 코드에 의해 수행 된 모든 명시적 또는 암시적 가정을 확인 하는 단위 테스트를 작성 해야 합니다. 또는 테스트 기반 개발을 사용 하 여 단위 테스트를 코드 보다 먼저 작성 합니다. 이 시나리오에서 단위 테스트는 디자인 설명서와 기능 사양으로 작동 합니다.

> [!NOTE]
> 단위 테스트는 작동 하는 데 사용 되지만 잘못 된 업데이트로 방해 받은 기능에 대해 매우 효과적입니다.

단위 테스트는 일반적으로 정렬-동작 어설션 패턴을 사용 합니다.

- 단위 테스트 메서드의 *정렬* 섹션은 개체를 초기화 하 고 테스트 중인 메서드에 전달 되는 데이터의 값을 설정 합니다.
- *Act* 섹션은 필요한 인수를 사용 하 여 테스트 중인 메서드를 호출 합니다.
- *Assert* 섹션은 테스트 중인 메서드의 작업이 예상 대로 작동 하는지 확인 합니다.

이 패턴을 따라 단위 테스트를 읽고 일관성 있게 유지할 수 있습니다.

## <a name="dependency-injection-and-unit-testing"></a>종속성 주입 및 유닛 테스트

느슨하게 결합 된 아키텍처를 채택 하는 동기 중 하나는 유닛 테스트를 용이 하 게 하는 것입니다. Autofac `OrderService` 에 등록 된 유형 중 하나는 클래스입니다. 다음 코드 예제에서는이 클래스의 개요를 보여 줍니다.

```csharp
public class OrderDetailViewModel : ViewModelBase  
{  
    private IOrderService _ordersService;  

    public OrderDetailViewModel(IOrderService ordersService)  
    {  
        _ordersService = ordersService;  
    }  
    ...  
}
```

클래스 `OrderDetailViewModel` 는 개체 `IOrderService` 를`OrderDetailViewModel` 인스턴스화할 때 컨테이너가 확인 하는 형식에 대 한 종속성을 갖습니다. 그러나 `OrderDetailViewModel` 클래스를 단위 테스트 하 `OrderService` 는 개체를 만드는 대신 테스트 목적을 위한 모의 `OrderService` 개체로 개체를 대체 합니다. 그림 10-1에서는이 관계를 보여 줍니다.

![](unit-testing-images/unittesting.png "IOrderService 인터페이스를 구현 하는 클래스")

**그림 10-1:** IOrderService 인터페이스를 구현 하는 클래스

이 방법을 사용 하면 `OrderService` 런타임에 개체를 `OrderDetailViewModel` 클래스로 전달할 수 있으며, 테스트 `OrderMockService` 용이성의 관심사에서 테스트 시간에 클래스를 `OrderDetailViewModel` 클래스로 전달할 수 있습니다. 이 방법의 주요 이점은 웹 서비스, 데이터베이스 등의 더 많은 리소스를 요구 하지 않고 단위 테스트를 실행할 수 있다는 것입니다.

## <a name="testing-mvvm-applications"></a>MVVM 응용 프로그램 테스트

MVVM 응용 프로그램에서 모델을 테스트 하 고 모델을 확인 하는 것은 다른 클래스를 테스트 하는 것과 동일 하며, 단위 테스트 및 mock와 같은 도구와 기법을 사용할 수 있습니다. 그러나 모델 클래스를 모델링 하 고 보는 데 일반적으로 사용할 수 있는 몇 가지 패턴이 있습니다. 이러한 패턴은 특정 유닛 테스트 기법을 활용 합니다.

> [!TIP]
> 각 단위 테스트를 사용 하 여 하나의 항목을 테스트 합니다. 단위 테스트를 실행 하는 데는 단위 동작의 두 가지 이상의 측면이 필요 하지 않습니다. 이렇게 하면 읽고 업데이트 하기 어려운 테스트가 진행 됩니다. 오류를 해석할 때 혼동을 일으킬 수도 있습니다.

EShopOnContainers 모바일 앱은 [Xunit](https://xunit.github.io/) 을 사용 하 여 두 가지 유형의 단위 테스트를 지 원하는 유닛 테스트를 수행 합니다.

- 팩트는 항상 true 인 테스트로, 고정 조건을 테스트 합니다.
- 이론는 특정 데이터 집합에만 적용 되는 테스트입니다.

EShopOnContainers 모바일 앱에 포함 된 단위 테스트는 팩트 테스트 이므로 각 단위 테스트 메서드는 `[Fact]` 특성으로 데코 레이트 됩니다.

> [!NOTE]
> xUnit 테스트는 test runner에 의해 실행 됩니다. Test runner를 실행 하려면 필요한 플랫폼에 대 한 eShopOnContainers. Testrunner.completed 프로젝트를 실행 합니다.

### <a name="testing-asynchronous-functionality"></a>비동기 기능 테스트

MVVM 패턴을 구현 하는 경우 뷰 모델은 일반적으로 서비스에 대 한 작업을 비동기적으로 호출 합니다. 이러한 작업을 호출 하는 코드에 대 한 테스트는 일반적으로 mock를 실제 서비스의 대체 항목으로 사용 합니다. 다음 코드 예제에서는 뷰 모델에 모의 서비스를 전달 하 여 비동기 기능을 테스트 하는 방법을 보여 줍니다.

```csharp
[Fact]  
public async Task OrderPropertyIsNotNullAfterViewModelInitializationTest()  
{  
    var orderService = new OrderMockService();  
    var orderViewModel = new OrderDetailViewModel(orderService);  

    var order = await orderService.GetOrderAsync(1, GlobalSetting.Instance.AuthToken);  
    await orderViewModel.InitializeAsync(order);  

    Assert.NotNull(orderViewModel.Order);  
}
```

이 단위 테스트는 `Order` 메서드를 `InitializeAsync` 호출한 후 `OrderDetailViewModel` 인스턴스의 속성에 값이 있는지 확인 합니다. 뷰 모델의 해당 뷰를 탐색 하면 메서드가호출됩니다.`InitializeAsync` 탐색에 대 한 자세한 내용은 [탐색](~/xamarin-forms/enterprise-application-patterns/navigation.md)을 참조 하세요.

인스턴스를 만들 때 인스턴스는 `OrderService` 인수로 지정 될 것으로 예상 됩니다. `OrderDetailViewModel` 그러나는 `OrderService` 웹 서비스에서 데이터를 검색 합니다. 따라서 클래스의 모의 버전인 `OrderDetailViewModel`인스턴스는 생성자에 대 한 인수로 지정 됩니다. `OrderMockService` `OrderService` 그런 다음 작업을 호출 `InitializeAsync` `IOrderService` 하는 뷰 모델의 메서드가 호출 되 면 웹 서비스와 통신 하는 대신 모의 데이터가 검색 됩니다.

### <a name="testing-inotifypropertychanged-implementations"></a>INotifyPropertyChanged 구현 테스트

인터페이스를 `INotifyPropertyChanged` 구현 하면 뷰가 뷰 모델 및 모델에서 발생 하는 변경 내용에 반응할 수 있습니다. 이러한 변경 내용은 컨트롤에 표시 되는 데이터로 제한 되지 않습니다. 또한 애니메이션을 시작 하거나 컨트롤을 사용 하지 않도록 설정 하는 모델 상태 보기와 같이 뷰를 제어 하는 데 사용 됩니다.

단위 테스트에서 직접 업데이트할 수 있는 속성은 이벤트 처리기를 `PropertyChanged` 이벤트에 연결 하 고 속성의 새 값을 설정한 후 이벤트가 발생 하는지 여부를 확인 하 여 테스트할 수 있습니다. 다음 코드 예제에서는 이러한 테스트를 보여 줍니다.

```csharp
[Fact]  
public async Task SettingOrderPropertyShouldRaisePropertyChanged()  
{  
    bool invoked = false;  
    var orderService = new OrderMockService();  
    var orderViewModel = new OrderDetailViewModel(orderService);  

    orderViewModel.PropertyChanged += (sender, e) =>  
    {  
        if (e.PropertyName.Equals("Order"))  
            invoked = true;  
    };  
    var order = await orderService.GetOrderAsync(1, GlobalSetting.Instance.AuthToken);  
    await orderViewModel.InitializeAsync(order);  

    Assert.True(invoked);  
}
```

이 단위 테스트는 `InitializeAsync` `OrderViewModel` 클래스의 메서드를 호출 하 여 해당 `Order` 속성을 업데이트 합니다. 속성에 대해 `PropertyChanged` 이벤트가 발생 하는 경우 단위 테스트가 통과 됩니다. `Order`

### <a name="testing-message-based-communication"></a>메시지 기반 통신 테스트

클래스를 [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) 사용 하 여 느슨하게 결합 된 클래스 간에 통신 하는 뷰 모델은 다음 코드 예제에서 보여 주는 것 처럼 테스트 중인 코드에서 보내는 메시지를 구독 하 여 단위 테스트를 수행할 수 있습니다.

```csharp
[Fact]  
public void AddCatalogItemCommandSendsAddProductMessageTest()  
{  
    bool messageReceived = false;  
    var catalogService = new CatalogMockService();  
    var catalogViewModel = new CatalogViewModel(catalogService);  

    Xamarin.Forms.MessagingCenter.Subscribe<CatalogViewModel, CatalogItem>(  
        this, MessageKeys.AddProduct, (sender, arg) =>  
    {  
        messageReceived = true;  
    });  
    catalogViewModel.AddCatalogItemCommand.Execute(null);  

    Assert.True(messageReceived);  
}
```

이 단위 테스트는에서 `CatalogViewModel` 실행 `AddCatalogItemCommand` 되는 `AddProduct` 에 대 한 응답으로 메시지를 게시 하는지 확인 합니다. 클래스가 멀티 캐스트 메시지 구독을 지원 하기 때문에 단위 테스트는 `AddProduct` 메시지를 구독 하 고 수신에 대 한 응답으로 콜백 대리자를 실행할 수 있습니다. [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) 람다 식으로 지정 되는이 콜백 대리자는 `boolean` `Assert` 문에서 테스트 동작을 확인 하는 데 사용 하는 필드를 설정 합니다.

### <a name="testing-exception-handling"></a>예외 처리 테스트

다음 코드 예제에서 보여 주는 것 처럼 잘못 된 작업 또는 입력에 대해 특정 예외가 throw 되었는지 확인 하는 단위 테스트를 작성할 수도 있습니다.

```csharp
[Fact]  
public void InvalidEventNameShouldThrowArgumentExceptionText()  
{  
    var behavior = new MockEventToCommandBehavior  
    {  
        EventName = "OnItemTapped"  
    };  
    var listView = new ListView();  

    Assert.Throws<ArgumentException>(() => listView.Behaviors.Add(behavior));  
}
```

컨트롤에 [`ListView`](xref:Xamarin.Forms.ListView) 라는 `OnItemTapped`이벤트가 없으므로이 단위 테스트에서 예외를 throw 합니다. 메서드는 `T` 가 예상 되는 예외의 형식인 제네릭 메서드입니다. `Assert.Throws<T>` `Assert.Throws<T>` 메서드에 전달 된 인수는 예외를 throw 하는 람다 식입니다. 따라서 람다 식에서을 throw `ArgumentException`하는 경우 단위 테스트가 전달 됩니다.

> [!TIP]
> 예외 메시지 문자열을 검사 하는 단위 테스트를 작성 하지 마십시오. 예외 메시지 문자열은 시간이 지남에 따라 변경 될 수 있으므로 현재 상태를 사용 하는 단위 테스트는 불안정로 간주 됩니다.

### <a name="testing-validation"></a>유효성 검사 테스트

유효성 검사 구현을 테스트 하는 데는 두 가지 측면이 있습니다. 유효성 검사 규칙이 올바르게 구현 되었는지 테스트 하 고 `ValidatableObject<T>` 클래스가 예상 대로 수행 되는지 테스트 합니다.

유효성 검사 논리는 일반적으로 출력이 입력에 따라 달라 지는 자체 포함 된 프로세스 이기 때문에 간단 하 게 테스트할 수 있습니다. 다음 코드 예제에서 보여 주는 것 처럼 하나 이상의 `Validate` 연결 된 유효성 검사 규칙을 포함 하는 각 속성에 대해 메서드를 호출 하는 결과에 대 한 테스트를 수행 해야 합니다.

```csharp
[Fact]  
public void CheckValidationPassesWhenBothPropertiesHaveDataTest()  
{  
    var mockViewModel = new MockViewModel();  
    mockViewModel.Forename.Value = "John";  
    mockViewModel.Surname.Value = "Smith";  

    bool isValid = mockViewModel.Validate();  

    Assert.True(isValid);  
}
```

이 단위 테스트는 `ValidatableObject<T>` `MockViewModel` 인스턴스의 두 속성 모두에 데이터가 있는 경우 유효성 검사가 성공 하는지 확인 합니다.

유효성 검사의 성공 여부를 확인 하는 것 외에도, 유효성 검사 단위 테스트 `Value`에서는 `IsValid`각 `ValidatableObject<T>` 인스턴스의 `Errors` , 및 속성 값을 검사 하 여 클래스가 예상 대로 수행 되는지 확인 해야 합니다. 다음 코드 예제에서는이 작업을 수행 하는 단위 테스트를 보여 줍니다.

```csharp
[Fact]  
public void CheckValidationFailsWhenOnlyForenameHasDataTest()  
{  
    var mockViewModel = new MockViewModel();  
    mockViewModel.Forename.Value = "John";  

    bool isValid = mockViewModel.Validate();  

    Assert.False(isValid);  
    Assert.NotNull(mockViewModel.Forename.Value);  
    Assert.Null(mockViewModel.Surname.Value);  
    Assert.True(mockViewModel.Forename.IsValid);  
    Assert.False(mockViewModel.Surname.IsValid);  
    Assert.Empty(mockViewModel.Forename.Errors);  
    Assert.NotEmpty(mockViewModel.Surname.Errors);  
}
```

이 단위 `Surname` 테스트는 `MockViewModel` 의 속성에 데이터가 `Value`없고 각 `IsValid` `ValidatableObject<T>` 인스턴스의, 및 `Errors` 속성이 올바르게 설정 된 경우 유효성 검사에 실패 하는지 확인 합니다.

## <a name="summary"></a>요약

단위 테스트는 응용 프로그램의 작은 단위 (일반적으로 메서드)를 사용 하 여 코드의 나머지 부분에서 분리 하 고 예상 대로 작동 하는지 확인 합니다. 이러한 목표는 각 기능 단위가 예상 대로 수행 되는지 확인 하 여 응용 프로그램 전체에서 오류가 전파 되지 않도록 하는 것입니다.

종속 개체를 종속 개체의 동작을 시뮬레이트하는 모의 개체로 바꿔서 테스트 중인 개체의 동작을 격리할 수 있습니다. 이렇게 하면 웹 서비스, 데이터베이스 등의 리소스를 사용 하지 않고도 단위 테스트를 실행할 수 있습니다.

MVVM 응용 프로그램에서 모델을 테스트 하 고 모델을 확인 하는 것은 다른 클래스를 테스트 하는 것과 동일 하며, 동일한 도구와 기법을 사용할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [다운로드 전자책 (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
