---
title: MonoGame UWP 프로젝트 만들기
description: MonoGame를 사용 하 여 하나의 코드 베이스와 하나의 콘텐츠 집합을 사용 하 여 여러 장치를 대상으로 하는 유니버설 Windows 플랫폼 게임 및 앱을 만들 수 있습니다.
ms.prod: xamarin
ms.assetid: C6B99E44-00C1-4139-A1B7-FCFBE8749AB1
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: aa43513154499a39c27f5ad35fce9584ce7827f8
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70763526"
---
# <a name="creating-a-monogame-uwp-project"></a>MonoGame UWP 프로젝트 만들기

_MonoGame를 사용 하 여 하나의 코드 베이스와 하나의 콘텐츠 집합을 사용 하 여 여러 장치를 대상으로 하는 유니버설 Windows 플랫폼 게임 및 앱을 만들 수 있습니다._

이 연습에서는 UWP (MonoGame 유니버설 Windows 플랫폼) 프로젝트 만들기 및 콘텐츠 로드에 대해 다룹니다. UWP 앱은 데스크톱, 태블릿, Windows phone 및 Xbox One을 비롯 한 모든 Windows 10 장치에서 실행할 수 있습니다.

이 연습에서는 *수레 국화 청색 blue* 배경 (XNA 응용 프로그램의 기존 배경색)을 표시 하는 빈 프로젝트를 만듭니다.

## <a name="requirements"></a>요구 사항

MonoGame UWP 앱을 개발 하려면 다음이 필요 합니다.

