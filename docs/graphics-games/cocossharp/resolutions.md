---
title: CocosSharp에서 여러 해결 방법 처리
description: 이 가이드에는 다양 한 해상도의 장치에서 제대로 표시 하는 게임을 개발 하는 CocosSharp를 사용 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 859ABF98-2646-431A-A4A8-3E7E48DA5A43
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 6803dc2668b89ee2d037da8b34e202191dd5465d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61307822"
---
# <a name="handling-multiple-resolutions-in-cocossharp"></a>CocosSharp에서 여러 해결 방법 처리

_이 가이드에는 다양 한 해상도의 장치에서 제대로 표시 하는 게임을 개발 하는 CocosSharp를 사용 하는 방법을 보여 줍니다._

CocosSharp는 장치의 디스플레이의 픽셀의 실제 수에 관계 없이 게임 개체 크기를 표준화 하는 것에 대 한 메서드를 제공 합니다. 일반적으로 Pc 및 콘솔용 게임을 개발 하는 게임 단일 해상도 대상으로 수 없습니다. 최신 게임 및 모바일 장치용 게임 특히 다양 한 장치에 맞게 빌드해야 합니다. 이 가이드는 실제 디스플레이 해상도 특성에 관계 없이 게임 CocosSharp를 표준화 하는 방법을 보여 줍니다.

기본 확인 CocosSharp의 동작은 물리적 픽셀 게임에서 좌표와 일치 해야 합니다. 다음 표에서 다양 한 장치 368 x 240의 너비와 높이 사용 하 여 백그라운드 환경 스프라이트 렌더링 하는 것입니다. 첫 번째 행에 기술적 하지 실제 장치를 중이지만 필요한 장치 해상도 관계 없이 스프라이트 렌더링 대신:


| **디바이스** | **디스플레이 해상도** | **예제 스크린 샷** |
|--- | --- |--- |
|원하는 표시|368 x 240 (사용 하 여 가로 세로 비율에 대 한 검은색 막대)| ![368 x 240 (사용 하 여 가로 세로 비율에 대 한 검은색 막대)](resolutions-images/image1.png) |
|iPhone 4s|960x640| ![iPhone 4s 960x640](resolutions-images/image2.png) |
|iPhone 6 Plus|1920x1080| ![iPhone 6 Plus 1920x1080](resolutions-images/image3.png) |

이 문서에서는 위의 표에 표시 된 문제를 해결 하려면 CocosSharp를 사용 하는 방법을 설명 합니다. 즉, 화면 해상도 관계 없이 – 첫 번째 행에 표시 된 대로 렌더링 하는 모든 장치를 확인 하는 방법을 설명 합니다.


## <a name="working-with-setdesignresolutionsize"></a>SetDesignResolutionSize 사용

합니다 `CCScene` 라는 정적 메서드를 제공 하면서 모든 시각적 개체에 대 한 루트 컨테이너 클래스는 일반적으로 사용 `SetDesignResolutionSize` 모든 장면에 대 한 기본 크기를 지정 하는 것에 대 한 합니다. 즉 `SetDesignResolutionSize` 메서드를 사용 하면 개발자가 다양 한 하드웨어 해상도에 올바르게 표시 하는 게임을 개발할 수 있습니다. 다음 코드에 표시 된 대로 기본 프로젝트 크기 1024 x 768로 설정 하려면이 메서드를 사용 하는 CocosSharp 프로젝트 템플릿:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    application.PreferMultiSampling = false;
    application.ContentRootDirectory = "Content";
    application.ContentSearchPaths.Add ("animations");
    application.ContentSearchPaths.Add ("fonts");
    application.ContentSearchPaths.Add ("sounds");
    CCSize windowSize = mainWindow.WindowSizeInPixels;
    float desiredWidth = 1024.0f;
    float desiredHeight = 768.0f;
    
    // This will set the world bounds to (0,0, w, h)
    // CCSceneResolutionPolicy.ShowAll will ensure that the aspect ratio is preserved
    CCScene.SetDesignResolutionSize (desiredWidth, desiredHeight, CCSceneResolutionPolicy.ShowAll);
    ...
