---
title: 종속성 주입
ms.prod: xamarin
ms.assetid: a150f2d1-06f8-4aed-ab4e-7a847d69f103
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 8db8e5b756fe770bdf292ec03c28eb5ed54acf9e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="dependency-injection"></a>종속성 주입

일반적으로 개체를 인스턴스화할 때 클래스 생성자가 호출 됩니다 및 개체가 필요한 모든 값은 생성자에 인수로 전달 됩니다. 종속성 주입의 예 이며 특별히 라고 *생성자 삽입*합니다. 개체 종속성은 생성자에 삽입 합니다.

종속성 주입 종속성을 인터페이스 형식으로 지정 하 여 이러한 형식에 의존 하는 코드에서 구체적인 형식의 분리할 수 있습니다. 일반적으로 구현 하거나 이러한 형식을 확장 하는 구체적인 형식 고 등록과 인터페이스 및 추상 형식 간의 매핑을의 목록을 보유 하는 컨테이너를 사용 합니다.

밖에도 다른 종류의 종속성 주입와 같은 *속성 setter 삽입*, 및 *메서드 호출 삽입*, 일반적으로 표시 되며 하지만 합니다. 따라서이 장의 중점적으로 전적으로 종속성 주입 컨테이너와 생성자 삽입을 수행 합니다.

<a name="introduction_to_dependency_injection" />

## <a name="introduction-to-dependency-injection"></a>종속성 주입 소개

종속성 주입 여기서 반전 되 문제는 필수 종속성을 가져오는 과정 Inversion of Control (IoC) 패턴의 특수 버전입니다. 종속성 주입와 다른 클래스는 런타임에 개체에 종속성을 삽입 하는 일을 담당 합니다. 다음 코드 예제는 방법을 `ProfileViewModel` 종속성 주입을 사용 하는 경우 구조 클래스:

```csharp
public class ProfileViewModel : ViewModelBase  
{  
    private IOrderService _orderService;  

    public ProfileViewModel(IOrderService orderService)  
    {  
        _orderService = orderService;  
    }  
    ...  
}
```

`ProfileViewModel` 생성자 받습니다는 `IOrderService` 인스턴스 다른 클래스에 의해 삽입 인수로 합니다. 에 있는 유일한 종속성의 `ProfileViewModel` 클래스는 인터페이스 형식에 있습니다. 따라서는 `ProfileViewModel` 클래스 인스턴스화를 담당 하는 클래스에 대 한 지식이 없는 경우는 `IOrderService` 개체입니다. 인스턴스화를 담당 하는 클래스는 `IOrderService` 개체를 삽입 하기에 `ProfileViewModel` 클래스, 라고는 *종속성 주입 컨테이너*합니다.

종속성 주입 컨테이너를 클래스 인스턴스를 인스턴스화할 때 컨테이너의 구성에 따라 해당 수명을 관리 하는 기능을 제공 하 여 개체 간의 결합을 줄입니다. 개체를 만드는 동안 컨테이너에는 개체에 필요한 모든 종속성을 삽입 합니다. 이러한 종속성 아직 만들지 않은, 컨테이너 만들고 먼저 해당 종속성을 해결 합니다.

> [!NOTE]
> 종속성 주입 팩터리를 사용 하 여 수동으로 구현할 수 있습니다. 그러나 컨테이너를 사용 하 여 수명 관리 및 검색 하는 어셈블리를 통해 등록 같은 추가 기능을 제공 합니다.

종속성 주입 컨테이너를 사용 하 여 여러 가지 이점이 있습니다.

-   컨테이너의 종속성을 찾고의 수명을 관리 하는 클래스에 대 한 필요성을 제거 합니다.
-   컨테이너는 클래스에 영향을 주지 않고 구현 된 종속성 매핑 수 있습니다.
-   컨테이너는 모의 종속성을 허용 하 여 테스트 가능성을 지원 합니다.
-   컨테이너 응용 프로그램에 쉽게 추가할 수 하는 새 클래스를 허용 하 여 유지 관리 용이성을 증가 합니다.

MVVM를 사용 하는 Xamarin.Forms 앱의 컨텍스트에서 종속성 주입 컨테이너 일반적으로 사용될지를 등록 하 고 모델 보기를 확인 하 고 모델 보기에 삽입 하 고 서비스 등록에 대 한 합니다.

