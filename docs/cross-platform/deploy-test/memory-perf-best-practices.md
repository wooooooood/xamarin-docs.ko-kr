---
title: 플랫폼 간 성능
description: 이 문서에서는 모바일 애플리케이션의 성능을 개선하는 데 사용할 수 있는 다양한 기술을 설명합니다. 프로파일러, IDisposable 리소스, 약한 참조, SGen 가비지 수집기, 크기 감소 기법 등을 설명합니다.
ms.prod: xamarin
ms.assetid: 9ce61f18-22ac-4b93-91be-5b499677d661
author: davidortinau
ms.author: daortin
ms.date: 03/24/2017
ms.openlocfilehash: d21394b3c33b3f415cbe45ae13c84cabab1ec30b
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571040"
---
# <a name="cross-platform-performance"></a>플랫폼 간 성능

낮은 애플리케이션 성능은 여러가지 방법으로 나타납니다. 이 경우에 애플리케이션이 응답하지 않는 것처럼 보이고, 스크롤 속도가 느려지고, 배터리 수명이 줄어들 수 있습니다. 그러나 성능을 최적화하려면 효율적인 코드를 구현하는 것 이상이 필요합니다. 애플리케이션 성능에 대한 사용자 환경도 고려해야 합니다. 예를 들어 사용자가 다른 활동을 수행하지 못하도록 차단하지 않고 작업을 실행하면 사용자 환경을 향상시키는 데 도움이 될 수 있습니다.

<a name="profiler"></a>

## <a name="use-the-profiler"></a>프로파일러 사용

애플리케이션을 개발하는 경우 프로파일링된 후에만 코드를 최적화하려고 해야 합니다. 프로파일링은 코드 최적화가 성능 문제를 줄이는 데 가장 큰 영향을 주는지를 확인하는 기술입니다. 프로파일러는 애플리케이션의 메모리 사용을 추적하고, 애플리케이션에서 메서드의 실행 시간을 기록합니다. 이 데이터를 통해 최적화할 시점을 검색할 수 있도록 애플리케이션의 실행 경로 및 코드의 실행 비용을 탐색할 수 있습니다.

Xamarin Profiler는 애플리케이션에서 성능 관련 문제를 측정하고, 평가하고, 찾을 수 있습니다. Mac용 Visual Studio 또는 Visual Studio 내에서 Xamarin.iOS 및 Xamarin.Android 애플리케이션을 프로파일링하는 데 사용할 수 있습니다. Xamarin Profiler에 대한 자세한 내용은 [Xamarin Profiler 소개](~/tools/profiler/index.md)를 참조하세요.

앱을 프로파일링할 때 다음과 같은 모범 사례를 사용하는 것이 좋습니다.

- 시뮬레이터가 애플리케이션 성능을 왜곡할 수 있으므로 시뮬레이터에서 애플리케이션을 프로파일링하지 않도록 방지합니다.
- 하나의 디바이스에서 성능 측정을 수행하더라도 다른 디바이스의 성능 특성을 표시하지 않기 때문에 이상적으로 다양한 디바이스에서 프로파일링을 수행할 수 있어야 합니다. 그러나 프로파일링은 최소한 가장 낮은 사양이 예상되는 디바이스에서 수행되어야 합니다.
- 다른 애플리케이션이 아닌 프로파일링된 애플리케이션의 전체 영향을 측정할 수 있도록 다른 모든 애플리케이션을 닫습니다.

<a name="idisposable"></a>

## <a name="release-idisposable-resources"></a>IDisposable 리소스 릴리스

`IDisposable` 인터페이스는 리소스를 릴리스하는 메커니즘을 제공합니다. 명시적으로 리소스를 릴리스하기 위해 구현해야 하는 `Dispose` 메서드를 제공합니다. `IDisposable`은 소멸자가 아니며 다음과 같은 경우에만 구현되어야 합니다.

