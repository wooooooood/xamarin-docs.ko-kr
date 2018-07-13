---
title: 엔터프라이즈 앱을 테스트 하는 단위
description: 이 장에서 eShopOnContainers 모바일 앱에서 단위 테스트는 수행 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 4af82e52-f99b-4cad-b278-1745f190c240
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 02aeedd5498c47950e2fbc0d218de05bc0bb3204
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998685"
---
# <a name="unit-testing-enterprise-apps"></a>엔터프라이즈 앱을 테스트 하는 단위

모바일 앱 문제가 고유 데스크톱 및 웹 기반 응용 프로그램에 걱정할 필요가 없습니다. 모바일 사용자는 장치에 대 한 네트워크 연결에서 사용할 서비스의 가용성 및 다양 한 요인에 의해 달라 집니다. 따라서 모바일 앱의 품질, 안정성 및 성능을 개선 하기 위해 실제 환경에서 사용할 수는 테스트 되어야 합니다. 단위 테스트, 통합, 테스트 및 테스트, 단위 테스트에 대 한 가장 일반적인 형태의 중인 테스트를 사용 하 여 사용자 인터페이스를 포함 하 여 앱에서 수행 해야 하는 여러 종류의 테스트 있습니다.

단위 테스트 메서드를 일반적으로 앱의 작은 단위는 코드의 나머지 부분에서 격리 하며 예상 대로 작동 하는지 확인 합니다. 목표는 각 기능 단위를 수행 하는 예상 대로 앱 전체에서 오류가 전파 되지 않도록 확인 하는 것입니다. 버그 발생 위치를 검색 하는 것이 더 효율적 해당 보조 오류 지점에서 간접적으로 버그의 영향을 관찰 합니다.

단위 테스트는 소프트웨어 개발 워크플로의 핵심 사용 될 때 코드 품질에 큰 영향을 미칩니다. 메서드를 작성 하는 즉시 표준, 경계 및 잘못 된 입력된 데이터 사례에 대 한 응답에서 메서드를 확인의 동작을 코드에 의해 만들어진 모든 명시적 또는 암시적 가정을 확인 하는 단위 테스트를 작성 되어야 합니다. 또는 테스트 구동 방식 개발을 사용 하 여 단위 테스트는 코드 보다 먼저 기록 됩니다. 이 시나리오에서는 단위 테스트 기능 사양 및 디자인 설명서로 작동합니다.

> [!NOTE]
> 단위 테스트는 회귀 – 즉, 있지만 기능을 작동 하는 데에 잘못 된 업데이트 된 노력할 것에 매우 효과적입니다.

단위 테스트는 일반적으로 정렬 act assert 패턴을 사용합니다.

-   합니다 *정렬* 단위 테스트 메서드의 섹션 개체를 초기화 하 고 테스트 중인 메서드에 전달 되는 데이터의 값을 설정 합니다.
-   합니다 *작동* 섹션 필수 인수를 사용 하 여 테스트 중인 메서드를 호출 합니다.
-   합니다 *assert* 섹션은 테스트 중인 메서드의 작업이 예상한 대로 작동 하는지 확인 합니다.

이 패턴을 따르는 단위 테스트는 읽을 수 있고 일관성을 보장 합니다.

## <a name="dependency-injection-and-unit-testing"></a>종속성 주입 및 단위 테스트

느슨하게 결합 된 아키텍처를 채택 해야 하는 동기 중 하나는 단위 테스트를 용이 하 게 합니다. Autofac을 사용 하 여 등록 된 형식 중 하나는 `OrderService` 클래스입니다. 다음 코드 예제에서는이 클래스의 개요를 보여 줍니다.

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

`OrderDetailViewModel` 클래스에 종속 되어 합니다 `IOrderService` 컨테이너를 인스턴스화할 때를 확인 하는 입력을 `OrderDetailViewModel` 개체입니다. 그러나 작성 하기 보다는 `OrderService` 단위 테스트에는 개체를 `OrderDetailViewModel` 클래스 대신 대체는 `OrderService` 테스트 목적으로 mock 사용 하 여 개체입니다. 그림 10-1에서는이 관계를 보여 줍니다.

![](unit-testing-images/unittesting.png "IOrderService 인터페이스를 구현 하는 클래스")

**그림 10-1:** IOrderService 인터페이스를 구현 하는 클래스

이 방법을 사용 하면를 `OrderService` 에 전달 될 개체를 `OrderDetailViewModel` 수 있도록 테스트 용이성의 관심에 런타임 시 클래스를 `OrderMockService` 에 전달 될 클래스를 `OrderDetailViewModel` 테스트 시 클래스. 이 방법의 주요 이점은 웹 서비스 또는 데이터베이스와 같은 리소스를 다루기를 요구 하지 않고 실행할 단위 테스트 하는 것입니다.

