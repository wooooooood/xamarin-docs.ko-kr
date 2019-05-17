---
title: Xamarin.Forms Shell 탐색
description: Xamarin.Forms Shell 애플리케이션은 세트 탐색 계층 구조를 따르지 않고도 애플리케이션의 모든 페이지로 이동을 허용하는 URI 기반 탐색 환경을 이용할 수 있습니다.
ms.prod: xamarin
ms.assetid: 57079D89-D1CB-48BD-9FEE-539CEC29EABB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: b1709330678a201521fd226d473cd334bcd86f94
ms.sourcegitcommit: 9d90a26cbe13ebd106f55ba4a5445f28d9c18a1a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65054273"
---
# <a name="xamarinforms-shell-navigation"></a>Xamarin.Forms Shell 탐색

![](~/media/shared/preview.png "이 API는 현재 시험판임")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/Xaminals/)

Xamarin.Forms Shell에는 세트 탐색 계층 구조를 따르지 않고도 애플리케이션의 모든 페이지로 이동하는 데 경로를 사용하는 URI 기반 탐색 환경이 포함됩니다. 또한 탐색 스택의 모든 페이지를 방문하지 않고도 뒤로 이동할 수 있는 기능을 제공합니다.

`Shell`은 다음 탐색 관련 속성을 정의합니다.

- `BackButtonBehavior` 형식의 `BackButtonBehavior` - [뒤로] 단추의 동작을 정의하는 연결된 속성입니다.
- `FlyoutItem` 형식의 `CurrentItem` - 현재 선택된 `FlyoutItem`입니다.
- `ShellNavigationState` 형식의 `CurrentState` - `Shell`의 현재 탐색 상태입니다.
- `Shell` 형식의 `Current` - `Application.Current.MainPage`의 형식 캐스팅된 별칭입니다.

`BackButtonBehavior`, `CurrentItem` 및 `CurrentState` 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에서 지원되며, 이는 속성이 데이터 바인딩의 대상이 될 수 있음을 의미합니다.

`Shell` 클래스에서 `GoToAsync` 메서드를 호출하여 탐색을 수행합니다. 탐색이 수행되려고 하면 `Navigating` 이벤트가 실행되고 탐색이 완료되면 `Navigated` 이벤트가 실행됩니다.

> [!NOTE]
> [Navigation](xref:Xamarin.Forms.NavigableElement.Navigation) 속성을 사용하여 Xamarin.Forms Shell 애플리케이션에서 탐색을 계속 수행할 수 있습니다. 자세한 내용은 [계층적 탐색](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)을 참조하세요.

## <a name="routes"></a>경로

이동할 URI를 지정하여 셸 애플리케이션에서 탐색을 수행합니다. 탐색 URI에는 세 가지 구성 요소가 포함될 수 있습니다.

- 경로 - 셸 시각적 계층 구조의 일부로 존재하는 콘텐츠의 경로를 정의합니다.
- 페이지. 셸 시각적 계층 구조에 없는 페이지는 셸 애플리케이션 내의 어디에서나 탐색 스택으로 푸시할 수 있습니다. 예를 들어 항목 세부 정보 페이지는 셸 시각적 계층 구조에서 정의되지 않지만, 필요에 따라 탐색 스택으로 푸시할 수 있습니다.
- 하나 이상의 쿼리 매개 변수. 쿼리 매개 변수는 탐색하는 동안 대상 페이지에 전달할 수 있는 매개 변수입니다.

탐색 URI에 세 가지 구성 요소가 모두 포함되는 경우 구조체는 //route/page?queryParameters입니다.

### <a name="register-routes"></a>경로 등록

해당 `Route` 속성을 통해 `FlyoutItem`, `Tab` 및 `ShellContent` 개체에서 경로를 정의할 수 있습니다.

```xaml
<Shell ...>
    <FlyoutItem ...
                Route="animals">
        <Tab ...
             Route="domestic">
            <ShellContent ...
                          Route="cats" />
            <ShellContent ...
                          Route="dogs" />
        </Tab>
        <ShellContent ...
                      Route="monkeys" />
        <ShellContent ...
                      Route="elephants" />  
        <ShellContent ...
                      Route="bears" />
    </FlyoutItem>
    <ShellContent ...
                  Route="about" />                  
    ...
</Shell>
```

