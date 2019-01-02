---
title: 애플리케이션 리소스 비디오 로드
description: 이 문서에서는 Xamarin.Forms를 사용하여 비디오 플레이어 애플리케이션에서 애플리케이션 리소스로 저장된 비디오를 로드하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: F75BD540-9354-4C17-A119-57F3DEC66D54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 0fb9ed06ef58c4350f479021f0c18e48c693cf7f
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53059884"
---
# <a name="loading-application-resource-videos"></a>애플리케이션 리소스 비디오 로드

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)

`VideoPlayer` 보기에 대한 사용자 지정 렌더러는 개별 플랫폼 프로젝트에 애플리케이션 리소스로 포함된 비디오 파일을 재생할 수 있습니다. 그러나 현재 버전의 `VideoPlayer`는 .NET Standard 라이브러리에 포함된 리소스에 액세스할 수 없습니다.

이러한 리소스를 로드하려면 `Path` 속성을 리소스의 파일 이름(또는 폴더 및 파일 이름)로 설정하여 `ResourceVideoSource`라는 인스턴스를 만듭니다 또는 정적 `VideoSource.FromResource` 메서드를 호출하여 리소스를 참조할 수 있습니다. 그런 다음, `ResourceVideoSource` 개체를 `VideoPlayer`의 `Source` 속성으로 설정합니다.

## <a name="storing-the-video-files"></a>비디오 파일 저장

플랫폼 프로젝트에서 비디오 파일을 저장하는 작업은 각 플랫폼마다 다릅니다.

### <a name="ios-video-resources"></a>iOS 비디오 리소스

iOS 프로젝트의 **Resources** 폴더 또는 **Resources** 폴더의 하위 폴더에 비디오를 저장할 수 있습니다. 비디오 파일에는 `BundleResource`의 `Build Action`이 포함되어야 합니다. `ResourceVideoSource`의 `Path` 속성을 파일 이름으로 설정합니다(예: **Resources** 폴더의 파일의 경우 **MyFile.mp4** 또는 **MyFolder**가 **Resources**의 하위 폴더인 경우 **MyFolder/MyFile.mp4**).

**VideoPlayerDemos** 솔루션에서 **VideoPlayerDemos.iOS** 프로젝트에는 **iOSApiVideo.mp4**라는 파일이 포함된 **Videos**라는 **Resources**의 하위 폴더가 포함됩니다. Xamarin 웹 사이트를 사용하여 iOS `AVPlayerViewController` 클래스에 대한 설명서를 찾는 방법을 보여주는 짧은 비디오입니다.

### <a name="android-video-resources"></a>Android 비디오 리소스

Android 프로젝트에서 비디오는 **raw**라는 **Resources**의 하위 폴더에 저장되어야 합니다. **raw** 폴더에는 하위 폴더가 포함될 수 없습니다. 비디오 파일에 `AndroidResource`의 `Build Action`을 추가합니다. `ResourceVideoSource`의 `Path` 속성을 파일 이름으로 설정합니다(예: **MyFile.mp4**).

**VideoPlayerDemos.Android** 프로젝트에는 **raw**라는 **Resources**의 하위 폴더가 포함되며 여기에는 **AndroidApiVideo.mp4**라는 파일이 포함됩니다.

### <a name="uwp-video-resources"></a>UWP 비디오 리소스

유니버설 Windows 플랫폼 프로젝트에서는 프로젝트의 폴더에 비디오를 저장할 수 있습니다. 파일에 `Content`의 `Build Action`을 추가합니다. `ResourceVideoSource`의 `Path` 속성을 폴더 및 파일 이름으로 설정합니다(예: **MyFolder/MyVideo.mp4**).

**VideoPlayerDemos.UWP** 프로젝트에는 **UWPApiVideo.mp4** 파일이 있는 **Videos**라는 폴더가 포함됩니다.

## <a name="loading-the-video-files"></a>비디오 파일 로드

각 플랫폼 렌더러 클래스에는 리소스로 저장된 비디오 파일을 로드하기 위해 해당 `SetSource` 메서드의 코드가 포함됩니다.

### <a name="ios-resource-loading"></a>iOS 리소스 로드