```

변경할 수 있습니다 합니다 `desiredWidth` 및 `desiredHeight` 게임의 원하는 해상도 맞게 표시를 수정 하려면 위의 변수입니다. 예를 들어 위의 코드는 전체 화면으로 368 x 240 배경을 표시 하려면 다음과 같이 변경할 수 있습니다.


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    application.PreferMultiSampling = false;
    application.ContentRootDirectory = "Content";
    application.ContentSearchPaths.Add ("animations");
    application.ContentSearchPaths.Add ("fonts");
    application.ContentSearchPaths.Add ("sounds");
    CCSize windowSize = mainWindow.WindowSizeInPixels;
    float desiredWidth = 368.0f;
    float desiredHeight = 240.0f;
    
    // This will set the world bounds to(0,0, w, h)
    // CCSceneResolutionPolicy.ShowAll will ensure that the aspect ratio is preserved
    CCScene.SetDesignResolutionSize (desiredWidth, desiredHeight, CCSceneResolutionPolicy.ShowAll);
    ...
```


## <a name="ccsceneresolutionpolicy"></a>CCSceneResolutionPolicy

`SetDesignResolutionSize` 이 기능을 사용 하면 게임 창 원하는 해상도를 조정 하는 방법을 지정할 수 있습니다. 다음 섹션에서는 다른 500x500 이미지를 표시 되는 방법을 보여 주려면 `CCSceneResolutonPolicy` 에 전달 된 값을 `SetDesignResolutionSize` 메서드. 다음 값을 제공 합니다 `CCSceneResolutionPolicy` 열거형:

 - `ShowAll` – 가로 세로 비율 유지 전체 요청된 확인을 표시 합니다.
 - `ExactFit` – 전체 요청 된 해결 방법을 표시 합니다. 하지만 가로 세로 비율을 유지 하지 않습니다.
 - `FixedWidth` – 가로 세로 비율을 유지 관리 하는 전체 요청 된 너비를 표시 합니다.
 - `FixedHeight` – 가로 세로 비율을 유지 관리 하는 전체 요청 된 높이 표시 합니다.
 - `NoBorder` – 전체 높이 또는 가로 세로 비율을 유지 하는 전체 너비를 표시 합니다. 장치 가로 세로 비율 원하는 해상도의 가로 세로 비율 보다 크면 전체 너비를 표시 됩니다. 장치 가로 세로 비율을 사용 하면 원하는 해상도의 가로 세로 비율 보다 작으면, 전체 높이 표시 됩니다.
 - `Custom` – 표시에 따라 달라 집니다 합니다 `Viewport` 속성이 현재 `CCScene`합니다.

모든 스크린샷은 가로 방향의 iPhone 4 초 해상도 (960 x 640)에서 생성 되 고이 이미지 사용:

![](resolutions-images/image4.png "모든 스크린샷은 가로 방향의 iPhone 4 초 해상도 960 x 640에서 생성 되 고이 이미지 사용")


### <a name="ccsceneresolutionpolicyshowall"></a>CCSceneResolutionPolicy.ShowAll

`ShowAll` 지정 하 고 전체 게임 확인 화면에서 볼 수 있지만 표시 될 수 있습니다 *letterboxing* 다른 가로 세로 비율에 맞게 (검은색 막대)입니다. 이 정책은 전체 게임 보기 표시할 화면의 모든 왜곡 없이 보장 하는 대로 주로 사용 됩니다.

코드 예제:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ShowAll);
```

Letterboxing 왼쪽 및 원하는 해상도 보다 넓은 되 고 실제 가로 세로 비율에 맞게 이미지의 오른쪽에 표시 됩니다.

![](resolutions-images/image5.png "Letterboxing 왼쪽과 원하는 해상도 보다 넓은 되 고 실제 가로 세로 비율에 맞게 이미지의 오른쪽에 표시 됩니다.")


### <a name="ccsceneresolutionpolicyexactfit"></a>CCSceneResolutionPolicy.ExactFit

`ExactFit` 전체 게임 확인 없는 letterboxing를 사용 하 여 화면 표시 되도록 지정 합니다. 표시 된 영역을 왜곡 될 수 있습니다 (가로 세로 비율을 관리할 수 있습니다.) 하드웨어 가로 세로 비율에 따라 합니다.

코드 예제:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ExactFit);
```

없습니다 letterboxing 표시 되었지만 장치 해상도 사각형 이므로 게임 보기 왜곡 됩니다.

![](resolutions-images/image6.png "없는 letterboxing은 표시 되지만 장치 해상도 사각형 이므로 게임 보기 왜곡 됩니다.")


### <a name="ccsceneresolutionpolicyfixedwidth"></a>CCSceneResolutionPolicy.FixedWidth

