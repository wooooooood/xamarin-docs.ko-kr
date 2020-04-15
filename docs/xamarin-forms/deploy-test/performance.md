---
title: Xamarin.Forms 앱 성능 향상
description: Xamarin.Forms 애플리케이션의 성능을 높이기 위한 많은 기술이 있습니다. 이러한 기술은 전체적으로 CPU에서 수행하는 작업의 양과 애플리케이션에서 소비하는 메모리의 양을 크게 줄일 수 있습니다.
ms.prod: xamarin
ms.assetid: 0be84c56-6698-448d-be5a-b4205f1caa9f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/27/2019
ms.openlocfilehash: 4427d347723284a2f8897612f10857270c9631bf
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "79303884"
---
# <a name="improve-xamarinforms-app-performance"></a>Xamarin.Forms 앱 성능 향상

> [!VIDEO https://youtube.com/embed/RZvdql3Ev0E]

**Evolve 2016: Xamarin.Forms를 사용하여 앱 성능 최적화**

낮은 애플리케이션 성능은 여러가지 방법으로 나타납니다. 이 경우에 애플리케이션이 응답하지 않는 것처럼 보이고, 스크롤 속도가 느려지고, 디바이스 배터리 수명이 줄어들 수 있습니다. 그러나 성능을 최적화하려면 효율적인 코드를 구현하는 것 이상이 필요합니다. 애플리케이션 성능에 대한 사용자 환경도 고려해야 합니다. 예를 들어 사용자가 다른 활동을 수행하지 못하도록 차단하지 않고 작업을 실행하면 사용자 환경을 향상시키는 데 도움이 될 수 있습니다.

Xamarin.Forms 애플리케이션의 성능과 인식 성능을 높이는 여러 가지 기술이 있습니다. 이러한 기술은 전체적으로 CPU에서 수행하는 작업의 양과 애플리케이션에서 소비하는 메모리의 양을 크게 줄일 수 있습니다.

> [!NOTE]
> 이 아티클을 읽기 전에 먼저 Xamarin 플랫폼을 사용하여 빌드된 애플리케이션의 메모리 사용 및 성능을 향상시키기 위한 비플랫폼 특정 기술에 대해 설명하는 [플랫폼 간 성능](~/cross-platform/deploy-test/memory-perf-best-practices.md)을 참조해야 합니다.

## <a name="enable-the-xaml-compiler"></a>XAML 컴파일러 활성화

필요한 경우 XAML 컴파일러(XAMLC)를 사용하여 XAML을 중간 언어(IL)로 바로 컴파일할 수 있습니다. XAMLC는 여러 가지 이점을 제공합니다.

- 컴파일 시 XAML 검사를 수행하여 발견된 오류를 사용자에게 알립니다.
- XAML 요소의 일부 로드 및 인스턴스화 시간을 제거합니다.
- 더 이상 .xaml 파일을 포함하지 않아 최종 어셈블리의 파일 크기를 줄이는 데 도움이 됩니다.

XAMLC는 새 Xamarin.Forms 솔루션에서 기본적으로 사용하도록 설정되어 있습니다. 그러나 이전 솔루션에서 사용하도록 설정해야 할 수도 있습니다. 자세한 내용은 [XAML 컴파일](~/xamarin-forms/xaml/xamlc.md)을 참조하세요.

## <a name="use-compiled-bindings"></a>컴파일된 바인딩 사용

컴파일된 바인딩은 리플렉션을 사용하는 런타임 대신 컴파일 시간에 바인딩 식을 확인하여 Xamarin.Forms 애플리케이션에서 데이터 바인딩 성능을 향상시킵니다. 바인딩 식을 컴파일하면 일반적으로 클래식 바인딩을 사용하는 것보다 8~20배 더 빠르게 해결하는 컴파일된 코드가 생성됩니다. 자세한 내용은 [컴파일된 바인딩](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md)을 참조하세요.

## <a name="reduce-unnecessary-bindings"></a>불필요한 바인딩 줄이기

정적으로 쉽게 설정할 수 있는 콘텐츠에 바인딩을 사용하지 마세요. 바인딩할 필요가 없는 데이터를 바인딩할 경우 어떠한 이점도 없습니다. 바인딩은 비용 효율적이지 않기 때문입니다. 예를 들어, `Button.Text = "Accept"`를 설정하는 것은 "Accept" 값을 사용하여 viewmodel `string` 속성에 [`Button.Text`](xref:Xamarin.Forms.Button.Text)를 바인딩하는 것보다 오버헤드가 적습니다.

## <a name="use-fast-renderers"></a>빠른 렌더러 사용

빠른 렌더러는 결과 네이티브 컨트롤 계층을 평면화하여 Android에서 Xamarin.Forms 컨트롤의 인플레이션 및 렌더링 비용을 줄여줍니다. 이는 더 적은 개체를 만들어 시각적 트리의 복잡성을 낮추고 메모리 사용량을 줄여 성능을 개선합니다.

Xamarin.Forms 4.0부터 `FormsAppCompatActivity`를 대상으로 하는 모든 애플리케이션에서는 기본적으로 빠른 렌더러를 사용합니다. 자세한 내용은 [빠른 렌더러](~/xamarin-forms/internals/fast-renderers.md)를 참조하세요.

## <a name="enable-startup-tracing-on-android"></a>Android에서 시작 추적 사용

Android에서 AOT(Ahead of Time) 컴파일을 사용하면 JIT(Just-in-time) 애플리케이션 시작 오버헤드와 메모리 사용을 최소화하지만, 훨씬 큰 APK가 작성됩니다. 또 다른 방법은 시작 추적을 사용하는 것입니다. 이 방법은 기존 AOT 컴파일과 비교하여, Android APK 크기와 시작 시간이 서로 절충될 수 있습니다.

비관리형 코드에서 가능한 한 많은 애플리케이션을 컴파일하는 대신, 시작 추적은 빈 Xamarin.Forms 애플리케이션에서 애플리케이션 시작 부분 중 가장 자원이 많이 소모되는 부분을 나타내는 관리형 메서드 집합만 컴파일합니다. 이 접근 방식을 사용하면 기존 AOT 컴파일과 비교할 때 APK 크기가 줄어들지만 시작 개선사항은 여전히 비슷합니다.

## <a name="enable-layout-compression"></a>레이아웃 압축 활성화

레이아웃 압축은 페이지 렌더링 성능을 개선하기 위해 시각적 트리에서 지정된 레이아웃을 제거합니다. 이로 인해 얻을 수 있는 성능 이점은 페이지의 복잡성, 사용 중인 운영 체제의 버전, 그리고 애플리케이션이 실행 중인 디바이스에 따라 다릅니다. 하지만 가장 큰 성능 이점은 이전 버전의 디바이스에서 볼 수 있습니다. 자세한 내용은 [레이아웃 압축](~/xamarin-forms/user-interface/layouts/layout-compression.md)을 참조하세요.

## <a name="choose-the-correct-layout"></a>올바른 레이아웃 선택

여러 자식 요소를 표시할 수 있지만 단일 자식만 있어 불필요한 레이아웃입니다. 예를 들어 다음 코드 예제는 단일 자식이 있는 [`StackLayout`](xref:Xamarin.Forms.StackLayout)을 보여줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DisplayImage.HomePage">
    <StackLayout>
        <Image Source="waterfront.jpg" />
    </StackLayout>
</ContentPage>
```

이는 불필요하며, 다음 코드 예제에 나와 있는 것처럼 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 요소를 제거해야 합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DisplayImage.HomePage">
    <Image Source="waterfront.jpg" />
</ContentPage>
```

또한 다른 레이아웃의 조합을 사용하여 특정 레이아웃의 모양을 재현하려고 하지 마세요. 불필요한 레이아웃 계산이 수행됩니다. 예를 들어 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 인스턴스의 조합을 사용하여 [`Grid`](xref:Xamarin.Forms.Grid) 레이아웃을 재현하려고 하지 마세요. 다음 코드 예제는 이러한 나쁜 사례의 예를 보여 줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Details.HomePage"
             Padding="0,20,0,0">
    <StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Name:" />
            <Entry Placeholder="Enter your name" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Age:" />
            <Entry Placeholder="Enter your age" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Occupation:" />
            <Entry Placeholder="Enter your occupation" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Address:" />
            <Entry Placeholder="Enter your address" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

불필요한 레이아웃 계산이 수행되기 때문에 이는 불필요합니다. 대신에 다음 코드 예제에 나와 있는 것처럼 [`Grid`](xref:Xamarin.Forms.Grid)를 사용하면 원하는 레이아웃을 더 잘 구현할 수 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Details.HomePage"
             Padding="0,20,0,0">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="100" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="30" />
            <RowDefinition Height="30" />
            <RowDefinition Height="30" />
            <RowDefinition Height="30" />
        </Grid.RowDefinitions>
        <Label Text="Name:" />
        <Entry Grid.Column="1" Placeholder="Enter your name" />
        <Label Grid.Row="1" Text="Age:" />
        <Entry Grid.Row="1" Grid.Column="1" Placeholder="Enter your age" />
        <Label Grid.Row="2" Text="Occupation:" />
        <Entry Grid.Row="2" Grid.Column="1" Placeholder="Enter your occupation" />
        <Label Grid.Row="3" Text="Address:" />
        <Entry Grid.Row="3" Grid.Column="1" Placeholder="Enter your address" />
    </Grid>
</ContentPage>
```

## <a name="optimize-layout-performance"></a>레이아웃 성능 최적화

최적의 레이아웃 성능을 얻으려면 다음 지침을 따르세요.

- 더 적은 래핑 뷰를 사용하여 레이아웃을 만들 수 있는 [`Margin`](xref:Xamarin.Forms.View.Margin) 속성 값을 지정하여 레이아웃 계층의 깊이를 줄입니다. 자세한 내용은 [여백 및 패딩](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)을 참조하세요.
- [`Grid`](xref:Xamarin.Forms.Grid)를 사용할 경우 가능한 한 적은 행과 열을 [`Auto`](xref:Xamarin.Forms.GridLength.Auto) 크기로 설정하도록 하세요. 자동으로 크기가 조정된 행이나 열은 레이아웃 엔진이 추가적인 레이아웃 계산을 수행하도록 합니다. 가능한 경우 고정된 크기의 행과 열을 대신 사용하세요. 또는 부모 트리가 이러한 레이아웃 지침을 따르는 경우 [`GridUnitType.Star`](xref:Xamarin.Forms.GridUnitType.Star) 열거 값을 사용하여 행과 열이 일정한 비율의 공간을 차지하도록 하세요.
- 필요하지 않은 경우 레이아웃의 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 및 [`HorizontalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 속성을 설정하지 마세요. [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) 및 [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)의 기본값은 최고의 레이아웃 최적화를 지원합니다. 이러한 속성을 변경하는 경우 기본값으로 설정하더라도 비용이 들고 메모리가 소모됩니다.
- 가능한 경우 [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)을 사용하지 마세요. 그러면 CPU가 훨씬 더 많은 작업을 수행해야 합니다.
- [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)을 사용할 경우 가능하다면 [`AbsoluteLayout.AutoSize`](xref:Xamarin.Forms.AbsoluteLayout.AutoSize) 속성을 사용하지 마세요.
- [`StackLayout`](xref:Xamarin.Forms.StackLayout)을 사용할 경우 하나의 자식만 [`LayoutOptions.Expands`](xref:Xamarin.Forms.LayoutOptions.Expands)로 설정되도록 합니다. 이 속성은 지정된 자식 요소가 `StackLayout`에서 허용하는 최대의 공간을 차지하도록 하며, 이는 이러한 계산이 두 번 이상 수행되도록 하여 불필요합니다.
- [`Layout`](xref:Xamarin.Forms.Layout) 클래스의 메서드를 호출하지 마세요. 비용이 많이 드는 레이아웃 계산이 수행됩니다. 대신에 [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) 및 [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) 속성을 설정하면 원하는 레이아웃 동작을 구현할 가능성이 높습니다. 또는 [`Layout<View>`](xref:Xamarin.Forms.Layout`1) 클래스를 서브클래스로 지정하여 원하는 레이아웃 동작을 구현할 수 있습니다.
- [`Label`](xref:Xamarin.Forms.Label) 인스턴스를 필요 이상으로 자주 업데이트하지 마세요. 레이블의 크기가 변경되면 전체 화면 레이아웃이 다시 계산될 수 있습니다.
- 필요하지 않은 경우 [`Label.VerticalTextAlignment`](xref:Xamarin.Forms.Label.VerticalTextAlignment) 속성을 설정하지 마세요.
- 가능할 때마다 [`Label`](xref:Xamarin.Forms.Label) 인스턴스의 [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode)를 [`NoWrap`](xref:Xamarin.Forms.LineBreakMode.NoWrap)으로 설정합니다.

## <a name="use-asynchronous-programming"></a>비동기 프로그래밍 사용

비동기 프로그래밍을 사용하여 애플리케이션의 전반적인 응답성을 향상시키고 많은 경우에 성능 병목 현상을 방지할 수 있습니다. .NET에서 [TAP(작업 기반 비동기 패턴)](/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap)은 비동기 작업에 권장되는 디자인 패턴입니다. 그러나 TAP을 잘못 사용하면 성능이 저하될 수 있습니다. 따라서 TAP을 사용하는 경우 다음 지침을 따라야 합니다.

### <a name="fundamentals"></a>기본 사항

- `TaskStatus` 열거형으로 표시되는 작업 수명 주기를 이해합니다. 자세한 내용은 [TaskStatus의 의미](https://devblogs.microsoft.com/pfxteam/the-meaning-of-taskstatus/) 및 [작업 상태](/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap#task-status)를 참조하세요.
- `Task.WhenAll` 메서드를 사용하여 일련의 비동기 작업을 개별적으로 `await`하는 것이 아니라 여러 비동기 작업이 완료될 때까지 비동기적으로 대기합니다. 자세한 내용은 [Task.WhenAll](/dotnet/standard/asynchronous-programming-patterns/consuming-the-task-based-asynchronous-pattern#taskwhenall)을 참조하세요.
- `Task.WhenAny` 메서드를 사용하여 여러 비동기 작업 중 하나가 완료될 때까지 비동기적으로 대기합니다. 자세한 내용은 [Task.WhenAny](/dotnet/standard/asynchronous-programming-patterns/consuming-the-task-based-asynchronous-pattern#taskwhenall)를 참조하세요.
- `Task.Delay` 메서드를 사영하여 지정된 시간 이후에 완료되는 `Task` 개체를 생성합니다. 이는 데이터 폴링 및 미리 정해진 시간 동안 사용자 입력 처리 지연 등의 시나리오에 유용합니다. 자세한 내용은 [Task.Delay](/dotnet/standard/asynchronous-programming-patterns/consuming-the-task-based-asynchronous-pattern#taskdelay)를 참조하세요.
- `Task.Run` 메서드를 사용하여 스레드 풀에서 집약적 동기 CPU 작업을 실행합니다. 이 메서드는 가장 최적의 인수가 설정된 `TaskFactory.StartNew` 메서드의 바로 가기입니다. 자세한 내용은 [Task.Run](/dotnet/standard/asynchronous-programming-patterns/consuming-the-task-based-asynchronous-pattern#taskrun)을 참조하세요.
- 비동기 생성자를 만들지 마십시오. 대신, 수명 주기 이벤트 또는 별도의 초기화 논리를 사용하여 초기화를 올바르게 `await`합니다. 자세한 내용은 blog.stephencleary.com에서 [비동기 생성자](https://blog.stephencleary.com/2013/01/async-oop-2-constructors.html)를 참조하세요.
- 지연 작업 패턴을 사용하여 애플리케이션 시작 중에 비동기 작업이 완료될 때까지 기다리지 않도록 합니다. 자세한 내용은 [AsyncLazy](https://devblogs.microsoft.com/pfxteam/asynclazyt/)를 참조하세요.
- `TaskCompletionSource<T>` 개체를 만들어 TAP을 사용하지 않는 기존 비동기 작업에 대한 작업 래퍼를 만듭니다. 이 개체를 사용하여 `Task` 프로그래밍의 이점을 얻고 관련된 `Task`의 수명과 완료를 제어할 수 있도록 합니다. 자세한 내용은 [TaskCompletionSource의 특성](https://devblogs.microsoft.com/pfxteam/the-nature-of-taskcompletionsourcetresult/)을 참조하세요.
 
- 비동기 작업의 결과를 처리할 필요가 없는 경우 대기 `Task` 개체를 반환하는 대신, `Task` 개체를 반환합니다. 이렇게 하면 컨텍스트 전환이 수행되지 않으므로 성능이 향상됩니다.
- 사용 가능한 경우 데이터 처리와 같은 시나리오에서 또는 비동기적으로 서로 통신해야 하는 작업이 많은 경우에 TPL(작업 병렬 라이브러리) 데이터 흐름 라이브러리를 사용합니다. 자세한 내용은 [데이터 흐름(작업 병렬 라이브러리)](/dotnet/standard/parallel-programming/dataflow-task-parallel-library)을 참조하세요.

### <a name="ui"></a>UI

- 사용할 수 있는 경우, API의 비동기 버전을 호출합니다. 그러면 UI 스레드의 차단이 해제됩니다. 따라서 애플리케이션에서 사용자의 환경을 개선할 수 있습니다.
- UI 요소를 UI 스레드에 대한 비동기 작업의 데이터로 업데이트하여 예외가 throw되지 않도록 합니다. 그러나 `ListView.ItemsSource` 속성의 업데이트는 자동으로 UI 스레드로 마샬링됩니다. 코드가 UI 스레드에서 실행되고 있는지 확인하는 방법에 대한 자세한 내용은 [Xamarin.Essentials: MainThread](~/essentials/main-thread.md?content=xamarin/xamarin-forms)를 참조하세요.

    > [!IMPORTANT]
    > 데이터 바인딩을 통해 업데이트되는 모든 컨트롤 속성은 자동으로 UI 스레드로 마샬링됩니다.

### <a name="error-handling"></a>오류 처리

- 비동기 예외 처리에 대해 알아봅니다. 비동기적으로 실행되는 코드에 의해 throw된 처리되지 않은 예외는 특정 시나리오를 제외하고는 호출 스레드로 다시 전파됩니다. 자세한 내용은 [예외 처리(작업 병렬 라이브러리)](/dotnet/standard/parallel-programming/exception-handling-task-parallel-library)를 참조하세요.
- `async void` 메서드를 만들지 말고 대신 `async Task` 메서드를 만듭니다. 이를 통해 오류 처리, 구성 가능성 및 테스트 가능성을 보다 손쉽게 사용할 수 있습니다. 이 지침의 예외는 `void`를 반환해야 하는 비동기 이벤트 처리기입니다. 자세한 내용은 [비동기 Void 방지](/archive/msdn-magazine/2013/march/async-await-best-practices-in-asynchronous-programming#avoid-async-void)를 참조하세요.
- 교착 상태가 발생할 수 있으므로 `Task.Wait`, `Task.Result` 또는 `GetAwaiter().GetResult` 메서드를 호출하여 차단 및 비동기 코드를 혼합하지 마세요. 그러나 이 지침을 위반해야 하는 경우, 작업 예외를 유지하기 때문에 `GetAwaiter().GetResult` 메서드를 호출하는 것이 좋습니다. 자세한 내용은 [Async All the Way](/archive/msdn-magazine/2013/march/async-await-best-practices-in-asynchronous-programming#async-all-the-way)(계속 비동기) 및 [Task Exception Handling in .NET 4.5](https://devblogs.microsoft.com/pfxteam/task-exception-handling-in-net-4-5/)(.NET 4.5의 작업 예외 처리)를 참조하세요.
- 가능하면 항상 `ConfigureAwait` 메서드를 사용하여 컨텍스트를 구별하지 않는 코드를 만듭니다. 컨텍스트를 구별하지 않는 코드는 모바일 애플리케이션에 더 나은 성능을 제공하며, 부분적으로 비동기 코드베이스로 작업할 때 교착 상태를 방지하는 데 유용한 기술입니다. 자세한 내용은 [컨텍스트 구성](/archive/msdn-magazine/2013/march/async-await-best-practices-in-asynchronous-programming#configure-context)을 참조하세요.
- 이전 비동기 작업에서 throw된 예외를 처리하는 기능 및 연속 작업 시작 전에 또는 실행 중에 해당 작업을 취소하는 기능에 *연속 작업*을 사용합니다. 자세한 내용은 [연속 작업을 사용하여 작업 연결](/dotnet/standard/parallel-programming/chaining-tasks-by-using-continuation-tasks)을 참조하세요.
- 비동기 작업이 `ICommand`에서 호출될 때 비동기 `ICommand` 구현을 사용합니다. 이렇게 하면 비동기 명령 논리의 모든 예외를 처리할 수 있습니다. 자세한 내용은 [비동기 프로그래밍: 비동기 MVVM 애플리케이션의 패턴: 명령](/archive/msdn-magazine/2014/april/async-programming-patterns-for-asynchronous-mvvm-applications-commands)을 참조하세요.

## <a name="choose-a-dependency-injection-container-carefully"></a>종속성 주입 컨테이너를 신중하게 선택

종속성 주입 컨테이너를 사용하면 모바일 애플리케이션에 추가 성능 제약 조건이 생깁니다. 컨테이너를 사용하여 형식을 등록하고 확인하는 경우, 특히 앱의 각 페이지 탐색에 대한 종속성을 다시 생성하는 경우 컨테이너의 리플렉션 사용으로 인해 성능비용이 발생합니다. 종속성이 많거나 깊은 경우에는 생성 비용이 많이 증가할 수 있습니다. 또한 일반적으로 애플리케이션 시작 중에 발생하는 형식 등록은 사용 중인 컨테이너에 따라 시작 시간에 상당한 영향을 미칠 수 있습니다.

또는 팩터리를 사용하여 수동으로 구현하여 종속성 주입을 보다 효율적으로 만들 수 있습니다.

## <a name="create-shell-applications"></a>셸 애플리케이션 만들기

Xamarin.Form 셸 애플리케이션에서는 플라이아웃 및 탭을 기반으로 한 독자적인 탐색 환경을 제공합니다. 애플리케이션 사용자 환경을 셸로 구현할 수 있는 경우 이 작업을 수행하는 것이 좋습니다. 셸 애플리케이션을 사용하면 [‘TabbedPage’](xref:Xamarin.Forms.TabbedPage)를 사용하는 애플리케이션에서 발생하는 애플리케이션 시작 시가 아닌 탐색에 대한 응답으로 요청 시 페이지를 만들기 때문에 잘못된 시작 환경을 방지하는 데 도움이 됩니다. 자세한 내용은 [Xamarin.Forms Shell](~/xamarin-forms/app-fundamentals/shell/index.md)을 참조하세요.

## <a name="use-collectionview-instead-of-listview"></a>ListView 대신 CollectionView 사용

[`CollectionView`](xref:Xamarin.Forms.CollectionView)는 다른 레이아웃 사양을 사용하여 데이터 목록을 표시하는 뷰입니다. [`ListView`](xref:Xamarin.Forms.ListView)를 대체하여 보다 유연하고 뛰어난 성능을 제공합니다. 자세한 내용은 [Xamarin.Forms CollectionView](~/xamarin-forms/user-interface/collectionview/index.md)를 참조하세요.

## <a name="optimize-listview-performance"></a>ListView 성능 최적화

[`ListView`](xref:Xamarin.Forms.ListView)를 사용할 때 최적화해야 하는 여러 가지 사용자 환경이 있습니다.

- **초기화** - 컨트롤을 만들 때부터 항목이 화면에 표시될 때까지의 시간 간격입니다.
- **스크롤** - 목록을 스크롤하는 기능이며, UI가 터치 제스처보다 지연되지 않도록 합니다.
- **상호 작용** - 항목을 추가, 삭제 및 선택합니다.

[`ListView`](xref:Xamarin.Forms.ListView) 컨트롤을 사용하려면 애플리케이션이 데이터 및 셀 템플릿을 제공해야 합니다. 이것이 구현되는 방식은 컨트롤의 성능에 큰 영향을 줍니다. 자세한 내용은 [ListView 성능](~/xamarin-forms/user-interface/listview/performance.md)을 참조하세요.

## <a name="optimize-image-resources"></a>이미지 리소스 최적화

이미지 리소스를 표시하면 애플리케이션의 메모리 사용 공간이 많이 증가할 수 있습니다. 따라서 필요한 경우에만 만들어야 하며, 애플리케이션에 더 이상 필요하지 않을 경우 즉시 해제되어야 합니다. 예를 들어 애플리케이션이 스트림에서 데이터를 읽어 이미지를 표시하는 경우, 필요할 경우에만 스트림이 생성되도록 하고, 더 이상 필요 없는 경우에는 스트림이 해제되도록 합니다. 이는 페이지가 생성될 때나 [`Page.Appearing`](xref:Xamarin.Forms.Page.Appearing) 이벤트가 실행될 때 스트림을 만든 후 [`Page.Disappearing`](xref:Xamarin.Forms.Page.Disappearing) 이벤트가 실행될 때 스트림을 폐기하는 방식으로 구현할 수 있습니다.

[`ImageSource.FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) 메서드를 사용하여 표시할 이미지를 다운로드할 때는 [`UriImageSource.CachingEnabled`](xref:Xamarin.Forms.UriImageSource.CachingEnabled) 속성을 `true`로 설정하여 다운로드된 이미지를 캐시에 저장합니다. 자세한 내용은 [이미지 작업](~/xamarin-forms/user-interface/images.md)을 참조하세요.

자세한 내용은 [이미지 리소스 최적화](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages)를 참조하세요.

## <a name="reduce-the-visual-tree-size"></a>시각적 트리 크기 줄이기

페이지의 요소 수를 줄이면 페이지 렌더러의 속도가 더 빨라집니다. 이는 크게 두 가지 방법으로 구현할 수 있습니다. 첫 번째는 표시되지 않는 요소를 숨기는 것입니다. 각 요소의 [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible) 속성은 해당 요소가 시각적 트리의 일부가 되어야 하는지 여부를 결정합니다. 따라서 요소가 다른 요소 뒤에 숨겨져 있어 표시되지 않을 경우 해당 요소를 제거하거나 `IsVisible` 속성을 `false`로 설정합니다.

두 번째 방법은 불필요한 요소를 제거하는 것입니다. 예를 들어 다음 코드 예제에서는 여러 [`Label`](xref:Xamarin.Forms.Label) 개체를 포함하는 페이지 레이아웃을 보여줍니다.

```xaml
<StackLayout>
    <StackLayout Padding="20,20,0,0">
        <Label Text="Hello" />
    </StackLayout>
    <StackLayout Padding="20,20,0,0">
        <Label Text="Welcome to the App!" />
    </StackLayout>
    <StackLayout Padding="20,20,0,0">
        <Label Text="Downloading Data..." />
    </StackLayout>
</StackLayout>
```

다음 코드 예제와 같이 더 적은 요소 수로 동일한 페이지 레이아웃을 유지 관리할 수 있습니다.

```xaml
<StackLayout Padding="20,35,20,20" Spacing="25">
  <Label Text="Hello" />
  <Label Text="Welcome to the App!" />
  <Label Text="Downloading Data..." />
</StackLayout>
```

## <a name="reduce-the-application-resource-dictionary-size"></a>애플리케이션 리소스 사전 크기 줄이기

애플리케이션에서 사용되는 모든 리소스는 중복을 피하기 위해 애플리케이션의 리소스 사전에 저장되어야 합니다. 이는 애플리케이션에서 구문 분석되어야 하는 XAML의 양을 줄이는 데 도움이 됩니다. 다음 코드 예제는 애플리케이션 전체에서 사용되므로 애플리케이션의 리소스 사전에 정의된 `HeadingLabelStyle` 리소스를 보여 줍니다.

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Resources.App">
     <Application.Resources>
         <ResourceDictionary>
            <Style x:Key="HeadingLabelStyle" TargetType="Label">
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="TextColor" Value="Red" />
            </Style>
         </ResourceDictionary>
     </Application.Resources>
</Application>
```

그러나 페이지에만 적용되는 XAML은 앱의 리소스 사전에 포함되어서는 안 됩니다. 그러면 애플리케이션 시작 시 페이지에서 요구할 경우 리소스가 대신 구문 분석되기 때문입니다. 시작 페이지가 아닌 페이지에서 사용하는 리소스는 애플리케이션이 시작될 때 구문 분석되는 XAML을 줄이기 위해 해당 페이지의 리소스 사전에 저장해야 합니다. 다음 코드 예제는 단일 페이지에만 있으므로 페이지의 리소스 사전에 정의된 `HeadingLabelStyle` 리소스를 보여 줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Test.HomePage"
             Padding="0,20,0,0">
    <ContentPage.Resources>
        <ResourceDictionary>
          <Style x:Key="HeadingLabelStyle" TargetType="Label">
              <Setter Property="HorizontalOptions" Value="Center" />
              <Setter Property="FontSize" Value="Large" />
              <Setter Property="TextColor" Value="Red" />
          </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

애플리케이션 리소스에 대한 자세한 내용은 [XAML 스타일](~/xamarin-forms/user-interface/styles/xaml/index.md)을 참조하세요.

## <a name="use-the-custom-renderer-pattern"></a>사용자 지정 렌더러 패턴 사용

대부분의 Xamarin.Forms 렌더러 클래스는 해당하는 네이티브 컨트롤을 렌더링하기 위해 Xamarin.Forms 사용자 지정 컨트롤이 생성될 때 호출되는 `OnElementChanged` 메서드를 노출합니다. 그러면 각 플랫폼 프로젝트에서 사용자 정의 렌더러 클래스가 이 메서드를 재정의하여 네이티브 컨트롤을 인스턴스화하고 사용자 지정합니다. `SetNativeControl` 메서드가 네이티브 컨트롤을 인스턴스화하는 데 사용되며, 이 메서드는 또한 `Control` 속성에 컨트롤 참조를 할당합니다.

그러나 일부 경우에는 `OnElementChanged` 메서드가 여러 번 호출될 수 있습니다. 따라서 성능에 영향을 줄 수 있는 메모리 누수를 방지하려면 새로운 네이티브 컨트롤을 인스턴스화할 때 주의를 기울여야 합니다. 사용자 지정 렌더러에서 새 네이티브 컨트롤을 인스턴스화할 때 사용하는 방법은 다음 코드 예제에 표시되어 있습니다.

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (e.OldElement != null)
  {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null)
  {
    if (Control == null)
    {
      // Instantiate the native control with the SetNativeControl method
    }
    // Configure the control and subscribe to event handlers
  }
}
```

`Control` 속성이 `null`일 경우 새 네이티브 컨트롤은 한 번만 인스턴스화되어야 합니다. 또한 사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결된 경우에만 컨트롤을 만들어서 구성하고 이벤트 처리기를 구독해야 합니다. 마찬가지로, 사용자 지정 렌더러가 변경된 항목에 연결된 경우 구독 대상 이벤트 처리기의 구독이 취소되어야 합니다. 이러한 방식을 도입하면 메모리 누수의 영향을 받지 않으며 효율적으로 작동하는 사용자 지정 렌더러를 만들 수 있습니다.

> [!IMPORTANT]
> `SetNativeControl` 메서드는 `e.NewElement` 속성이 `null`이 아니고 `Control` 속성이 `null`인 경우에만 호출해야 합니다.

사용자 지정 렌더러에 대한 자세한 내용은 [각 플랫폼에서 컨트롤 사용자 지정](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)을 참조하세요.

## <a name="related-links"></a>관련 링크

- [플랫폼 간 성능](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [XAML 컴파일](~/xamarin-forms/xaml/xamlc.md)
- [컴파일된 바인딩](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md)
- [빠른 렌더러](~/xamarin-forms/internals/fast-renderers.md)
- [레이아웃 압축](~/xamarin-forms/user-interface/layouts/layout-compression.md)
- [Xamarin.Forms Shell](~/xamarin-forms/app-fundamentals/shell/index.md)
- [Xamarin.Forms CollectionView](~/xamarin-forms/user-interface/collectionview/index.md)
- [ListView 성능](~/xamarin-forms/user-interface/listview/performance.md)
- [이미지 리소스 최적화](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages)
- [XAML 스타일](~/xamarin-forms/user-interface/styles/xaml/index.md)
- [각 플랫폼의 컨트롤 사용자 지정](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
