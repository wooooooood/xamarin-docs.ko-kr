---
title: 응용 프로그램 리소스 비디오를 로드합니다.
ms.prod: xamarin
ms.assetid: F75BD540-9354-4C17-A119-57F3DEC66D54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: a9dbf77b85fe7f39c152d4dfb33bdd44c72dca40
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="loading-application-resource-videos"></a>응용 프로그램 리소스 비디오를 로드합니다.

에 대 한 사용자 지정 렌더러는 `VideoPlayer` 보기는 응용 프로그램 리소스로 개별 플랫폼 프로젝트에 포함 된 비디오 파일을 재생할 수 있습니다. 그러나 현재 버전의 `VideoPlayer` 이식 가능한 클래스 라이브러리에 포함 된 리소스에 액세스할 수 없습니다.

인스턴스를 만들고 이러한 리소스를 로드 하려면 `ResourceVideoSource` 설정 하 여는 `Path` 파일 이름 (또는 폴더와 파일 이름) 리소스의 속성입니다. 정적을 호출할 수 또는 `VideoSource.FromResource` 리소스를 참조 하는 메서드. 다음 설정의 `ResourceVideoSource` 개체를 `Source` 속성 `VideoPlayer`합니다. 

## <a name="storing-the-video-files"></a>비디오 파일을 저장합니다.

플랫폼 프로젝트에 비디오 파일을 저장 하는 것은 세 플랫폼에 따라 다릅니다.

### <a name="ios-video-resources"></a>iOS 비디오 리소스

IOS 프로젝트에 비디오를 저장할 수 있습니다는 **리소스** 폴더 또는 하위 폴더는 **리소스** 폴더입니다. 비디오 파일 있어야는 `Build Action` 의 `BundleResource`합니다. 설정의 `Path` 속성 `ResourceVideoSource` 파일 이름으로, 예를 들어 **MyFile.mp4** 파일에 대 한는 **리소스** 폴더 또는 **MyFolder/MyFile.mp4**, 여기서 **MyFolder** 의 하위 폴더가 **리소스**합니다.

에 **VideoPlayerDemos** 솔루션을는 **VideoPlayerDemos.iOS** 프로젝트의 하위 폴더 포함 **리소스** 라는 **비디오** 명명 된 파일이 들어 있는 **iOSApiVideo.mp4**합니다. 이것은 iOS에 대 한 설명서를 찾을 수 Xamarin 웹 사이트를 사용 하는 방법을 보여 주는 짧은 비디오 `AVPlayerViewController` 클래스입니다.

### <a name="android-video-resources"></a>Android 비디오 리소스

Android 프로젝트, 비디오의 하위 폴더에 저장 되어 있어야 **리소스** 라는 **원시**합니다. **원시** 폴더 하위 폴더를 포함할 수 없습니다. 비디오 파일는 `Build Action` 의 `AndroidResource`합니다. 설정의 `Path` 속성 `ResourceVideoSource` 파일 이름으로, 예를 들어 **MyFile.mp4**합니다. 

**VideoPlayerDemos.Android** 프로젝트의 하위 폴더 포함 **리소스** 라는 **원시**, 라는 파일에 포함 된 **AndroidApiVideo.mp4**. 

### <a name="uwp-video-resources"></a>UWP 비디오 리소스

유니버설 Windows 플랫폼 프로젝트에서는 프로젝트의 모든 폴더에서 비디오를 저장할 수 있습니다. 파일을 `Build Action` 의 `Content`합니다. 설정의 `Path` 속성 `ResourceVideoSource` 폴더와 파일 이름, 예를 들어 **MyFolder/MyVideo.mp4**합니다. 

**VideoPlayerDemos.UWP** 프로젝트 라는 폴더가 **비디오** 파일 **UWPApiVideo.mp4**합니다.

## <a name="loading-the-video-files"></a>비디오 파일 로드

코드를 포함 하는 각 플랫폼 렌더러 클래스 해당 `SetSource` 리소스로 저장 된 비디오 파일을 로드 하기 위한 방법입니다.

### <a name="ios-resource-loading"></a>iOS 리소스 로드

IOS 버전의 `VideoPlayerRenderer` 사용 하 여는 `GetUrlForResource` 메서드 `NSBundle` 리소스를 로드 합니다. 전체 경로 파일 이름, 확장명 및 디렉터리도 나눌 수 있어야 합니다. 코드를 사용 하 여는 `Path` .NET 클래스 `System.IO` 이러한 구성 요소 프로젝트로 분리 하는 파일 경로 대 한 네임 스페이스:

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

                if (!String.IsNullOrWhitespace(path))
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

Android `VideoPlayerRenderer` 파일 이름 및 패키지 이름을 사용 하 여 생성 한 `Uri` 개체입니다. 패키지 이름은 응용 프로그램의 이름을 경우 **VideoPlayerDemos.Android**, 정적에서 가져올 수 있는 `Context.PackageName` 속성입니다. 결과 `Uri` 에 다음 전달 된 개체는 `SetVideoURI` 방식의 `VideoView`:

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

UWP `VideoPlayerRenderer` 생성 한 `Uri` 경로 대 한 개체를 설정 하 고는 `Source` 속성의 `MediaElement`:

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

## <a name="playing-the-resource-file"></a>리소스 파일을 재생

**비디오 리소스 재생** 페이지에 **VideoPlayerDemos** 솔루션에서 사용 하 여 `OnPlatform` 각 플랫폼에 대 한 비디오 파일을 지정 하는 클래스:

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

IOS 리소스에 저장 된 경우는 **리소스** 폴더, 리소스 UWP 프로젝트의 루트 폴더에 저장 된 경우 세 플랫폼에 같은 파일 이름을 사용할 수 있습니다. 경우에 해당 되는 경우 다음에 직접 해당 이름을 설정할 수 있습니다는 `Source` 속성 `VideoPlayer`합니다. 

세 가지 플랫폼에서 실행 되 고 해당 페이지는 다음과 같습니다.

[![비디오 리소스 재생](loading-resources-images/playvideoresource-small.png "비디오 리소스 재생")](loading-resources-images/playvideoresource-large.png#lightbox "리소스 비디오 재생")

이제 살펴 보았으며 하는 방법을 [웹 URI에서 비디오를 로드할](web-videos.md) 및 포함 된 리소스를 재생 하는 방법입니다. 수 또한 [비디오 장치의 비디오 라이브러리에서 로드](accessing-library.md)합니다.


## <a name="related-links"></a>관련 링크

- [비디오 플레이어 데모 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
