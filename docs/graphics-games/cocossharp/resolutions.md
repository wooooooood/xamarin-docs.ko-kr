---
title: CocosSharp에 여러 해상도 처리합니다.
description: 이 가이드에는 다양 한 해상도의 장치에서 제대로 표시 하는 게임을 개발 하는 CocosSharp를 사용 하는 방법을 보여 줍니다.
ms.topic: article
ms.prod: xamarin
ms.assetid: 859ABF98-2646-431A-A4A8-3E7E48DA5A43
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 772b0d6408a5ba438c5eb0be04a9b549e29b40f9
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/03/2018
---
# <a name="handling-multiple-resolutions-in-cocossharp"></a>CocosSharp에 여러 해상도 처리합니다.

_이 가이드에는 다양 한 해상도의 장치에서 제대로 표시 하는 게임을 개발 하는 CocosSharp를 사용 하는 방법을 보여 줍니다._

CocosSharp 물리적 장치의 디스플레이에서 픽셀 수에 관계 없이 게임의 개체 크기를 표준화 하기 위한 메서드를 제공 합니다. 일반적으로 게임 개발 Pc 및 게임 콘솔에 대 한 단일 해상도 대상 지정할 수 있습니다. 최신 게임 및 특히, 게임 모바일 장치에 대 한 다양 한 장치를 수용 하기 위해 빌드해야 합니다. 이 가이드 CocosSharp 표준화 하는 방법의 실제 디스플레이 해상도 특성에 관계 없이 게임을 보여 줍니다.

기본 해상도 CocosSharp 동작은 물리적 픽셀 게임 좌표와 일치 합니다. 다음 표에서 다양 한 장치를 다시 설정 하 게 렌더링 배경 환경 스프라이트 368 x 240의 너비와 높이를 보여 줍니다. 첫 번째 행은 기술적으로 하지는 실제 장치를 장치 해상도 관계 없이 sprite의 예상된 렌더링 아니라:


| **장치** | **디스플레이 해상도** | **예제 스크린 샷** |
|--- | --- |--- |
|원하는 표시 오프셋|368 x 240 (된 가로 세로 비율에 대 한 검은 막대)| ![368 x 240 (된 가로 세로 비율에 대 한 검은 막대)](resolutions-images/image1.png) |
|iPhone 4s|960x640| ![iPhone 4s 960x640](resolutions-images/image2.png) |
|iPhone 6 Plus|1920x1080| ![iPhone 6 Plus 1920 x 1080](resolutions-images/image3.png) |

이 문서에서는 위 표에 표시 된 문제를 해결 하려면 CocosSharp를 사용 하는 방법을 설명 합니다. 즉, 모든 장치 – 화면 해상도 관계 없이 첫 번째 행에 표시 된 대로 렌더링 하는 방법을 설명 합니다.


## <a name="working-with-setdesignresolutionsize"></a>SetDesignResolutionSize 작업

`CCScene` 모든 시각적 개체에 대 한 루트 컨테이너 클래스는 일반적으로 사용 하지만 정적 메서드 호출 또한 제공 `SetDesignResolutionSize` 모든 장면에 대 한 기본 크기를 지정 하는 데 있습니다. 즉 `SetDesignResolutionSize` 메서드를 사용 하면 개발자가 다양 한 하드웨어 해상도에 올바르게 표시 하는 게임을 개발할 수 있습니다. CocosSharp 프로젝트 템플릿은 다음 코드 에서처럼를 1024 x 768 기본 프로젝트 크기를 설정 하려면이 메서드를 사용 합니다.


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

변경할 수는 `desiredWidth` 및 `desiredHeight` 게임의 원하는 해상도 일치 하도록 표시를 수정 하려면 위의 변수입니다. 예를 들어 위의 코드는 전체 화면으로 368 x 240 배경을 표시 하려면 다음과 같이 변경할 수 있습니다.


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

`SetDesignResolutionSize` 게임 창을 원하는 해상도를 조정 하는 방법을 지정할 수 있도록 합니다. 다음 섹션에서는 서로 다른 500 x 500 이미지 표시 되는 방식을 보여 줍니다. `CCSceneResolutonPolicy` 에 전달 된 값은 `SetDesignResolutionSize` 메서드. 다음 값을 제공 된 `CCSceneResolutionPolicy` enum:

 - `ShowAll` – 가로 세로 비율 유지 전체 요청 된 해상도 표시 됩니다.
 - `ExactFit` – 전체 요청 된 확인을 표시 하지만 가로 세로 비율 유지 관리 하지 않습니다.
 - `FixedWidth` – 가로 세로 비율 유지 전체 요청 된 너비를 표시 합니다.
 - `FixedHeight` – 가로 세로 비율 유지 전체 요청 된 높이 표시 합니다.
 - `NoBorder` – 전체 높이 또는 가로 세로 비율 유지 전체 너비를 표시 합니다. 장치의 가로 세로 비율 원하는 해상도의 가로 세로 비율 보다 큰 경우 전체 너비 표시 됩니다. 장치의 가로 세로 비율을 사용 하면 원하는 해상도의 가로 세로 비율 보다 작으면, 전체 높이 표시 됩니다.
 - `Custom` – 디스플레이에 따라 달라 집니다는 `Viewport` 현재 `CCScene`합니다.