> [!NOTE]
> 셸 계층 구조의 모든 항목에는 연결된 경로가 있습니다. 개발자가 경로를 설정하지 않으면 런타임에 경로가 생성됩니다. 그러나 생성된 경로가 다른 애플리케이션 세션에서 일관된다는 보장은 없습니다.

이 예제에서는 샘플 애플리케이션에서 프로그래밍 방식 탐색에 사용할 수 있는 다음 경로 계층 구조를 만듭니다.

```
animals
  domestic
    cats
    dogs
  monkeys
  elephants
  bears
about
```

`dogs` 경로의 `ShellContent` 개체로 이동하려면 절대 경로 URI는 `//animals/domestic/dogs`입니다. 마찬가지로, `about` 경로의 `ShellContent` 개체로 이동하려면 절대 경로 URI는 `//about`입니다.

> [!IMPORTANT]
> 중복 경로 이름이 허용됩니다. 그러나 중복 경로는 허용되지 않습니다. 중복 경로가 검색되는 경우 애플리케이션 시작 시 `ArgumentException`이 throw됩니다.

#### <a name="register-page-routes"></a>페이지 경로 등록

셸 서브클래스 생성자 또는 경로를 호출하기 전에 실행되는 다른 위치에서는 셸 시각적 계층 구조에 표시되지 않는 모든 페이지에 대해 추가 경로를 명시적으로 등록할 수 있습니다.

```csharp
Routing.RegisterRoute("monkeydetails", typeof(MonkeyDetailPage));
Routing.RegisterRoute("beardetails", typeof(BearDetailPage));
Routing.RegisterRoute("catdetails", typeof(CatDetailPage));
Routing.RegisterRoute("dogdetails", typeof(DogDetailPage));
Routing.RegisterRoute("elephantdetails", typeof(ElephantDetailPage));
```

이 예제에서는 셸 서브클래스에 정의되지 않은 항목 세부 정보 페이지를 경로로 등록합니다. 그런 다음, 애플리케이션 내의 어디에서나 URI 기반 탐색을 사용하여 이 페이지로 이동할 수 있습니다. 해당 페이지의 경로를 전역 경로라고 합니다.

> [!NOTE]
> `Routing.RegisterRoute` 메서드를 사용하여 경로가 등록된 페이지는 필요한 경우 `Routing.UnRegisterRoute` 메서드를 사용하여 등록 취소할 수 있습니다.

또는 필요한 경우 다른 경로 계층 구조에서 페이지를 등록할 수 있습니다.

```csharp
Routing.RegisterRoute("monkeys/details", typeof(MonkeyDetailPage));
Routing.RegisterRoute("bears/details", typeof(BearDetailPage));
Routing.RegisterRoute("cats/details", typeof(CatDetailPage));
Routing.RegisterRoute("dogs/details", typeof(DogDetailPage));
Routing.RegisterRoute("elephants/details", typeof(ElephantDetailPage));
```

이 예제에서는 `monkeys` 경로에 대한 페이지에서 `details` 경로로 이동하면 `MonkeyDetailPage`가 표시되는 상황별 페이지 탐색을 사용하도록 설정합니다. 마찬가지로, `elephants` 경로에 대한 페이지에서 `details` 경로로 이동하면 `ElephantDetailPage`가 표시됩니다.

> [!IMPORTANT]
> 현재는 이전 등록을 덮어쓰는 중복 등록과 함께 `Routing.RegisterRoute` 메서드를 사용할 때 중복 경로 이름이 허용됩니다.

## <a name="perform-navigation"></a>탐색 수행

탐색을 수행하려면 먼저 `Shell` 서브클래스 참조를 가져와야 합니다. 이 참조는 `App.Current.MainPage` 속성을 `Shell` 개체로 캐스팅하거나 `Shell.Current` 속성을 통해 가져올 수 있습니다. 그런 다음 `Shell` 개체에서 `GoToAsync` 메서드를 호출하여 탐색을 수행할 수 있습니다. 이 메서드는 `ShellNavigationState`로 이동되고 탐색 애니메이션이 완료된 후 완료되는 `Task`를 반환합니다. `ShellNavigationState` 개체는 `string` 또는 `Uri`에서 `GoToAsync` 메서드를 통해 생성되고 해당 `Location` 속성이 `string` 또는 `Uri` 인수로 설정되어 있습니다.

