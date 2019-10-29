---
title: Xamarin에서 tvOS 아이콘 및 이미지 작업
description: 이 문서에서는 Xamarin으로 빌드된 tvOS 앱에서 아이콘과 이미지를 사용 하는 방법을 설명 합니다. 시작 이미지, 계층화 된 이미지, 앱 아이콘 등을 설명 합니다.
ms.prod: xamarin
ms.assetid: A2DA4347-0563-4C72-A8D7-5B9DE9E28712
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: b1b6d07b221f702b54833bd87161d6abbadbd4e8
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030847"
---
# <a name="working-with-tvos-icons-and-images-in-xamarin"></a>Xamarin에서 tvOS 아이콘 및 이미지 작업

Captivating 아이콘과 이미지를 만드는 것은 Apple TV 앱에 대 한 몰입 형 사용자 환경을 개발 하는 데 중요 한 부분입니다. 이 가이드는 tvOS 앱에 필요한 그래픽 자산을 만들고 포함 하는 데 필요한 단계를 다룹니다.

- [시작 이미지](#Launch-Image) -앱이 처음 시작 될 때 시작 이미지가 표시 되 고, 시작이 완료 되 면 앱의 첫 번째 화면으로 교체 됩니다.
- [계층화 된 이미지](#Layered-Images) -apple TV와 관련 된, apple의 새 계층화 된 이미지는 시차 효과와 함께 작동 하 여 선택한 항목에 대 한 3d 효과를 만듭니다. 여러 가지 방법으로 [계층화 된 이미지를 만들](#Creating-Layered-Images)수 있습니다.
- [앱 아이콘](#App-Icons) -Apple TV 홈 화면은 있지만 앱 스토어에는 아이콘이 필요 합니다. 이러한 이미지는 계층화 된 이미지로 제공 되어야 합니다.
- [상위 선반 이미지](#Top-Shelf-Image) -앱이 홈 화면의 맨 위 행에 배치 되는 경우 앱의 기능을 강조 표시 하는 데 인기 이미지가 필요 합니다. 필요에 따라 [동적 최고 선반 콘텐츠](#Dynamic-Top-Shelf-Content) 를 제공 하 여 앱의 콘텐츠를 강조 표시할 수 있습니다.
- [이미지 Game Center](#Game-Center-Images) -앱이 게임 이며 Game Center를 사용 하는 경우 몇 가지 추가 이미지가 필요 합니다.
- [TvOS 프로젝트 이미지 설정](#Setting-Xamarin.tvOS-Project-Images) -tvOS 앱에 대 한 시작 이미지 및 앱 아이콘을 설정 하는 데 필요한 단계를 설명 합니다.

> [!IMPORTANT]
> Apple TV의 모든 이미지는 1x 해상도 (`@1x`) 이며이 크기의 _이미지만 사용 해야 합니다._ 더 크고 해상도가 높은 그래픽을 포함 하면 메모리와 저장소를 다운로드 하 여 사용 하는 데 시간이 소요 될 뿐만 아니라 런타임에 동적으로 재조정 하 고 그리기 성능에 부정적인 영향을 미칠 수 있습니다.

<a name="Launch-Image" />

## <a name="launch-image"></a>시작 이미지

시작 이미지는 tvOS 앱이 Apple TV에서 처음 시작 될 때 가장 먼저 표시 되는 것 이므로 모든 tvOS 앱에서 시작 이미지를 제공 해야 합니다. 

시작 이미지가 빠르게 나타나고 앱이 빠르며 응답성이 뛰어난 느낌을 제공 합니다. Apple TV는 시작 이미지를 즉시 앱의 첫 번째 화면으로 바꿉니다.

시작 이미지는 광고 또는 미술 식의 기회가 아니므로 앱이 신속 하 게 시작 되 고 사용할 준비가 되었다는 느낌을 제공 하기 위해 존재 합니다.

|시작 이미지 크기|노트|
|---|---|
|1920x1080px|계층화 되지 않은 .png 파일만|

Apple은 앱의 시작 이미지를 설계할 때 다음과 같은 제안을 합니다.

- **첫 번째 화면과 거의 동일** 합니다. 시작 화면은 가능한 한 앱의 첫 화면 가까이에 있어야 합니다. 다른 그래픽 또는 요소를 포함 하면 첫 번째 화면이 나타날 때 "flash"가 발생 하지 않을 수 있습니다.
- 텍스트 시작 이미지를 **사용 하지 않는** 것이 정적 이므로 표시 되기 전에 지역화 되지 않습니다.
- **다운 플레이 시작** -Apple TV 사용자는 자주 앱을 전환 하므로 앱 시작 프로세스에 주의를 기울여야 합니다.
- **광고 또는 브랜딩 없음** -시작 이미지를 정보 화면으로 사용 하거나 응용 프로그램의 첫 번째 화면에서 정적 부분이 아니면 브랜딩을 포함 하지 않아야 합니다. 광고는 엄격히 금지 됩니다.

<a name="Setting-the-Launch-Image" />

### <a name="setting-the-launch-image"></a>시작 이미지 설정

TvOS 프로젝트에 대 한 시작 이미지를 설정 하려면 다음을 수행 하십시오.

1. **솔루션 탐색기**에서 `Assets.xcassets`를 두 번 클릭 하 여 편집용으로 엽니다. 

    [![](icons-images-images/asset01.png "The Assets.xcassets file")](icons-images-images/asset01.png#lightbox)
2. **자산 편집기**에서 `LaunchImages` 자산을 클릭 합니다. 

    [![](icons-images-images/asset02.png "The LaunchImages asset")](icons-images-images/asset02.png#lightbox)
3. **1X APPLE TV** 항목을 클릭 하 고 시작 이미지를 선택 하거나 필요에 따라 파일 시스템에서 새 이미지를 끌어 놓습니다. 

    [![](icons-images-images/asset03.png "Select a Launch Image")](icons-images-images/asset03.png#lightbox)
4. 변경 내용을 저장합니다.

<a name="Layered-Images" />

## <a name="layered-images"></a>계층화 된 이미지

Apple TV를 처음 사용 하는 경우 계층화 된 이미지는 시차 효과와 함께 작동 하 여 사용자가 소파에 걸친 화면에 있는 콘텐츠에 연결 된 상태를 유지 하는 데 도움이 되는 3D 효과를 생성 합니다.

계층화 된 이미지에는 완전 한 이미지를 형성 하기 위해 결합 된 별도의 두 계층 (5)이 포함 됩니다. 배경 계층을 제외 하 고 각 계층은 투명도와 함께 Z 순서를 사용 하 여 깊이의 효과를 만듭니다. 사용자가 계층화 된 이미지와 상호 작용 하는 경우 더 높은 Z 순서 계층이 확장 되 고 겹쳐진 효과를 만듭니다.

[![](icons-images-images/layered01.png "Layered Images Z-ordered diagram")](icons-images-images/layered01.png#lightbox)

> [!IMPORTANT]
> 계층화 된 이미지는 앱 아이콘에 필요 하며, 포커스를 받을 수 있는 기타 [항목](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection) (예: 최상위 선반 이미지)의 경우 선택 사항입니다. 그러나 Apple은 앱에서 포커스를 받을 수 있는 모든 이미지에 대해 계층화 된 이미지를 사용 하도록 제안 합니다.

Apple은 계층화 된 이미지를 설계할 때 다음과 같은 제안을 합니다.

- **배경 계층을 불투명 하 게 만들기** -배경 계층 (계층 1)이 불투명 **해야 합니다** . 그렇지 않으면 Apple TV에서 계층화 된 이미지를 사용 하려고 할 때 오류가 발생 합니다. 다른 모든 계층은 3D 효과를 향상 시키기 위해 여러 수준의 투명도를 포함할 수 있습니다.
- **전경, 중간 및 배경 요소 격리** -포그라운드에서 중요 한 요소 (예: 게임 문자)를 사용 하 여 보조 요소나 그림자에 대해 중간을 사용 합니다. 마지막으로 상위 계층에 대 한 단계를 제공 하기 위해 중립 배경을 포함 합니다.
- 텍스트를 더 높은 수준으로 표시 하지 않으려는 경우에만 **전경 텍스트를 유지** 합니다. 일반적으로 최상위 계층에 있어야 합니다.
- **단순 계층화 사용** -시차 효과는 미묘한 효과를 갖도록 설계 되었으므로 jarring가 비현실적인 효과를 방지 하기 위해 계층을 최소로 유지 합니다.
- **안전한 영역 포함** -시차 효과 중에 상위 계층을 자를 수 있으므로 각 계층에 안전 영역 테두리를 만들어야 합니다. 콘텐츠를 너무 가까이 만들면 레이어가 잘릴 수 있습니다. 상위 계층에는 하위 계층 보다 더 많은 크기 조정 및 자르기가 발생 합니다. 아래의 [이미지 계층 크기 조정](#Sizing-Image-Layers) 섹션을 참조 하세요.
- **미리 보기 자주** 계층화 된 이미지는 원하는 3d 효과가 발생 하 고 개별 레이어의 콘텐츠를 잘라내는 것이 없도록 자주 미리 보아야 합니다. 실제 Apple TV 하드웨어에서 계층화 된 이미지를 미리 보고 제대로 렌더링 되는지 확인 해야 합니다.

가능 하면 항상 기본 제공 `UIKit` 컨트롤을 사용 하 여 계층화 된 이미지를 표시 해야 합니다 .이는 포커스를 받을 때 시차 효과가 자동으로 발생 하기 때문입니다.

<a name="Sizing-Image-Layers" />

### <a name="sizing-image-layers"></a>이미지 계층 크기 조정

계층화 된 이미지를 구성 하는 각 계층에 _안전 영역_ 테두리를 포함 해야 합니다. 개별 레이어는 크기를 조정 하 고 시차 효과를 낼 수 있으므로 계층의 가장자리에 너무 가까운 경우에는 레이어의 콘텐츠를 자를 수 있습니다.

[![](icons-images-images/layered02.png "35 pixel border")](icons-images-images/layered02.png#lightbox)

<a name="Creating-Layered-Images" />

### <a name="creating-layered-images"></a>계층화 된 이미지 만들기

tvOS는 다음과 같은 형식의 계층화 된 이미지와 함께 작동 합니다.

- **CAR 파일** -Apple에서 만든 소유 자산 카탈로그 형식입니다. 직접 자동차 파일을 만들지 않고, 모든 LSR 파일에서 컴파일 시간에 생성 되며, 앱 번들에 포함 됩니다.
- **Lsr 이미지** -Apple에서 만든 소유 이미지 형식입니다. [시차 내보내기 Adobe Photoshop 플러그 인](https://itunespartner.apple.com/assets/downloads/ParallaxExporter_Apps.zip) 또는 [시차 미리 보기](https://itunespartner.apple.com/assets/downloads/Parallax%20Previewer.dmg) 를 사용 하 여 Lsr 형식으로 계층화 된 이미지를 만들고 미리 볼 수 있습니다.
- **Assets.xcassets** -2 (2)에서 5 (5) 표준 `.png` 형식 이미지는 컴파일 시간에 CAR 또는 Lsr 형식의 계층화 된 이미지로 컴파일될 자산 카탈로그에 포함 됩니다.
- **LCR 파일** -Apple에서 만든 독점 파일 형식입니다. LCR 파일은 콘텐츠 서버 중 하나에서 다운로드 된 추가 내용으로 사용 됩니다. LCR 파일은 앱 번들에 포함 되어서는 안 됩니다. Xcode에 포함 된 `layerutil` 명령줄 도구를 사용 하 여 LSR 또는 Photoshop 파일에서 LCR 파일이 생성 됩니다.

<a name="The-Parallax-Previewer" />

### <a name="the-parallax-previewer"></a>시차 미리 보기

Apple은 [시차](https://itunespartner.apple.com/assets/downloads/Parallax%20Previewer.dmg) 미리 보기를 만들어 앱 아이콘 및 선택적 포커스 가능한 항목에 필요한 계층화 된 이미지를 미리 보고 만들었습니다. 미리 보기에는 완료 된 계층화 이미지를 형성 하는 모든 계층이 표시 됩니다.

[![](icons-images-images/layered03.png "The Parallax Previewer")](icons-images-images/layered03.png#lightbox)

계층화 된 이미지를 미리 보는 동안 마우스를 사용 하 여 이미지를 회전 하 고 시차 효과를 미리 볼 수 있습니다. **+** (더하기) 및 **-** (빼기) 단추를 사용 하 여 레이어를 추가 하 고 제거 합니다.

새 계층화 된 이미지를 만들 때 LSR 형식으로 내보내고 앱의 번들에 포함할 수 있습니다.

계층화 된 이미지 만들기 및 미리 보기에 대 한 자세한 내용은 [앱 프로그래밍 가이드 tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)의 Apple [시차 아트 워크 만들기](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/CreatingParallaxArtwork.html#//apple_ref/doc/uid/TP40015241-CH19-SW1) 섹션을 참조 하세요.

<a name="App-Icons" />

## <a name="app-icons"></a>앱 아이콘

TvOS 앱에는 Apple TV 홈 화면의 앱 아이콘 뿐만 아니라 앱 스토어에 대 한 아이콘도 필요 합니다. 앱 아이콘은 잠재적 사용자에 게 멋진 느낌을 주는 첫 번째 변경 사항이 며, 앱의 용도를 한눈에 파악할 수 있습니다.

[![](icons-images-images/icon01.png "The App Icon")](icons-images-images/icon01.png#lightbox)

모든 앱은 앱 아이콘의 작은 버전과 많은 버전을 모두 제공 해야 합니다. 앱이 설치 되 면 Apple TV 홈 화면에서 작은 아이콘이 사용 됩니다. 앱 스토어에서 많은 버전이 사용 됩니다. 크게 된 앱 아이콘은 작은 아이콘 버전의 모양과 느낌을 모방 해야 합니다.

|작은 아이콘||크게 아이콘||
|---|---|---|---|
|실제 크기|400x240px|Size|1280x768px|
|안전한 영역 크기|370x222px|||
|포커스가 없는 크기|300x180px|||
|포커스가 있는 크기|370x222px|||

> [!IMPORTANT]
> 앱 아이콘은 **계층화 된 이미지로**제공 되어야 합니다. 자세한 내용은 위의 [계층화 된 이미지](#Layered-Images) 섹션을 참조 하세요.

Apple은 앱 아이콘을 만들기 위한 다음과 같은 제안 사항을 제공 합니다.

- **단일 포커스 지점 제공** – 아이콘 가운데에 직접 배치 된 단일 포커스 지점을 사용 하 여 아이콘을 디자인 합니다.
- **간단한 배경 사용** – 위쪽 계층이 보이도록 아이콘 배경을 단순하게 유지 합니다. 단순 색 또는 미묘한 그라데이션을 사용 하는 것이 좋습니다.
- **텍스트 크기 제한** – 사용자가 선택할 때 앱 이름이 아이콘 아래에 표시 되므로 아이콘 디자인에 필수적인 경우에만 텍스트를 포함 해야 합니다.
- **스크린샷 사용 안 함** -스크린 샷는 아이콘에 대해 너무 복잡 하며 사용자가 응용 프로그램의 용도를 한눈에 볼 수 없습니다.
- **아이콘을 사각형으로 유지** – tvOS는 아이콘의 모퉁이를 약간 둥글게 하는 마스크를 자동으로 적용 합니다. 이 반올림은 포함 하지 마세요.
- **계층을 신중** 하 게 구분 합니다. 텍스트는 위쪽 계층에, 가운데에는 보조 항목, 상위 계층은 빛을 표시할 수 있도록 하는 중립 배경으로 되어 있어야 합니다.
- **그라데이션 및 그림자를 신중 하 게 사용** – 그라데이션과 그림자는 시차 효과와 충돌 하 여 신중 하 게 사용 해야 합니다. 간단한 위쪽에서 아래쪽, 밝은-짙은 그라데이션 스타일은 가장 적합 합니다. 그림자는 일반적으로 선명한 선명한 선명한 색조로 가장 잘 작동 합니다.
- **레이어 투명도 변경** – 응용 프로그램 아이콘의 상위 수준에서 다양 한 수준의 투명도를 사용 하 여 3d 효과를 늘립니다. 배경 계층은 불투명 해야 하며 그렇지 않으면 오류가 발생 합니다.

<a name="Setting-the-App-Icons" />

### <a name="setting-the-app-icons"></a>앱 아이콘 설정

TvOS 프로젝트에 필요한 앱 아이콘을 설정 하려면 다음을 수행 하세요.

1. **솔루션 탐색기**에서 `Assets.xcassets`를 두 번 클릭 하 여 편집용으로 엽니다. 

    [![](icons-images-images/asset01.png "The Assets.xcassets fileg")](icons-images-images/asset01.png#lightbox)
2. **자산 편집기**에서 `App Icon & Top Shelf Image` 자산을 확장 합니다. 

    [![](icons-images-images/asset04.png "Expand the Top Shelf Image asset")](icons-images-images/asset04.png#lightbox)
3. 다음으로 `App Icon - Small` 자산을 확장 합니다. 

    [![](icons-images-images/asset05.png "Expand the App Icon - Small asset")](icons-images-images/asset05.png#lightbox)
4. 그런 다음 `Back` 자산을 확장 하 고 `Contents` 항목을 클릭 합니다. 

    [![](icons-images-images/asset06.png "Then expand the Back asset")](icons-images-images/asset06.png#lightbox)
5. **1X APPLE TV 항목** 을 클릭 하 고 이미지 파일을 선택 합니다.
6. `Front` 및 `Middle` 자산에 대해 위의 단계를 반복 합니다.
7. 그런 다음 동일한 단계를 반복 하 여 `App Icon - Large` 자산을 정의 합니다.
8. 변경 내용을 저장합니다.

<a name="Top-Shelf-Image" />

## <a name="top-shelf-image"></a>상위 선반 이미지

사용자가 Apple TV 홈 화면의 맨 위 행에 tvOS 앱을 배치 하는 경우 사용자가 앱을 선택 하면 최상위 선반 이미지가 표시 됩니다. 이 이미지는 앱의 기능을 강조 표시 하거나 콘텐츠에 대 한 직접 링크를 제공 해야 합니다.

[![](icons-images-images/topshelf01.png "Top Shelf Image example")](icons-images-images/topshelf01.png#lightbox)

최상위 선반 이미지는 단일 정적 `.png` 또는 `.lsr` 파일로 제공 하거나 ( [계층화 된 이미지 만들기](#Creating-Layered-Images)참조) 런타임에 포커스를 받을 수 있는 항목의 단일 행으로 동적으로 만들 수 있습니다 (아래의 [동적 상위 선반 내용](#Dynamic-Top-Shelf-Content) 참조).

|상위 선반 이미지 크기|노트|
|---|---|
|1920x720px|정적 .png 또는 계층화 된 .lsr 파일|

Apple은 최고 선반 이미지를 만들기 위해 다음과 같은 제안 사항을 제공 합니다.

- **Rich Static 이미지 사용** – 앱이 동적 콘텐츠를 제공 하지 않는 경우에는 최상위 선반 이미지가 포커스를 받을 수 없습니다. 이 이미지를 사용 하 여 앱 또는 브랜드의 기능을 강조 표시 합니다.
- **앱 콘텐츠에 대 한 링크** – 동적 최고 선반 레이아웃은 사용자가 앱에서 가장 중요 한 콘텐츠를 찾는 빠른 링크를 제공 합니다. 이 영역을 사용 하 여 앱을 시작 하 고 즉시 지정 된 콘텐츠로 이동 하는 빠른 링크를 제공 합니다.
- **최신 콘텐츠 소개** – 풍부한 인기 있는 콘텐츠를 통해 사용자를 앱에 그려 더 사용할 수 있습니다. 이를 영역으로 사용 하 여 가장 높은 등급 또는 최신 콘텐츠를 소개 합니다.
- **개인 설정 된 콘텐츠** – 사용자가 가장 자주 사용 하거나 즐겨 사용 하는 앱을 홈 화면의 맨 위 행에 저장 합니다. 가장 관심 있는 콘텐츠를 표시 하려면 최상위 선반을 사용 합니다.
- **광고 허용 안** 됨 – 광고는 맨 위 선반에 표시 되지 않도록 엄격히 금지 됩니다. 최신 구입 가능 콘텐츠를 표시할 수 있지만 가격 정보는 표시 되지 않습니다.

### <a name="setting-the-top-shelf-image"></a>최고 선반 이미지 설정

TvOS 프로젝트에 필요한 최상위 선반 이미지를 설정 하려면 다음을 수행 하십시오.

1. **솔루션 탐색기**에서 `Assets.xcassets`를 두 번 클릭 하 여 편집용으로 엽니다. 

    [![](icons-images-images/asset01.png "The Assets.xcassets file")](icons-images-images/asset01.png#lightbox)
2. **자산 편집기**에서 `App Icon & Top Shelf Image` 자산을 확장 합니다. 

    [![](icons-images-images/asset04.png "Expand the Top Shelf Image asset")](icons-images-images/asset04.png#lightbox)
3. `Top Shelf Image` 자산을 클릭 합니다. 

    [![](icons-images-images/asset07.png "The Top Shelf Image asset")](icons-images-images/asset07.png#lightbox)
4. **1X APPLE TV 항목** 을 클릭 하 고 이미지 파일을 선택 합니다.
5. 변경 내용을 저장합니다.

<a name="Dynamic-Top-Shelf-Content" />

### <a name="dynamic-top-shelf-content"></a>동적 최고 선반 콘텐츠

정적 최상위 선반 이미지를 표시 하는 대신 맨 위 선반에는 포커스를 받을 수 있는 [항목](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection) 의 동적 행 이나 스크롤 배너의 동적 집합이 포함 될 수 있습니다. 이러한 동적 스타일을 사용 하면 앱에서 제공 하는 콘텐츠를 강조 표시 하거나 가장 많이 사용 하는 기능으로 바로 이동할 수 있습니다.

<a name="Sectioned-Content-Row" />

#### <a name="sectioned-content-row"></a>단면화 콘텐츠 행

이 동적 상위 선반 콘텐츠 유형은 스크롤 된 단일 행을 제공 합니다. 포커스 가능 항목은 선택적으로 섹션으로 세분화 됩니다. 일반적으로 새로 만들기, 즐겨찾기 또는 최근 본 앱 콘텐츠를 강조 표시 하는 데 사용 됩니다.

콘텐츠는 현재 선택 된 콘텐츠 (현재 포커스가 있음) 아래에 레이블이 표시 된 단일 가로 스크롤 콘텐츠 목록으로 표시 됩니다. 사용자가 지정 된 콘텐츠를 선택 하면 앱이 시작 되 고 해당 콘텐츠로 직접 가져와야 합니다.

필요한 콘텐츠 크기는 다음과 같습니다.

||포스터 (2:3)|제곱 (1:1)|HDTV (16:9)|
|---|---|---|---|
|실제 크기|404x608px|608x608px|908x512px|
|안전한 영역 크기|380x570px|570x570px|852x479px|
|포커스가 없는 크기|333x500px|500x500px|782x440px|
|포커스가 있는 크기|380x570px|570x570px|852x479px|

Apple은 단면화 된 콘텐츠 행에 대해 다음과 같은 제안 사항을 제공 합니다.

- **행 완료** – 화면의 전체 너비를 확장할 수 있는 충분 한 콘텐츠를 제공 해야 합니다.
- **혼합 이미지 크기 조정** – 단면화 된 콘텐츠 행은 위에 제공 된 목록의 이미지 크기를 혼합 하 여 저장 하도록 디자인 되었습니다. 그러나 이미지 크기를 혼합 하는 경우 콘텐츠 표시를 정규화 하기 위해 추가 크기가 적용 됩니다.

<a name="Scrolling-Inset-Banners" />

#### <a name="scrolling-inset-banners"></a>스크롤 삽입 배너

필요에 따라 tvOS 앱은 화면을 거의 채우는 배너의 자동 스크롤 및 반복 컬렉션으로 최상위 선반에 콘텐츠를 제공할 수 있습니다. 이 스타일은 일반적으로 새로운 TV 쇼와 같은 풍부한 새 콘텐츠를 소개 하는 데 사용 됩니다.

자동 스크롤 외에도 사용자가 배너를 제어 하 고 Siri 원격을 사용 하 여 어느 방향으로 든 스크롤할 수 있습니다. 배너가 포커스가 있을 때 Siri 원격에서 작은 원형 제스처를 만들면 해당 배너에 대 한 시차 효과가 활성화 됩니다.

**배너 이미지 (추가 너비)**

|   |   |
|---|---|
|실제 크기|1940x624px|
|안전한 영역 크기|1740x620px|
|포커스가 없는 크기|1740x560px|
|포커스가 있는 크기|1740x620px|

스크롤 삽입 배너는 정적 `.png` 또는 계층화 된 `.lsr` 파일로 제공 될 수 있습니다.

Apple은 스크롤 삽입 배너에 대해 다음과 같은 제안 사항을 제공 합니다.

- **내용 금액** -자연스럽 게 스크롤할 수 있도록 최소 3 개의 배너를 제공 해야 합니다. 8 개 이하의 배너를 포함 하거나 최종 사용자를 위해 탐색을 수행 해야 합니다.
- **콘텐츠 텍스트** -배너 이미지에 배너를 포함 해야 하는 경우에는 텍스트를 포함 해야 합니다. 계층화 된 이미지를 사용 하는 경우 텍스트가 최상위 계층에 있어야 합니다.

동적 최고 선반 콘텐츠를 제공 하기 위해 앱에 최상위 선반 확장을 추가 하는 방법에 대 한 자세한 내용은 Apple의 [Tvservices 프레임 워크 참조](https://developer.apple.com/library/prerelease/tvos/documentation/TVServices/Reference/TVServices_Ref/index.html#//apple_ref/doc/uid/TP40016412) 를 참조 하세요.

<a name="Game-Center-Images" />

## <a name="game-center-images"></a>Game Center 이미지

TvOS 앱이 게임 이며 지원 Game Center 포함 된 경우 몇 가지 추가 이미지 자산이 필요 합니다.

- **성과 아이콘** -원으로 자동 잘려는 각 성과에 불투명 이미지가 필요 합니다. 업적은 포커스를 받을 수 없는 항목입니다.
- **대시보드 아트 워크** -Game Center 내에서 앱 대시보드의 맨 위에 표시 되는 선택적 이미지를 제공할 수 있습니다. 이러한 이미지는 포커스를 받을 수 없습니다.
- **순위표 아트 워크** -앱에서 지 원하는 각 순위표에 대해 1 ~ 3 개의 16:9 가로 세로 비율 이미지를 제공 해야 합니다. 이러한 파일은 정적 `.png` 또는 계층화 된 `.lsr` 파일 일 수 있습니다. 순위표 아트 워크는 포커스를 받을 수 있습니다.

||성과 아이콘|대시보드 아트 워크|순위표 아트 워크|
|---|---|---|---|
|표시 크기|200x200px|923x150px|N/A|
|실제 크기|320x320px|N/A|659x371px|
|안전한 영역 크기|N/A|N/A|618x348px|
|포커스가 없는 크기|N/A|N/A|548x309px|
|포커스가 있는 크기|N/A|N/A|618x348px|

Game Center 작업에 대 한 자세한 내용은 Apple의 [Game Center 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/NetworkingInternet/Conceptual/GameKit_Guide/Introduction/Introduction.html)를 참조 하세요.

<a name="Working-with-Images" />

## <a name="working-with-images"></a>이미지 작업

TvOS 9는 iOS 9의 하위 집합 이므로 Xamarin.ios 앱에 이미지를 포함 하 고 표시 하는 데 사용 되는 것과 동일한 기법은 tvOS 앱 에서도 작동 합니다. 자세한 내용은 [이미지 표시](~/ios/app-fundamentals/images-icons/displaying-an-image.md) 설명서를 참조 하세요.

<a name="Setting-Xamarin.tvOS-Project-Images" />

## <a name="setting-xamarintvos-project-images"></a>TvOS 프로젝트 이미지 설정

위에서 설명한 것 처럼 모든 tvOS apps에는 [시작 이미지](#Launch-Image)및 [앱 아이콘이](#App-Icons)필요 합니다. 이 섹션에서는 자산 카탈로그에 설정 된 후 tvOS 앱 프로젝트에 대 한 시작 이미지 및 앱 아이콘을 선택 하는 방법을 설명 합니다.

다음을 수행합니다.

1. **솔루션 탐색기**에서 `Info.plist`를 두 번 클릭 하 여 편집용으로 엽니다. 

    [![](icons-images-images/info01.png "The Info.plist file")](icons-images-images/info01.png#lightbox)
2. **Info.plist 편집기**에서 **앱 아이콘**에 대 한 자산 카탈로그 ( [앱 아이콘 설정](#Setting-the-App-Icons) 섹션에서 위에 구성 됨)를 선택 합니다. 

    [![](icons-images-images/info02.png "The Info.Plist Editor")](icons-images-images/info02.png#lightbox)
3. 다음으로 **시작 이미지**에 대 한 자산 카탈로그 ( [시작 이미지 설정](#Setting-the-Launch-Image) 섹션에서 구성 됨)를 선택 합니다.
4. 변경 내용을 저장합니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 tvOS 앱에서 사용 되는 모든 이미지 형식 및 크기를 다루었습니다. 먼저 이미지 시작, 계층화 된 이미지, 앱 아이콘, 인기 이미지 및 Game Center 이미지를 살펴보았습니다. 그런 다음 tvOS 앱에서 이미지 작업에 대해 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 가이드](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
