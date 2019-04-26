---
title: 종속성 주입
description: 이 장에서 eShopOnContainers 모바일 앱을 구체적인 형식을 이러한 형식에 의존 하는 코드에서 분리 하려면 종속성 주입을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: a150f2d1-06f8-4aed-ab4e-7a847d69f103
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: fb225349b9ffb1c950486a817897b3c26c6ffbe4
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61300263"
---
# <a name="dependency-injection"></a>종속성 주입

일반적으로 개체를 인스턴스화할 때 클래스 생성자가 호출 하 고 개체는 모든 값은 생성자에 인수로 전달 됩니다. 종속성 주입의 예제 이며 특히 이라고 *생성자 주입*합니다. 개체 종속성은 생성자에 삽입 됩니다.

종속성을 인터페이스 형식으로 지정 하 여 종속성 주입 구체적인 형식을 이러한 형식에 의존 하는 코드에서 분리할 수 있습니다. 일반적으로 등록 및 인터페이스 및 추상 형식 간의 매핑 목록을 보유 하는 컨테이너 및 구현 하거나 이러한 형식을 확장 하는 구체적인 형식을 사용 합니다.

가지도 다른 종속성 주입을 같은 *속성 setter 주입*, 및 *메서드 호출 삽입*, 하지만 일반적으로 표시 될 때입니다. 따라서이 장에서 데만 집중 종속성 주입 컨테이너를 사용 하 여 생성자 주입을 수행 합니다.

<a name="introduction_to_dependency_injection" />

## <a name="introduction-to-dependency-injection"></a>종속성 주입 소개

종속성 주입은 여기서 반전 되는 문제는 필요한 종속성을 얻는 프로세스는 제어 반전 (IoC) 패턴의 특수 버전입니다. 종속성 주입을 사용 하 여 다른 클래스는 런타임에 개체에 종속성을 삽입 하는 일을 담당 합니다. 다음 코드 예제에서는 방법을 `ProfileViewModel` 클래스는 종속성 주입을 사용 하는 경우 구조화 됩니다.

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

합니다 `ProfileViewModel` 생성자를 받습니다는 `IOrderService` 다른 클래스에 의해 삽입 된 인수로 인스턴스. 에 종속 된 유일한는 `ProfileViewModel` 클래스는 인터페이스 형식에서입니다. 따라서 합니다 `ProfileViewModel` 클래스 인스턴스화를 담당 하는 클래스에 대 한 지식이 없는 `IOrderService` 개체입니다. 인스턴스화를 담당 하는 클래스를 `IOrderService` 개체에 삽입 하는 `ProfileViewModel` 클래스, 라고 합니다 *종속성 주입 컨테이너*합니다.

종속성 주입 컨테이너 클래스 인스턴스를 인스턴스화하고 컨테이너의 구성에 따라 해당 수명을 관리 하는 기능을 제공 하 여 개체 간의 결합을 줄입니다. 개체를 만드는 동안 컨테이너에 있는 개체에 필요한 모든 종속성을 삽입 합니다. 이러한 종속성이 아직 만들지 않은 경우, 컨테이너를 만들고 먼저 해당 종속성을 확인 합니다.

> [!NOTE]
> 종속성 주입 팩터리를 사용 하 여 수동으로 구현할 수 있습니다. 그러나 컨테이너를 사용 하 여 수명 관리 및 검색 하는 어셈블리를 통해 등록와 같은 추가 기능을 제공 합니다.

가지 종속성 주입 컨테이너를 사용 하 여 몇 가지 이점이 있습니다.

-   컨테이너에는 해당 종속성을 찾아 해당 수명을 관리 하는 클래스에 대 한 필요가 없습니다.
-   컨테이너는 클래스에 영향을 주지 않고 매핑의 구현 된 종속성을 허용 합니다.
-   컨테이너는 모의 종속성을 허용 하 여 테스트 용이성을 지원 합니다.
-   컨테이너는 유지 관리 앱에 쉽게 추가 될 새 클래스를 허용 하는 것을 늘립니다.

MVVM을 사용 하는 Xamarin.Forms 앱의 컨텍스트에서 종속성 주입 컨테이너를 일반적으로 사용 됩니다 등록 하 고 모델을 보거나 해결 및 서비스를 등록 하 고 보기 모델을 삽입 합니다.