- 클래스가 관리되지 않는 리소스를 소유하는 경우 릴리스해야 하는 일반적인 관리되지 않는 리소스에는 파일, 스트림 및 네트워크 연결이 포함됩니다.
- 클래스가 관리 `IDisposable` 리소스를 소유하는 경우

인스턴스가 더 이상 필요하지 않은 경우 형식 소비자는 `IDisposable.Dispose` 구현을 호출하여 리소스를 해제할 수 있습니다. 이를 위한 두 가지 방법이 있습니다.

- `using` 문에서 `IDisposable` 개체를 래핑합니다.
- `try`/`finally` 블록에서 `IDisposable.Dispose`에 대한 호출을 래핑합니다.

### <a name="wrapping-the-idisposable-object-in-a-using-statement"></a>using 문에서 IDisposable 개체 래핑

다음 코드 예제에서는 `using` 문에서 `IDisposable` 개체를 래핑하는 방법을 보여줍니다.

```csharp
public void ReadText (string filename)
{
  ...
  string text;
  using (StreamReader reader = new StreamReader (filename)) {
    text = reader.ReadToEnd ();
  }
  ...
}
```

`StreamReader` 클래스는 `IDisposable`을 구현하고, `using` 문은 범위를 벗어나기 전에 `StreamReader` 개체에서 `StreamReader.Dispose` 메서드를 호출하는 편리한 구문을 제공합니다. `using` 블록 내에서 `StreamReader` 개체는 읽기 전용이며 다시 할당할 수 없습니다. 또한 `using` 문을 사용하면 컴파일러가 `try`/`finally` 블록에서 IL(중간 언어)을 구현하므로 예외가 발생하는 경우에도 `Dispose` 메서드를 호출할 수 있습니다.

### <a name="wrapping-the-call-to-idisposabledispose-in-a-tryfinally-block"></a>Try/Finally 블록에서 IDisposable.Dispose에 대한 호출 래핑

다음 코드 예제에서는 `try`/`finally` 블록에서 `IDisposable.Dispose`에 대한 호출을 래핑하는 방법을 보여줍니다.

```csharp
public void ReadText (string filename)
{
  ...
  string text;
  StreamReader reader = null;
  try {
    reader = new StreamReader (filename);
    text = reader.ReadToEnd ();
  } finally {
    if (reader != null) {
      reader.Dispose ();
    }
  }
  ...
}
```

`StreamReader` 클래스는 `IDisposable`을 구현하고, `finally` 블록은 `StreamReader.Dispose` 메서드를 호출하여 리소스를 릴리스합니다.

자세한 내용은 [IDisposable 인터페이스](xref:System.IDisposable)를 참조하세요.

<a name="events"></a>

## <a name="unsubscribe-from-events"></a>이벤트 구독 취소

메모리 누수를 방지하려면 구독자 개체를 삭제하기 전에 이벤트 구독을 취소해야 합니다. 이벤트 구독을 취소할 때까지 게시 개체에서 이벤트의 대리자에는 구독자의 이벤트 처리기를 캡슐화하는 대리자에 대한 참조가 있습니다. 게시 개체에 해당 참조가 있다면 가비지 수집은 구독자 개체 메모리를 회수하지 않습니다.

다음 코드 예제에서는 이벤트의 구독을 취소하는 방법을 보여줍니다.

```csharp
public class Publisher
{
  public event EventHandler MyEvent;

  public void OnMyEventFires ()
  {
    if (MyEvent != null) {
      MyEvent (this, EventArgs.Empty);
    }
  }
}

public class Subscriber : IDisposable
{
  readonly Publisher publisher;

  public Subscriber (Publisher publish)
  {
    publisher = publish;
    publisher.MyEvent += OnMyEventFires;
  }

  void OnMyEventFires (object sender, EventArgs e)
  {
    Debug.WriteLine ("The publisher notified the subscriber of an event");
  }

  public void Dispose ()
  {
    publisher.MyEvent -= OnMyEventFires;
  }
}
```

`Subscriber` 클래스가 해당 `Dispose` 메서드의 이벤트 구독을 해제합니다.