모든 스크린 샷을 가로 방향의 iPhone 4s 해상도 (960 x 640)에서 생성 되 고이 이미지를 사용 합니다.

![](resolutions-images/image4.png "모든 스크린 샷을 가로 방향의 iPhone 4s 해상도 960 x 640에서 생성 되 고이 이미지 사용")


### <a name="ccsceneresolutionpolicyshowall"></a>CCSceneResolutionPolicy.ShowAll

`ShowAll` 전체 게임 해상도 화면에서 볼 수 있지만 표시 될 수 있습니다 지정 *letterboxing* 다른 가로 세로 비율에 맞게 (검은 막대)입니다. 보장 된 왜곡 없이 전체 게임 보기를 화면에 나타나는 표시 됩니다.이 정책은 일반적으로 사용 됩니다.

코드 예:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ShowAll);
```

Letterboxing 원하는 해상도 보다 넓은 되 고 실제 가로 세로 비율에 맞게 이미지의 오른쪽 및 왼쪽에 표시 됩니다.

![](resolutions-images/image5.png "Letterboxing 왼쪽 및 오른쪽 이미지에 원하는 해상도 보다 넓은 되 고 실제 가로 세로 비율을 고려 하 게 표시 됩니다.")


### <a name="ccsceneresolutionpolicyexactfit"></a>CCSceneResolutionPolicy.ExactFit

`ExactFit` 전체 게임 해상도 없는 letterboxing와 화면 표시 되도록 지정 합니다. 표시 가능 영역 왜곡 될 수 있습니다 (가로 세로 비율 유지 되지 않을) 하드웨어 가로 세로 비율에 따라 합니다.

코드 예:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ExactFit);
```

없음 letterboxing은 표시 되지만 게임 보기 왜곡 장치 해상도 사각형 이므로:

![](resolutions-images/image6.png "없음 letterboxing은 표시 되지만 게임 보기 왜곡 장치 해상도 사각형 이므로")


### <a name="ccsceneresolutionpolicyfixedwidth"></a>CCSceneResolutionPolicy.FixedWidth

`FixedWidth` 보기의 너비는 전달 되는 너비 값 일치 하도록 지정 `SetDesignResolutionSize`, 되지만 볼 수 있는 높이 물리적 장치의 가로 세로 비율이 될 수 있습니다. 전달 되는 높이 값 `SetDesignResolutionSize` 물리적 장치의 가로 세로 비율에 따라 런타임에 계산 됩니다 하므로 무시 됩니다. 즉, 계산된 된 높이 (결과적 중인 화면에서 벗어난 게임 보기의 일부) 원하는 높이 또는 계산된 된 높이 보다 클 수 있습니다 (그 결과 표시 되 고 게임 보기 중) 원하는 높이 것 보다 작은 수 있습니다. 이 표시 되는 게임 중 발생할 수 있습니다, 때문 후 보일 수 있습니다 처럼 letterboxing 발생 했습니다. 그러나 추가 공간이 반드시 됩니다 검정 모든 시각적 개체 표시 되는 경우. 

코드 예:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedWidth);
```

IPhone 4s 가로 세로 비율이 3: 2의 계산된 된 높이 약 333 단위 이므로:

![](resolutions-images/image7.png "IPhone 4s 가로 세로 비율이 3: 2의 계산된 된 높이 약 333 단위 이므로")


### <a name="ccsceneresolutionpolicyfixedheight"></a>CCSceneResolutionPolicy.FixedHeight

개념적으로 `FixedHeight` 유사 하 게 동작 `FixedWidth` – 게임은 전달 되는 높이 값 따릅니다 `SetDesignResolutionSize,` 하지만 실제 해상도 기반으로 런타임에 너비를 계산 합니다. 표시 너비 값이 즉 위에서 설명한 대로 되 고 있는 게임 부분 결과 원하는 너비 보다 크거나 작을 화면 또는 각각 표시 되는 게임 중입니다.

코드 예:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedHeight);
```

다음 스크린 샷에서 가로 모드에서 실행 중인 앱으로 생성 된 후 계산 된 너비는 500-특히 750 보다 큽니다. 이 정책을 유지 0 X 값 왼쪽 맞춤 해상도 화면 오른쪽에 표시 됩니다.

![](resolutions-images/image8.png "이 정책을 유지 0 X 값 왼쪽 맞춤 해상도 화면 오른쪽에 표시 됩니다.")


