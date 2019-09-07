---
title: 종속성 주입
description: 이 장에서는 eShopOnContainers 모바일 앱에서 종속성 주입을 사용 하 여 이러한 형식에 종속 된 코드에서 구체적인 형식을 분리 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: a150f2d1-06f8-4aed-ab4e-7a847d69f103
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 72ffa4508f2c8f050f505313a28ce8278f2570b4
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760261"
---
# <a name="dependency-injection"></a>종속성 주입

일반적으로 클래스 생성자는 개체를 인스턴스화할 때 호출 되며, 개체가 필요로 하는 모든 값은 생성자에 인수로 전달 됩니다. 이는 종속성 주입의 한 예제 이며 특히 *생성자 주입*이라고 합니다. 개체에 필요한 종속성이 생성자에 삽입 됩니다.

종속성을 인터페이스 형식으로 지정 하 여 종속성 주입을 통해 이러한 형식에 종속 된 코드에서 구체적인 형식을 분리할 수 있습니다. 일반적으로 인터페이스와 추상 형식 간의 등록 및 매핑 목록과 이러한 형식을 구현 하거나 확장 하는 구체적인 형식을 포함 하는 컨테이너를 사용 합니다.

*속성 setter 삽입*및 *메서드 호출 삽입*과 같은 다른 유형의 종속성 주입도 있지만 일반적으로는 표시 되지 않습니다. 따라서이 챕터는 종속성 주입 컨테이너를 사용 하 여 생성자 삽입을 수행 하는 경우에만 중점을 둡니다.

<a name="introduction_to_dependency_injection" />

## <a name="introduction-to-dependency-injection"></a>종속성 주입 소개

종속성 주입은 IoC (반전 제어) 패턴의 특수화 된 버전입니다. 반전 하는 우려는 필요한 종속성을 가져오는 프로세스입니다. 종속성 주입을 사용 하는 경우 다른 클래스는 런타임에 개체에 종속성을 삽입 하는 일을 담당 합니다. 다음 코드 예제에서는 종속성 주입을 `ProfileViewModel` 사용할 때 클래스를 구성 하는 방법을 보여 줍니다.

```csharp
public class ProfileViewModel : ViewModelBase  
{  
    private IOrderService _orderService;  

    public ProfileViewModel(IOrderService orderService)  
    {  
        _orderService = orderService;  
    }  
    ...  
}
```

생성자 `ProfileViewModel` 는 다른 클래스 `IOrderService` 에 의해 삽입 된 인수로 인스턴스를 수신 합니다. 인터페이스 형식에는 `ProfileViewModel` 클래스의 유일한 종속성이 있습니다. 따라서 클래스는 `ProfileViewModel` `IOrderService` 개체의 인스턴스화를 담당 하는 클래스를 알지 못합니다. `IOrderService` 개체를 인스턴스화하고 `ProfileViewModel` 클래스에 삽입 하는 것을 담당 하는 클래스를 *종속성 주입 컨테이너*라고 합니다.

종속성 주입 컨테이너를 사용 하면 클래스 인스턴스를 인스턴스화하고 컨테이너의 구성에 따라 수명을 관리 하는 기능을 제공 하 여 개체 간의 결합을 줄일 수 있습니다. 개체를 만드는 동안 컨테이너는 개체에 필요한 모든 종속성을 삽입 합니다. 이러한 종속성을 아직 만들지 않은 경우 컨테이너는 먼저 종속성을 만들고 확인 합니다.

> [!NOTE]
> 팩터리를 사용 하 여 종속성 주입을 수동으로 구현할 수도 있습니다. 그러나 컨테이너를 사용 하면 수명 관리, 어셈블리 검색을 통한 등록 등의 추가 기능이 제공 됩니다.

종속성 주입 컨테이너를 사용 하는 경우 다음과 같은 몇 가지 이점이 있습니다.