- Windows 10 운영 체제
- 모든 버전의 Visual Studio 2017
- Windows 10 개발자 도구
- 장치를 개발자 모드로 설정
- [MonoGame 3.7.1 For Visual Studio](http://community.monogame.net/t/monogame-3-7-1-release/11173) 이상

자세한 내용은 [Windows 10 UWP 개발에 대 한 설정에서이 페이지](https://msdn.microsoft.com/windows/uwp/get-started/get-set-up)를 참조 하세요.

Xbox one 게임은 소매 Xbox one 하드웨어에서 개발할 수 있습니다. 개발 PC와 Xbox one 모두에 추가 소프트웨어가 필요 합니다. 게임 개발용 Xbox one을 구성 하는 방법에 대 한 자세한 내용은 [xbox one 설정](https://msdn.microsoft.com/windows/uwp/xbox-apps/index)페이지를 참조 하세요.

## <a name="creating-an-empty-template"></a>빈 템플릿 만들기

모든 필수 리소스를 설치 하 고 Windows 10 컴퓨터에서 개발자 모드를 사용 하도록 설정한 후에는 다음 단계에 따라 Visual Studio를 사용 하 여 새 MonoGame 프로젝트를 만들 수 있습니다.

1. **파일** > **새로**만들기프로젝트 > ...를 선택 합니다.
1. **설치 된** > **템플릿** **Visual C#**  **MonoGame** category를 선택 합니다. >   > 

    ![](uwp-images/image1.png "MonoGame 범주")

1. **MonoGame Windows 10 유니버설 프로젝트** 옵션을 선택 합니다.

    ![](uwp-images/image2.png "MonoGame Windows 10 유니버설 프로젝트 옵션을 선택 합니다.")

1. 새 프로젝트의 이름을 입력 하 고 **확인**을 클릭 합니다.
확인을 클릭 한 후 Visual Studio에 오류가 표시 되 면 Windows 10 tools가 설치 되어 있고 장치가 개발자 모드에 있는지 확인 합니다.

Visual Studio에서 템플릿 만들기를 완료 한 후 실행 하 여 빈 프로젝트를 실행 하는 것을 확인할 수 있습니다.

![](uwp-images/image3.png "Visual Studio에서 템플릿 만들기를 완료 한 후 실행 하 여 빈 프로젝트를 실행 하는 것을 확인 합니다.")

모퉁이의 숫자는 진단 정보를 제공 합니다. `App.xaml.cs` 메서드의 블록`DEBUG` 에서 코드를 삭제 하 여이 정보를 제거할 수 있습니다. `OnLaunched`

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

UWP 프로젝트는 동일한 프로젝트에서 모든 Windows 10 장치에 배포할 수 있습니다. Windows 10 개발 컴퓨터 및 Xbox One을 설정한 후에는 대상을 원격 컴퓨터로 전환 하 고 Xbox One의 IP 주소를 입력 하 여 UWP 앱을 배포할 수 있습니다.

![](uwp-images/remote.png "대상을 원격 컴퓨터로 전환 하 고 Xbox One IP 주소를 입력 하 여 UWP 앱을 배포할 수 있습니다.")

Xbox One에서 흰색 테두리는 Tv에 대 한 안전 하지 않은 영역을 나타냅니다. 자세한 내용은 [안전 영역 섹션](#safe-area-on-xbox-one)을 참조 하세요.

![](uwp-images/safearea.png "Xbox One에서 흰색 테두리는 Tv에 대 한 안전 하지 않은 영역을 나타냅니다.")

### <a name="safe-area-on-xbox-one"></a>Xbox One의 안전 영역

콘솔에 대 한 게임을 개발 하려면 모든 중요 한 시각적 개체 (예: UI 또는 HUD)를 포함 해야 하는 화면 중심의 영역인 안전 영역을 고려해 야 합니다. 안전 영역 외부의 영역은 모든 tv에서 표시 되지 않을 수 있으므로이 영역에 배치 된 시각적 개체는 일부 디스플레이에서 부분적으로 또는 완전히 보이지 않을 수 있습니다.

Xbox One 용 MonoGame 템플릿은 안전 영역을 고려 하 여 흰색 테두리로 렌더링 합니다. 이 영역은 Visual Studio의 조사식 창 이미지에 표시 된 `Window.ClientBounds` 것 처럼 게임의 속성에서 런타임에도 반영 됩니다. 1920 x 1080 디스플레이 해상도에도 불구 하 고 클라이언트 경계의 높이는 1016입니다.

![](uwp-images/clientbounds.png "1920 x 1080 디스플레이 해상도에도 불구 하 고 클라이언트 경계의 높이는 1016입니다.")

## <a name="referencing-content-in-uwp-projects"></a>UWP 프로젝트에서 콘텐츠 참조

MonoGame 프로젝트의 콘텐츠는 파일에서 직접 또는 [MonoGame 콘텐츠 파이프라인](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/content-pipeline/introduction.md)을 통해 참조할 수 있습니다. 작은 게임 프로젝트는 파일에서 로드를 간소화 하는 이점을 누릴 수 있습니다. 대규모 프로젝트에서는 콘텐츠 파이프라인을 사용 하 여 콘텐츠를 최적화 함으로써 크기와 로드 시간을 줄일 수 있습니다. Xbox 360의 XNA와 달리 클래스는 `System.IO.File` xbox one UWP 앱에서 사용할 수 있습니다.

콘텐츠 파이프라인을 사용 하 여 콘텐츠를 로드 하는 방법에 대 한 자세한 내용은 [콘텐츠 파이프라인 가이드](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/content-pipeline/introduction.md)를 참조 하세요.

### <a name="loading-content-from-file"></a>파일에서 콘텐츠를 로드 하는 중

IOS 및 Android와 달리 UWP 프로젝트는 실행 파일을 기준으로 파일을 참조할 수 있습니다. 간단한 게임에서는이 기법을 사용 하 여 콘텐츠 파이프라인 프로젝트를 수정 하 고 빌드할 필요 없이 콘텐츠를 로드할 수 있습니다.

파일에서를 `Texture2D` 로드 하려면:

1. UWP 프로젝트의 Content 폴더에 .png 파일을 추가 합니다. 콘텐츠 폴더에 콘텐츠를 추가 하는 것은 XNA 및 MonoGame의 규칙입니다.
1. 새로 추가 된 PNG를 마우스 오른쪽 단추로 클릭 하 고 속성을 선택 합니다.
1. **출력 디렉터리로 복사** 를 변경 된 **내용만 복사**로 변경 합니다.
1. 게임의 Initialize 메서드에 다음 코드를 추가 하 여을 로드 합니다 `Texture2D`.

    ```csharp
    Texture2D texture;
    using (var stream = System.IO.File.OpenRead("Content/YourPngName.png"))
    {
        texture = Texture2D.FromStream(graphics.GraphicsDevice, stream);
    }
    ```

을 사용 하는 `Texture2D`방법에 대 한 자세한 내용은 [MonoGame 소개 가이드](~/graphics-games/monogame/introduction/index.md)를 참조 하세요.

## <a name="summary"></a>요약

이 가이드에서는 파일을 로드할 때 새 UWP 프로젝트 및 UWP 관련 고려 사항을 만드는 방법을 설명 합니다. 전체 UWP 게임을 만드는 데 관심이 있는 개발자는 [MonoGame 소개 가이드](~/graphics-games/monogame/introduction/index.md)에서 MonoGame에 대해 자세히 알아볼 수 있습니다.
