---
title: 플레이어에 비디오 소스 바인딩
description: 이 문서에서는 Xamarin.Forms를 사용하여 비디오 플레이어에 비디오 소스를 바인딩하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 504E0C7E-051A-4AF2-B654-BAB4D0957928
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 93383376c9167900bd69e43e8d83044bfdc3b607
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65924968"
---
# <a name="binding-video-sources-to-the-player"></a>플레이어에 비디오 소스 바인딩

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/CustomRenderers/VideoPlayerDemos/)

`VideoPlayer` 보기의 `Source` 속성이 새 비디오 파일로 설정되면 기본 비디오는 재생이 중지되고 새 비디오가 시작됩니다. 이는 [**VideoPlayerDemos**](https://developer.xamarin.com/samples/xamarin-forms/CustomRenderers/VideoPlayerDemos/) 샘플의 **웹 비디오 선택** 페이지에서 확인할 수 있습니다. 페이지에는 **App.xaml** 파일에서 참조된 세 비디오의 제목이 있는 `ListView`가 포함됩니다.

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

비디오를 선택하면 코드 숨김 파일의 `ItemSelected` 이벤트 처리기가 실행됩니다. 처리기는 제목에서 공백과 아포스트로피를 제거하고 이를 **App.xaml** 파일에 정의된 리소스 중 하나를 얻기 위한 키로 사용합니다. 그런 다음, `UriVideoSource` 개체가 `VideoPlayer`의 `Source` 속성으로 설정됩니다.

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

페이지가 처음 로드되면 `ListView`에서 항목이 선택되지 않으므로 재생을 시작하려면 비디오에 대해 한 항목을 선택해야 합니다.

[![웹 비디오 선택](source-bindings-images/selectwebvideo-small.png "웹 비디오 선택")](source-bindings-images/selectwebvideo-large.png#lightbox "웹 비디오 선택")

`VideoPlayer`의 `Source` 속성은 바인딩 가능한 속성에서 지원되므로 데이터 바인딩의 대상이 될 수 있습니다. 이 내용은 **VideoPlayer에 바인딩** 페이지에서 확인할 수 있습니다. **BindToVideoPlayer.xaml** 파일의 태그는 비디오 제목과 해당 `VideoSource` 개체를 캡슐화하는 다음 클래스에 의해 지원됩니다.

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

**BindToVideoPlayer.xaml** 파일의 `ListView`에는 각각 **App.xaml**에 있는 리소스 사전의 `UriVideoSource` 개체와 비디오 제목으로 초기화된 `VideoInfo` 개체 배열이 포함되어 있습니다.

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

`VideoPlayer`의 `Source` 속성은 `ListView`에 바인딩됩니다. 바인딩의 `Path`는 `SelectedItem.VideoSource`로 지정되며 이는 두 속성으로 구성된 복합 경로입니다. `SelectedItem`은 `ListView`의 속성입니다. 선택한 항목은 `VideoInfo` 형식이며 여기에는 `VideoSource` 속성이 포함됩니다.

처음 **웹 비디오 선택** 페이지에서처럼 초기에는 `ListView`에서 항목이 선택되지 않으므로 재생을 시작하기 전에 비디오 중 하나를 선택해야 합니다.


## <a name="related-links"></a>관련 링크

- [비디오 플레이어 데모(샘플)](https://developer.xamarin.com/samples/xamarin-forms/CustomRenderers/VideoPlayerDemos/)
