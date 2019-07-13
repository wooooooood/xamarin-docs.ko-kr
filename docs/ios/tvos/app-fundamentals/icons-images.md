---
title: TvOS 아이콘 및 Xamarin에서 이미지를 사용 하 여 작업
description: 이 문서에서는 아이콘 및 Xamarin에 내장 된 tvOS 앱에서 이미지를 사용 하는 방법을 설명 합니다. 시작 이미지, 계층화 된 이미지를 앱 아이콘 및 자세히 설명합니다.
ms.prod: xamarin
ms.assetid: A2DA4347-0563-4C72-A8D7-5B9DE9E28712
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: dab1b0f7bf7aabb4dfcfbfdcb5e202baa48e664d
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2019
ms.locfileid: "67865161"
---
# <a name="working-with-tvos-icons-and-images-in-xamarin"></a>TvOS 아이콘 및 Xamarin에서 이미지를 사용 하 여 작업

영상을 만들기 아이콘 및 이미지는 Apple TV 앱에 대 한 생생한 사용자 환경을 개발 하는 중요 한 부분입니다. 이 가이드는 만들고 Xamarin.tvOS 앱에 대 한 필요한 그래픽 자산을 포함 하는 데 필요한 단계를 설명 합니다.

- [이미지를 시작한 다음](#Launch-Image) -앱을 처음 시작 하 고 시작이 완료 되 면 앱의 첫 번째 화면으로 바뀝니다 시작 이미지를 표시 합니다.
- [계층화 된 이미지](#Layered-Images) -Apple TV에서 선택한 항목에 대 한 3D 효과 만드는 시차 효과 사용 하 여 Apple의 새 계층화 된 이미지 작업에 특정 합니다. 하는 방법은 여러 가지가 [계층화 된 이미지 만들기](#Creating-Layered-Images)합니다.
- [앱 아이콘](#App-Icons) -뿐 아니라 Apple TV 홈 화면에 대 한 하지만 앱 스토어 아이콘은 필수입니다. 계층화 된 이미지 형식으로 제공 되어야 합니다.
- [코너 이미지 상위](#Top-Shelf-Image) -앱 홈 화면 위쪽 행에 배치 되 면 앱의 기능을 강조 하는 인기 코너 이미지 필요 합니다. 제공할 수 있습니다 [동적 위쪽 선반 콘텐츠](#Dynamic-Top-Shelf-Content) 앱의 콘텐츠를 강조 표시 합니다.
- [게임 센터 이미지](#Game-Center-Images) -앱은 게임을 Game Center를 사용 하 여 면 몇 가지 추가 이미지가 필요 없게 됩니다.
- [Xamarin.tvOS 프로젝트 이미지 설정](#Setting-Xamarin.tvOS-Project-Images) -Xamarin.tvOS 앱에 대 한 앱 아이콘 및 시작 이미지를 설정 하는 데 필요한 단계를 설명 합니다.

> [!IMPORTANT]
> Apple TV에서 모든 이미지는 1 x 해상도 (`@1x`)을 수행 해야 하 고 _만_ 이 크기의 이미지를 사용 합니다. 더 큰 등 더 높은 해상도 그래픽 뿐만 아니라 시간이 걸릴를 다운로드 하 여 더 많은 메모리와 저장소를 사용 하 여 있지만 런타임에 동적으로 크기가 다시 조정 해야 하며 그리기 성능이 저하 됩니다.

<a name="Launch-Image" />

## <a name="launch-image"></a>시작 이미지

시작 이미지 Xamarin.tvOS 앱 Apple TV에서 처음 시작 될 때 표시 되는 먼저 이며 이와 같이 모든 tvOS 앱 시작 이미지를 제공 해야 합니다. 

시작 이미지 신속 하 게 나타나고 앱이 빠르고 응답성을 느낄 수 있습니다. Apple TV 바뀝니다 시작 이미지를 앱의 첫 번째 화면을 사용 하 여 곧 있습니다 후.

시작 이미지 꾸밈 식이나 광고에 대 한 영업 기회에 앱 신속 하 게 시작 하 고 준비 하는 효과 제공 하기 위한 것만 사용 하도록 합니다.

|시작 이미지 크기|참고|
|---|---|
|1920x1080px|비-레이어.png 파일에만|

Apple에서는 앱의 시작 이미지를 디자인 하기 위한 다음 제안 합니다.

- **첫 번째 화면에 거의 동일한** -Your 시작 화면 앱의 첫 번째 화면 최대한 근접해 야 합니다. 다른 그래픽 또는 요소를 포함 하 여 발생할 수 있습니다는 성가신 첫 번째 화면에 표시 되 면 "플래시".
- **텍스트를 사용 하 여 방지** -시작 이미지는 정적이 고 따라서 지역화 되지 않는 표시 하기 전에 합니다.
- **시작 downplay** -때문에 Apple TV 사용자가 앱을 자주 전환, 앱 시작 프로세스에 주목 하지 않아야 합니다.
- **브랜딩 없거나 광고** -Your 시작 이미지를 정보 화면으로 사용할 수 없습니다 또는 앱의 첫 번째 화면의 정적 일부가 아닌 한 모든 브랜딩 포함 합니다. 광고 엄격 하 게 사용할 수 없습니다.

<a name="Setting-the-Launch-Image" />

### <a name="setting-the-launch-image"></a>시작 이미지 설정

TvOS 프로젝트 시작 이미지를 설정 하려면 다음을 수행 하십시오.

1. 에 **솔루션 탐색기**를 두 번 클릭 `Assets.xcassets` 열어 편집 하려면: 

    [![](icons-images-images/asset01.png "Assets.xcassets 파일")](icons-images-images/asset01.png#lightbox)
2. 에 **자산 편집기**를 클릭 합니다 `LaunchImages` 자산: 

    [![](icons-images-images/asset02.png "LaunchImages 자산")](icons-images-images/asset02.png#lightbox)
3. 클릭 합니다 **Apple TV x 1** 항목 및 시작 이미지를 선택 하거나 필요에 따라 끌어서 새 이미지를 파일 시스템에서: 

    [![](icons-images-images/asset03.png "시작 이미지 선택")](icons-images-images/asset03.png#lightbox)
4. 변경 내용을 저장합니다.

<a name="Layered-Images" />

## <a name="layered-images"></a>계층화 된 이미지

새 Apple TV, 소파 사항을 마음속를 통해 연결 된 화면의 콘텐츠 대화방에 사용자를 유지 하는 데 도움이 되는 3D 효과 생성 하기 위해 시차 효과 사용 하 여 계층화 된 이미지 작업을 합니다.

계층화 된 이미지에는 다음이 포함 되어에서 두 개의 (2) 5 (5)로 결합 되어 전체 이미지를 구성 하는 계층을 구분 합니다. 배경 계층을 제외 하 고 각 계층 투명도 함께 해당 Z 순서 사용 하 여 깊이 효과를 만듭니다. 계층화 된 이미지를 사용 하 여 사용자 상호 작용, 더 높은 Z 순서 계층 확장 되 고 overlapped이 효과를 만듭니다.

[![](icons-images-images/layered01.png "계층화 된 이미지 Z 순서 다이어그램")](icons-images-images/layered01.png#lightbox)

> [!IMPORTANT]
> 계층화 된 이미지는 앱의 아이콘에 대 한 필수 항목이 며 다른 선택적 [포커스 항목](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection) (예: 인기 코너 이미지). 그러나 Apple에서 계층화 된 이미지를 사용 하 여 앱에서 포커스를 얻을 수 있는 모든 이미지에 대 한 제안 합니다.




Apple에서는 계층화 된 이미지를 디자인 하기 위한 다음 제안 합니다.

- **배경 계층 불투명 확인** -배경 계층 (계층 1) **해야** 불투명 또는 Apple TV에 계층화 된 이미지를 사용 하려고 할 때 오류를 얻게 됩니다. 다른 모든 계층에는 여러 수준의 투명도 3 차원 효과 향상 시키기 위해 포함할 수 있습니다.
- **전경, 중간 및 배경 요소 격리** -주요 요소 (예: 게임 자) 전경에서 사용 하 여 중간 보조 요소에 그림자입니다. 마지막으로, 중립 백그라운드에 상위 계층에 대 한 단계를 제공 하기를 포함 합니다.
- **텍스트 전경에 유지** -텍스트 수준이 높은 가립니다 수를 원하는 경우가 아니면 일반적으로 것 최상위 계층에 있습니다.
- **간단한 계층화를 사용 하 여** -The 시차 효과 미묘한 설계 되었습니다 따라서 jarring, 비현실적 효과 방지 하기 위해 최소 사용자 계층을 유지 합니다.
- **안전 영역 포함** -시차 효과 중 상위 계층을 자를 수 있습니다, 때문에 각 계층에 안전한 영역 테두리를 구축 해야 합니다. 너무 가깝게 계층 edge 내용을 받게 되 면에서 잘리는 크기 될 수 있습니다. 더 많은 크기 조정 및 자르기 하위 계층 보다 상위 계층 경험할 수 있습니다. 참조를 [크기 조정 이미지 계층](#Sizing-Image-Layers) 아래의 섹션입니다.
- **주로 미리** -계층화 된 이미지를 원하는 경우 3D 효과가 발생 하 고 개별 계층에서 콘텐츠 하나도 자가 되는 빈도를 미리 볼 수 해야 합니다. 예상 대로 렌더링 되도록 실제 Apple TV 하드웨어에서 계층화 된 이미지를 미리 봐야 합니다.

기본 제공 가능 하면 항상 사용 해야 `UIKit` 포커스에 나올 때 시차 효과 자동으로 얻게 될 것 처럼 계층화 된 이미지를 표시 하는 컨트롤입니다.

<a name="Sizing-Image-Layers" />

### <a name="sizing-image-layers"></a>크기 조정 이미지 계층

반드시 포함 해야 하는 _안전한 영역_ 테두리는 계층화 된 이미지를 구성 하는 각 계층에 합니다. 개별 계층을 확장 하 고 시차 효과 중 자를 수, 있으므로 계층의 가장자리에 너무 가까워지면 경우 계층의 콘텐츠 해제 자를 수 있습니다.

[![](icons-images-images/layered02.png "35 픽셀 테두리")](icons-images-images/layered02.png#lightbox)

<a name="Creating-Layered-Images" />

### <a name="creating-layered-images"></a>계층화 된 이미지 만들기

tvOS는 계층화 된 이미지를 사용 하 여 다음 형식으로 작동합니다.

- **자동차 파일** -Apple에서 만든를 전용 자산 카탈로그 형식입니다. 자동차 파일을 직접 만들지 않으면, 모든 LSR 파일에서 컴파일 타임에 생성 되 고 앱 번들에 포함 합니다.
- **LSR 이미지** -이 Apple에서 만든 전용 이미지 형식입니다. 사용 하 여는 [Parallax 내보내기 Adobe Photoshop 플러그 인](https://itunespartner.apple.com/assets/downloads/ParallaxExporter_Apps.zip) 또는 [Parallax 미리 보기](http://itunespartner.apple.com/assets/downloads/Parallax%20Previewer.dmg) 만들고 LSR 형식으로 계층화 된 이미지를 미리 봅니다.
- **Assets.xcassets** -에서 두 개의 (2)에 5 개의 표준 `.png` 자동차 또는 LSR 컴파일되는 자산 카탈로그에 포함 된 형식이 지정 된 이미지 형식이 컴파일 타임에 계층화 된 이미지를 지정 합니다.
- **LCR 파일** -Apple에서 만든는 독점 파일 형식입니다. LCR 파일 콘텐츠 서버 중 하나에서 다운로드 하는 추가 콘텐츠로 사용할 수는 있습니다. LCR 파일 앱 번들에 포함 하지 않을 합니다. LCR 파일이 사용 하 여 LSR 또는 Photoshop 파일에서 생성 됩니다는 `layerutil` Xcode 명령줄 도구입니다.

<a name="The-Parallax-Previewer" />

### <a name="the-parallax-previewer"></a>시차 미리 보기

생성 하는 Apple 합니다 [시차 미리 보기](http://itunespartner.apple.com/assets/downloads/Parallax%20Previewer.dmg) 미리 보기를 앱 아이콘 및 포커스를 받을 수 있는 선택 항목에 대 한 필요한 만든된 계층화 된 이미지입니다. 미리 보기에서는 완료 된 계층화 된 이미지를 구성 하는 모든 계층을 보여 줍니다.

[![](icons-images-images/layered03.png "시차 미리 보기")](icons-images-images/layered03.png#lightbox)

계층화 된 이미지를 미리 보면서, 이미지를 회전 하 고 시차 효과 미리 보기에 마우스를 사용할 수 있습니다. 사용 된 **+** (더하기) 및 **-** (빼기) 단추를 추가 하 고 계층을 제거 합니다.

새 계층화 된 이미지를 만들 때 LSR 형식으로 내보내는 하 고 앱의 번들에 포함 될 수 있습니다.

만들고 계층화 된 이미지를 미리 보기에 대 한 자세한 내용은 Apple의를 참조 하세요 [시차 아트 워크를 만드는](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/CreatingParallaxArtwork.html#//apple_ref/doc/uid/TP40015241-CH19-SW1) 섹션을 [tvOS 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)합니다.

<a name="App-Icons" />

## <a name="app-icons"></a>앱 아이콘

Xamarin.tvOS 앱 Apple TV 홈 화면에서 뿐만 아니라 앱 스토어 아이콘에 대 한 앱 아이콘 뿐 아니라 해야 합니다. 앱 아이콘은의 첫 번째 잠재적 사용자에서 매우 깊은 인상을 변경 및 한눈에 앱의 목적을 전달 해야 합니다.

[![](icons-images-images/icon01.png "앱 아이콘")](icons-images-images/icon01.png#lightbox)

모든 앱에는 작은 해당 앱 아이콘의 큰 버전을 제공 해야 합니다. 작은 아이콘은 앱을 설치할 때 Apple TV 홈 화면에서 사용 됩니다. 큰 버전을 앱 스토어에서 사용 됩니다. 큰 앱 아이콘에는 작은 아이콘 버전의 모양과 느낌와 비슷해야 합니다.

|작은 아이콘||큰 아이콘||
|---|---|---|---|
|실제 크기|400x240px|크기|1280x768px|
|안전 영역 크기|370x222px|||
|포커스가 없는 크기|300x180px|||
|포커스가 있는 크기|370x222px|||

> [!IMPORTANT]
> 앱 아이콘으로 제공 되어야 합니다 **계층화 된 이미지**합니다. 참조 하십시오 합니다 [계층화 된 이미지](#Layered-Images) 대 한 자세한 내용은 위의 섹션입니다.




Apple에 앱 아이콘을 만들기 위한 다음 제안 사항을 제공 합니다.

- **단일 포커스 지점을 제공** – 단일 포커스 지점과 아이콘 디자인 아이콘의 중심에 직접 배치 합니다.
- **간단한 배경 사용** – 상위 계층을 차별화 하 여 아이콘 배경을 단순하게 유지 합니다. 단순한 색 또는 미묘한 그라데이션을 사용 하는 것이 좋습니다.
- **텍스트의 양을 제한** – 앱의 이름을 사용자가 선택 하면 아이콘 아래에 표시 됩니다, 때문만 포함 해야 텍스트 때 아이콘을 디자인 하는 데 필수적입니다.
- **스크린샷을 사용 하지 않는** – 스크린 샷 아이콘에 대 한 너무 복잡 하 고 한눈에 앱의 용도 보려는 사용자를 허용 하지 않습니다.
- **유지 아이콘 사각형** – tvOS 약간 아이콘이 둥글게 하는 마스크를 자동으로 적용 됩니다. 사용자가 직접이 반올림을 포함 하지 마십시오.
- **사용자 계층을 주의 깊게 분리** – 대부분의 계층, 중간에 상위 계층 빛을 허용 하는 중립 백그라운드에 보조 항목 텍스트 위에 있어야 합니다.
- **그라데이션 및 신중 하 게 그림자를 사용 하 여** – 신중 하 게 사용 해야 하므로 그라데이션 및 그림자 시차 효과 사용 하 여 충돌 수입니다. 간단한 위쪽-아래쪽을 밝게-어둡게 그라데이션 스타일 가장 효과적입니다. 그림자는 일반적으로 날카로운, 선명한 색조로 가장 잘 작동 합니다.
- **계층 투명도 다** – 다양 한 3D 효과 높이기 위해 앱 아이콘의 상위 수준에서 투명도 수준을 사용 합니다. 배경 계층 불투명 이거나 오류가 발생 합니다.

<a name="Setting-the-App-Icons" />

### <a name="setting-the-app-icons"></a>앱 아이콘 설정

TvOS 프로젝트에 필요한 앱 아이콘을 설정 하려면 다음을 수행 하십시오.

1. 에 **솔루션 탐색기**를 두 번 클릭 `Assets.xcassets` 열어 편집 하려면: 

    [![](icons-images-images/asset01.png "Assets.xcassets fileg")](icons-images-images/asset01.png#lightbox)
2. 에 **자산 편집기**를 확장 하 고는 `App Icon & Top Shelf Image` 자산: 

    [![](icons-images-images/asset04.png "인기 코너 이미지 자산 확장")](icons-images-images/asset04.png#lightbox)
3. 다음으로, 확장 된 `App Icon - Small` 자산: 

    [![](icons-images-images/asset05.png "앱 아이콘-작은 자산 확장")](icons-images-images/asset05.png#lightbox)
4. 다음를 확장 합니다 `Back` 자산 및 클릭을 `Contents` 항목: 

    [![](icons-images-images/asset06.png "백 자산을 확장 한 다음")](icons-images-images/asset06.png#lightbox)
5. 클릭 합니다 **Apple TV 항목 x 1** 이미지 파일을 선택 합니다.
6. 에 대해 위의 단계를 반복 합니다 `Front` 및 `Middle` 자산입니다.
7. 다음 정의 하려면 동일한 단계를 반복 합니다 `App Icon - Large` 자산입니다.
8. 변경 내용을 저장합니다.

<a name="Top-Shelf-Image" />

## <a name="top-shelf-image"></a>인기 코너 이미지

Apple TV 홈 화면의 위쪽 행에 Xamarin.tvOS 앱을 배치 하는 경우 사용자가 앱을 선택한 경우 큰 인기 코너 이미지 표시 됩니다. 이 이미지는 앱의 기능을 강조 하거나 해당 내용에 대 한 직접 링크를 제공 해야 합니다.

[![](icons-images-images/topshelf01.png "맨 위 코너 이미지 예제")](icons-images-images/topshelf01.png#lightbox)

단일 정적으로 인기 코너 이미지 제공 `.png` 하거나 `.lsr` 파일 (참조 [계층화 된 이미지 만들기](#Creating-Layered-Images))를 동적으로 만들기 런타임에 포커스 가능 항목의 단일 행으로 또는 (참조 [ 위쪽 선반 동적 콘텐츠](#Dynamic-Top-Shelf-Content) 아래).

|위쪽 선반 이미지 크기|참고|
|---|---|
|1920x720px|정적.png 또는 계층화 된.lsr 파일|

Apple에서는 위쪽 선반 이미지를 만들기 위한 다음 제안 사항을 제공 합니다.

- **다양 한 정적 이미지를 사용 하 여** – 해당 인기 코너 이미지 포커스 될 앱 동적 콘텐츠를 제공 하지 않습니다. 이 이미지를 사용 하 여 앱 또는 브랜드의 기능을 강조 합니다.
- **앱 콘텐츠 링크** – 동적 위쪽 선반 레이아웃 빠른 링크를 사용자 앱에서 가장 중요 한 찾는 콘텐츠를 제공 합니다. 앱을 시작 하 고 지정 된 콘텐츠를 즉시 이동에 대 한 빠른 링크를 제공 하려면이 영역을 사용 합니다.
- **최신 콘텐츠를 표시** – 다양 한 인기 코너 콘텐츠를 앱에 사용자를 그리는 자세한 하는 데 사용할 수 있도록 합니다. 최고 등급 또는 최신 콘텐츠를 소개 하는 영역으로 사용 합니다.
- **콘텐츠를 개인 설정 된** –가 가장 자주 사용 하는 사용자 환경이 나 홈 화면의 위쪽 행의 즐겨 찾는 앱. 가장 관심이 되는 콘텐츠를 표시할 인기 코너를 사용 합니다.
- **광고를 사용할 수 없습니다** – 광고 인기 코너에 표시 되지 않도록 금지 엄격 하 게 됩니다. 최신 구입 가능한 콘텐츠를 표시 될 수 있습니다 하지만 없는 가격 책정 정보를 표시 합니다.

### <a name="setting-the-top-shelf-image"></a>인기 코너 이미지 설정

TvOS 프로젝트에 필요한 인기 코너 이미지를 설정 하려면 다음을 수행 하십시오.

1. 에 **솔루션 탐색기**를 두 번 클릭 `Assets.xcassets` 열어 편집 하려면: 

    [![](icons-images-images/asset01.png "Assets.xcassets 파일")](icons-images-images/asset01.png#lightbox)
2. 에 **자산 편집기**를 확장 하 고는 `App Icon & Top Shelf Image` 자산: 

    [![](icons-images-images/asset04.png "인기 코너 이미지 자산 확장")](icons-images-images/asset04.png#lightbox)
3. 클릭 된 `Top Shelf Image` 자산: 

    [![](icons-images-images/asset07.png "인기 코너 이미지 자산")](icons-images-images/asset07.png#lightbox)
4. 클릭 합니다 **Apple TV 항목 x 1** 이미지 파일을 선택 합니다.
5. 변경 내용을 저장합니다.

<a name="Dynamic-Top-Shelf-Content" />

### <a name="dynamic-top-shelf-content"></a>위쪽 선반 동적 콘텐츠

위쪽 선반 정적 인기 코너 이미지를 표시 하는 대신 동적 행을 포함할 수 있습니다 [포커스 항목](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection) 또는 배너를 스크롤의 동적 집합입니다. 둘 다 이러한 동적 스타일의 앱 또는 자주 사용 하는 기능에 대 한 점프 하 여 제공 된 콘텐츠를 강조 표시할 수 있습니다.

<a name="Sectioned-Content-Row" />

#### <a name="sectioned-content-row"></a>구역이 구분 되어 있으며 콘텐츠 행

이 동적 인기 코너 콘텐츠 형식 스크롤, 필요에 따라 섹션으로 세분화 포커스 가능 항목의 단일 행을 표시 합니다. 즐겨 찾는 새 강조 하려면 일반적으로 사용 됩니다 또는 최근에 앱 콘텐츠를 표시 합니다.

콘텐츠는 콘텐츠의 현재 선택한 콘텐츠 부분 아래에 나타나는 레이블 사용 하 여 (현재 포커스를가지고) 단일, 가로 스크롤 목록으로 표시 됩니다. 사용자가 지정 된 부분 콘텐츠를 선택 하는 경우 앱 시작 및 해당 콘텐츠를 직접 수행 해야 합니다.

다음 콘텐츠 크기를 지정 해야 합니다.

||포스터 (2:3)|Square (1:1)|HDTV (16:9)|
|---|---|---|---|
|실제 크기|404x608px|608x608px|908x512px|
|안전 영역 크기|380x570px|570x570px|852x479px|
|포커스가 없는 크기|333x500px|500x500px|782x440px|
|포커스가 있는 크기|380x570px|570x570px|852x479px|

Apple에서는 콘텐츠 단면화 행에 대해 다음 제안 사항을 제공합니다.

- **행을 완료** – 화면의 너비 전체에 걸쳐 충분 한 콘텐츠를 제공 해야 합니다.
- **혼합 이미지 크기를 조정** – The 콘텐츠 단면화 행 설계 된 다양 한 이미지 크기 (에서 위에 제공 된 목록)을 보관 합니다. 하지만 이미지 크기를 혼합 수행 하는 경우는 추가 확장에 적용할 정규화 콘텐츠 표시를 알아야 합니다.

<a name="Scrolling-Inset-Banners" />

#### <a name="scrolling-inset-banners"></a>스크롤 Inset 배너

필요에 따라 Xamarin.tvOS 앱을 자동으로 스크롤 및 거의 화면을 채우는 배너의 컬렉션을 반복으로 위쪽 선반에 해당 콘텐츠를 제공할 수 있습니다. 이 스타일은 일반적으로 새 tv와 같은 다양 한, 새 콘텐츠를 보여 주기 위해 사용 됩니다.

자동 스크롤 하는 것 외에도 사용자는 배너를 제어 하 고 Siri 원격을 사용 하 여 어느 방향으로 스크롤할 수 있습니다. 배너에 포커스가 있을 때 Siri 원격 순환 제스처는 작은 하는 경우의 시차 효과 배너를 정품 인증 됩니다.

**배너 이미지 (추가 Wide)**

|   |   |
|---|---|
|실제 크기|1940x624px|
|안전 영역 크기|1740x620px|
|포커스가 없는 크기|1740x560px|
|포커스가 있는 크기|1740x620px|

Inset 배너 스크롤 하거나 지정할 수 있습니다 정적 `.png` 계층화 또는 `.lsr` 파일입니다.

Apple에서는 스크롤 Inset 배너에 대 한 다음 제안 사항을 제공합니다.

- **콘텐츠 크기** -자연 스러운 느낌으로 스크롤 하는 것에 대 한 최소 3 개의 배너를 제공 해야 합니다. 개 이하의 8 (8) 배너를 포함 해야 하거나 해당 탐색 하드 최종 사용자에 대 한 합니다.
- **텍스트 콘텐츠** -배너의 텍스트는 배너 이미지에 포함할지를 요구 하는 경우. 계층화 된 이미지를 사용 하는 경우 텍스트는 최상위 계층에 있어야 합니다.

Apple의를 참조 하세요 [TVServices 프레임 워크 참조](https://developer.apple.com/library/prerelease/tvos/documentation/TVServices/Reference/TVServices_Ref/index.html#//apple_ref/doc/uid/TP40016412) 동적 인기 코너 콘텐츠를 제공 하기 위해 앱으로 위쪽 선반 확장명을 추가 하는 방법은 합니다.

<a name="Game-Center-Images" />

## <a name="game-center-images"></a>게임 센터 이미지

Game Center 지원을 포함 한 Xamarin.tvOS 앱은 게임을 하는 경우에 몇 가지 자세한 이미지 자산 필요한 됩니다.

- **도전 과제 아이콘** -불투명 이미지 원에 자동으로 잘립니다는 각 도전 과제에 필요 합니다. 성과 포커스를 받을 수 없는 항목입니다.
- **대시보드 아트 워크** -Game Center 앱의 대시보드 위쪽에 표시할 선택적 이미지로 사용할 수 있습니다. 이러한 이미지는 비-포커스를 받을 수 있습니다.
- **순위표 아트 워크** -제공 해야 간에 1 개를 3 개 16:9 가로 세로 비율 이미지 앱을 지 원하는 각 순위표에 대 한 합니다. 이러한 정적 될 `.png` 계층화 또는 `.lsr` 파일입니다. 순위표 아트 워크는 포커스를 받을 수 있습니다.

||도전 과제 아이콘|대시보드 아트 워크|순위표 아트 워크|
|---|---|---|---|
|표시 크기|200x200px|923x150px|n/a|
|실제 크기|320x320px|n/a|659x371px|
|안전 영역 크기|n/a|n/a|618x348px|
|포커스가 없는 크기|n/a|n/a|548x309px|
|포커스가 있는 크기|n/a|n/a|618x348px|

Game Center 사용에 대 한 자세한 내용은 Apple의를 참조 하세요 [Game Center 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/NetworkingInternet/Conceptual/GameKit_Guide/Introduction/Introduction.html)합니다.

<a name="Working-with-Images" />

## <a name="working-with-images"></a>이미지 작업

TvOS 9 iOS 9 포함 하 고 Xamarin.iOS 응용 프로그램에서 이미지를 표시 하는 데 동일한 기술의 하위 집합 이므로 Xamarin.tvOS 앱 에서도 작동 합니다. 참조 하세요 우리의 [이미지로 표시](~/ios/app-fundamentals/images-icons/displaying-an-image.md) 자세한 정보에 대 한 설명서입니다.

<a name="Setting-Xamarin.tvOS-Project-Images" />

## <a name="setting-xamarintvos-project-images"></a>Xamarin.tvOS 프로젝트 이미지 설정

위에서 설명한 대로 모든 tvOS 앱 필요는 [시작 이미지](#Launch-Image), 및 [앱 아이콘](#App-Icons)합니다. 이 섹션에서는 자산 카탈로그에서 설정한 후 Xamarin.tvOS 앱 프로젝트에 대 한 시작 이미지 및 앱 아이콘을 선택에 대해 설명 합니다.

다음을 수행합니다.

1. 에 **솔루션 탐색기**, 두 번 클릭는 `Info.plist` 열어 편집 하려면: 

    [![](icons-images-images/info01.png "Info.plist 파일")](icons-images-images/info01.png#lightbox)
2. 에 **Info.Plist 편집기**, 자산 카탈로그를 선택 (에서 위의 구성는 [앱 아이콘 설정](#Setting-the-App-Icons) 섹션)에 대 한를 **앱 아이콘**: 

    [![](icons-images-images/info02.png "Info.Plist 편집기")](icons-images-images/info02.png#lightbox)
3. 자산 카탈로그를 선택 하는 다음으로, (에서 위의 구성 합니다 [시작 이미지 설정](#Setting-the-Launch-Image) 섹션)에 대 한는 **시작 이미지**.
4. 변경 내용을 저장합니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 이미지 형식 및 Xamarin.tvOS 앱에서 사용 된 크기의 모든 설명 했습니다. 첫째, 시작 이미지, 계층화 된 이미지를 앱 아이콘, 위쪽 선반 이미지 및 게임 센터 이미지 설명합니다. 그런 다음 Xamarin.tvOS 앱에서 이미지 작업에 대해서 설명합니다.

## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
