---
title: "MonoGame UWP 프로젝트 만들기"
description: "MonoGame 유니버설 Windows 플랫폼, 코드 베이스 하나를 사용 하 여 여러 장치를 대상으로 지정 및 콘텐츠 집합이 하나에 대 한 게임 및 응용 프로그램을 만드는 데 사용할 수 있습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: C6B99E44-00C1-4139-A1B7-FCFBE8749AB1
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 3990b226b74c17fb5cccc907dd50b46578c3ef6b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="creating-a-monogame-uwp-project"></a>MonoGame UWP 프로젝트 만들기

_MonoGame 유니버설 Windows 플랫폼, 코드 베이스 하나를 사용 하 여 여러 장치를 대상으로 지정 및 콘텐츠 집합이 하나에 대 한 게임 및 응용 프로그램을 만드는 데 사용할 수 있습니다._

이 연습에서는 MonoGame 유니버설 Windows 플랫폼 (UWP) 프로젝트 만들기 및 로드 하는 콘텐츠를 설명 합니다. UWP 앱은 데스크톱, 태블릿, Windows Phone 및 Xbox One을 포함 하 여 모든 Windows 10 장치에서 실행할 수 있습니다.

이 연습에서는 표시 하는 빈 프로젝트를 만듭니다.는 *수레 국화 파란색* 백그라운드 (XNA 응용 프로그램의 기존 배경 색).


# <a name="requirements"></a>요구 사항

MonoGame UWP 앱 개발에 필요 합니다.

 - Windows 10 운영 체제
 - 모든 버전의 Visual Studio 2015
 - Windows 10 개발자 도구
 - 장치가 개발자 모드 설정
