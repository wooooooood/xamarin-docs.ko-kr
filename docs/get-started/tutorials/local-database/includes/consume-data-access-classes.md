---
ms.openlocfilehash: 27a3393e6eda9f26ea15003edc5022246ff4deff
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67659858"
---
이 연습에서는 이전에 만든 데이터 액세스 클래스를 사용하는 사용자 인터페이스를 만듭니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **솔루션 탐색기**의 **LocalDatabaseTutorial** 프로젝트에서 **MainPage.xaml**을 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="LocalDatabaseTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Entry x:Name="nameEntry"
                   Placeholder="Enter name" />
            <Entry x:Name="ageEntry"
                   Placeholder="Enter age" />
            <Button Text="Add to Database"
                    Clicked="OnButtonClicked" />
            <ListView x:Name="listView">
                <ListView.ItemTemplate>
                    <DataTemplate>
                        <TextCell Text="{Binding Name}"
                                  Detail="{Binding Age}" />
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
        </StackLayout>
    </ContentPage>
    ```

    이 코드는 [`StackLayout`](xref:Xamarin.Forms.StackLayout)에서 [`Entry`](xref:Xamarin.Forms.Entry) 인스턴스 두 개, [`Button`](xref:Xamarin.Forms.Button) 한 개 및 [`ListView`](xref:Xamarin.Forms.ListView) 한 개로 구성된 페이지에 대한 사용자 인터페이스를 선언적으로 정의합니다. 각 `Entry`에는 해당하는 [`Placeholder`](xref:Xamarin.Forms.Entry.Placeholder) 속성 세트가 포함되며 여기서는 사용자 입력 이전에 표시되는 자리 표시자 텍스트를 지정합니다. `Button`은 해당하는 [`Clicked`](xref:Xamarin.Forms.Button.Clicked) 이벤트를 다음 단계에서 만들 `OnButtonClicked`라는 이벤트 처리기로 설정합니다. `ListView`는 해당하는 [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) 속성을 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)으로 설정합니다. 여기서는 [`TextCell`](xref:Xamarin.Forms.TextCell)을 사용하여 [`ListView`](xref:Xamarin.Forms.ListView)에 있는 각 행의 모양을 정의합니다. `TextCell` 데이터는 해당하는 [`Text`](xref:Xamarin.Forms.TextCell.Text) 및 [`Detail`](xref:Xamarin.Forms.TextCell.Detail) 속성을 `Person` 개체의 `Name` 및 `Age` 속성으로 각각 바인딩합니다.

    또한 [`Entry`](xref:Xamarin.Forms.Entry) 인스턴스 및 [`ListView`](xref:Xamarin.Forms.ListView)의 이름은 `x:Name` 특성을 사용하여 지정됩니다. 그러면 코드 숨김 파일이 할당된 이름을 사용하여 이러한 개체에 액세스할 수 있습니다.

1. **솔루션 탐색기**의 **LocalDatabaseTutorial** 프로젝트에서 **MainPage.xaml**을 확장하고 **MainPage.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml.cs**에서 `OnAppearing` 재정의 및 `OnButtonClicked` 이벤트 처리기를 클래스에 추가합니다.

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();
        listView.ItemsSource = await App.Database.GetPeopleAsync();
    }

    async void OnButtonClicked(object sender, EventArgs e)
    {
        if (!string.IsNullOrWhiteSpace(nameEntry.Text) && !string.IsNullOrWhiteSpace(ageEntry.Text))
        {
            await App.Database.SavePersonAsync(new Person
            {
                Name = nameEntry.Text,
                Age = int.Parse(ageEntry.Text)
            });

            nameEntry.Text = ageEntry.Text = string.Empty;
            listView.ItemsSource = await App.Database.GetPeopleAsync();
        }
    }
    ```

    `OnAppearing` 메서드는 [`ListView`](xref:Xamarin.Forms.ListView)를 데이터베이스에 저장된 데이터로 채웁니다. [`Button`](xref:Xamarin.Forms.Button)을 누를 때 실행되는 `OnButtonClicked` 메서드에서는 [`Entry`](xref:Xamarin.Forms.Entry) 인스턴스를 모두 지우고 `ListView`에 있는 데이터를 새로 고치기 전에 입력된 데이터를 데이터베이스에 저장합니다.

    > [!NOTE]
    > [`ContentPage`](xref:Xamarin.Forms.ContentPage)를 레이아웃하고 나서 표시되기 전에 `OnAppearing` 메서드 재정의가 실행됩니다. 따라서 여기에서 Xamarin.Forms 보기 콘텐츠를 설정하는 것이 좋습니다.

