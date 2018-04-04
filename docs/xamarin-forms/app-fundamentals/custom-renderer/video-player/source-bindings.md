---
title: 플레이어에 게 비디오 소스 바인딩
ms.prod: xamarin
ms.assetid: 504E0C7E-051A-4AF2-B654-BAB4D0957928
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: bebf6fd905dd374822eb6974b28f1ac92a36c1bc
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="binding-video-sources-to-the-player"></a>플레이어에 게 비디오 소스 바인딩

경우는 `Source` 의 속성은 `VideoPlayer` 보기가 새 비디오 파일에 설정 된 기존 비디오 재생을 중지 하 고 새 비디오 시작 합니다. 이 확인할는 **웹 비디오 선택** 의 페이지는 [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) 샘플. 페이지에 포함 된 `ListView` 에서 참조 하는 세 개의 비디오의 제목으로는 **App.xaml** 파일:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.SelectWebVideoPage"
             Title="Select Web Video">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="2*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        
        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0" />

        <ListView Grid.Row="1"
                  ItemSelected="OnListViewItemSelected">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type x:String}">
                    <x:String>Elephant's Dream</x:String>
                    <x:String>Big Buck Bunny</x:String>
                    <x:String>Sintel</x:String>
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </Grid>
</ContentPage>
```

비디오 옵션을 선택 하면는 `ItemSelected` 코드 숨김 파일에 이벤트 처리기가 실행 됩니다. 제목에서 모든 공백 및 아포스트로피를 제거 하 고에 정의 된 리소스 중 하나를 가져오려면 키로 사용 하는 처리기는 **App.xaml** 파일입니다. `UriVideoSource` 개체가 다음으로 설정 되는 `Source` 속성은 `VideoPlayer`합니다.

```csharp
namespace VideoPlayerDemos
{
    public partial class SelectWebVideoPage : ContentPage
    {
        public SelectWebVideoPage()
        {
            InitializeComponent();
        }

        void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs args)
        {
            if (args.SelectedItem != null)
            {
                string key = ((string)args.SelectedItem).Replace(" ", "").Replace("'", "");
                videoPlayer.Source = (UriVideoSource)Application.Current.Resources[key];
            }
        }
    }
}
```

이 페이지가 처음 로드 될 때는 `ListView`이므로 비디오 재생을 시작 하려면 하나를 선택 해야 합니다.

[![웹 비디오 선택](source-bindings-images/selectwebvideo-small.png "웹 비디오 선택")](source-bindings-images/selectwebvideo-large.png#lightbox "웹 비디오를 선택 합니다.")

`Source` 속성 `VideoPlayer` 는에서 대상 데이터 바인딩을 사용할 수 있다는 것을 의미 하는 바인딩 가능한 속성을 지원 합니다. 이 확인할는 **VideoPlayer 바인딩할** 페이지. 태그는 **BindToVideoPlayer.xaml** 파일은 비디오와 그의 제목을 캡슐화 하는 다음 클래스에 의해 지원 `VideoSource` 개체:

```csharp
namespace VideoPlayerDemos
{
    public class VideoInfo
    {
        public string DisplayName { set; get; }

        public VideoSource VideoSource { set; get; }

        public override string ToString()
        {
            return DisplayName;
        }
    }
}
```

`ListView` 에 **BindToVideoPlayer.xaml** 이러한 배열을 파일에 포함 되어 `VideoInfo` 개체, 비디오 제목으로 초기화 하는 한 개의 각 및 `UriVideoSource` 개체에서 리소스 사전에서  **App.xaml**:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:VideoPlayerDemos"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.BindToVideoPlayerPage"
             Title="Bind to VideoPlayer">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="2*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0"
                           Source="{Binding Source={x:Reference listView},
                                            Path=SelectedItem.VideoSource}" />

        <ListView x:Name="listView"
                  Grid.Row="1">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type local:VideoInfo}">
                    <local:VideoInfo DisplayName="Elephant's Dream"
                                     VideoSource="{StaticResource ElephantsDream}" />

                    <local:VideoInfo DisplayName="Big Buck Bunny"
                                     VideoSource="{StaticResource BigBuckBunny}" />

                    <local:VideoInfo DisplayName="Sintel"
                                     VideoSource="{StaticResource Sintel}" />
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </Grid>
</ContentPage>
```

`Source` 속성은 `VideoPlayer` 바인딩되는 `ListView`합니다. `Path` 로 지정 된 바인딩의 `SelectedItem.VideoSource`, 두 가지 속성으로 구성 된 복합 경로: `SelectedItem` 의 속성인 `ListView`합니다. 선택한 항목은 형식의 `VideoInfo`을는 `VideoSource` 속성.

첫 번째와 마찬가지로 **웹 비디오 선택** 페이지에서 선택 된 항목이 처음에서 `ListView`이므로 재생을 시작 하기 전에 동영상 중 하나를 선택 해야 합니다.


## <a name="related-links"></a>관련 링크

- [비디오 플레이어 데모 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
