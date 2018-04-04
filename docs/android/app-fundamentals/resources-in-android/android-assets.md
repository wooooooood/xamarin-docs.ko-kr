---
title: Android 자산 사용 하 여
ms.prod: xamarin
ms.assetid: 70ECDDC9-FA40-03B4-BF04-E7CFFFE4260D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/13/2018
ms.openlocfilehash: 337a1ed82010658adce40e8946ed1b0361fb6094
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="using-android-assets"></a>Android 자산 사용 하 여

_자산_ 응용 프로그램에 텍스트, xml, 글꼴, 음악 및 비디오와 같은 임의 파일을 포함 하는 방법을 제공 합니다. "리소스" 그룹으로 이러한 파일을 포함 하려는 경우 Android 해당 리소스 시스템으로 처리 합니다 및 원시 데이터를 가져올 수 없습니다. 변경 되지 않은 데이터에 액세스 하려는 경우 한 가지 방법은 자산은.입니다.

프로젝트에 추가 하는 자산 파일 시스템에서 사용 하 여 응용 프로그램에서 읽을 수 있는 것 처럼 표시 됩니다 [AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/)합니다.
이 간단한 데모는 텍스트 파일 자산을 위해 프로젝트에 추가 하겠습니다 읽기를 사용 하 여 `AssetManager`, TextView에 표시 합니다.


## <a name="add-asset-to-project"></a>자산을 프로젝트에 추가

자산에서 go는 `Assets` 프로젝트의 폴더입니다. 새 텍스트 파일을 호출 하는이 폴더에 추가 `read_asset.txt`합니다. "또 자산에서!"과 같은 일부 텍스트를 배치 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio를 설정 해야 함는 **빌드 작업** 이 파일에 대해 **AndroidAsset**:

![빌드 작업 AndroidAsset을로 설정](android-assets-images/asset-properties-vs.png) 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac 용 visual Studio를 설정 해야 함는 **빌드 작업** 이 파일에 대해 **AndroidAsset**:

[![빌드 작업 AndroidAsset을로 설정](android-assets-images/asset-properties-xs-sml.png)](android-assets-images/asset-properties-xs.png#lightbox)

-----

선택 하 고 올바른 **빌드 작업** 파일을 컴파일 타임에 APK에 패키지 됩니다 있는지 확인 합니다.


## <a name="reading-assets"></a>자산 읽기

사용 하 여 자산 읽혀집니다는 [AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/)합니다. 인스턴스는 `AssetManager` 를 액세스 하 여 사용할 수는 [자산](https://developer.xamarin.com/api/property/Android.Content.Context.Assets/) 속성에는 `Android.Content.Context`, 작업 등입니다.
다음 코드에서에서는 열 우리의 **read_asset.txt** 자산에서 콘텐츠를 읽고 TextView를 사용 하 여 표시 합니다.

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
