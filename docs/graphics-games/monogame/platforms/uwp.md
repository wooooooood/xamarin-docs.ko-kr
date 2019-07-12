---
title: MonoGame UWP 프로젝트 만들기
description: MonoGame 유니버설 Windows 플랫폼, 코드 베이스 하나를 사용 하 여 여러 장치 대상 지정 및 콘텐츠 집합에 대 한 게임 및 앱을 만드는 데 사용할 수 있습니다.
ms.prod: xamarin
ms.assetid: C6B99E44-00C1-4139-A1B7-FCFBE8749AB1
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 7db73759cb4a1b1a8d7fe40426b03a163c3ebdc4
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67831057"
---
# <a name="creating-a-monogame-uwp-project"></a>MonoGame UWP 프로젝트 만들기

_MonoGame 유니버설 Windows 플랫폼, 코드 베이스 하나를 사용 하 여 여러 장치 대상 지정 및 콘텐츠 집합에 대 한 게임 및 앱을 만드는 데 사용할 수 있습니다._

이 연습에서는 MonoGame 유니버설 Windows 플랫폼 (UWP) 프로젝트 생성 및 로드 하는 콘텐츠를 설명 합니다. UWP 앱은 데스크톱, 태블릿, Windows Phone 및 Xbox One을 포함 하 여 모든 Windows 10 장치에서 실행할 수 있습니다.

이 연습에서는 표시 하는 빈 프로젝트를 *수레 국화 파란색* 백그라운드 (XNA 응용 프로그램의 기존 배경 색).

## <a name="requirements"></a>요구 사항

MonoGame UWP 앱 개발에 필요 합니다.

