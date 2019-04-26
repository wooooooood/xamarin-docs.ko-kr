---
title: Android 자산 사용
ms.prod: xamarin
ms.assetid: 70ECDDC9-FA40-03B4-BF04-E7CFFFE4260D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/13/2018
ms.openlocfilehash: 1e9a71de7725c8382e133d85977407bcc859fc58
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61013735"
---
# <a name="using-android-assets"></a>Android 자산 사용

_자산_ 응용 프로그램에서 텍스트, xml, 글꼴, 음악 및 비디오와 같은 임의 파일을 포함 하는 방법을 제공 합니다. "Resources"으로 이러한 파일을 포함 하려는 경우 Android 해당 리소스 시스템으로 처리 됩니다 및 원시 데이터를 가져올 수 없습니다. 변경 되지 않은 데이터에 액세스 하려는 경우 작업을 수행 하는 한 가지 방법은 자산은입니다.

프로젝트에 추가 하는 자산으로 사용 하 여 응용 프로그램에서 읽을 수 있는 파일 시스템 처럼 표시 됩니다 [AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/)합니다.
이 간단한 데모를 사용 하 여 읽을 텍스트 파일 자산 프로젝트를 추가할 예정 `AssetManager`, TextView를에 표시할 수 있습니다.


## <a name="add-asset-to-project"></a>프로젝트에 자산 추가

자산 이동 된 `Assets` 프로젝트의 폴더입니다. 라는이 폴더에 새 텍스트 파일을 추가 `read_asset.txt`합니다. "또 자산에서!"와 같은 일부 텍스트를 배치 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio 설정 해야 합니다 **빌드 작업** 이 파일이 **AndroidAsset**:

![빌드 작업 AndroidAsset을로 설정](android-assets-images/asset-properties-vs.png) 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Mac 용 visual Studio 설정 해야 합니다 **빌드 작업** 이 파일이 **AndroidAsset**:

[![빌드 작업 AndroidAsset을로 설정](android-assets-images/asset-properties-xs-sml.png)](android-assets-images/asset-properties-xs.png#lightbox)

-----

올바른 선택 **BuildAction** 컴파일 타임에 파일 APK에 패키지 수를 확인 합니다.


## <a name="reading-assets"></a>자산 읽기

사용 하 여 자산 읽힙니다.을 [AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/)합니다. 인스턴스를 `AssetManager` 를 액세스 하 여 사용할 수는 [자산](https://developer.xamarin.com/api/property/Android.Content.Context.Assets/) 속성에는 `Android.Content.Context`, 작업 등.
다음 코드에서를 열겠습니다. 우리의 **read_asset.txt** 자산에 콘텐츠를 읽고 TextView를 사용 하 여 표시 합니다.

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


## <a name="running-the-application"></a>응용 프로그램 실행

응용 프로그램을 실행 하 고 다음을 확인할 수 있습니다.

![예제 스크린 샷](android-assets-images/screenshot.png)


## <a name="related-links"></a>관련 링크

- [AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/)
- [컨텍스트](https://developer.xamarin.com/api/type/Android.Content.Context/)