1. Visual Studio 도구 모음에서 선택한 원격 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    데이터의 여러 항목을 입력하고 데이터의 각 항목에 대한 [`Button`](xref:Xamarin.Forms.Button)을 누릅니다. 그러면 데이터를 데이터베이스에 저장하고, [`ListView`](xref:Xamarin.Forms.ListView)를 모든 데이터베이스 데이터로 다시 채웁니다.

    [![iOS 및 Android에서 로컬 SQLite.NET 데이터베이스 데이터 지속성의 스크린샷](../images/consume-data-access-classes.png "로컬 데이터베이스 데이터 지속성")](../images/consume-data-access-classes-large.png#lightbox "로컬 데이터베이스 데이터 지속성")

    Xamarin.Forms에서 로컬 데이터베이스에 대한 자세한 내용은 [Xamarin.Forms 로컬 데이터베이스(가이드)](~/xamarin-forms/data-cloud/data/databases.md)를 참조하세요.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **Solution Pad**의 **LocalDatabaseTutorial** 프로젝트에서 **MainPage.xaml**을 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="LocalDatabaseTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Entry x:Name="nameEntry"
                   Placeholder="Enter name" />
            <Entry x:Name="ageEntry"
                   Placeholder="Enter age" />
            <Button Text="Add to Database"
                    Clicked="OnButtonClicked" />
            <ListView x:Name="listView">
                <ListView.ItemTemplate>
                    <DataTemplate>
                        <TextCell Text="{Binding Name}"
                                  Detail="{Binding Age}" />
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
        </StackLayout>
    </ContentPage>
    ```

    이 코드는 [`StackLayout`](xref:Xamarin.Forms.StackLayout)에서 [`Entry`](xref:Xamarin.Forms.Entry) 인스턴스 두 개, [`Button`](xref:Xamarin.Forms.Button) 한 개 및 [`ListView`](xref:Xamarin.Forms.ListView) 한 개로 구성된 페이지에 대한 사용자 인터페이스를 선언적으로 정의합니다. 각 `Entry`에는 해당하는 [`Placeholder`](xref:Xamarin.Forms.Entry.Placeholder) 속성 세트가 포함되며 여기서는 사용자 입력 이전에 표시되는 자리 표시자 텍스트를 지정합니다. `Button`은 해당하는 [`Clicked`](xref:Xamarin.Forms.Button.Clicked) 이벤트를 다음 단계에서 만들 `OnButtonClicked`라는 이벤트 처리기로 설정합니다. `ListView`는 해당하는 [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) 속성을 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)으로 설정합니다. 여기서는 [`TextCell`](xref:Xamarin.Forms.TextCell)을 사용하여 [`ListView`](xref:Xamarin.Forms.ListView)에 있는 각 행의 모양을 정의합니다. `TextCell` 데이터는 해당하는 [`Text`](xref:Xamarin.Forms.TextCell.Text) 및 [`Detail`](xref:Xamarin.Forms.TextCell.Detail) 속성을 `Person` 개체의 `Name` 및 `Age` 속성으로 각각 바인딩합니다.

    또한 [`Entry`](xref:Xamarin.Forms.Entry) 인스턴스 및 [`ListView`](xref:Xamarin.Forms.ListView)의 이름은 `x:Name` 특성을 사용하여 지정됩니다. 그러면 코드 숨김 파일이 할당된 이름을 사용하여 이러한 개체에 액세스할 수 있습니다.

1. **Solution Pad**의 **LocalDatabaseTutorial** 프로젝트에서 **MainPage.xaml**을 확장하고 **MainPage.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml.cs**에서 `OnAppearing` 재정의 및 `OnButtonClicked` 이벤트 처리기를 클래스에 추가합니다.

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();
        listView.ItemsSource = await App.Database.GetPeopleAsync();
    }

    async void OnButtonClicked(object sender, EventArgs e)
    {
        if (!string.IsNullOrWhiteSpace(nameEntry.Text) && !string.IsNullOrWhiteSpace(ageEntry.Text))
        {
            await App.Database.SavePersonAsync(new Person
            {
                Name = nameEntry.Text,
                Age = int.Parse(ageEntry.Text)
            });

            nameEntry.Text = ageEntry.Text = string.Empty;
            listView.ItemsSource = await App.Database.GetPeopleAsync();
        }
    }
    ```

    `OnAppearing` 메서드는 [`ListView`](xref:Xamarin.Forms.ListView)를 데이터베이스에 저장된 데이터로 채웁니다. [`Button`](xref:Xamarin.Forms.Button)을 누를 때 실행되는 `OnButtonClicked` 메서드에서는 [`Entry`](xref:Xamarin.Forms.Entry) 인스턴스를 모두 지우고 `ListView`에 있는 데이터를 새로 고치기 전에 입력된 데이터를 데이터베이스에 저장합니다.

    > [!NOTE]
    > [`ContentPage`](xref:Xamarin.Forms.ContentPage)를 레이아웃하고 나서 표시되기 전에 `OnAppearing` 메서드 재정의가 실행됩니다. 따라서 여기에서 Xamarin.Forms 보기 콘텐츠를 설정하는 것이 좋습니다.

1. Mac용 Visual Studio 도구 모음에서 선택한 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    데이터의 여러 항목을 입력하고 데이터의 각 항목에 대한 [`Button`](xref:Xamarin.Forms.Button)을 누릅니다. 그러면 데이터를 데이터베이스에 저장하고, [`ListView`](xref:Xamarin.Forms.ListView)를 모든 데이터베이스 데이터로 다시 채웁니다.

    [![iOS 및 Android에서 로컬 SQLite.NET 데이터베이스 데이터 지속성의 스크린샷](../images/consume-data-access-classes.png "로컬 데이터베이스 데이터 지속성")](../images/consume-data-access-classes-large.png#lightbox "로컬 데이터베이스 데이터 지속성")

    Xamarin.Forms에서 로컬 데이터베이스에 대한 자세한 내용은 [Xamarin.Forms 로컬 데이터베이스(가이드)](~/xamarin-forms/data-cloud/data/databases.md)를 참조하세요.