`FixedWidth` 보기의 너비는 전달할 너비 값과 일치 하는 지정 `SetDesignResolutionSize`, 높이 볼 수 있는 실제 장치의 가로 세로 비율에 따라 이지만 합니다. 전달 되는 높이 값 `SetDesignResolutionSize` 물리적 장치의 가로 세로 비율에 따라 런타임에 계산 하 고 있으므로 무시 됩니다. 즉, 계산된 된 높이 (그러면 되 오프 스크린 게임 뷰의 부분에서) 원하는 높이 또는 계산된 된 높이 보다 클 수 있습니다 (그러면 표시 되는 게임 뷰의 더) 원하는 높이 보다 작을 수 있습니다. 이 표시 되는 게임 중에 발생할 수, 있으므로 다음 것 처럼 보이지만 처럼 letterboxing 발생 했습니다. 그러나 추가 공간이 반드시 됩니다 모든 시각적 개체 표시 하는 경우에 검은색입니다. 

코드 예제:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedWidth);
```

IPhone 4 초에 가로 세로 비율은 3:2의 계산된 된 높이 약 333 단위 이므로

![](resolutions-images/image7.png "IPhone 4 초 있으므로 가로 세로 비율은 3:2의 계산된 된 높이 약 333 단위")


### <a name="ccsceneresolutionpolicyfixedheight"></a>CCSceneResolutionPolicy.FixedHeight

개념상 `FixedHeight` 유사 하 게 동작 `FixedWidth` – 게임 전달 되는 높이 값을 준수 합니다 `SetDesignResolutionSize,` 실제 해상도 기반으로 런타임 시 너비를 계산 합니다. 표시 너비 수이 즉 위에서 언급 한 것 처럼 게임은 부분 결과 원하는 너비 보다 크거나 작은 화면 이상의 각각 표시 되 고 게임입니다.

코드 예제:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedHeight);
```

다음 스크린 샷에서 가로 모드에서 실행 되는 앱을 사용 하 여 만든 후 계산된 된 너비는 500 – 특히 750 보다 큽니다. 이 정책을 유지 X 값이 0 왼쪽 맞춤 해상도 화면 오른쪽에 볼 수 있도록 합니다.

![](resolutions-images/image8.png "이 정책을 유지 X 값이 0 왼쪽 맞춤 해상도 화면 오른쪽에 볼 수 있도록")


### <a name="ccsceneresolutionpolicynoborder"></a>CCSceneResolutionPolicy.NoBorder

`NoBorder` 원래 가로 세로 비율 (왜곡 없음)을 유지 하면서 없습니다 letterboxing 사용 하 여 응용 프로그램을 표시 하려고 합니다. 요청 된 해상도 가로 세로 비율 일치 하는 장치의 실제 가로 세로 비율 클리핑은 발생 합니다. 가로 세로 비율, 일치 하지 않는 경우 다음 클리핑 발생 합니다.

코드 예제:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedHeight);
```

다음 스크린 샷은 잘린 상태로 표시 너비의 모든 500 픽셀로 표시 되 면 화면의 위쪽 및 아래쪽 부분을 표시 합니다.

![](resolutions-images/image9.png "이 스크린 샷에서 잘린 상태로 표시 너비의 모든 500 픽셀로 표시 되 면 화면의 위쪽 및 아래쪽 일부를 보여 줍니다.")


### <a name="ccsceneresolutionpolicycustom"></a>CCSceneResolutionPolicy.Custom

`Custom` 각 사용 하도록 설정 `CCScene` 에 지정 된 해상도 기준으로 사용자 지정 자체 뷰포트를 지정 하려면 `SetDesignResolutionSize`합니다.

백그라운드에서 정의 해당 `Viewport` 생성 하 여 속성을 `CCViewport`를 차례로 사용 하 여 작성 되는 `CCRect`합니다. 에 대 한 자세한 `CCViewport` 하 고 `CCRect` 참조를 [CocosSharp API 설명서](https://developer.xamarin.com/api/namespace/CocosSharp/)합니다. 만들기의 컨텍스트에서 `CCViewport` 는 `CCRect` 생성자의 매개 변수 지정 (순서로 정렬):

 - x – 크기 각 단위는 하나의 전체 화면 너비를 나타냅니다 뷰포트를 가로 방향으로 오프셋입니다. 예를 들어, 화면 너비의 절반 만큼 오른쪽으로 이동 하는 장면에서.5f 결과 값을 사용 합니다.
 - y-세로 방향으로 오프셋할 크기 뷰포트 각 단위는 하나의 전체 화면 높이 나타냅니다. 예를 들어 화면 높이의 절반 만큼 이동 하는 장면에서.5f 결과 값을 사용 합니다.
 - width – 장면 차지 하는 가로 부분입니다. 예를 들어 값 1 사용 하 여 / 3.0f 가로 화면의 1/3을 차지 하는 장면에서 발생 합니다.
 - 높이 – 장면 차지 하는 세로 부분입니다. 예를 들어 값 1 사용 하 여 / 3.0f 세로로 화면의 1/3을 차지 하는 장면에서 발생 합니다.

코드 예제:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.Custom);
...
float horizontalDisplayPortion = 2 / 3.0f;
float verticalDisplayPortion = 1 / 2.0f;
float displayOffsetInScreenWidths = 0;
float displayOffsetInScreenHeights = .5f;
var rectangle = new CCRect (
    displayOffsetInScreenWidths, 
    displayOffsetInScreenHeights, 
    horizontalDisplayPortion, 
    verticalDisplayPortion);
scene.Viewport = new CCViewport (rectangle);
```

