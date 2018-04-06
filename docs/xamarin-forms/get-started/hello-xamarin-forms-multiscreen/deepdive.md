---
title: Xamarin.Forms 멀티스크린 심층 분석
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: e4faa36c-6600-48c0-94c4-b4431103a4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/02/2016
ms.openlocfilehash: f4e6dace59c14a0e30ef4cdea2c7dd490d645057
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinforms-multiscreen-deep-dive"></a>Xamarin.Forms 멀티스크린 심층 분석

[Xamarin.Forms Multiscreen Quickstart](~/xamarin-forms/get-started/hello-xamarin-forms-multiscreen/quickstart.md)에서 Phoneword 응용 프로그램은 응용 프로그램에 대한 통화 기록을 추적하는 두 번째 화면을 포함하도록 확장되었습니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에서 페이지 탐색 및 데이터 바인딩에 대한 이해를 갖추기 위해 무엇이 빌드되었는지 검토합니다

## <a name="navigation"></a>탐색

Xamarin.Forms는 페이지 스택의 탐색 및 사용자 환경을 관리하는 기본 제공 탐색 모델을 제공합니다. 이 모델은 `Page` 개체의 LIFO(후입선출) 스택을 구현합니다. 한 페이지에서 다른 페이지로 이동하려면 응용 프로그램은 새 페이지를 이 스택으로 푸시합니다. 이전 페이지로 돌아가려면 응용 프로그램은 스택으로부터 현재 페이지를 팝합니다.

Xamarin.Forms에는 [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 개체 스택을 관리하는 [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 클래스가 있습니다. `NavigationPage` 클래스는 또한 제목을 표시하는 페이지의 맨 위에 탐색 모음을 추가하고 이전 페이지로 돌아가게 하는 플랫폼에 적절한 <span class="uiitem">뒤로</span> 단추를 추가합니다. 다음 코드 예제에서는 응용프로그램의 첫 번째 페이지 주위에 `NavigationPage`을 래핑하는 방법을 보여줍니다.

```csharp
public App ()
{
    ...
    MainPage = new NavigationPage (new MainPage ());
}
```

모든 [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 인스턴스에는 페이지 스택을 수정하는 메서드를 공개하는 [`Navigation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) 속성이 있습니다. 이들 메서드는 응용 프로그램에 [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)가 포함되는 경우에만 호출해야합니다. `CallHistoryPage`로 이동하려면 아래 코드 예제에서 설명한 것처럼 [`PushAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync/p/Xamarin.Forms.Page/) 메서드를 호출해야 합니다.

```csharp
async void OnCallHistory(object sender, EventArgs e)
{
    await Navigation.PushAsync (new CallHistoryPage ());
}
```

새 `CallHistoryPage` 개체가 탐색 스택으로 푸시됩니다. 프로그래밍 방식으로 원래 페이지로 돌아가려면 `CallHistoryPage` 개체는 아래 코드 예제에서 설명한 것처럼 [`PopAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) 메서드를 호출해야 합니다.

```csharp
await Navigation.PopAsync();
```

그러나 Phoneword 응용 프로그램에서 [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 클래스가 탐색 모음을 이전 페이지로 돌아가게 하는 플랫폼에 적절한 <span class="uiitem">뒤로</span> 단추를 포함하는 페이지의 맨 위에 추가하는데 이 코드가 필요하지 않습니다.

## <a name="data-binding"></a>데이터 바인딩

데이터 바인딩은 Xamarin.Forms 응용 프로그램이 데이터를 나타내고 해당 데이터와 상호 작용하는 방법을 단순화하기 위해 사용됩니다. 그것을 통해 사용자 인터페이스와 기본 응용 프로그램 간에 연결이 설정됩니다. [`BindableObject`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) 클래스에는 데이터 바인딩을 지원하는 인프라의 대부분이 담겨 있습니다.

데이터 바인딩은 두 개체 간의 관계를 정의합니다. *원본* 개체는 데이터를 제공합니다. *대상* 개체는 원본 개체의 데이터를 사용(하고 흔히 표시)합니다. Phoneword 응용 프로그램에서 바인딩 대상은 전화번호를 표시하는 [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 컨트롤인 한편 `PhoneNumbers` 컬렉션은 바인딩 소스입니다.

`PhoneNumbers` 컬렉션은 다음 코드 예제와 같이 `App` 클래스에 선언되고 초기화됩니다.

```csharp
public partial class App : Application
{
   public static List<string> PhoneNumbers { get; set; }

   public App ()
   {
     PhoneNumbers = new List<string>();
     ...
   }
   ...
}
```

[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 인스턴스는 다음 코드 예제와 같이 `CallHistoryPage` 클래스에 선언되고 초기화됩니다.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage ...
             xmlns:local="clr-namespace:Phoneword;assembly=Phoneword"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             ...>
    ...
    <ContentPage.Content>
       ...
       <ListView ItemsSource="{x:Static local:App.PhoneNumbers}" />
       ...
    </ContentPage.Content>
</ContentPage>
```

이 예제에서 [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 컨트롤은 [`ItemsSource`](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView.ItemsSource/) 속성이 바인딩된 `IEnumerable` 데이터 컬렉션을 표시합니다. 데이터 컬렉션은 모든 종류의 개체일 수 있지만, 기본적으로 `ListView`는 각 항목의 `ToString` 메서드를 사용하여 해당 항목을 표시합니다. [`x:Static`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/) 태그 확장은 `ItemsSource` 속성이 `local` 네임스페이스에 위치할 수 있는 `App` 클래스의 정적인 `PhoneNumbers` 속성에 바인딩될 것임을 나타내기 위해 사용됩니다.

데이터 바인딩에 대한 자세한 내용은 [데이터 바인딩 기본](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)을 참조하세요. XAML 태그 확장에 대한 자세한 내용은 [XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)을 참조하세요.

## <a name="additional-concepts-introduced-in-phoneword"></a>Phoneword에 도입된 추가 개념

[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)은 항목 컬렉션의 화면 표시를 담당합니다. `ListView`의 각 항목은 단일 셀에 포함됩니다. `ListView` 컨트롤 사용에 대한 자세한 내용은 [ListView](~/xamarin-forms/user-interface/listview/index.md)를 참조하세요.

## <a name="summary"></a>요약

이 아티클은 페이지 탐색 및 데이터 바인딩을 Xamarin.Forms 응용 프로그램에 도입하고 다중 화면 플랫폼 간 응용 프로그램에서의 용도를 설명합니다.