람다 식에서 개체를 참조하고 활성 상태로 유지할 수 있으므로 참조 주기는 이벤트 처리기 및 람다 구문을 사용하는 경우에 발생할 수 있습니다. 따라서 무명 메서드에 대한 참조는 필드에 저장되고 다음 코드 예제와 같이 이벤트의 구독을 취소하는 데 사용될 수 있습니다.

```csharp
public class Subscriber : IDisposable
{
  readonly Publisher publisher;
  EventHandler handler;

  public Subscriber (Publisher publish)
  {
    publisher = publish;
    handler = (sender, e) => {
      Debug.WriteLine ("The publisher notified the subscriber of an event");
    };
    publisher.MyEvent += handler;
  }

  public void Dispose ()
  {
    publisher.MyEvent -= handler;
  }
}
```

`handler` 필드는 무명 메서드에 대한 참조를 유지 관리하고 이벤트를 구독하고 구독을 취소하는 데 사용됩니다.

<a name="weakreferences"></a>

## <a name="use-weak-references-to-prevent-immortal-objects"></a>약한 참조를 사용하여 유한 개체 방지

> [!NOTE]
> iOS 개발자는 [iOS에서 순환 참조를 방지](~/ios/deploy-test/performance.md#avoid-strong-circular-references)하는 방법에 대한 설명서를 검토하여 해당 앱이 메모리를 효율적으로 사용하도록 해야 합니다.

<a name="lazy"></a>

## <a name="delay-the-cost-of-creating-objects"></a>개체를 만드는 비용 연기

초기화 지연은 개체를 처음 사용할 때까지 생성을 지연하는 데 사용될 수 있습니다. 이 방법을 통해 기본적으로 성능을 향상시키고, 계산을 방지하고, 메모리 요구 사항을 줄입니다.

다음과 같은 두 가지 시나리오에서 만드는 데 비용이 많이 드는 개체에 초기화 지연을 사용하는 것이 좋습니다.

- 애플리케이션이 개체를 사용하지 않을 수 있습니다.
- 다른 비용이 많이 드는 작업은 개체를 만들기 전에 완료되어야 합니다.

다음 코드 예제에서 설명한 대로 `Lazy<T>` 클래스는 초기화 지연 형식을 정의하는 데 사용됩니다.

```csharp
void ProcessData(bool dataRequired = false)
{
  Lazy<double> data = new Lazy<double>(() =>
  {
    return ParallelEnumerable.Range(0, 1000)
                 .Select(d => Compute(d))
                 .Aggregate((x, y) => x + y);
  });

  if (dataRequired)
  {
    if (data.Value > 90)
    {
      ...
    }
  }
}

double Compute(double x)
{
  ...
}
```

처음으로 `Lazy<T>.Value` 속성에 액세스할 때 초기화 지연이 발생합니다. 처음 액세스할 때 래핑된 형식이 생성되고 반환되며 나중에 액세스하는 데 사용하기 위해 저장됩니다.

초기화 지연에 대한 자세한 내용은 [초기화 지연](https://msdn.microsoft.com/library/dd997286(v=vs.110).aspx)을 참조하세요.

<a name="async"></a>

## <a name="implement-asynchronous-operations"></a>비동기 작업 구현

.NET은 많은 API의 비동기 버전을 제공합니다. 동기 API와 달리 비동기 API는 활성 실행 스레드가 상당한 시간 동안 스레드 호출을 차단하지 않도록 합니다. 따라서 UI 스레드에서 API를 호출할 때 가능하면 비동기 API를 사용합니다. 그러면 UI 스레드의 차단이 해제됩니다. 따라서 애플리케이션에서 사용자의 환경을 개선할 수 있습니다.

또한 장기 실행 작업이 UI 스레드를 차단하지 않도록 백그라운드 스레드에서 실행되어야 합니다. .NET에서는 `async` 및 `await` 키워드를 제공하여 백그라운드 스레드에서 장기 실행 작업을 실행하고 완료 시 결과에 액세스하는 비동기 코드를 작성할 수 있습니다. 그러나 `await` 키워드를 사용하여 장기 실행 작업을 비동기적으로 실행할 수 있는 반면 작업이 백그라운드 스레드에서 실행된다고 보장할 수 없습니다. 대신, 다음 코드 예제에서 보여준 대로 장기 실행 작업을 `Task.Run`에 전달하여 수행할 수 있습니다.

```csharp
public class FaceDetection
{
  ...
  async void RecognizeFaceButtonClick(object sender, EventArgs e)
  {
    await Task.Run(() => RecognizeFace ());
    ...
  }

  async Task RecognizeFace()
  {
    ...
  }
}
```

계속하기 전에 `RecognizeFace` 메서드가 완료될 때까지 `RecognizeFaceButtonClick` 메서드가 수신 대기하여 백그라운드 스레드에서 `RecognizeFace` 메서드가 실행됩니다.

장기 실행 작업은 취소도 지원해야 합니다. 예를 들어 사용자가 애플리케이션 내에서 탐색하는 경우 장기 실행 작업을 계속할 필요가 없을 수 있습니다. 취소를 구현하는 패턴은 다음과 같습니다.

- `CancellationTokenSource` 인스턴스를 만듭니다. 이 인스턴스는 취소 알림을 관리하고 보냅니다.
- 취소할 수 있어야 하는 각 작업에 `CancellationTokenSource.Token` 속성 값을 전달합니다.
- 각 작업이 취소에 응답하는 메커니즘을 제공합니다.
- `CancellationTokenSource.Cancel` 메서드를 호출하여 취소 알림을 제공합니다.

> [!IMPORTANT]
> `CancellationTokenSource` 클래스가 `IDisposable` 인터페이스를 구현하므로 `CancellationTokenSource` 인스턴스가 완료되면 `CancellationTokenSource.Dispose` 메서드는 한 번만 호출되어야 합니다.

자세한 내용은 [동기 지원 개요](~/cross-platform/platform/async.md)를 참조하세요.

<a name="sgen"></a>

## <a name="use-the-sgen-garbage-collector"></a>SGen 가비지 수집기 사용

C#과 같은 관리 언어는 가비지 수집을 사용하여 더 이상 사용되지 않는 개체에 할당된 메모리를 회수합니다. Xamarin 플랫폼에서 사용하는 두 개의 가비지 수집기는 다음과 같습니다.

- [**SGen**](https://www.mono-project.com/docs/advanced/garbage-collector/sgen/) – 세대 가비지 수집기이며 Xamarin 플랫폼에서 기본 가비지 수집기입니다.
- [**Boehm**](http://www.hboehm.info/gc/) – 보수적인 비세대 가비지 수집기입니다. 클래식 API를 사용하는 Xamarin.iOS 애플리케이션에 사용되는 기본 가비지 수집기입니다.

SGen에서는 세 가지 힙 중 하나를 활용하여 개체에 공간을 할당합니다.

- **Nursery** – 새로운 작은 개체가 할당되는 위치입니다. Nursery에 공간이 없는 경우 보조 가비지 수집이 발생합니다. 모든 라이브 개체가 주요 힙으로 이동됩니다.
- **주요 힙** – 장기 실행 개체가 유지되는 위치입니다. 주요 힙에 충분한 메모리가 없는 경우 주요 가비지 수집이 발생합니다. 주요 가비지 수집이 충분한 메모리를 확보하지 못하는 경우 SGen은 시스템에 추가 메모리를 요청합니다.
- **LOB(Large Object) 공간** – 8000바이트를 초과해야 하는 개체가 유지되는 위치입니다. LOB(Large Object)는 nursery에서 시작되지 않지만 대신 이 힙에 할당됩니다.

SGen의 장점 중 하나는 보조 가비지 수집을 수행하는 데 걸리는 시간이 마지막 보조 가비지 수집 이후 생성된 새로운 라이브 개체의 수에 비례한다는 점입니다. 그러면 이러한 보조 가비지 수집에 걸리는 시간이 주요 가비지 수집보다 적기 때문에 애플리케이션의 성능에 대한 가비지 수집의 영향이 감소합니다. 주요 가비지 수집은 계속 발생하지만 빈도가 감소합니다.

SGen 가비지 수집기는 Xamarin.iOS 9.2.1 이상에서 기본값이므로 자동으로 사용됩니다. 최신 버전의 Visual Studio에서 가비지 수집기를 변경하는 기능이 제거되었습니다. 자세한 내용은 [새 참조 횟수 시스템](~/ios/internals/newrefcount.md)을 참조하세요.

### <a name="reducing-pressure-on-the-garbage-collector"></a>가비지 수집기에 대한 압력 감소

SGen이 가비지 수집을 시작하면 메모리를 회수하는 동안 애플리케이션의 스레드를 중지합니다. 메모리를 회수하는 동안 애플리케이션은 UI가 잠깐 일시 중지되거나 끊길 수도 있습니다. 이 일시 중지의 허용 정도는 다음과 같은 두 가지 요인에 따라 달라집니다.

1. **빈도** - 가비지 수집이 발생하는 빈도입니다. 컬렉션 간에 더 많은 메모리를 할당하면 가비지 수집의 빈도가 증가합니다.
1. **기간** – 각 개별 가비지 수집에 걸리는 시간입니다. 수집되는 라이브 개체의 수와 거의 비례합니다.

즉, 전체적으로 많은 개체가 할당되지만 활성 상태로 유지되지 않는 경우 여러 개의 짧은 가비지 수집이 생성됩니다. 반면에 새 개체가 느리게 할당되고 개체가 활성 상태인 경우 가비지 수집 횟수는 줄고 시간은 길어집니다.

가비지 수집기에 대한 압력을 줄이려면 다음과 같은 지침을 따르세요.

- 개체 풀을 사용하여 빽빽한 루프에서 가비지 수집을 방지합니다. 이 항목은 특히 게임에 관련되며 나중에 해당 개체를 만드는 데 필요합니다.
- 스트림, 네트워크 연결, 큰 블록의 메모리 및 파일 등의 리소스가 더 이상 필요 없는 경우 명시적으로 릴리스합니다. 자세한 내용은 [IDisposable 리소스 릴리스](#idisposable)를 참조하세요.
- 개체를 수집할 수 있도록 이벤트 처리기가 더 이상 필요 없는 경우 다시 등록합니다. 자세한 내용은 [이벤트 구독 취소](#events)를 참조하세요.

<a name="linker"></a>

## <a name="reduce-the-size-of-the-application"></a>애플리케이션의 크기 축소

애플리케이션의 실행 파일 크기를 제공한 위치를 이해하려면 각 플랫폼에서 컴파일 프로세스를 이해해야 합니다.

- iOS 애플리케이션은 ARM 어셈블리 언어로 AOT(Ahead Of Time) 컴파일됩니다. 적절한 링커 옵션을 사용하는 경우에만 사용하지 않는 클래스를 제거한 .NET Framework가 포함됩니다.
- Android 애플리케이션은 IL(중간 언어)로 컴파일되고 MonoVM 및 JIT(just-in-time) 컴파일로 패키지됩니다. 적절한 링커 옵션을 사용하는 경우에만 사용하지 않는 프레임워크 클래스를 제거합니다.
- Windows Phone 애플리케이션은 IL로 컴파일되고 기본 제공 런타임에 의해 실행됩니다.

또한 애플리케이션이 제네릭을 광범위하게 사용하는 경우 기본적으로 컴파일된 버전의 제네릭 가능성이 포함되기 때문에 최종 실행 파일 크기가 더욱 증가합니다.

애플리케이션의 크기를 줄이기 위해 Xamarin 플랫폼에는 빌드 도구의 일부로 링커가 포함됩니다. 기본적으로 링커는 사용되지 않고 애플리케이션의 프로젝트 옵션에서 사용할 수 있어야 합니다. 빌드 시 애플리케이션에서 실제로 사용하는 형식과 멤버를 확인하기 위해 애플리케이션의 정적 분석을 수행합니다. 그런 다음, 애플리케이션에서 사용하지 않는 형식과 메서드를 제거합니다.

다음과 같은 스크린샷에서는 Xamarin.iOS 프로젝트의 Mac용 Visual Studio에 있는 링커 옵션을 보여줍니다.

![](memory-perf-best-practices-images/linker-options-ios.png)

다음과 같은 스크린샷에서는 Xamarin.Android 프로젝트의 Mac용 Visual Studio에 있는 링커 옵션을 보여줍니다.

![](memory-perf-best-practices-images/linker-options-droid.png)

링커는 다음과 같은 세 가지 설정을 제공하여 해당 동작을 제어합니다.

- **링크하지 않음** – 사용되지 않는 형식과 메서드가 링커에 의해 제거됩니다. 성능상의 이유로 디버그 빌드의 기본 설정입니다.
- **프레임워크 SDK/SDK 어셈블리만 링크** – 이 설정은 Xamarin에서 제공되는 해당 어셈블리의 크기를 감소시킵니다. 사용자 코드가 영향을 받지 않습니다.
- **모든 어셈블리 링크** – SDK 어셈블리와 사용자 코드를 대상으로 하는 보다 적극적인 최적화입니다. 바인딩의 경우 사용되지 않는 지원 필드를 제거하고, 각 인스턴스(또는 바인딩된 개체)를 가볍게 해서 더 적은 메모리를 사용합니다.

*모든 어셈블리 링크*는 예상하지 못한 방법으로 애플리케이션을 중단할 수 있으므로 주의해서 사용해야 합니다. 링커에 의해 수행되는 정적 분석은 필요한 일부 코드를 잘못 식별할 수 있습니다. 그 결과 너무 많은 코드를 컴파일된 애플리케이션에서 제거하게 됩니다. 이 경우에 애플리케이션이 충돌할 때 런타임 시에만 스스로를 매니페스트합니다. 이로 인해 링커 동작을 변경한 후에 애플리케이션을 철저히 테스트해야 합니다.

테스트로 인해 링커가 클래스 또는 메서드에서 잘못 제거되었음이 드러난 경우 정적으로 참조되지 않지만 다음과 같은 특성 중 하나를 사용하여 애플리케이션에 필요한 형식이나 메서드를 표시할 수 있습니다.

- `Xamarin.iOS.Foundation.PreserveAttribute` - 이 특성은 Xamarin.iOS 프로젝트에 사용됩니다.
- `Android.Runtime.PreserveAttribute` - 이 특성은 Xamarin.Android 프로젝트에 사용됩니다.

예를 들어 동적으로 시작된 형식의 기본 생성자를 유지해야 할 수 있습니다. 또한 XML 직렬화를 사용하려면 형식의 속성을 유지해야 할 수 있습니다.

자세한 내용은 [iOS용 링커](~/ios/deploy-test/linker.md) 및 [Android용 링커](~/android/deploy-test/linker.md)를 참조하세요.

### <a name="additional-size-reduction-techniques"></a>추가 크기 감소 방법

모바일 디바이스에 전원을 공급하는 다양한 CPU 아키텍처가 있습니다. 따라서 Xamarin.iOS 및 Xamarin.Android는 각 CPU 아키텍처에 대한 컴파일된 버전의 애플리케이션을 포함하는 *FAT 이진 파일*을 생성합니다. 그러면 CPU 아키텍처에 관계 없이 모바일 애플리케이션을 디바이스에서 실행할 수 있습니다.

다음 단계를 사용하여 애플리케이션 실행 파일의 크기를 더 줄일 수 있습니다.

- 릴리스 빌드가 생성되어야 합니다.
- FAT 이진 파일이 생성되지 않도록 방지하기 위해 애플리케이션을 빌드할 아키텍처의 수를 감소시킵니다.
- 보다 최적화된 실행 파일을 생성하기 위해 LLVM 컴파일러를 사용해야 합니다.
- 애플리케이션의 관리 코드 크기를 줄입니다. 각 어셈블리에서 링커를 사용하여 이 작업을 수행할 수 있습니다(iOS 프로젝트의 경우 *모두 링크* 및 Android 프로젝트의 경우 *모든 어셈블리 링크*).

Android 앱은 각 ABI의 별도 APK로 분할될 수도 있습니다("아키텍처").
이 블로그 게시물에서 자세히 알아보기: [Android 앱 크기를 줄이는 방법](https://montemagno.com/how-to-keep-your-android-app-size-down/).

<a name="optimizeimages"></a>

## <a name="optimize-image-resources"></a>이미지 리소스 최적화

이미지는 애플리케이션에서 사용하는 가장 비싼 리소스 중 일부이며, 고해상도로 캡처되는 경우가 많습니다. 여기에서 활발하게 세부적인 전체 이미지를 만드는 동안 해당 이미지를 표시하는 애플리케이션에는 일반적으로 이미지를 디코딩할 더 많은 CPU 사용량 및 디코딩된 이미지를 저장할 더 많은 메모리가 필요합니다. 표시하기 위해 더 작은 크기로 축소하는 경우 메모리에서 고해상도 이미지를 디코딩할 필요가 없습니다. 대신, 예측된 디스플레이 크기에 가까운 저장된 이미지의 여러 해상도 버전을 만들어서 CPU 사용량 및 메모리 사용 공간을 줄입니다. 예를 들어 목록 보기에 표시되는 이미지는 전체 화면에 표시되는 이미지보다 해상도가 더 낮을 가능성이 높습니다. 또한 메모리 영향을 최소화하여 효율적으로 표시하도록 축소된 버전의 고해상도 이미지를 로드할 수 있습니다. 자세한 내용은 [효율적으로 큰 비트맵 로드](https://github.com/xamarin/recipes/tree/master/Recipes/android/resources/general/load_large_bitmaps_efficiently)를 참조하세요.

이미지 해상도와 관계 없이 이미지 리소스를 표시하면 앱의 메모리 사용 공간이 크게 증가할 수 있습니다. 따라서 필요한 경우에만 만들어야 하며, 애플리케이션에 더 이상 필요하지 않을 경우 즉시 해제되어야 합니다.

<a name="activationperiod"></a>

## <a name="reduce-the-application-activation-period"></a>애플리케이션 활성화 기간 축소

모든 애플리케이션에는 *정품 인증 기간*이 있습니다. 애플리케이션이 시작할 때와 애플리케이션을 사용할 준비가 될 때 사이의 시간입니다. 이 정품 인증 기간은 사용자에게 애플리케이션의 첫 번째 인상을 제공합니다. 따라서 애플리케이션이 좋은 첫 인상을 얻기 위해 정품 인증 기간 및 사용자 인식을 감소시켜야 합니다.

애플리케이션이 초기 UI를 표시하기 전에 애플리케이션이 시작되고 있음을 사용자에게 나타내는 시작 화면을 제공해야 합니다. 애플리케이션이 초기 UI를 신속하게 표시하지 않는 경우 애플리케이션이 중지되지 않았음을 확인하기 위해 시작 화면을 사용하여 사용자에게 정품 인증 기간 동안 진행률을 알려야 합니다. 이 작업은 진행률 표시줄 또는 비슷한 컨트롤에 있을 수 있습니다.

정품 인증 기간 동안 애플리케이션은 정품 인증 논리를 실행합니다. 여기에는 리소스의 로드 및 처리가 포함됩니다. 필수 리소스를 원격으로 검색하는 대신 앱 내에서 패키지되도록 하여 정품 인증 기간을 줄일 수 있습니다. 예를 들어 어떤 경우에는 정품 인증 기간 동안 로컬로 저장된 자리 표시자 데이터를 로드하는 것이 적절할 수 있습니다. 그런 다음, 초기 UI가 표시되고 사용자가 해당 앱과 상호 작용할 수 있게 되면 원격 원본에서 자리 표시자 데이터를 점진적으로 바꿀 수 있습니다. 또한 앱의 활성화 논리는 사용자가 애플리케이션을 사용하기 시작하는 데 필요한 작업만을 수행해야 합니다. 어셈블리가 처음으로 사용될 때 로드되면 추가 어셈블리를 로드하는 작업이 지연되는 경우 도움이 될 수 있습니다.

<a name="webservicecommunication"></a>

## <a name="reduce-web-service-communication"></a>웹 서비스 통신 축소

애플리케이션에서 웹 서비스로 연결하면 애플리케이션 성능에 영향을 줄 수 있습니다. 예를 들어 네트워크 대역폭의 사용량이 증가하면 디바이스 배터리의 사용량이 증가합니다. 또한 사용자가 대역폭 제한 환경에서 애플리케이션을 사용할 수 있습니다. 따라서 애플리케이션과 웹 서비스 간에 대역폭 사용률을 제한하는 것이 좋습니다.

애플리케이션의 대역폭 활용을 감소시키는 한 가지 방법은 네트워크를 통해 전송하기 전에 데이터를 압축하는 것입니다. 그러나 압축 프로세스에서 CPU 사용량이 추가되면 배터리 사용량도 증가할 수 있습니다. 따라서 네트워크를 통해 압축된 데이터를 이동할지를 결정하기 전에 이러한 상충 관계를 신중하게 평가해야 합니다.

고려해야 할 또 다른 문제는 애플리케이션과 웹 서비스 간에 전환되는 데이터의 형식입니다. 두 가지 기본 형식은 XML(Extensible Markup Language) 및 JSON(JavaScript Object Notation)입니다. XML은 많은 수의 서식 지정 문자를 포함하기 때문에 비교적 큰 데이터 페이로드를 생성하는 텍스트 기반 데이터 교환 형식입니다. JSON은 간단한 데이터 페이로드를 생성하는 텍스트 기반 데이터 교환 형식입니다. 이로 인해 데이터를 보내고 수신할 때 대역폭 요구 사항이 감소하게 됩니다. 따라서 모바일 애플리케이션에 사용하려는 형식은 주로 JSON입니다.

애플리케이션과 웹 서비스 간에 데이터를 전송할 때 DTO(데이터 전송 개체)를 사용하는 것이 좋습니다. DTO에는 네트워크에서 전송할 데이터 집합이 포함되어 있습니다. DTO를 활용하여 단일 원격 호출에서 더 많은 데이터를 전송할 수 있습니다. 그러면 애플리케이션에서 발생시킨 원격 호출의 수를 줄일 수 있습니다. 일반적으로 더 큰 데이터 페이로드를 전송하는 원격 호출은 작은 데이터 페이로드를 전송하는 호출과 비슷한 시간을 사용합니다.

캐시된 데이터를 웹 서비스에서 반복적으로 검색하지 않고 활용하여 웹 서비스에서 검색된 데이터를 로컬로 캐시해야 합니다. 그러나 이 방법을 채택하면 적절한 캐싱 전략을 구현하여 로컬 캐시에 있는 데이터가 웹 서비스에서 변경되는 경우 업데이트해야 합니다.

## <a name="summary"></a>요약

이 문서에서는 Xamarin 플랫폼을 사용하여 빌드된 애플리케이션의 성능을 높이는 기술에 대해 설명했습니다. 이러한 기술은 전체적으로 CPU에서 수행하는 작업의 양과 애플리케이션에서 소비하는 메모리의 양을 크게 줄일 수 있습니다.

## <a name="related-links"></a>관련 링크

- [Xamarin.iOS 성능](~/ios/deploy-test/performance.md)
- [Xamarin.Android Performance](~/android/deploy-test/performance.md)
- [Xamarin Profiler 소개](~/tools/profiler/index.md)
- [Xamarin.Forms 성능](~/xamarin-forms/deploy-test/performance.md)
- [비동기 지원 개요](~/cross-platform/platform/async.md)
- [IDisposable](xref:System.IDisposable)