위의 코드는 다음과 같은 결과:

![](resolutions-images/image10.png "위의 스크린 샷에서 결과 코드")


## <a name="defaulttexeltocontentsizeratio"></a>DefaultTexelToContentSizeRatio

`DefaultTexelToContentSizeRatio` 고해상도 화면을 사용 하 여 장치에서 더 높은 해상도 질감 사용을 간소화 합니다. 특히,이 속성 크기 또는 시각적 요소의 위치를 변경 하려면 필요 없이 더 높은 해상도 자산을 사용 하는 게임을 허용 합니다. 

예를 들어 게임은 200 픽셀에 불과하지만 게임 문자 및 게임 화면에 대 한 아트 자산의 포함할 수 있습니다 (지정 된 대로 `SetDesignResolutionSize`) 400 픽셀 높이 수 있습니다. 이 경우 문자 화면의 절반을 차지 합니다. 그러나 400 픽셀 높이 자산을 고해상도 장치에 대 한 문자에 사용 된 경우 문자는 표시의 전체 높이 차지 합니다. 의도 더 높은 해상도 장치를 활용 하기 위해 동일한 크기 문자를 유지 하려면 일부 추가 코드는 화면의 높이 반에 문자 스프라이트를 유지 해야 합니다.

위의 간단한 예제는 크기가 더 큰 자산 장치 해상도 따라 각 시각적 요소에 크기를 조정 하는 데 개발자 요구 하지 않고도 추가 – 게임 개발의 일반적인 문제를 나타냅니다. `DefaultTexelToContentSizeRatio` 시각적 요소 크기 조정에 대 한 전역 속성이 사용 됩니다. 이 값에 할당 된 값을 기준으로 특정 종류의 모든 시각적 요소 크기가 조정 됩니다.

이 속성은 다음 클래스에 있습니다. 

 - `CCApplication`
 - `CCSprite`
 - `CCLabelTtf`
 - `CCLabelBMFont`
 - `CCTMXLayer`

CocosSharp Visual Studio for Mac 템플릿 집합 `CCSprite.DefaultTexelToContentSizeRatio` 장치를 사용할 수 있는 방법을의 예제로의 너비에 따라 합니다. 다음 코드에 포함 된 `CCApplicationDelegate.ApplicationDidFinishLaunching`:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    ...
    float desiredWidth = 1024.0f;
    float desiredHeight = 768.0f;
           
    ...
    if (desiredWidth < windowSize.Width)
    {
        application.ContentSearchPaths.Add ("images/hd");
        CCSprite.DefaultTexelToContentSizeRatio = 2.0f;
    }
    else
    {
        application.ContentSearchPaths.Add ("images/ld");
        CCSprite.DefaultTexelToContentSizeRatio = 1.0f;
    }
    ...           
}
```


### <a name="defaulttexeltocontentsizeratio-example"></a>DefaultTexelToContentSizeRatio 예제

보려는 어떻게 `DefaultTexelToContentSizeRatio` 시각적 개체의 크기에 영향을 줍니다 요소 위에 나오는 코드는 것이 좋습니다.


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ShowAll);
```

500x500 질감을 사용 하 여 다음 이미지에서 만들어집니다.

![](resolutions-images/image5.png "이 이미지를 500x500 질감을 사용 하 여 결과")