셸 시각적 계층 구조의 경로로 이동하면 탐색 스택이 생성되지 않습니다. 그러나 셸 시각적 계층 구조에 없는 페이지로 이동하면 탐색 스택이 생성됩니다.

> [!NOTE]
> `Shell`의 현재 탐색 상태는 `Location` 속성에 표시된 경로의 URI가 포함된 `Shell.Current.CurrentState` 속성을 통해 검색할 수 있습니다.

### <a name="absolute-routes"></a>절대 경로

유효한 절대 URI를 `GoToAsync` 메서드의 인수로 지정하여 탐색을 수행할 수 있습니다.

```csharp
await Shell.Current.GoToAsync("//animals/monkeys");
```

이 예제에서는 `monkeys` 경로에 대한 페이지로 이동하고 경로는 `ShellContent` 개체에서 정의됩니다. `monkeys` 경로를 나타내는 `ShellContent` 개체는 경로가 `animals`인 `FlyoutItem` 개체의 자식입니다.

### <a name="relative-routes"></a>상대 경로

유효한 상대 URI를 `GoToAsync` 메서드의 인수로 지정하여 탐색을 수행할 수도 있습니다. 라우팅 시스템은 URI를 `ShellContent` 개체에 일치시키려고 합니다. 따라서 애플리케이션의 모든 경로가 고유할 경우 고유한 경로 이름을 상대 URI로 지정하기만 하면 탐색을 수행할 수 있습니다.

```csharp
await Shell.Current.GoToAsync("monkeydetails");
```

이 예제에서는 `monkeydetails` 경로에 대한 페이지로 이동합니다.

또한 다음 상대 경로 형식이 지원됩니다.

| 서식 | 설명 |
| --- | --- |
| //*route* | 지정된 경로에 대한 경로 계층 구조가 현재 표시된 경로에서 위쪽으로 검색됩니다. |
| ///*route* | 지정된 경로에 대한 경로 계층 구조가 현재 표시된 경로에서 아래쪽으로 검색됩니다. |

#### <a name="contextual-navigation"></a>상황별 탐색

상대 경로를 사용하여 상황별 탐색을 수행할 수 있습니다. 예를 들어 다음 경로 계층 구조를 고려합니다.

```
monkeys
  details
bears
  details
```