- 컨테이너는 클래스가 해당 종속성을 찾고 수명을 관리할 필요가 없습니다.
- 컨테이너를 사용 하면 클래스에 영향을 주지 않고 구현 된 종속성을 매핑할 수 있습니다.
- 컨테이너는 종속성을 모의 수 있도록 하 여 테스트 용이성을 용이 하 게 합니다.
- 컨테이너는 새 클래스를 앱에 쉽게 추가할 수 있도록 하 여 유지 관리 효율성을 높입니다.

MVVM를 사용 하는 Xamarin.ios 앱의 컨텍스트에서 종속성 주입 컨테이너는 일반적으로 뷰 모델을 등록 하 고 해결 하는 데 사용 되며, 서비스를 등록 하 고이를 뷰 모델에 삽입 하는 데 사용 됩니다.

앱에서 뷰 모델 및 서비스 클래스의 인스턴스화를 관리 하기 위해 Autofac를 사용 하 여 eShopOnContainers 모바일 앱과 함께 사용할 수 있는 많은 종속성 주입 컨테이너가 있습니다. Autofac는 느슨하게 결합 된 앱 빌드를 용이 하 게 하 고, 형식 매핑 및 개체 인스턴스를 등록 하 고, 개체를 확인 하 고, 개체 수명을 관리 하 고, 삽입 하는 메서드를 포함 하 여 종속성 주입 컨테이너에서 일반적으로 발견 되 종속 개체를 확인 하는 개체의 생성자로 변환 합니다. Autofac에 대 한 자세한 내용은 readthedocs.io의 [Autofac](http://autofac.readthedocs.io/en/latest/index.html) 를 참조 하세요.

Autofac에서 인터페이스는 `IContainer` 종속성 주입 컨테이너를 제공 합니다. 그림 3-1에서는 `IOrderService` 개체를 인스턴스화하고 `ProfileViewModel` 클래스에 삽입 하는이 컨테이너를 사용할 때의 종속성을 보여 줍니다.

![](dependency-injection-images/dependencyinjection.png "종속성 주입을 사용 하는 경우의 종속성 예제")

**그림 3-1:** 종속성 주입을 사용 하는 경우의 종속성

런타임에 `IOrderService` `ProfileViewModel` 개체를 인스턴스화하려면 컨테이너에서 인스턴스화해야 하는 인터페이스의 구현을 알아야 합니다. 여기에는 다음이 포함 됩니다.

- 컨테이너는 인터페이스를 `IOrderService` 구현 하는 개체를 인스턴스화하는 방법을 결정 합니다. 이를 *등록*이라고 합니다.
- `IOrderService` 인터페이스를 구현 하는 개체 `ProfileViewModel` 및 개체를 인스턴스화하는 컨테이너입니다. 이를 *해결*이라고 합니다.

결국 응용 프로그램은 `ProfileViewModel` 개체를 사용 하 여 완료 되 고 가비지 수집에 사용할 수 있게 됩니다. 이 시점에서 다른 클래스가 동일한 인스턴스를 공유 하지 않는 `IOrderService` 경우 가비지 수집기는 인스턴스를 삭제 해야 합니다.

> [!TIP]
> 컨테이너 독립적인 코드를 작성 합니다. 항상 컨테이너 독립적인 코드를 작성 하 여 사용 중인 특정 종속성 컨테이너에서 앱을 분리 합니다.

## <a name="registration"></a>등록

종속성을 개체에 삽입 하려면 먼저 종속성의 형식을 컨테이너에 등록 해야 합니다. 일반적으로 형식을 등록 하려면 컨테이너에 인터페이스를 전달 하 고 인터페이스를 구현 하는 구체적인 형식을 전달 해야 합니다.

코드를 통해 컨테이너에 형식 및 개체를 등록 하는 방법에는 두 가지가 있습니다.

- 컨테이너에 형식 또는 매핑을 등록 합니다. 필요한 경우 컨테이너는 지정 된 형식의 인스턴스를 작성 합니다.
- 컨테이너의 기존 개체를 singleton으로 등록 합니다. 필요한 경우 컨테이너는 기존 개체에 대 한 참조를 반환 합니다.

> [!TIP]
> 종속성 주입 컨테이너가 항상 적절 한 것은 아닙니다. 종속성 주입에는 작은 앱에 적합 하지 않거나 유용 하지 않을 수 있는 추가 복잡성 및 요구 사항이 도입 되었습니다. 클래스에 종속성이 없거나 다른 형식에 대 한 종속성이 아닌 경우 컨테이너에 배치 하는 것이 적합 하지 않을 수 있습니다. 또한 클래스에는 형식에 대 한 정수 계열 종속성 집합이 하나 있지만 변경 되지 않는 경우 컨테이너에 배치 하는 것이 적합 하지 않을 수 있습니다.

종속성 주입이 필요한 형식의 등록은 앱의 단일 메서드에서 수행 해야 하며, 앱이 해당 클래스 간의 종속성을 인식 하도록 앱의 수명 주기 초기에이 메서드를 호출 해야 합니다. EShopOnContainers 모바일 앱에서이는 `ViewModelLocator` `IContainer` 개체를 빌드하고 해당 개체에 대 한 참조를 포함 하는 응용 프로그램의 유일한 클래스인 클래스에 의해 수행 됩니다. 다음 코드 예제에서는 eShopOnContainers 모바일 앱이 `IContainer` `ViewModelLocator` 클래스에서 개체를 선언 하는 방법을 보여 줍니다.

```csharp
private static IContainer _container;
```

형식 및 인스턴스는 `RegisterDependencies` `ViewModelLocator` 클래스의 메서드에 등록 됩니다. 이를 위해 `ContainerBuilder` 먼저 다음 코드 예제에서 보여 주는 인스턴스를 만듭니다.

```csharp
var builder = new ContainerBuilder();
```

그런 다음 형식과 인스턴스는 `ContainerBuilder` 개체에 등록 되며, 다음 코드 예제에서는 형식 등록의 가장 일반적인 형태를 보여 줍니다.

```csharp
builder.RegisterType<RequestProvider>().As<IRequestProvider>();
```

여기 `RegisterType` 에 표시 된 메서드는 인터페이스 형식을 구체적 형식에 매핑합니다. 생성자를 통해를 `RequestProvider` `IRequestProvider` 삽입 해야 하는 개체를 인스턴스화할 때 개체를 인스턴스화하기 위해 컨테이너에 지시 합니다.

구체적 형식은 다음 코드 예제와 같이 인터페이스 형식에서 매핑을 사용 하지 않고 직접 등록할 수도 있습니다.

```csharp
builder.RegisterType<ProfileViewModel>();
```

`ProfileViewModel` 형식이 확인 되 면 컨테이너는 필요한 종속성을 삽입 합니다.

Autofac를 사용 하면 컨테이너에서 형식의 singleton 인스턴스에 대 한 참조를 유지 관리 하는 인스턴스를 등록할 수도 있습니다. 예를 들어 다음 코드 예제에서는 `ProfileViewModel` 인스턴스에 `IOrderService` 인스턴스가 필요할 때 eShopOnContainers mobile 앱에서 사용할 구체적인 형식을 등록 하는 방법을 보여 줍니다.

```csharp
builder.RegisterType<OrderService>().As<IOrderService>().SingleInstance();
```

여기 `RegisterType` 에 표시 된 메서드는 인터페이스 형식을 구체적 형식에 매핑합니다. 메서드 `SingleInstance` 는 모든 종속 개체가 동일한 공유 인스턴스를 받도록 등록을 구성 합니다. 따라서 생성자를 `IOrderService` 통해를 `OrderService` 삽입 해야 하는 개체에서 공유 하는 단일 인스턴스만 컨테이너에 존재 합니다.

인스턴스 등록은 다음 코드 예제에서 보여 `RegisterInstance` 주는 메서드를 사용 하 여 수행할 수도 있습니다.

```csharp
builder.RegisterInstance(new OrderMockService()).As<IOrderService>();
```

여기 `RegisterInstance` 에 표시 된 메서드는 새 `OrderMockService` 인스턴스를 만들어 컨테이너에 등록 합니다. 따라서 생성자를 `IOrderService` 통해를 `OrderMockService` 삽입 해야 하는 개체에서 공유 하는 단일 인스턴스만 컨테이너에 존재 합니다.

형식 및 인스턴스 등록 `IContainer` 후에 다음 코드 예제에서 보여 주는 개체를 빌드해야 합니다.

```csharp
_container = builder.Build();
```

인스턴스에서`ContainerBuilder` 메서드 `Build` 를 호출 하면 생성 된 등록을 포함 하는 새 종속성 주입 컨테이너가 작성 됩니다.

> [!TIP]
> 을 변경할 `IContainer` 수 없는 것으로 간주 합니다. Autofac는 기존 컨테이너 `Update` 에서 등록을 업데이트 하는 메서드를 제공 하지만 가능 하면이 메서드를 호출 하지 않아야 합니다. 컨테이너를 만든 후, 특히 컨테이너를 사용 하는 경우에는 컨테이너를 수정 하는 데 위험이 있습니다. 자세한 내용은 readthedocs.io에서 변경할 수 없도록 [컨테이너 고려](http://docs.autofac.org/en/latest/best-practices/#consider-a-container-as-immutable) 를 참조 하세요.

<a name="resolution" />

## <a name="resolution"></a>해결 방법

형식을 등록 한 후에는 종속성으로 확인 하거나 삽입할 수 있습니다. 형식을 확인 하 고 컨테이너가 새 인스턴스를 만들어야 하는 경우 모든 종속성을 인스턴스에 삽입 합니다.

일반적으로 형식이 확인 되 면 다음 세 가지 중 하나가 발생 합니다.

1. 형식이 등록 되지 않은 경우 컨테이너는 예외를 throw 합니다.
1. 형식이 singleton으로 등록 된 경우 컨테이너는 singleton 인스턴스를 반환 합니다. 이 형식이에 대해 처음으로 호출 되 면 컨테이너는 필요한 경우이를 만들고 해당에 대 한 참조를 유지 관리 합니다.
1. 형식이 singleton으로 등록 되지 않은 경우 컨테이너는 새 인스턴스를 반환 하 고이에 대 한 참조를 유지 하지 않습니다.

다음 코드 예제에서는 이전에 autofac에 등록 된 `RequestProvider` 형식을 확인할 수 있는 방법을 보여 줍니다.

```csharp
var requestProvider = _container.Resolve<IRequestProvider>();
```

이 예제에서 autofac는 종속성과 함께 `IRequestProvider` 형식에 대 한 구체적인 형식을 확인 하도록 요청 됩니다. 일반적으로 메서드 `Resolve` 는 특정 형식의 인스턴스가 필요할 때 호출 됩니다. 확인 된 개체의 수명을 제어 하는 방법에 대 한 자세한 내용은 [확인 된 개체의 수명 관리](#managing_the_lifetime_of_resolved_objects)를 참조 하세요.

다음 코드 예제에서는 eShopOnContainers 모바일 앱이 뷰 모델 유형 및 해당 종속성을 인스턴스화하는 방법을 보여 줍니다.

```csharp
var viewModel = _container.Resolve(viewModelType);
```

이 예에서는 요청 된 뷰 모델에 대 한 뷰 모델 유형을 확인 하도록 Autofac를 요청 하 고, 컨테이너는 종속성도 확인 합니다. `ProfileViewModel` 형식을 확인할 때 확인할 `IOrderService` 종속성은 개체입니다. 따라서 autofac는 먼저 `OrderService` 개체를 생성 한 다음 `ProfileViewModel` 클래스의 생성자에 전달 합니다. EShopOnContainers 모바일 앱에서 모델을 생성 하 고 보기에 연결 하는 방법에 대 한 자세한 내용은 [뷰 모델 로케이터를 사용 하 여 자동으로 뷰 모델 만들기](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator)를 참조 하세요.

> [!NOTE]
> 컨테이너를 사용하여 형식을 등록하고 확인하는 경우, 특히 앱의 각 페이지 탐색에 대한 종속성을 다시 생성하는 경우 컨테이너의 리플렉션 사용으로 인해 성능비용이 발생합니다. 종속성이 많거나 깊은 경우에는 생성 비용이 많이 증가할 수 있습니다.

<a name="managing_the_lifetime_of_resolved_objects" />

## <a name="managing-the-lifetime-of-resolved-objects"></a>확인 된 개체의 수명 관리

형식을 등록 한 후 Autofac의 기본 동작은 형식이 확인 될 때마다 또는 종속성 메커니즘이 다른 클래스에 인스턴스를 삽입 하는 경우에 등록 된 형식의 새 인스턴스를 만드는 것입니다. 이 시나리오에서 컨테이너는 확인 된 개체에 대 한 참조를 보유 하지 않습니다. 그러나 인스턴스를 등록할 때 Autofac의 기본 동작은 개체의 수명을 단일 항목으로 관리 하는 것입니다. 따라서 인스턴스가 범위 내에 있는 동안 인스턴스는 범위 내에 유지 되 고, 컨테이너가 범위를 벗어나면 가비지가 수집 되 고, 코드에서 명시적으로 컨테이너를 삭제 하면 삭제 됩니다.

Autofac 인스턴스 범위를 사용 하 여 등록 된 형식에서 Autofac가 만드는 개체에 대 한 singleton 동작을 지정할 수 있습니다. Autofac 인스턴스 범위는 컨테이너에 의해 인스턴스화된 개체 수명을 관리 합니다. `RegisterType` 메서드의 기본 인스턴스 범위는 범위입니다.`InstancePerDependency` 그러나이 범위 `SingleInstance` 는 `RegisterType` 메서드와 함께 사용 될 수 있으므로 컨테이너는 `Resolve` 메서드를 호출할 때 형식의 singleton 인스턴스를 만들거나 반환 합니다. 다음 코드 예제에서는 autofac가 `NavigationService` 클래스의 단일 인스턴스를 만들도록 지시 하는 방법을 보여 줍니다.

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

`INavigationService` 인터페이스를 처음으로 확인할 때 컨테이너는 새 `NavigationService` 개체를 만들고 해당 개체에 대 한 참조를 유지 관리 합니다. 이후에 `INavigationService` 인터페이스를 확인 하는 경우 컨테이너는 이전에 만든 `NavigationService` 개체에 대 한 참조를 반환 합니다.

> [!NOTE]
> SingleInstance 범위는 컨테이너를 삭제할 때 생성 된 개체를 삭제 합니다.

Autofac는 추가 인스턴스 범위를 포함 합니다. 자세한 내용은 readthedocs.io의 [인스턴스 범위](http://autofac.readthedocs.io/en/latest/lifetime/instance-scope.html) 를 참조 하세요.

## <a name="summary"></a>요약

종속성 주입을 통해 이러한 형식에 종속 된 코드에서 구체적인 형식을 분리할 수 있습니다. 일반적으로 인터페이스와 추상 형식 간의 등록 및 매핑 목록과 이러한 형식을 구현 하거나 확장 하는 구체적인 형식을 포함 하는 컨테이너를 사용 합니다.

Autofac는 느슨하게 결합 된 앱 빌드를 용이 하 게 하 고, 형식 매핑 및 개체 인스턴스를 등록 하 고, 개체를 확인 하 고, 개체 수명을 관리 하 고, 삽입 하는 메서드를 포함 하 여 종속성 주입 컨테이너에서 일반적으로 발견 되 종속 개체를 확인 하는 개체의 생성자로 변환 합니다.

## <a name="related-links"></a>관련 링크

- [다운로드 전자책 (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