보기 모델의 인스턴스화를 관리 하 고 서비스 응용 프로그램에서 클래스를 Autofac을 사용 하 여 eShopOnContainers 모바일 앱과 함께 사용할 수 있는 많은 종속성 주입 컨테이너가 있습니다. Autofac 느슨하게 결합 된 앱 구축을 지원 하 고 개체 확인, 개체 수명 관리 및 삽입의 모든 형식 매핑 및 개체 인스턴스를 등록 하는 메서드를 포함 하 여 종속성 주입 컨테이너에서 흔히 발견 되는 기능을 제공 합니다. 해결 하는 개체의 생성자에 종속 된 개체입니다. Autofac에 대 한 자세한 내용은 참조 하세요. [Autofac](http://autofac.readthedocs.io/en/latest/index.html) 방법에 있습니다.

Autofac을의 `IContainer` 인터페이스는 종속성 주입 컨테이너를 제공 합니다. 그림 3-1 인스턴스화하는이 컨테이너를 사용 하는 경우 종속성을 보여 줍니다는 `IOrderService` 에 삽입 하 고 개체를 `ProfileViewModel` 클래스입니다.

![](dependency-injection-images/dependencyinjection.png "종속성 주입을 사용 하는 종속성 등")

**그림 3-1:** 종속성 주입을 사용 하는 경우 종속성

런타임 시 컨테이너는 구현의 알고 있어야 합니다 `IOrderService` 고 인스턴스화해야를 인스턴스화할 수 전에 인터페이스를 `ProfileViewModel` 개체. 여기에 다음이 포함 됩니다.

-   구현 하는 개체를 인스턴스화하는 방법을 결정 하는 컨테이너를 `IOrderService` 인터페이스입니다. 이 이라고 *등록*합니다.
-   컨테이너를 구현 하는 개체를 인스턴스화하는 `IOrderService` 인터페이스 및 `ProfileViewModel` 개체입니다. 이 이라고 *해상도*합니다.

결과적으로 앱을 사용 하 여 완료 됩니다는 `ProfileViewModel` 개체 가비지 수집에 사용할 수 있게 됩니다. 가비지 수집기는 삭제 해야이 시점에서 `IOrderService` 다른 클래스는 동일한 인스턴스를 공유 하지 않을 경우 인스턴스.

> [!TIP]
> 컨테이너-알 수 없는 코드를 작성 합니다. 항상 사용 하 고 특정 종속성 컨테이너에서 앱 분리에 컨테이너 독립적인 코드를 작성 하려고 합니다.

## <a name="registration"></a>등록

개체에 종속성을 주입할 수 있습니다, 전에 종속성 유형의 컨테이너를 사용 하 여 먼저 등록 되어야 합니다. 일반적으로 형식을 등록 인터페이스 및 인터페이스를 구현 하는 구체적인 형식 컨테이너를 전달 해야 합니다.

코드를 통해 컨테이너의 형식 및 개체를 등록 하는 방법은 두 가지가 있습니다.

-   컨테이너를 사용 하 여 형식 또는 매핑을 등록 합니다. 필요한 경우 컨테이너는 지정 된 형식의 인스턴스를 작성 합니다.
-   단일 항목으로 컨테이너에 기존 개체를 등록 합니다. 필요한 경우 컨테이너는 기존 개체에 대 한 참조를 반환 합니다.

> [!TIP]
> 종속성 주입 컨테이너 항상 적합 한 것은 아닙니다. 종속성 주입에서는 추가적인 복잡성과 요구 하지 않을 수 있는 적절 한 또는 유용한 작은 앱을 소개 합니다. 클래스에 모든 종속성이 없는 다른 형식에 대해 종속 되지는 않습니다, 경우 컨테이너에 배치 하는 것이 만들지 않을 수 있습니다. 또한 클래스에 있는 경우 단일 정수 계열 형식에는 종속성의 설정 및 변경 되지 않습니다, 그리고 컨테이너에 배치 하는 것이 수행할 수 없습니다.

앱에서 단일 메서드에 종속성 주입을 필요로 하는 형식의 등록을 수행 해야 하 고 앱은 해당 클래스 간의 종속성 인식 되도록 앱의 수명 주기 초기에이 메서드를 호출 해야 합니다. EShopOnContainers 모바일 앱에서이 수행한를 `ViewModelLocator` 클래스에 빌드를 `IContainer` 개체 및 해당 개체에 대 한 참조를 보유 하는 앱에서 유일한 클래스입니다. 다음 코드 예제에서는 eShopOnContainers 모바일 앱을 선언 하는 방법을 보여 줍니다.는 `IContainer` 개체는 `ViewModelLocator` 클래스:

```csharp
private static IContainer _container;
```

에 등록 된 형식 및 인스턴스를 `RegisterDependencies` 의 메서드는 `ViewModelLocator` 클래스입니다. 첫 번째 만들어 이렇게는 `ContainerBuilder` 다음 코드 예제에서 설명 하는 경우:

```csharp
var builder = new ContainerBuilder();
```

형식 및 인스턴스는 다음 사용 하 여 등록을 `ContainerBuilder` 개체 및 다음 코드 예제에서는 가장 일반적인 형태의 형식 등록을 보여 줍니다.

```csharp
builder.RegisterType<RequestProvider>().As<IRequestProvider>();
```

`RegisterType` 여기에 표시 된 메서드는 구체적인 형식이 인터페이스 형식으로 매핑합니다. 인스턴스화하는 컨테이너 모음이 `RequestProvider` 개체를 삽입 해야 하는 개체를 인스턴스화할 때는 `IRequestProvider` 생성자를 통해.

다음 코드 예제와 같이 구체적인 형식은 인터페이스 형식에서 매핑 없이 직접 등록할 수도 있습니다.

```csharp
builder.RegisterType<ProfileViewModel>();
```

경우는 `ProfileViewModel` 형식이 확인 되, 필요한 종속성을 주입 하는 컨테이너입니다.

또한 Autofac 컨테이너 형식의 단일 인스턴스에 대 한 참조를 유지 관리를 담당 인 인스턴스 등록이 있습니다. 예를 들어 다음 코드 예제에서는 eShopOnContainers 모바일 앱 때 사용할 구체적인 형식을 등록 하는 방법을 표시를 `ProfileViewModel` 인스턴스에 필요는 `IOrderService` 인스턴스:

```csharp
builder.RegisterType<OrderService>().As<IOrderService>().SingleInstance();
```

`RegisterType` 여기에 표시 된 메서드는 구체적인 형식이 인터페이스 형식으로 매핑합니다. `SingleInstance` 메서드는 모든 종속 개체는 동일한 공유 인스턴스를 받을 수 있도록 등록을 구성 합니다. 따라서 단일 `OrderService` 인스턴스를 삽입 해야 하는 개체를 공유 하는 컨테이너의 존재는 `IOrderService` 생성자를 통해.

인스턴스 등록 사용 하 여 수행할 수도 있습니다는 `RegisterInstance` 다음 코드 예제에서 설명 하는 메서드:

```csharp
builder.RegisterInstance(new OrderMockService()).As<IOrderService>();
```

합니다 `RegisterInstance` 여기에 표시 된 메서드를 만듭니다 `OrderMockService` 인스턴스 및 컨테이너를 사용 하 여 등록 합니다. 따라서 단일 `OrderMockService` 인스턴스를 삽입 해야 하는 개체를 공유 하는 컨테이너의 존재는 `IOrderService` 생성자를 통해.

유형 및 인스턴스 등록을 수행 합니다 `IContainer` 다음 코드 예제에 설명 되어 있는 개체를 빌드해야 합니다.

```csharp
_container = builder.Build();
```

호출을 `Build` 메서드는 `ContainerBuilder` 인스턴스에 적용 된 등록을 포함 하는 새 종속성 주입 컨테이너를 빌드.

>💡 **팁**: 고려해 야는 `IContainer` 으로 변경할 수 없게 합니다. Autofac을 제공 하는 동안는 `Update` 가능한 경우이 메서드를 호출 하는 기존 컨테이너에서 등록을 업데이트 하는 메서드를 피해 야 합니다. 컨테이너에 사용 되는 경우에 특히 작성 된 후 컨테이너를 수정 하는 위험이 있습니다. 자세한 내용은 [변경할 수 없는으로 컨테이너는 것이 좋습니다](http://docs.autofac.org/en/latest/best-practices/#consider-a-container-as-immutable) 방법에 있습니다.

<a name="resolution" />

## <a name="resolution"></a>해결

형식에 등록 한 후에 해결 또는 종속성으로 삽입 수 있습니다. 형식을 확인 하는 경우 새 인스턴스를 만드는 데 필요한 컨테이너 인스턴스로 종속성 삽입 합니다.

일반적으로 형식 확인 되 면 다음 세 가지 중 하나가 발생 합니다.

1.  컨테이너 형식에 등록 되지 않은 경우 예외가 발생 합니다.
1.  형식을 단일 항목으로 등록 되 면 컨테이너는 singleton 인스턴스를 반환 합니다. 처음 형식에 대 한 호출 될 경우 컨테이너, 필요한 경우 만들고 유지 관리에 대 한 참조입니다.
1.  형식을 단일 항목으로 등록 되지 않은, 경우 컨테이너는 새 인스턴스를 반환 하 고에 대 한 참조를 유지 하지 않습니다.

다음 코드 예제에서는 방법을 `RequestProvider` Autofac을 사용 하 여 이전에 등록 된 유형을 확인할 수 있습니다.

```csharp
var requestProvider = _container.Resolve<IRequestProvider>();
```

이 예제에서는 Autofac에 대 한 구체적인 형식을 확인 하 라는 메시지가 표시 된 `IRequestProvider` 모든 종속성과 함께 형식입니다. 일반적으로 `Resolve` 메서드는 특정 유형의 인스턴스가 필요 합니다. 확인할된 개체의 수명을 제어 하는 방법에 대 한 내용은 [개체를 관리 수명의 해결](#managing_the_lifetime_of_resolved_objects)합니다.

다음 코드 예제에서는 eShopOnContainers 모바일 앱 보기 모델 유형 및 해당 종속성을 인스턴스화하는 방법을 보여 줍니다.

```csharp
var viewModel = _container.Resolve(viewModelType);
```

이 예제에서는 Autofac 요청한 보기 모델에 대해 뷰 모델 형식을 확인 하 라는 메시지가 표시 됩니다 하 고 컨테이너 종속성 확인 됩니다. 확인할 때 합니다 `ProfileViewModel` 형식을 해결 하려면 종속성이는 `IOrderService` 개체입니다. Autofac 먼저 생성 되므로 `OrderService` 개체를 생성자로 전달 합니다 `ProfileViewModel` 클래스. EShopOnContainers 모바일 앱에서 뷰를 생성 하는 방법에 대 한 자세한 내용은 모델 보기에 연결을 참조 하세요 [자동으로 보기 모델 로케이터를 사용 하 여 뷰 모델을 만들기](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator)합니다.

> [!NOTE]
> 등록 하 고 컨테이너를 사용 하 여 형식을 확인 앱에서 각 페이지 탐색에 대 한 종속성을 재생성 되 됩니다 하는 경우에 특히 컨테이너의 각 형식 만들기에 대 한 리플렉션 사용 비용은 성능을 있습니다. 여러 경우에 전체 종속성 생성 비용이 크게 증가할 수 있습니다.

<a name="managing_the_lifetime_of_resolved_objects" />

## <a name="managing-the-lifetime-of-resolved-objects"></a>확인할된 개체의 수명을 관리합니다.

형식에 등록 한 후 기본 Autofac 동작은 형식을 만드는 데 등록 된 형식의 새 인스턴스를 각 시간, 해결 되거나 종속성 메커니즘은 다른 클래스에 인스턴스를 삽입 하는 경우. 이 시나리오에서는 컨테이너 확인 된 개체에 대 한 참조를 포함 하지 않습니다. 그러나 인스턴스를 등록할 때 Autofac의 기본 동작은 단일 항목으로 개체의 수명을 관리 하는 합니다. 따라서 인스턴스 컨테이너 범위에 있고 컨테이너 범위를 벗어날 때 가비지 수집 되 면 삭제 되는 동안 또는 코드 컨테이너를 명시적으로 삭제 하는 경우 범위에 남아 있습니다.

Autofac 등록 된 형식에서 만들어진 개체에 대 한 단일 동작을 지정 하는 Autofac 인스턴스 범위를 사용할 수 있습니다. Autofac 인스턴스 범위 컨테이너에 의해 인스턴스화되고 개체 수명을 관리 합니다. 기본 인스턴스 범위를 `RegisterType` 메서드는 `InstancePerDependency` 범위입니다. 그러나 합니다 `SingleInstance` 범위를 사용 하 여 사용할 수는 `RegisterType` 메서드를 컨테이너를 만들거나 호출 하는 경우 형식의 singleton 인스턴스를 반환 하는 `Resolve` 메서드. 다음 코드 예제에서는 Autofac의 singleton 인스턴스를 만들고 하도록 지시 하는 방법을 보여 줍니다는 `NavigationService` 클래스:

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

처음 합니다 `INavigationService` 인터페이스는 해결, 컨테이너를 만듭니다 `NavigationService` 개체 및 해당 참조를 유지 관리 합니다. 모든 후속 해상도 `INavigationService` 인터페이스를 컨테이너 참조를 반환 합니다.는 `NavigationService` 이전에 만든 개체입니다.

> [!NOTE]
> SingleInstance 범위 컨테이너 삭제 될 때 생성된 된 개체를 삭제 합니다.

Autofac 추가 인스턴스 범위를 포함합니다. 자세한 내용은 [인스턴스 범위](http://autofac.readthedocs.io/en/latest/lifetime/instance-scope.html) 방법에 있습니다.

## <a name="summary"></a>요약

종속성 주입 구체적인 형식을 이러한 형식에 의존 하는 코드에서 분리할 수 있습니다. 일반적으로 등록 및 인터페이스 및 추상 형식 간의 매핑 목록을 보유 하는 컨테이너 및 구현 하거나 이러한 형식을 확장 하는 구체적인 형식을 사용 합니다.

Autofac 느슨하게 결합 된 앱 구축을 지원 하 고 개체 확인, 개체 수명 관리 및 삽입의 모든 형식 매핑 및 개체 인스턴스를 등록 하는 메서드를 포함 하 여 종속성 주입 컨테이너에서 흔히 발견 되는 기능을 제공 합니다. 확인할 개체의 생성자에 종속 된 개체입니다.


## <a name="related-links"></a>관련 링크

- [2mb PDF 전자책 다운로드](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