Autofac 뷰 모델의 인스턴스화를 관리 하 고 서비스 응용 프로그램에서 클래스를 사용 하 여 eShopOnContainers 모바일 앱 사용 가능한 많은 종속성 주입 컨테이너 있습니다. Autofac 느슨하게 연결 된 앱 개발을 용이 하 게 하 고 해결 개체, 개체 수명 관리 및 삽입의 모든 형식 매핑 및 개체 인스턴스를 등록 하는 메서드를 비롯 한 종속성 주입 컨테이너에서 흔히 발견 기능 제공 해결 하는 개체의 생성자에 종속 된 개체입니다. Autofac에 대 한 자세한 내용은 참조 [Autofac](http://autofac.readthedocs.io/en/latest/index.html) readthedocs.io에 있습니다.

Autofac에 `IContainer` 인터페이스는 종속성 주입 컨테이너를 제공 합니다. 그림 3-1 인스턴스화하는이 컨테이너를 사용 하는 경우 종속성을 표시는 `IOrderService` 개체를 삽입 합니다.에 `ProfileViewModel` 클래스입니다.

![](dependency-injection-images/dependencyinjection.png "종속성 주입을 사용 하는 경우 종속성 예")

**그림 3-1:** 종속성 종속성 주입을 사용 하는 경우

런타임 시 컨테이너의 어떤 구현을 알고 있어야는 `IOrderService` 것 인스턴스화해야를 인스턴스화할 수 전에 인터페이스는 `ProfileViewModel` 개체입니다. 여기에 다음이 포함 됩니다.

-   구현 하는 개체를 인스턴스화하는 방법을 결정 하는 컨테이너는 `IOrderService` 인터페이스입니다. 로 알려져 *등록*합니다.
-   컨테이너를 구현 하는 개체를 인스턴스화하는 `IOrderService` 인터페이스 및 `ProfileViewModel` 개체입니다. 로 알려져 *해상도*합니다.

결국 사용 하 여 응용 프로그램은 완료는 `ProfileViewModel` 개체 가비지 수집을 사용할 수 있게 됩니다. 가비지 수집기는 삭제 해야이 시점에서 `IOrderService` 다른 클래스는 동일한 인스턴스를 공유 하지 않는 경우 인스턴스.

> [!TIP]
> 컨테이너를 알 수 없는 코드를 작성 합니다. 항상 사용 하 고 특정 종속성 컨테이너에서 응용 프로그램을 분리 하는 컨테이너를 알 수 없는 코드를 작성 하려고 합니다.

## <a name="registration"></a>등록

종속성 개체에 삽입할 수 있습니다, 전에 종속성 유형의 컨테이너와 등록 해야 합니다. 일반적으로 형식을 등록 인터페이스와 인터페이스를 구현 하는 구체적인 형식 컨테이너를 전달 하는 것입니다.

코드를 통해 컨테이너의 형식 및 개체를 등록 하는 방법은 두 가지가 있습니다.

-   컨테이너와 형식이 나 매핑을 등록 합니다. 필요한 경우 컨테이너는 지정 된 형식의 인스턴스를 작성 합니다.
-   단일 항목으로 컨테이너의 기존 개체를 등록 합니다. 필요한 경우 컨테이너는 기존 개체에 대 한 참조를 반환 합니다.

> [!TIP]
> 종속성 주입 컨테이너 항상 적합 하지 않습니다. 종속성 주입 복잡성이 추가 및 하지 않을 수 있는 적절 한 하거나 유용한을 작은 응용 프로그램 요구 사항을 소개 합니다. 클래스에 모든 종속성이 없는 없거나 없으면 다른 형식에 대 한 종속성을 좋을 수 있습니다 하지 컨테이너에 배치 하 합니다. 또한 클래스에는 단일 정수 계열 형식에는 종속성을 하 고 변경 되지 않을 것임, 컨테이너에 배치 하 라는 있는 적합할 수 있습니다.

응용 프로그램에서 단일 메서드에 종속성 주입 해야 하는 형식의 등록을 수행 해야 하 고 앱이 해당 클래스 간의 종속성을 확인 하도록 응용 프로그램의 수명 주기 초반에이 메서드를 호출 해야 합니다. EShopOnContainers 모바일 앱에 의해 수행 됩니다이 `ViewModelLocator` 빌드는 클래스는 `IContainer` 개체 및 해당 개체에 대 한 참조를 보유 하는 앱에서 유일한 클래스입니다. 다음 코드 예제에서는 eShopOnContainers 모바일 앱 선언 하는 방법을 보여 줍니다.는 `IContainer` 개체는 `ViewModelLocator` 클래스:

```csharp
private static IContainer _container;
```

에 등록 된 형식 및 인스턴스는 `RegisterDependencies` 에서 메서드는 `ViewModelLocator` 클래스입니다. 먼저 만들어 이렇게는 `ContainerBuilder` 다음 코드 예제에서 설명 하는 인스턴스:

```csharp
var builder = new ContainerBuilder();
```

유형 및 인스턴스 다음 등록 된는 `ContainerBuilder` 개체 및 다음 코드 예제에서는 형식 등록의 가장 일반적인 형태를 보여 줍니다.

```csharp
builder.RegisterType<RequestProvider>().As<IRequestProvider>();
```

`RegisterType` 여기에 표시 된 메서드는 구체적인 형식이 인터페이스 형식을 매핑합니다. 인스턴스화하는 컨테이너 인지는 `RequestProvider` 개체의 삽입을 필요로 하는 개체를 인스턴스화할 때는 `IRequestProvider` 생성자를 통해서만 합니다.

다음 코드 예제에 표시 된 대로 구체적인 형식은 인터페이스 형식에서 매핑 없이 직접 등록할 수도 있습니다.

```csharp
builder.RegisterType<ProfileViewModel>();
```

경우는 `ProfileViewModel` 유형 해결 됨, 컨테이너는 필요한 종속성 주입 됩니다.

또한 Autofac 컨테이너인 형식의 singleton 인스턴스에 대 한 참조를 유지 관리를 담당 인스턴스 등록이 있습니다. 예를 들어 다음 코드 예제에서는 eShopOnContainers 모바일 앱 때 사용 하는 구체적 유형은 등록 하는 방법을 표시는 `ProfileViewModel` 인스턴스에 필요는 `IOrderService` 인스턴스:

```csharp
builder.RegisterType<OrderService>().As<IOrderService>().SingleInstance();
```

`RegisterType` 여기에 표시 된 메서드는 구체적인 형식이 인터페이스 형식을 매핑합니다. `SingleInstance` 메서드 모든 종속 개체가 동일한 공유 인스턴스를 받을 수 있도록 등록을 구성 합니다. 따라서 단일 `OrderService` 의 삽입을 필요로 하는 개체에서 공유 컨테이너에 인스턴스는 존재는 `IOrderService` 생성자를 통해서만 합니다.

인스턴스를 등록으로도 수행할 수는 `RegisterInstance` 다음 코드 예제에서 설명 하는 메서드:

```csharp
builder.RegisterInstance(new OrderMockService()).As<IOrderService>();
```

`RegisterInstance` 여기에 표시 된 메서드를 새로 만들고 `OrderMockService` 인스턴스 및 컨테이너를 등록 합니다. 따라서 단일 `OrderMockService` 인스턴스가의 삽입을 필요로 하는 개체에서 공유 컨테이너에 있는 한 `IOrderService` 생성자를 통해서만 합니다.

유형 및 인스턴스 등록은 `IContainer` 다음 코드 예제에 나와 있는 개체를 빌드해야 합니다.

```csharp
_container = builder.Build();
```

호출 하는 `Build` 메서드를는 `ContainerBuilder` 인스턴스에서 적용 된 등록을 포함 하는 새 종속성 주입 컨테이너를 작성 합니다.

>💡 **팁**: 고려는 `IContainer` 변경할 수 있는 것으로 합니다. Autofac 제공 하지만 한 `Update` 가능한 경우이 메서드를 호출 하는 기존 컨테이너에 대 한 등록을 업데이트 하는 방법을 피해 야 합니다. 컨테이너를 사용 하는 경우에 특히 작성 된 후 컨테이너를 수정 하는 위험이 있습니다. 자세한 내용은 참조 [불변와 컨테이너는 것이 좋습니다](http://docs.autofac.org/en/latest/best-practices/#consider-a-container-as-immutable) readthedocs.io에 있습니다.

<a name="resolution" />

## <a name="resolution"></a>해결

형식을 등록 한 후 해결 또는 종속성으로 삽입할 수 있습니다. 형식을 확인 하는 경우 새 인스턴스를 만드는 데 필요한 컨테이너 인스턴스로 종속성 삽입 합니다.

일반적으로 형식 확인 되 면 다음 중 하나의 상황이 발생 합니다.

1.  형식, 등록 되지 않은 예외를 throw 하는 컨테이너입니다.
1.  형식을 단일 항목으로 등록 하는 경우 컨테이너 singleton 인스턴스를 반환 합니다. 처음으로 유형이 호출 될 경우 컨테이너, 필요한 경우 만들고 유지 관리에 대 한 참조입니다.
1.  유형을 단일 항목으로 등록 되지 않은, 컨테이너 새 인스턴스를 반환 하 고 그에 대 한 참조를 유지 하지 않습니다.

다음 코드 예제는 방법을 `RequestProvider` Autofac에 이전에 등록 되었던 형식을 확인할 수 있습니다.

```csharp
var requestProvider = _container.Resolve<IRequestProvider>();
```

이 예제에서는 Autofac에 대 한 구체적인 형식을 확인 하 라는 메시지가 표시는 `IRequestProvider` 모든 종속성과 함께 유형입니다. 일반적으로 `Resolve` 메서드는 특정 형식의 인스턴스가 필요 합니다. 확인 된 개체의 수명을 제어 하는 방법에 대 한 정보를 참조 하십시오. [수명의 해결 개체 관리](#managing_the_lifetime_of_resolved_objects)합니다.

다음 코드 예제에서는 eShopOnContainers 모바일 앱 보기 모델 유형 및 해당 종속성을 인스턴스화합니다 하는 방법을 보여 줍니다.

```csharp
var viewModel = _container.Resolve(viewModelType);
```

이 예에서 Autofac는 요청한 보기 모델에 대 한 보기 모델 형식을 확인 하 라는 메시지가 표시 하 고 컨테이너 종속성 확인 하기도 합니다. 확인할 때는 `ProfileViewModel` 를 해결 하려면 종속성 형식이 `IOrderService` 개체입니다. 따라서 Autofac 생성 먼저는 `OrderService` 개체를 다음의 생성자에 전달는 `ProfileViewModel` 클래스입니다. EShopOnContainers 모바일 앱에서 보기를 생성 하는 방법에 대 한 자세한 내용은 모델 보기에 연결에 대 한 참조 [보기 모델 로케이터에 뷰 모델을 자동으로 만드는](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator)합니다.

> [!NOTE]
> 등록 및 컨테이너와 형식 확인 기능은 성능을 컨테이너의 각 형식을 만들기 위한 리플렉션 사용으로 인해 앱에서 각 페이지 탐색에 대 한 종속성 다시 구성 해야 하는 경우에 특히 합니다. 많은 또는 깊은 종속성이 있는 생성 비용이 크게 증가할 수 있습니다.

<a name="managing_the_lifetime_of_resolved_objects" />

## <a name="managing-the-lifetime-of-resolved-objects"></a>확인 된 개체의 수명을 관리합니다.

등록 한 후 형식, Autofac에 대 한 기본 동작은는 등록 된 형식의 새 인스턴스를 될 때마다를 만들려면 형식을 확인 또는 종속성 메커니즘 다른 클래스에 인스턴스를 삽입 하는 경우입니다. 이 시나리오에서는 컨테이너 확인 된 개체에 대 한 참조를 보유 하지 않습니다. 그러나 인스턴스를 등록할 때 Autofac에 대 한 기본 동작 단일 항목으로 개체의 수명을 관리 하는 합니다. 따라서 인스턴스 컨테이너 범위에가 컨테이너 범위를 벗어날 때 가비지 수집, 삭제 하는 동안 또는 코드는 컨테이너를 명시적으로 삭제 하는 경우 범위에 유지 됩니다.

등록 된 형식에서 Autofac 만들어지는 개체에 대 한 단일 동작을 지정 하 Autofac 인스턴스 범위를 사용할 수 있습니다. Autofac 인스턴스 범위 컨테이너에 의해 인스턴스화될 개체 수명 관리 합니다. 기본 인스턴스 범위에서 `RegisterType` 방법은 `InstancePerDependency` 범위입니다. 그러나는 `SingleInstance` 범위와 함께 사용할 수는 `RegisterType` 메서드, 컨테이너를 만들거나를 호출할 때 형식의 singleton 인스턴스를 반환 하는 `Resolve` 메서드. 다음 코드 예제에서는의 singleton 인스턴스를 만드는 Autofac가 지시 하는 방법을 보여 줍니다.는 `NavigationService` 클래스:

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

처음으로는 `INavigationService` 인터페이스 해결 됨, 컨테이너를 새로 만들고 `NavigationService` 개체 하 고 그에 대 한 참조를 유지 관리 합니다. 모든 후속 해상도 `INavigationService` 인터페이스를 컨테이너에 대 한 참조를 반환 합니다는 `NavigationService` 이전에 만든 개체입니다.

> [!NOTE]
> SingleInstance 범위 컨테이너를 삭제할 때 만든된 개체를 삭제 합니다.

Autofac 추가 인스턴스 범위를 포함합니다. 자세한 내용은 참조 [인스턴스 범위](http://autofac.readthedocs.io/en/latest/lifetime/instance-scope.html) readthedocs.io에 있습니다.

## <a name="summary"></a>요약

종속성 주입 이러한 형식에 의존 하는 코드에서 구체적인 형식을 분리할 수 있습니다. 일반적으로 구현 하거나 이러한 형식을 확장 하는 구체적인 형식 고 등록과 인터페이스 및 추상 형식 간의 매핑을의 목록을 보유 하는 컨테이너를 사용 합니다.

Autofac 느슨하게 연결 된 앱 개발을 용이 하 게 하 고 해결 개체, 개체 수명 관리 및 삽입의 모든 형식 매핑 및 개체 인스턴스를 등록 하는 메서드를 비롯 한 종속성 주입 컨테이너에서 흔히 발견 기능 제공 확인할 개체의 생성자에 종속 된 개체입니다.


## <a name="related-links"></a>관련 링크

- [EBook (2mb PDF)를 다운로드 합니다.](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