iOS 버전의 `VideoPlayerRenderer`는 리소스를 로드하기 위해 `NSBundle`의 `GetUrlForResource` 메서드를 사용합니다. 전체 경로는 파일 이름, 확장명 및 디렉터리로 나눠져야 합니다. 코드는 파일 경로를 다음과 같은 구성 요소로 구분하기 위해 .NET `System.IO` 네임스페이스에서 `Path` 클래스를 사용합니다.

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        void SetSource()
        {
            AVAsset asset = null;
            ···
            else if (Element.Source is ResourceVideoSource)
            {
                string path = (Element.Source as ResourceVideoSource).Path;

                if (!String.IsNullOrWhiteSpace(path))
                {
                    string directory = Path.GetDirectoryName(path);
                    string filename = Path.GetFileNameWithoutExtension(path);
                    string extension = Path.GetExtension(path).Substring(1);
                    NSUrl url = NSBundle.MainBundle.GetUrlForResource(filename, extension, directory);
                    asset = AVAsset.FromUrl(url);
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="android-resource-loading"></a>Android 리소스 로드

Android `VideoPlayerRenderer`는 파일 이름 및 패키지 이름을 사용하여 `Uri` 개체를 생성합니다. 패키지 이름은 애플리케이션의 이름(이 경우 **VideoPlayerDemos.Android**)이며 정적 `Context.PackageName` 속성에서 가져올 수 있습니다. 그런 다음, 결과 `Uri` 개체를 `VideoView`의 `SetVideoURI` 메서드에 전달합니다.

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···    
        void SetSource()
        {
            isPrepared = false;
            bool hasSetSource = false;
            ···
            else if (Element.Source is ResourceVideoSource)
            {
                string package = Context.PackageName;
                string path = (Element.Source as ResourceVideoSource).Path;

                if (!String.IsNullOrWhiteSpace(path))
                {
                    string filename = Path.GetFileNameWithoutExtension(path).ToLowerInvariant();
                    string uri = "android.resource://" + package + "/raw/" + filename;
                    videoView.SetVideoURI(Android.Net.Uri.Parse(uri));
                    hasSetSource = true;
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="uwp-resource-loading"></a>UWP 리소스 로드

UWP `VideoPlayerRenderer`는 경로에 대해 `Uri` 개체를 생성하고 `MediaElement`의 `Source` 속성으로 설정합니다.

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        async void SetSource()
        {
            bool hasSetSource = false;
            ···
            else if (Element.Source is ResourceVideoSource)
            {
                string path = "ms-appx:///" + (Element.Source as ResourceVideoSource).Path;

                if (!String.IsNullOrWhiteSpace(path))
                {
                    Control.Source = new Uri(path);
                    hasSetSource = true;
                }
            }
        }
        ···
    }
}
```

## <a name="playing-the-resource-file"></a>리소스 파일 재생

**VideoPlayerDemos** 솔루션의 **비디오 리소스 재생** 페이지에서는 `OnPlatform` 클래스를 사용하여 각 플랫폼에 대한 비디오 파일을 지정합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayVideoResourcePage"
             Title="Play Video Resource">
    <video:VideoPlayer>
        <video:VideoPlayer.Source>
            <video:ResourceVideoSource>
                <video:ResourceVideoSource.Path>
                    <OnPlatform x:TypeArguments="x:String">
                        <On Platform="iOS" Value="Videos/iOSApiVideo.mp4" />
                        <On Platform="Android" Value="AndroidApiVideo.mp4" />
                        <On Platform="UWP" Value="Videos/UWPApiVideo.mp4" />
                    </OnPlatform>
                </video:ResourceVideoSource.Path>
            </video:ResourceVideoSource>
        </video:VideoPlayer.Source>
    </video:VideoPlayer>
</ContentPage>
```

iOS 리소스를 **Resources** 폴더에 저장한 경우 및 UWP 리소스를 프로젝트의 루트 폴더에 저장한 경우 각 플랫폼에 동일한 파일을 사용할 수 있습니다. 이 경우에 해당 이름을 직접 `VideoPlayer`의 `Source` 속성으로 설정할 수 있습니다.

실행되는 해당 페이지는 다음과 같습니다.

[![비디오 리소스 재생](loading-resources-images/playvideoresource-small.png "비디오 리소스 재생")](loading-resources-images/playvideoresource-large.png#lightbox "리소스 비디오 재생")

이제 [웹 URI에서 비디오를 로드](web-videos.md)하는 방법 및 포함 리소스를 재생하는 방법을 살펴보았습니다. 또한 [디바이스의 비디오 라이브러리에서 비디오를 로드](accessing-library.md)할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [비디오 플레이어 데모(샘플)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