- Windows 10 운영 체제
- Visual Studio 2017의 모든 버전
- Windows 10 개발자 도구
- 장치가 개발자 모드 설정
- [Visual Studio 용 3.7.1 MonoGame](http://community.monogame.net/t/monogame-3-7-1-release/11173) 이상

자세한 내용은 참조 [Windows 10 UWP 개발에 대 한 설정 페이지](https://msdn.microsoft.com/windows/uwp/get-started/get-set-up)합니다.

소매 Xbox One 하드웨어에서 Xbox One 게임을 개발할 수 있습니다. 추가 소프트웨어 개발 PC 및 Xbox One에 필요 합니다. Xbox One 게임 개발에 대 한 구성에 대 한 내용은이 페이지를 참조 [Xbox One 설정](https://msdn.microsoft.com/windows/uwp/xbox-apps/index)합니다.

## <a name="creating-an-empty-template"></a>빈 템플릿 만들기

필요한 모든 리소스를 설치한 후 Windows 10 컴퓨터에서 개발자 모드가 활성화 된 Visual Studio를 사용 하 여 이러한 단계에 따라 새 MonoGame 프로젝트를 만들 수 있습니다.

1. 선택 **파일** > **새** > **프로젝트...**
1. 선택 된 **설치** > **템플릿** > **시각적 C#**   >  **MonoGame** 범주:

    ![](uwp-images/image1.png "MonoGame 범주")

1. 선택 된 **MonoGame Windows 10 유니버설 프로젝트** 옵션:

    ![](uwp-images/image2.png "MonoGame Windows 10 유니버설 프로젝트 옵션 선택")

1. 새 프로젝트의 이름을 입력 하 고 클릭 **확인**합니다.
확인을 클릭 한 후 오류를 표시 하는 Visual Studio, Windows 10 도구가 설치 되어 있는지 및 장치가 개발자 모드에서 인지를 확인 합니다.

Visual Studio 템플릿 만들기가 완료 되 면 실행 하는 빈 프로젝트 참조를 실행할 수 있습니다.

![](uwp-images/image3.png "Visual Studio 템플릿 만들기가 완료 되 면 실행을 실행 하는 빈 프로젝트를 참조 하세요.")

모퉁이에 있는 숫자는 진단 정보를 제공 합니다. 코드를 삭제 하 여이 정보를 제거할 수 있습니다 `App.xaml.cs` 에 `DEBUG` 블록을 `OnLaunched` 메서드:


```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
#if DEBUG
    if (System.Diagnostics.Debugger.IsAttached)
    {
        this.DebugSettings.EnableFrameRateCounter = true;
    }
#endif
    ...
```

## <a name="running-on-xbox-one"></a>Xbox One에서 실행

UWP 프로젝트는 동일한 프로젝트에서 모든 Windows 10 장치에 배포할 수 있습니다. Windows 10 개발 컴퓨터 및 Xbox One을 설정한 후 원격 컴퓨터에 대상 전환 및 Xbox One의 IP 주소를 입력 하 여 UWP 앱을 배포할 수 있습니다.

![](uwp-images/remote.png "원격 컴퓨터에 대상 전환 하 고 Xbox 된 IP 주소를 입력 하 여 UWP 앱을 배포할 수 있습니다.")

Xbox One에 흰색 테두리 Tv에 대 한 안전 하지 않은 영역을 나타냅니다. 자세한 내용은 참조는 [안전 영역 섹션](#safe-area-on-xbox-one)합니다.

![](uwp-images/safearea.png "Xbox One에 흰색 테두리 Tv에 대 한 안전 하지 않은 영역을 나타내는")

### <a name="safe-area-on-xbox-one"></a>Xbox One에 안전 영역

콘솔에 대 한 게임 개발 영역 (예: UI 또는 HUD) 모든 중요 한 시각적 개체를 포함 해야 하는 화면 가운데에는 안전한 영역을 고려할 필요 합니다. 안전 영역 외부 영역이이 영역에 배치 하는 시각적 개체 부분적으로 나 완전히 표시 일부 표시 될 수 있습니다 수 있으므로 모든 텔레비전에 표시 되도록 보장 되지 않습니다.

MonoGame Xbox One에 대 한 안전 영역 고려 템플릿과 흰색 테두리로 렌더링 됩니다. 이 영역 에서도 게임의 런타임 시 반영 되었습니다 `Window.ClientBounds` Visual Studio에서 조사식 창의이 이미지에 표시 된 대로 속성입니다. 클라이언트 경계 높이 1920 x 1080 디스플레이 해상도도 불구 하 고 1016 인지 확인 합니다.

![](uwp-images/clientbounds.png "클라이언트 경계 높이 해상도 1920 x 1080 불구 하 고 1016")

## <a name="referencing-content-in-uwp-projects"></a>UWP 프로젝트의 참조 콘텐츠

파일에서 직접 또는 MonoGame 프로젝트에서 콘텐츠를 참조할 수 있습니다 합니다 [MonoGame 콘텐츠 파이프라인](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/content-pipeline/introduction.md)합니다. 작은 게임 프로젝트 파일에서 로드 하는 단순성에서 이점을 얻을 수 있습니다. 대규모 프로젝트를 콘텐츠 파이프라인을 사용 하 여 콘텐츠 크기를 줄이고 로드 시간이 최적화를 활용 합니다. Xbox 360에서 XNA와 달리는 `System.IO.File` 클래스는 Xbox One UWP 앱에서 사용할 수 있습니다.

콘텐츠 파이프라인을 사용 하 여 콘텐츠를 로드에 대 한 자세한 내용은 참조는 [콘텐츠 파이프라인 가이드](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/content-pipeline/introduction.md)합니다.

### <a name="loading-content-from-file"></a>파일에서 콘텐츠를 로드합니다.

UWP 프로젝트는 iOS 및 Android와 달리 실행 파일과 관련 된 파일을 참조할 수 있습니다. 간단한 게임 수정 하 고 콘텐츠 파이프라인 프로젝트를 빌드할 필요 없이이 기술은 부하 콘텐츠를 사용할 수 있습니다.

로드 하는 `Texture2D` 파일에서:

1. UWP 프로젝트에서 콘텐츠 폴더에.png 파일을 추가 합니다. 콘텐츠 폴더에 추가 콘텐츠는 XNA에 MonoGame 규칙입니다.
1. 새로 추가 된 PNG 단추로 클릭 하 고 속성을 선택 합니다.
1. 변경 된 **출력 디렉터리로 복사** 하 **변경 된 내용만 복사**합니다.
1. 다음 코드를 추가 합니다 로드 게임의 Initialize 메서드는 `Texture2D`:

    ```csharp
    Texture2D texture;
    using (var stream = System.IO.File.OpenRead("Content/YourPngName.png"))
    {
        texture = Texture2D.FromStream(graphics.GraphicsDevice, stream);
    }
    ```

사용 하 여 대 한 자세한 내용은 `Texture2D`를 참조 합니다 [MonoGame 가이드 소개](~/graphics-games/monogame/introduction/index.md)합니다.

## <a name="summary"></a>요약

이 가이드에서는 파일을 로드할 때 새 UWP 프로젝트 및 UWP 관련 고려 사항을 만드는 방법을 설명 합니다. 전체 UWP 게임을 만들어 관심 있는 개발자 읽어보세요 MonoGame에 대 한 합니다 [MonoGame 가이드 소개](~/graphics-games/monogame/introduction/index.md)합니다.