### <a name="ccsceneresolutionpolicynoborder"></a>CCSceneResolutionPolicy.NoBorder

`NoBorder` 원래 가로 세로 비율 (왜곡 제외)을 유지 하면서 없는 letterboxing와 응용 프로그램을 표시 하려고 합니다. 요청 된 해상도 가로 세로 비율에서 일치 하는 장치의 실제 가로 세로 비율 오려낸 없습니다 발생 합니다. 가로 세로 비율, 일치 하지 않는 경우 다음 클리핑 발생 합니다.

코드 예:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedHeight);
```

다음 스크린 샷에 표시 너비의 모든 500 픽셀로 표시 되 면 잘린 디스플레이의 위쪽 및 아래쪽 부분을 표시 합니다.

![](resolutions-images/image9.png "표시 너비의 모든 500 픽셀로 표시 되 면 잘린 디스플레이의 위쪽 및 아래쪽 부분을 표시 하는이 스크린 샷")


### <a name="ccsceneresolutionpolicycustom"></a>CCSceneResolutionPolicy.Custom

`Custom` 각 수 있도록 `CCScene` 에 지정 된 해상도 기준으로 사용자 지정 뷰포트를 지정 하려면 `SetDesignResolutionSize`합니다.

장면 정의 해당 `Viewport` 구성 하 여 속성은 `CCViewport`를 차례로로 구성 되며는 `CCRect`합니다. 대 한 자세한 내용은 `CCViewport` 및 `CCRect` 참조는 [CocosSharp API 설명서](https://developer.xamarin.com/api/namespace/CocosSharp/)합니다. 만드는의 컨텍스트에서 `CCViewport` 는 `CCRect` 생성자의 매개 변수 순서에 따라 지정:

 - x-를 가로로 각 단위가 하나의 전체 화면 너비를 나타내는 뷰포트를 오프셋할 크기입니다. 예를 들어.5f 결과의 값을 사용 하 여 장면의 화면 너비의 절반으로 오른쪽으로 이동 합니다.
 - y-를 세로로 각 단위가 하나의 전체 화면 높이 나타내는 뷰포트를 오프셋할 크기입니다. 예를 들어.5f 결과의 값을 사용 하 여 장면의 화면 높이의 절반으로 아래로 이동 합니다.
 - 너비 장면 차지 하는 가로 부분입니다. 예를 들어 값 1 사용 하 여 / 3.0f 화면의 1 / 3을 사용 하는 가로 장면에 발생 합니다.
 - 높이 장면 차지 하는 세로 부분입니다. 예를 들어 값 1 사용 하 여 / 3.0f 화면의 1 / 3을 사용 하는 세로 장면에 발생 합니다.

코드 예:


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

위의 코드는 다음에 발생합니다.

![](resolutions-images/image10.png "위의 코드 발생이 스크린 샷")


## <a name="defaulttexeltocontentsizeratio"></a>DefaultTexelToContentSizeRatio

`DefaultTexelToContentSizeRatio` 고해상도 화면을 사용 하 여 장치에서 더 높은 해상도 질감을 사용 하 여 간소화 합니다. 특히,이 속성 크기 또는 시각적 요소의 위치를 변경 하려면 필요 없이 고해상도 자산을 사용 하는 게임을 허용 합니다. 

예를 들어 게임은 200 픽셀, 높이 게임 문자 및 게임 화면에 대 한 아트 자산을 포함 될 수 있습니다 (에 지정 된 대로 `SetDesignResolutionSize`) 400 픽셀 높이 수 있습니다. 이 경우에 문자 화면 절반을 차지 합니다. 그러나 400 픽셀 높이 자산 고해상도 장치에 대 한 문자에 대 한 사용 되었다면 문자가 디스플레이의 전체 높이 차지 합니다. 더 높은 해상도 장치를 활용 하기 위해 동일한 크기는 문자를 유지 하는 것을 하는 경우에 두는 몇 가지 추가 코드를 화면의 높이 절반으로 문자 sprite를 유지할 필요.

위의 간단한 예제 장치 해상도 따라 각 visual 요소에 대해 크기 조정을 수행 하기 위해 개발자 하지 않고도 더 큰 크기의 자산을 추가 하는 게임 개발 –의 일반적인 문제를 나타냅니다. `DefaultTexelToContentSizeRatio` 시각적 요소 크기 조정에 대 한 전역 속성이 사용 됩니다. 이 값이 할당 된 값으로 특정 형식의 모든 시각적 요소를 조정합니다.

이 속성은 다음 클래스에 있습니다. 

 - `CCApplication`
 - `CCSprite`
 - `CCLabelTtf`
 - `CCLabelBMFont`
 - `CCTMXLayer`

Mac 템플릿 집합에 대 한 CocosSharp Visual Studio `CCSprite.DefaultTexelToContentSizeRatio` 장치 사용 방법에 대 한 예로 너비에 따라 합니다. 다음 코드에 포함 된 `CCApplicationDelegate.ApplicationDidFinishLaunching`:


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


### <a name="defaulttexeltocontentsizeratio-example"></a>DefaultTexelToContentSizeRatio example

확인 하려면 어떻게 `DefaultTexelToContentSizeRatio` 시각적 개체의 크기 영향 요소를 위에서 설명한 코드를 고려 하세요.:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ShowAll);
```