## <a name="testing-mvvm-applications"></a>MVVM 응용 프로그램 테스트

모델 및 MVVM 응용 프로그램에서 뷰 모델을 테스트 하는 것은 다른 클래스를 테스트와 동일 하 고 동일한 도구 및 기술을-단위 테스트와 같이 등을 사용할 수 있습니다. 그러나 몇 가지 일반적인 모델 패턴 및 특정 단위 테스트 기술을 활용할 수 있는 보기 모델 클래스입니다.

> [!TIP]
> 각 단위 테스트를 사용 하 여 한 가지를 테스트 합니다. 단위 테스트 연습 장치의 동작의 측면 둘 중 하나를 확인 하 고 싶은 생각이 들 수 있습니다. 이렇게 하면 읽기 및 업데이트 하기 어려운 테스트 합니다. 오류를 해석 하는 경우 혼란이 발생할 수도 있습니다.

모바일 앱에서 사용 하는 eShopOnContainers [xUnit](https://xunit.github.io/) 는 두 가지 유형의 단위 테스트는 단위 테스트를 수행 하려면:

-   팩트는 true 이면 항상 테스트가 고정 조건을 테스트 하는 합니다.
-   이론은 테스트 데이터의 특정 집합에입니다.

EShopOnContainers 모바일 앱에 포함 된 단위 테스트는 팩트 테스트 하며 따라서 각 단위 테스트 메서드는 `[Fact]` 특성입니다.

> [!NOTE]
> xUnit 테스트는 test runner에서 실행 됩니다. Test runner를 실행 하려면 필요한 플랫폼에 대 한 eShopOnContainers.TestRunner 프로젝트를 실행 합니다.

### <a name="testing-asynchronous-functionality"></a>비동기 기능 테스트

MVVM 패턴을 구현 하는 경우 모델 보기 일반적으로 서비스에 대 한 작업 종종 비동기적으로 호출 합니다. 일반적으로 이러한 작업을 호출 하는 코드에 대 한 테스트는 실제 서비스에 대 한 대체도 모의 개체를 사용 합니다. 다음 코드 예제에서는 보기 모델에 모의 서비스를 전달 하 여 비동기 기능을 테스트 하는 방법을 보여 줍니다.

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

이 단위 테스트를 확인 하는 `Order` 의 속성을 `OrderDetailViewModel` 인스턴스 후 값이 포함 됩니다는 `InitializeAsync` 메서드를 호출 했습니다. `InitializeAsync` 메서드는 보기 모델의 해당 뷰를 탐색 하는 경우 호출 됩니다. 탐색에 대 한 자세한 내용은 참조 하세요. [탐색](~/xamarin-forms/enterprise-application-patterns/navigation.md)합니다.

경우는 `OrderDetailViewModel` 인스턴스를 만든 것으로 예상는 `OrderService` 인스턴스를 인수로 지정 해야 합니다. 그러나는 `OrderService` 웹 서비스에서 데이터를 검색 합니다. 따라서를 `OrderMockService` 인스턴스는 모의 버전의를 `OrderService` 클래스에 대 한 인수로 지정 된는 `OrderDetailViewModel` 생성자입니다. 그런 다음 경우 보기 모델의 `InitializeAsync` 메서드를 호출 하면 호출 하는 `IOrderService` 작업 모의 데이터를 검색 하지 않고 웹 서비스와 통신 합니다.

### <a name="testing-inotifypropertychanged-implementations"></a>INotifyPropertyChanged 구현 테스트

구현 된 `INotifyPropertyChanged` 인터페이스 보기에서 발생 하는 변경에 반응 하는 뷰를 사용 하면 모델 및 모델입니다. 이러한 변경 내용을 컨트롤에 표시 된 데이터에 제한 되지 않습니다.-애니메이션 시작 또는 사용 하지 않도록 설정할 컨트롤 발생 하는 보기 모델 상태와 같은 보기를 제어할 수도 있습니다.

이벤트 처리기를 연결 하 여 단위 테스트에서 직접 업데이트할 수 있는 속성을 테스트할 수 있습니다는 `PropertyChanged` 이벤트 및 속성에 대 한 새 값을 설정한 후 이벤트를 발생시킬지 여부를 확인 합니다. 다음 코드 예제에는 이러한 테스트 보여 줍니다.

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

이 단위 테스트를 호출 하는 `InitializeAsync` 메서드를 `OrderViewModel` 클래스를 유발 하는 해당 `Order` 업데이트할 속성. 제공 하는 단위 테스트는 성공 합니다 `PropertyChanged` 에 대 한 이벤트가 발생 합니다 `Order` 속성.

### <a name="testing-message-based-communication"></a>메시지 기반 통신 테스트

보기에 사용 하는 모델을 [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) 느슨하게 결합 된 클래스 간의 단위 테스트할 수 다음 코드 예제에서 설명한 것 처럼 테스트 중인 코드에 의해 전송 되는 메시지를 구독 하 여 통신 하는 클래스:

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

이 단위 테스트 검사를 `CatalogViewModel` 게시를 `AddProduct` 대 한 응답으로 메시지 해당 `AddCatalogItemCommand` 실행 중인 합니다. 때문에 합니다 [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) 멀티 캐스트 메시지 구독을 지원 하는 클래스, 단위 테스트를 구독할 수 있습니다는 `AddProduct` 메시지 및 응답 수신으로 콜백 대리자를 실행 합니다. 이 콜백 대리자, 람다 식으로 지정 된 설정를 `boolean` 에서 사용 되는 필드는 `Assert` 테스트의 동작을 확인 하는 문입니다.

### <a name="testing-exception-handling"></a>예외 처리를 테스트합니다.

단위 테스트도 쓸 수 잘못 된 동작 또는 입력에 대 한 특정 예외가 throw 되는 확인 다음 코드 예제에서 설명한 것 처럼:

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

이 단위 테스트 하기 때문에 예외를 throw 합니다는 [ `ListView` ](xref:Xamarin.Forms.ListView) 컨트롤에 라는 이벤트가 없는 `OnItemTapped`합니다. 합니다 `Assert.Throws<T>` 메서드는 제네릭 메서드가 있는 `T` 예상 되는 예외의 형식입니다. 전달 된 인수를 `Assert.Throws<T>` 메서드는 예외를 throw 하는 람다 식입니다. 람다 식에서 발생 된 단위 테스트는 성공 하는 따라서는 `ArgumentException`합니다.

>💡 **팁**: 예외 메시지 문자열을 검사 하는 단위 테스트 작성을 방지 합니다. 예외 메시지 문자열은 시간이 지남에 따라 변경 될 수 있습니다 및 현재 상태에 의존 하는 단위 테스트 불안정으로 간주 하므로 합니다.

### <a name="testing-validation"></a>유효성 검사 테스트

다음 두 가지 유효성 검사 구현 테스트:는 유효성 검사 규칙은 올바르게 구현, 테스트 및 테스트 하는 `ValidatableObject<T>` 클래스 예상 대로 수행 합니다.

유효성 검사 논리를 테스트 하려면 일반적으로 간단한 이므로 작업은 일반적으로 자체 포함 된 과정 출력 입력에 따라 달라 집니다. 호출의 결과에 테스트 해야 합니다.는 `Validate` 메서드에 다음 코드 예제에 설명 된 대로 연결된 유효성 검사 규칙이 하나 이상 있는 각 속성:

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

이 단위 테스트 유효성 검사에 성공한 경우는 확인 두 `ValidatableObject<T>` 속성에는 `MockViewModel` 인스턴스는 모두 데이터를 포함 합니다.

유효성 검사에 성공 했는지를 검사 및 유효성 검사 단위 테스트도 확인 해야의 값을 `Value`, `IsValid`, 및 `Errors` 의 각 속성 `ValidatableObject<T>` 클래스 예상 대로 수행 하는 확인 하려면 인스턴스. 다음 코드 예제에서는이 작업을 수행 하는 단위 테스트를 보여 줍니다.

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

이 단위 테스트 유효성 검사에 실패할 때를 확인 합니다 `Surname` 의 속성을 `MockViewModel` 모든 데이터가 없습니다. 및 `Value`, `IsValid`, 및 `Errors` 각 속성 `ValidatableObject<T>` 인스턴스 올바르게 설정 합니다.

## <a name="summary"></a>요약

단위 테스트 메서드를 일반적으로 앱의 작은 단위는 코드의 나머지 부분에서 격리 하며 예상 대로 작동 하는지 확인 합니다. 목표는 각 기능 단위를 수행 하는 예상 대로 앱 전체에서 오류가 전파 되지 않도록 확인 하는 것입니다.

종속 개체의 동작을 시뮬레이션 하는 모의 개체를 사용 하 여 종속 개체를 대체 하 여 테스트 중인 개체의 동작을 격리 될 수 있습니다. 따라서 단위 테스트를 웹 서비스 또는 데이터베이스와 같은 리소스를 다루기를 요구 하지 않고 실행할 수 있습니다.

모델 및 MVVM 응용 프로그램에서 뷰 모델을 테스트 하는 것은 다른 클래스를 테스트와 동일 하 고 동일한 도구 및 기술을 사용할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [2mb PDF 전자책 다운로드](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
