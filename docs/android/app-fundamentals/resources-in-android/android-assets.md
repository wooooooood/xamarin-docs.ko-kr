---
title: Android 자산 사용
ms.prod: xamarin
ms.assetid: 70ECDDC9-FA40-03B4-BF04-E7CFFFE4260D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/13/2018
ms.openlocfilehash: f8a542b58fa891b63f43d1c87dea911b83e01949
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68509316"
---
# <a name="using-android-assets"></a>Android 자산 사용

_자산은_ 텍스트, xml, 글꼴, 음악, 비디오 등의 임의의 파일을 응용 프로그램에 포함 하는 방법을 제공 합니다. 이러한 파일을 "리소스"로 포함 하려고 하면 Android에서 해당 파일을 리소스 시스템으로 처리 하므로 원시 데이터를 가져올 수 없습니다. 데이터에 액세스 하려는 경우 자산이이 작업을 수행 하는 한 가지 방법입니다.

프로젝트에 추가 된 자산은 [Assetmanager](xref:Android.Content.Res.AssetManager)를 사용 하 여 응용 프로그램에서 읽을 수 있는 파일 시스템과 똑같이 표시 됩니다.
이 간단한 데모에서는 텍스트 파일 자산을 프로젝트에 추가 하 고,을 사용 하 여 `AssetManager`읽고,이를 TextView에 표시 하겠습니다.


## <a name="add-asset-to-project"></a>프로젝트에 자산 추가

자산은 프로젝트의 `Assets` 폴더에 있습니다. 라는 `read_asset.txt`폴더에 새 텍스트 파일을 추가 합니다. "자산에서 제공 했습니다!"와 같은 텍스트를 입력 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio에서이 파일에 대 한 **빌드 작업** 을 **Androidasset**로 설정 해야 합니다.

![빌드 작업을 AndroidAsset로 설정](android-assets-images/asset-properties-vs.png) 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

이 파일에 대 한 **빌드 작업** 을 **Androidasset**로 설정 Mac용 Visual Studio 해야 합니다.

[![빌드 작업을 AndroidAsset로 설정](android-assets-images/asset-properties-xs-sml.png)](android-assets-images/asset-properties-xs.png#lightbox)

-----

올바른 **빌드** 를 선택 하면 컴파일 타임에 파일이 apk로 패키지 됩니다.


## <a name="reading-assets"></a>자산 읽기

자산은 [Assetmanager](xref:Android.Content.Res.AssetManager)를 사용 하 여 읽습니다. 활동 등의 `AssetManager` [자산](xref:Android.Content.Context.Assets) 속성 `Android.Content.Context`에 액세스 하 여의 인스턴스를 사용할 수 있습니다.
다음 코드에서는 **read_asset** 자산을 열고 콘텐츠를 읽은 다음 TextView를 사용 하 여 표시 합니다.

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Create a new TextView and set it as our view
    TextView tv = new TextView (this);
    
    // Read the contents of our asset
    string content;
    AssetManager assets = this.Assets;
    using (StreamReader sr = new StreamReader (assets.Open ("read_asset.txt")))
    {
        content = sr.ReadToEnd ();
    }

    // Set TextView.Text to our asset content
    tv.Text = content;
    SetContentView (tv);
}
```


## <a name="running-the-application"></a>애플리케이션 실행

응용 프로그램을 실행 하면 다음이 표시 됩니다.

![예제 스크린 샷](android-assets-images/screenshot.png)


## <a name="related-links"></a>관련 링크

- [AssetManager](xref:Android.Content.Res.AssetManager)
- [측면](xref:Android.Content.Context)