되 고 500 x 500 텍스처를 사용 하 여 다음 이미지에서 만들어집니다.

![](resolutions-images/image5.png "500 x 500 텍스처를 사용 하 여이 이미지 결과적 됩니다.")

IPad 레 티 나 2048 x 1536의 실제 해상도로 실행 됩니다. 이 위에 표시 된 그대로 게임 연장 500 픽셀 물리적 픽셀에 1536 의미 합니다. 게임 활용할 수 해상도 일반적으로 지칭 만들어 *hd* 자산 – 자산에서 고해상도 화면을 실행 하도록 설계 됩니다. 예를 들어이 게임은 hd 버전 1000 x 1000의 크기이 질감의를 사용할 수 있습니다. 그러나 더 큰 이미지를 사용 하 여 초래는 `CCSprite` 너비 및 높이 1000 단위입니다. 게임 500 단위 컨트롤이 항상 표시 됩니다 (으로 인해는 `SetDesignResolutioSize` 호출), 있으면 우리의 1000 x 1000 sprite 합니다 너무 큽니다 (만 왼쪽된 아래 부분에 표시 됨):

![](resolutions-images/image11.png "1000 x 1000 sprite 너무 큰만 왼쪽된 하단에 표시 됩니다 것 게임 SetDesignResolutioSize 호출으로 인해 500 단위 컨트롤이 항상 표시 됩니다,")

설정할 수 있습니다 `CCSprite.DefaultTexelToContentSizeRatio` 같이 너비와 높이 원래 500 x 500 질감으로 두 번 되 고 1000 x 1000 질감을 설명 하기 위해:


```csharp
CCSprite.DefaultTexelToContentSizeRatio = 2;
```

이제 게임을 실행 하는 경우 1000 x 1000 질감은 완벽 하 게 표시 됩니다.

![](resolutions-images/image12.png "이제는 게임을 실행 하는 경우 1000 x 1000 질감 완벽 하 게 표시 됩니다.")


### <a name="defaulttexeltocontentsizeratio-details"></a>DefaultTexelToContentSizeRatio details

`DefaultTexelToContentSizeRatio` 속성은 `static,` 응용 프로그램에서 모든 스프라이트를 의미 하는 동일한 값을 공유 합니다. 다른 해상도 대해 발생 자산 게임에 대 한 일반적인 방법은 각 해결 범주에 대 한 자산 전체 집합을 포함 하는 것입니다. Mac 템플릿용으로 CocosSharp Visual Studio는 기본적으로 제공 **ld** 및 **hd** 질감의 두 집합을 지 원하는 게임에 대 한 유용할 수 있는 자산에 대 한 폴더입니다. 내용으로는 예제 콘텐츠 폴더 같을 수 있습니다.

![](resolutions-images/image13.png "내용으로는 예제 콘텐츠 폴더 처럼 보일 수 있습니다.")

기본 서식 파일에도 응용 프로그램에 추가할 조건에 따라 코드를 포함 하는 예 고 `ContentSearchPaths`:


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

즉, 응용 프로그램의 높은 해상도 또는 일반 해결 모드에서 실행 되는 여부에 따라 경로에 있는 파일에 대 한 검색 합니다. 예를 들어 경우는 **hd** 및 **ld** 폴더 라는 파일에 들어 **background.png** 다음 코드는 실행 하 고 확인을 위한 올바른 파일을 선택 합니다.


```csharp
backgroundSprite  = new CCSprite ("background");
```


## <a name="summary"></a>요약

이 문서에서는 장치 해상도 관계 없이 올바르게 표시 하는 게임을 작성 하는 방법에 설명 합니다. 예를 보여 줍니다 사용 하 여 다른 `CCSceneResolutionPolicy` 게임 장치 해상도 따라 크기 조정에 대 한 값입니다. 또한 방법의 예로 제공 `DefaultTexelToContentSizeRatio` 시각적 요소를 개별적으로 크기를 조정할 수를 요구 하지 않고 여러 집합의 콘텐츠를 수용 하기 위해 사용할 수 있습니다.

## <a name="related-links"></a>관련된 링크

- [CocosSharp 소개](~/graphics-games/cocossharp/index.md)
- [CocosSharp API 설명서](https://developer.xamarin.com/api/namespace/CocosSharp/)
