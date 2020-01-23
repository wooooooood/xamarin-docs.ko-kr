---
ms.openlocfilehash: 2185fa243d2bccea046be5c91a2b1e9ed365edfe
ms.sourcegitcommit: 3f0e4f10e5def19122588bb05f26ab2baa9df6eb
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/23/2020
ms.locfileid: "74062907"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **MainPage.xaml**에서 [`ListView`](xref:Xamarin.Forms.ListView) 선언을 수정하여 [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) 및 [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) 이벤트에 대한 처리기를 설정합니다.

    ```xaml
    <ListView ItemsSource="{Binding Monkeys}"
              ItemSelected="OnListViewItemSelected"
              ItemTapped="OnListViewItemTapped" />
    ```

    이 코드는 [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) 이벤트를 `OnListViewItemSelected`라는 이벤트 처리기로 설정하고, [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) 이벤트를 `OnListViewItemTapped`라는 이벤트 처리기로 설정합니다. 두 가지 이벤트 처리기는 다음 단계에서 생성됩니다.

1. **솔루션 탐색기**의 **ListViewTutorial** 프로젝트에서 **MainPage.xaml**을 확장하고 **MainPage.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml.cs**에서 `OnListViewItemSelected` 및 `OnListViewItemTapped` 이벤트 처리기를 클래스에 추가합니다.

    ```csharp
    void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
    {
        Monkey selectedItem = e.SelectedItem as Monkey;
    }

    void OnListViewItemTapped(object sender, ItemTappedEventArgs e)
    {
        Monkey tappedItem = e.Item as Monkey;
    }
    ```

    [`ListView`](xref:Xamarin.Forms.ListView)의 항목을 탭하면 [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)과 [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) 이벤트가 발생하여 각각 `OnListViewItemSelected`과 `OnListItemTapped` 메서드를 실행합니다. 두 메서드에 대한 `sender` 인수는 이벤트의 실행을 담당하는 `ListView` 개체이며 `ListView` 개체에 액세스하는 데 사용될 수 있습니다. `OnListViewItemSelected` 메서드의 [`SelectedItemChangedEventArgs`](xref:Xamarin.Forms.SelectedItemChangedEventArgs) 인수는 선택된 항목을 제공하고 `OnListViewItemTapped` 메서드의 [`ItemTappedEventArgs`](xref:Xamarin.Forms.ItemTappedEventArgs) 인수는 탭된 항목을 제공합니다.

    > [!IMPORTANT]
    > [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) 이벤트는 [`ListView`](xref:Xamarin.Forms.ListView)에서 새 항목을 선택할 때만 발생합니다. 따라서 동일한 항목을 두 번 탭하면 두 개의 [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) 이벤트가 발생하지만 하나의 `ItemSelected` 이벤트만 발행합니다.

1. Visual Studio 도구 모음에서 선택한 원격 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    [![iOS 및 Android의 항목 선택 및 탭에 응답하는 ListView 스크린샷](../images/item-selection.png "ListView 항목 선택")](../images/item-selection-large.png#lightbox "ListView 항목 선택")

    두 이벤트 처리기에 중단점을 설정하고 [`ListView`](xref:Xamarin.Forms.ListView)에 있는 항목을 탭니다. [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) 이벤트는 [`ListView`](xref:Xamarin.Forms.ListView)에서 새 항목을 선택할 때만 발생하고, [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) 이벤트는 항목을 탭할 때마다 발생합니다.

    항목 선택 및 탭에 대한 자세한 내용은 [ListView 대화형 작업](~/xamarin-forms/user-interface/listview/interactivity.md) 안내서의 [선택 및 탭](~/xamarin-forms/user-interface/listview/interactivity.md#selection-and-taps)을 참조하세.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac용 Visual Studio](#tab/vsmac)

1. **MainPage.xaml**에서 [`ListView`](xref:Xamarin.Forms.ListView) 선언을 수정하여 [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) 및 [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) 이벤트에 대한 처리기를 설정합니다.

    ```xaml
    <ListView ItemsSource="{Binding Monkeys}"
              ItemSelected="OnListViewItemSelected"
              ItemTapped="OnListViewItemTapped" />
    ```

    이 코드는 [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) 이벤트를 `OnListViewItemSelected`라는 이벤트 처리기로 설정하고, [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) 이벤트를 `OnListViewItemTapped`라는 이벤트 처리기로 설정합니다. 두 가지 이벤트 처리기는 다음 단계에서 생성됩니다.

1. **Solution Pad**의 **ListViewTutorial** 프로젝트에서 **MainPage.xaml**을 확장하고 **MainPage.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml.cs**에서 `OnListViewItemSelected` 및 `OnListViewItemTapped` 이벤트 처리기를 클래스에 추가합니다.

    ```csharp
    void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
    {
        Monkey selectedItem = e.SelectedItem as Monkey;
    }

    void OnListViewItemTapped(object sender, ItemTappedEventArgs e)
    {
        Monkey tappedItem = e.Item as Monkey;
    }
    ```

    [`ListView`](xref:Xamarin.Forms.ListView)의 항목을 탭하면 [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)과 [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) 이벤트가 발생하여 각각 `OnListViewItemSelected`과 `OnListItemTapped` 메서드를 실행합니다. 두 메서드에 대한 `sender` 인수는 이벤트의 실행을 담당하는 `ListView` 개체이며 `ListView` 개체에 액세스하는 데 사용될 수 있습니다. `OnListViewItemSelected` 메서드의 [`SelectedItemChangedEventArgs`](xref:Xamarin.Forms.SelectedItemChangedEventArgs) 인수는 선택된 항목을 제공하고 `OnListViewItemTapped` 메서드의 [`ItemTappedEventArgs`](xref:Xamarin.Forms.ItemTappedEventArgs) 인수는 탭된 항목을 제공합니다.

    > [!IMPORTANT]
    > [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) 이벤트는 [`ListView`](xref:Xamarin.Forms.ListView)에서 새 항목을 선택할 때만 발생합니다. 따라서 동일한 항목을 두 번 탭하면 두 개의 [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) 이벤트가 발생하지만 하나의 `ItemSelected` 이벤트만 발행합니다.

1. Mac용 Visual Studio 도구 모음에서 선택한 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    [![iOS 및 Android의 항목 선택 및 탭에 응답하는 ListView 스크린샷](../images/item-selection.png "ListView 항목 선택")](../images/item-selection-large.png#lightbox "ListView 항목 선택")

    두 이벤트 처리기에 중단점을 설정하고 [`ListView`](xref:Xamarin.Forms.ListView)에 있는 항목을 탭니다. [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) 이벤트는 [`ListView`](xref:Xamarin.Forms.ListView)에서 새 항목을 선택할 때만 발생하고, [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) 이벤트는 항목을 탭할 때마다 발생합니다.

    항목 선택 및 탭에 대한 자세한 내용은 [선택 및 탭](~/xamarin-forms/user-interface/listview/interactivity.md#selection-and-taps)을 참조하세요.