`monkeys` 경로의 등록된 페이지가 표시될 때 `details` 경로로 이동하면 `monkeys/details` 경로의 등록된 페이지가 표시됩니다. 마찬가지로, `bears` 경로의 등록된 페이지가 표시될 때 `details` 경로로 이동하면 `bears/details` 경로의 등록된 페이지가 표시됩니다. 이 예제에서 경로를 등록하는 방법에 대한 자세한 내용은 [페이지 경로 등록](#register-page-routes)을 참조하세요.

### <a name="invalid-routes"></a>잘못된 경로

다음 경로 형식은 유효하지 않습니다.

| 서식 | 설명 |
| --- | --- |
| *route* 또는 /*route* | 시각적 계층 구조의 경로는 탐색 스택으로 푸시할 수 없습니다. |
| //*page* 또는 ///*page* | 현재는 전역 경로가 탐색 스택의 유일한 페이지가 될 수 없습니다. 따라서 전역 경로에 대한 절대 라우팅은 지원되지 않습니다. |

이 경로 형식을 사용하면 `Exception`이 throw됩니다.

> [!IMPORTANT]
> 존재하지 않는 경로로 이동하려고 하면 `ArgumentException` 예외가 throw됩니다.

### <a name="debugging-navigation"></a>탐색 디버그

일부 셸 클래스는 디버거에서 클래스나 필드를 표시하는 방식을 지정하는 `DebuggerDisplayAttribute`로 데코레이팅됩니다. 이 특성을 사용하면 탐색 요청에 관련된 데이터를 표시하여 탐색 요청을 디버그할 수 있습니다. 예를 들어 다음 스크린샷은 `Shell.Current` 개체의 `CurrentItem` 및 `CurrentState` 속성을 보여 줍니다.

![디버거 스크린샷](navigation-images/debugger.png "디버거")

이 예제에서 `FlyoutItem` 형식의 `CurrentItem` 속성은 `FlyoutItem` 개체의 제목 및 경로를 표시합니다. 마찬가지로, `ShellNavigationState` 형식의 `CurrentState` 속성은 셸 애플리케이션 내에서 표시된 경로의 URI를 표시합니다.

### <a name="tab-class"></a>Tab 클래스

`Tab` 클래스는 `Tab` 내에서 현재 푸시한 탐색 스택을 나타내는 `IReadOnlyList<Page>` 형식의 `Stack` 속성을 정의합니다. 또한 이 클래스는 다음 재정의 가능한 탐색 메서드를 제공합니다.

- `GetNavigationStack` - 현재 탐색 스택인 `IReadOnlyList<Page`>를 반환합니다.
- `OnInsertPageBefore` - `INavigation.InsertPageBefore`가 호출될 때 호출됩니다.
- `OnPopAsync` - `Task<Page>`를 반환하고 `INavigation.PopAsync`가 호출될 때 호출됩니다.
- `OnPopToRootAsync` - `Task`를 반환하고 `INavigation.OnPopToRootAsync`가 호출될 때 호출됩니다.
- `OnPushAsync` - `Task`를 반환하고 `INavigation.PushAsync`가 호출될 때 호출됩니다.
- `OnRemovePage` - `INavigation.RemovePage`가 호출될 때 호출됩니다.

## <a name="navigation-events"></a>탐색 이벤트

`Shell` 클래스는 프로그래밍 방식 탐색 또는 사용자 조작으로 인해 탐색이 수행되려고 할 때 실행되는 `Navigating` 이벤트를 정의합니다. `Navigating` 이벤트와 함께 제공되는 `ShellNavigatingEventArgs` 개체는 다음 속성을 제공합니다.

| 속성 | 형식 | 설명 |
|---|---|---|
| 현재 | `ShellNavigationState` | 현재 페이지의 URI입니다. |
| 소스 | `ShellNavigationSource` | 발생한 탐색의 형식입니다. |
| Target | `ShellNavigationState`  | 탐색의 목적지를 나타내는 URI입니다. |
| CanCancel  | `bool` | 탐색을 취소할 수 있는지 여부를 나타내는 값입니다. |
| 취소됨  | `bool` | 탐색이 취소되었는지 여부를 나타내는 값입니다. |

또한 `ShellNavigatingEventArgs` 클래스는 탐색을 취소하는 데 사용될 수 있는 `Cancel` 메서드를 제공합니다.

> [!NOTE]
> `Navigated` 이벤트는 `Shell` 클래스에서 재정의 가능한 `OnNavigating` 메서드를 통해 실행됩니다.

`Shell` 클래스는 탐색이 완료될 때 실행되는 `Navigated` 이벤트도 정의합니다. `Navigating` 이벤트와 함께 제공되는 `ShellNavigatedEventArgs` 개체는 다음 속성을 제공합니다.

| 속성 | 형식 | 설명 |
|---|---|---|
| 현재 | `ShellNavigationState` | 현재 페이지의 URI입니다. |
| 이전| `ShellNavigationState` | 이전 페이지의 URI입니다. |
| 소스  | `ShellNavigationSource` | 발생한 탐색의 형식입니다. |

> [!NOTE]
> `Navigating` 이벤트는 `Shell` 클래스에서 재정의 가능한 `OnNavigated` 메서드를 통해 실행됩니다.

`ShellNavigatedEventArgs` 및 `ShellNavigatingEventArgs` 클래스에는 둘 다 `ShellNavigationSource` 형식의 `Source` 속성이 포함됩니다. 이 열거형은 다음 값을 제공합니다.

- `Unknown`
- `Push`
- `Pop`
- `PopToRoot`
- `Insert`
- `Remove`
- `ShellItemChanged`
- `ShellSectionChanged`
- `ShellContentChanged`

따라서 `Navigating` 이벤트의 처리기에서 탐색을 가로챌 수 있고 탐색 소스에 따라 작업을 수행합니다. 예를 들어 다음 코드는 페이지의 데이터가 저장되지 않은 경우 역방향 탐색을 취소하는 방법을 보여 줍니다.

```csharp
void OnNavigating(object sender, ShellNavigatingEventArgs e)
{
    // Cancel back navigation if data is unsaved
    if (e.Source == ShellNavigationSource.Pop && !dataSaved)
    {
        e.Cancel();
    }
}
```

## <a name="pass-data"></a>데이터 전달

프로그래밍 방식 탐색을 수행할 때 데이터를 쿼리 매개 변수로 전달할 수 있습니다. 예를 들어 다음 코드는 사용자가 `ElephantsPage`에서 코끼리를 선택할 때 샘플 애플리케이션에서 실행됩니다.

```csharp
async void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string elephantName = (e.CurrentSelection.FirstOrDefault() as Animal).Name;
    await Shell.Current.GoToAsync($"//animals/elephants/elephantdetails?name={elephantName}");
}
```

이 코드 예제는 `CollectionView`에서 현재 선택된 코끼리를 검색하고 `elephantdetails` 경로로 이동하여 `elephantName`을 쿼리 매개 변수로 전달합니다. 쿼리 매개 변수는 탐색용으로 인코딩된 URL이므로 “Indian Elephant”는 “Indian%20Elephant”가 됩니다.

데이터를 수신하려면 이동할 페이지를 나타내는 클래스 또는 페이지의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)에 대한 클래스가 각 쿼리 매개 변수에 대해 `QueryPropertyAttribute`로 데코레이팅되어야 합니다.

```csharp
[QueryProperty("Name", "name")]
public partial class ElephantDetailPage : ContentPage
{
    public string Name
    {
        set
        {
            BindingContext = ElephantData.Elephants.FirstOrDefault(m => m.Name == Uri.UnescapeDataString(value));
        }
    }
    ...
}
```

`QueryPropertyAttribute`의 첫 번째 인수는 데이터를 수신할 속성의 이름을 지정하고, 두 번째 인수는 쿼리 매개 변수 ID를 지정합니다. 따라서 위 예제에서 `QueryPropertyAttribute`는 `Name` 속성이 `GoToAsync` 메서드 호출의 URI에서 `name` 쿼리 매개 변수에 전달된 데이터를 수신하도록 지정합니다. `Name` 속성은 쿼리 매개 변수 값을 URL 디코드하고 이를 사용하여 페이지의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)를 표시될 개체로 설정합니다.

> [!NOTE]
> 클래스는 여러 `QueryPropertyAttribute` 개체로 데코레이팅할 수 있습니다.

## <a name="back-button-behavior"></a>뒤로 단추 동작

`BackButtonBehavior` 클래스는 [뒤로] 단추 모양 및 동작을 제어하는 다음 속성을 정의합니다.

- `ICommand` 형식의 `Command` - [뒤로] 단추를 누를 때 실행됩니다.
- `object` 형식의 `CommandParameter` - `Command`에 전달되는 매개 변수입니다.
- [`ImageSource`](xref:Xamarin.Forms.ImageSource) 형식의 `IconOveride` - [뒤로] 단추에 사용되는 아이콘입니다.
- `boolean` 형식의 `IsEnabled` - [뒤로] 단추를 사용할 수 있는지 여부를 나타냅니다. 기본값은 `true`입니다.
- `string` 형식의 `TextOverride` - [뒤로] 단추에 사용되는 텍스트입니다.

이 모든 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에서 지원되며, 이는 속성이 데이터 바인딩의 대상이 될 수 있음을 의미합니다.

`BackButtonBehavior` 클래스는 `Shell.BackButtonBehavior` 연결된 속성을 `BackButtonBehavior` 개체로 설정하여 사용할 수 있습니다.

```xaml
<ContentPage ...>    
    <Shell.BackButtonBehavior>
        <BackButtonBehavior Command="{Binding BackCommand}"
                            IconOverride="back.png" />   
    </Shell.BackButtonBehavior>
    ...
</ContentPage>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
Shell.SetBackButtonBehavior(this, new BackButtonBehavior
{
    Command = new Command(() =>
    {
        ...
    }),
    IconOverride = "back.png"
});
```

`Command` 속성은 [뒤로] 단추를 누를 때 실행할 `ICommand`로 설정되고, `IconOverride` 속성은 [뒤로] 단추에 사용되는 아이콘으로 설정됩니다.

[![iOS 및 Android에서 셸 뒤로 단추 아이콘 재정의의 스크린샷](navigation-images/back-button.png "셸 뒤로 단추 아이콘 재정의")](navigation-images/back-button-large.png#lightbox "셸 뒤로 단추 아이콘 재정의")

## <a name="related-links"></a>관련 링크

- [Xaminals(샘플)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/Xaminals/)