- [Visual Studio에 대 한 MonoGame 3.5](http://www.monogame.net/2016/03/17/monogame-3-5/) 이상 버전

이 참조에 대 한 자세한 내용은 [Windows 10 UWP 개발을 위한 설정에 대 한 페이지](https://msdn.microsoft.com/en-us/windows/uwp/get-started/get-set-up)합니다.

소매 Xbox One 하드웨어에서 Xbox One 게임을 개발할 수 있습니다. 추가 소프트웨어 개발 PC 및 Xbox One 모두에 필요 합니다. 이 페이지를 참조는 Xbox One 게임 개발에 대 한 구성에 대 한 [는 Xbox One 설정](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/index)합니다.


# <a name="creating-an-empty-template"></a>빈 템플릿 만들기

필요한 리소스를 모두 설치 된 Windows 10 컴퓨터에서 개발자 모드가 활성화 된 후 Visual Studio를 사용 하 여 다음이 단계를 수행 하 여 새 MonoGame 프로젝트를 만들 수 있습니다.

1. 선택 **파일** > **새** > **프로젝트...**
1. 선택 된 **설치 됨** > **템플릿** > **Visual C#** > **MonoGame** 범주: 


    ![](uwp-images/image1.png "MonoGame 범주")


1. 선택 된 **MonoGame Windows 10 유니버설 프로젝트** 옵션: 


    ![](uwp-images/image2.png "MonoGame Windows 10 유니버설 프로젝트 옵션 선택")


1. 새 프로젝트에 대 한 이름을 입력 하 고 클릭 **확인**합니다.
확인을 클릭 하면 모든 오류를 표시 하는 Visual Studio, Windows 10 도구 설치 되 고 장치가 개발자 모드에서 인지 확인 하십시오. 

Visual Studio 서식 파일 만들기를 완료 한 후 실행 빈 프로젝트를 볼 수 실행할 수 있습니다.

![](uwp-images/image3.png "Visual Studio를 실행 하는 빈 프로젝트 참조를 실행 하 고 서식 파일을 만들기를 완료 한 번")

모퉁이에 있는 숫자의 진단 정보를 제공 합니다. 코드를 삭제 하 여이 정보를 제거할 수 `App.xaml.cs` 에 `DEBUG` 블록은 `OnLaunched` 메서드:


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

# <a name="running-on-xbox-one"></a>Xbox 하나에서 실행

UWP 프로젝트에서는 동일한 프로젝트에서 Windows 10 장치에 배포할 수 있습니다. Windows 10 개발 컴퓨터 및 Xbox One을 설정한 후 원격 컴퓨터에 대상 전환 하 고 Xbox One의 IP 주소를 입력 하 여 UWP 앱을 배포할 수 있습니다.

![](uwp-images/remote.png "원격 컴퓨터에 대상 전환 Xbox 것 IP 주소를 입력 하 여 UWP 앱을 배포할 수 있습니다.")

Xbox One에 흰색 테두리 Tv에 대 한 안전 하지 않은 영역을 나타냅니다. 자세한 내용은 참조는 [안전 영역 섹션](#Safe_Area_on_Xbox_One)합니다.

![](uwp-images/safearea.png "Xbox One에 흰색 테두리 Tv에 대 한 안전 하지 않은 영역을 나타냅니다.")

## <a name="safe-area-on-xbox-one"></a>Xbox 하나에 안전 하 게 보호 영역

콘솔에 대 한 게임 개발 영역 (예: I 또는 HUD) 중요 한 모든 시각적 개체가 포함 되어야 하는 화면 가운데에는 안전 하 게 보호 영역을 고려할 필요 합니다. 이 영역에 배치 하는 시각적 개체를 부분적으로 나 완전히 표시 일부 디스플레이 않을 모든 텔레비전에 표시 되려면 영역 외부에 안전 하 게 보호 영역은 아닙니다.

Xbox one MonoGame 템플릿 보호 영역을 고려 하 고 흰색 테두리로 렌더링 합니다. 이 영역은의 게임에서 런타임에 반영 됩니다. `Window.ClientBounds` 이 이미지의 Visual Studio에서 조사식 창에 표시 된 것 처럼 속성입니다. 클라이언트 경계 높이 1920 x 1080 디스플레이 해상도 불구 하 고 1016 인지 확인 합니다.

![](uwp-images/clientbounds.png "클라이언트 영역 높이 1016 1920 x 1080 디스플레이 해상도 불구 하 고")


# <a name="referencing-content-in-uwp-projects"></a>UWP 프로젝트에서 콘텐츠를 참조합니다.

파일에서 직접 또는 MonoGame 프로젝트에서 콘텐츠를 참조할 수 있습니다는 [MonoGame 콘텐츠 파이프라인](~/graphics-games/cocossharp/content-pipeline/index.md)합니다. 작은 게임 프로젝트 파일에서 로드의 단순성에서 활용할 수 있습니다. 대규모 프로젝트의 콘텐츠 크기를 줄이고 로드 시간을 최적화 하기 위해 콘텐츠 파이프라인을 사용 하 여에서 도움이 됩니다. Xbox 360에서 XNA와 달리는 `System.IO.File` 클래스는 Xbox 하나 UWP 앱에서 사용할 수 있습니다.

콘텐츠 파이프라인을 사용 하 여 콘텐츠를 로드 합니다. 자세한 내용은 참조는 [콘텐츠 파이프라인 가이드](~/graphics-games/cocossharp/content-pipeline/index.md)합니다. 


## <a name="loading-content-from-file"></a>파일에서 콘텐츠를 로드합니다.

IOS 및 Android와 달리 UWP 프로젝트에서는 실행 파일을 기준으로 파일을 참조할 수 있습니다. 간단한 게임 수정 하 고 콘텐츠 파이프라인 프로젝트를 빌드할 필요 없이이 기술은 부하 콘텐츠를 사용할 수 있습니다.

로드 한 `Texture2D` 파일에서:

1. UWP 프로젝트에서 콘텐츠 폴더에.png 파일을 추가 합니다. 콘텐츠 폴더에 콘텐츠 추가 XNA 및 MonoGame 규칙입니다.
1. 새로 추가한 PNG 단추로 클릭 하 고 속성을 선택 합니다.
1. 변경 된 **출력 디렉터리로 복사** 를 **변경 된 내용만 복사**합니다.
1. 다음 코드를 로드 하 여 게임의 Initialize 메서드를 추가 `Texture2D`:

    ```csharp
    Texture2D texture;
    using (var stream = System.IO.File.OpenRead("Content/YourPngName.png"))
    {
        texture = Texture2D.FromStream(graphics.GraphicsDevice, stream);
    }
    ```

사용 하 여 대 한 자세한 내용은 `Texture2D`, 참조는 [MonoGame 가이드 소개](~/graphics-games/monogame/introduction/index.md)합니다.


# <a name="summary"></a>요약

이 가이드에서는 파일을 로드할 때 UWP 별 고려 사항 및 새 UWP 프로젝트를 만드는 방법을 설명 합니다. 개발자가 전체 UWP 게임을 만들기에 관심이 자세히 알아볼 수에서 MonoGame에 대 한는 [MonoGame 가이드 소개](~/graphics-games/monogame/introduction/index.md)합니다.