IPad 레 티 나 2048 x 1,536 실제 해상도로 실행 됩니다. 이 위에 표시 된 대로 게임 연장 500 픽셀 물리적 픽셀에 1536 것을 의미 합니다. 게임 활용할 수 있습니다이 추가 해상도로 참조 되는 일반적으로 무엇을 만들어서 *hd* 자산, 자산을 더 높은 해상도 화면에서 실행 되도록 설계 됩니다. 예를 들어이 게임 1000 x 1000 크기가이 질감의 hd 버전을 사용 수 있습니다. 그러나 더 큰 이미지를 사용 하 여 초래 합니다 `CCSprite` 너비 및 높이 1000 단위입니다. 게임이 500 단위를 항상 표시 되므로 (인해는 `SetDesignResolutioSize` 호출), 1000 x 1000 스프라이트가 너무 큽니다 (만 왼쪽된 하단으로 표시 됩니다) 수는:

![](resolutions-images/image11.png "1000 x 1000 스프라이트 너무만 왼쪽된 하단으로 표시 됩니다 것 이므로 게임은 항상 SetDesignResolutioSize 호출으로 인해 500 단위를 표시 하 고,")

설정할 수 있습니다 `CCSprite.DefaultTexelToContentSizeRatio` 되 두 번으로 다양 하 고 원래 500x500 질감 높이가 1000 x 1000 질감에 대 한 계정에:


```csharp
CCSprite.DefaultTexelToContentSizeRatio = 2;
```

이제 게임을 실행 하는 경우 1000 x 1000 질감은 완벽 하 게 표시 됩니다.

![](resolutions-images/image12.png "이제 게임을 실행 하는 경우 1000 x 1000 질감 완벽 하 게 표시 됩니다.")


### <a name="defaulttexeltocontentsizeratio-details"></a>DefaultTexelToContentSizeRatio 세부 정보

합니다 `DefaultTexelToContentSizeRatio` 속성은 `static,` 응용 프로그램에서 모든 스프라이트를 의미 하는 동일한 값을 공유 합니다. 다른 해결 방법에 대 한 자산을 사용 하 여 게임에 대 한 일반적인 접근 방법은 각 해결 범주에 대 한 자산의 전체 집합을 포함 하는 것입니다. CocosSharp Visual Studio for Mac 템플릿 기본적으로 제공 **ld** 하 고 **hd** 텍스처의 두 집합을 지 원하는 게임에도 유용 하다는 자산에 대 한 폴더입니다. 콘텐츠를 사용 하 여 예제 콘텐츠 폴더 처럼 보일 수 있습니다.

![](resolutions-images/image13.png "콘텐츠를 사용 하 여 예제 콘텐츠 폴더 처럼 보일 수 있습니다.")

기본 템플릿은 응용 프로그램의 조건에 따라 추가 하는 코드를 포함 하는 공지 `ContentSearchPaths`:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    ...
    if (desiredWidth < windowSize.Width)
    {
        application.ContentSearchPaths.Add ("images/hd");
        CCSprite.DefaultTexelToContentSizeRatio = 2.0f;
    }
    else
    {
        application.ContentSearchPaths.Add ("images/ld");
        CCSprite.DefaultTexelToContentSizeRatio = 1.0f;
    }
    ...           
}
```

이 응용 프로그램의 높은 해상도 또는 일반 해결 모드에서 실행 되는 여부에 따라 경로 있는 파일에 대 한 검색가 것을 의미 합니다. 예를 들어 경우는 **hd** 하 고 **ld** 폴더 라는 파일에 포함 **background.png** 다음 코드는 실행 하 고 확인을 위한 올바른 파일 선택:


```csharp
backgroundSprite  = new CCSprite ("background");
```


## <a name="summary"></a>요약

이 문서에서는 장치 해상도 관계 없이 올바르게 표시 하는 게임을 작성 하는 방법에 설명 합니다. 예를 보여 줍니다 사용 하 여 다른 `CCSceneResolutionPolicy` 게임 장치 해상도 따라 크기를 조정 하는 값입니다. 또한 방법의 예로 제공 `DefaultTexelToContentSizeRatio` 시각적 요소를 개별적으로 조정할 필요 없이 여러 콘텐츠 집합을 수용 하기 위해 사용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [CocosSharp 소개](~/graphics-games/cocossharp/index.md)
- [CocosSharp API 설명서](https://developer.xamarin.com/api/namespace/CocosSharp/)
