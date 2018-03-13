---
title: "아이콘 및 이미지 작업"
description: "이 문서에서는 디자인 및 아이콘과 Xamarin.tvOS 앱 내에서 이미지 작업을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: A2DA4347-0563-4C72-A8D7-5B9DE9E28712
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: d1052695bb7337a18d1a2f1f7015e9079f86f6f5
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-icons-and-images"></a>아이콘 및 이미지 작업

_이 문서에서는 디자인 및 아이콘과 Xamarin.tvOS 앱 내에서 이미지 작업을 설명 합니다._

영상을 만들기 아이콘 및 이미지는 Apple TV 앱에 대 한 풍부한 사용자 환경을 개발의 중요 한 부분입니다. 이 가이드에서는 만들고 Xamarin.tvOS 앱에 대 한 필요한 그래픽 자산을 포함 하는 데 필요한 단계를 설명 합니다.

- [이미지 시작](#Launch-Image) -시작 이미지 표시 앱을 처음 시작 하 고 시작이 완료 되 면 응용 프로그램의 첫 번째 화면으로 대체 됩니다.
- [이미지를 계층화](#Layered-Images) -선택한 항목에 대 한 3D 효과를 만들려면 시차 효과 함께 새 계층화 이미지 작업 Apple의 Apple TV에 특정 합니다. 여러 가지 방법으로 수 [계층화 된 이미지 만들기](#Creating-Layered-Images)합니다.
- [응용 프로그램 아이콘](#App-Icons) -뿐만 아니라 Apple TV 홈 화면에 대 한 하지만 앱 스토어에 대 한 아이콘은 필수입니다. 계층화 된 이미지 형식으로 제공 되어야 합니다.
- [위쪽 선반 이미지](#Top-Shelf-Image) -앱의 홈 화면 위쪽 행에 배치 되 면 앱의 기능을 강조 표시를 위쪽 선반 이미지 필요 합니다. 제공할 수 있습니다 [동적 위쪽 선반 콘텐츠](#Dynamic-Top-Shelf-Content) 를 응용 프로그램의 콘텐츠를 강조 표시 합니다.
- [게임 센터 이미지](#Game-Center-Images) -앱은 게임을 사용 하 여 게임 센터 면 몇 가지 추가 이미지 필요 없게 됩니다.
- [Xamarin.tvOS 프로젝트 이미지 설정](#Setting-Xamarin.tvOS-Project-Images) -Xamarin.tvOS 앱에 대 한 시작 이미지 및 응용 프로그램 아이콘을 설정 하는 데 필요한 단계에 설명 합니다.

> [!IMPORTANT]
> **참고:** 1 x 해상도에서 Apple TV에 모든 이미지는 (`@1x`)을 수행 해야 하 고 _만_ 이 크기의 이미지를 사용 합니다. 포함 하 여 더 큰 해상도 높을수록 그래픽 뿐만 아니라를 다운로드 하 여 더 많은 메모리와 저장소를 사용 하 여 시간 사용 하지만 및 런타임에 동적으로 크기가 다시 조정 하는 그리기 성능이 저하 됩니다.

<a name="Launch-Image" />

## <a name="launch-image"></a>이미지를 시작 합니다.

시작 이미지 Xamarin.tvOS 앱 Apple TV에서 처음 시작 될 때 표시 되는 먼저 이며 이와 같이 모든 tvOS 응용 프로그램 시작 이미지를 제공 해야 합니다. 

시작 이미지 신속 하 게 나타나고 앱이 신속 하 고 응답을 느낄 수 있습니다. Apple TV 바꿉니다 시작 이미지 응용 프로그램의 첫 번째 화면 곧 발생 후.

시작 이미지가 하지 않은 광고 또는 꾸밈 식에 대 한 기회, 앱 신속 하 게 시작 하 고 준비 되어 효과 제공 하기 위한 것만 사용 하도록 합니다.

<table width="100%" border="1px">
<tr>
    <td colspan="2"><b>이미지를 시작 합니다.</b></td>
</tr>
<tr>
    <td><b>Size</b></td>
    <td>1920px x 1080px

    Non-layered `.png` files only</td>
</tr>
</table>

Apple 앱의 시작 이미지를 디자인 하기 위한 다음 방법을 사용 하면:

- **거의 동일한 첫 번째 화면 지** -Your 시작 화면 앱의 첫 번째 화면 최대한 근접해 야 합니다. 다른 그래픽 또는 요소를 포함 하 여 발생할 수 있습니다는 첫 번째 화면이 나타나면 "플래시".
- **텍스트를 사용 하 여 방지할** -시작 이미지는 정적이 고 따라서 지역화 되지 않는 표시 되기 전에 먼저 합니다.
- **시작 downplay** -Apple TV 때문에 사용자가 앱을 자주 전환, 응용 프로그램 시작 프로세스에 주목 하지 않아야 합니다.
- **브랜딩 없음이나 광고** -Your 시작 이미지 정보 화면으로 사용할 수 없습니다 또는 응용 프로그램의 첫 번째 화면의 정적 부분 경우가 아니라면 모든 브랜딩 포함 합니다. 광고 엄격 하 게 사용할 수 없습니다.

<a name="Setting-the-Launch-Image" />

### <a name="setting-the-launch-image"></a>시작 이미지 설정

TvOS 프로젝트에 대 한 시작 이미지를 설정 하려면 다음을 수행 하십시오.

1. 에 **솔루션 탐색기**, 두 번 클릭 `Assets.xcassets` 열어 편집 하려면: 

    [![](icons-images-images/asset01.png "Assets.xcassets 파일")](icons-images-images/asset01.png#lightbox)
2. 에 **자산 편집기**, 클릭는 `LaunchImages` 자산: 

    [![](icons-images-images/asset02.png "LaunchImages 자산")](icons-images-images/asset02.png#lightbox)
3. 클릭는 **1 Apple TV x** 항목 시작 이미지를 선택 하거나 필요에 따라 파일 시스템에서 새 이미지 끌어: 

    [![](icons-images-images/asset03.png "시작 이미지 선택")](icons-images-images/asset03.png#lightbox)
4. 변경 내용을 저장합니다.

<a name="Layered-Images" />

## <a name="layered-images"></a>계층화 된 이미지

Apple TV, 게임를 통해 연결 된 화면의 콘텐츠 대화방 소파에 사용자를 유지 하는 데 도움이 되는 3 차원 효과 생성 하기 위해 시차 효과 함께 계층 이미지 작업에 새입니다.

계층화 된 이미지에는 다음이 포함 되어에서 두 개의 (2)를 5 개 결합 되어 이미지 완료 구성 하는 계층을 구분 합니다. 배경 계층을 제외한 각 계층 투명도 함께 해당 Z 순서를 사용 하 여 깊이 감을 만듭니다. 계층화 된 이미지와 상호 작용할 더 높은 Z 순서 계층을 크기가 조정 하 고 이러한 효과를 만들려면 겹쳐진 됩니다.

[![](icons-images-images/layered01.png "계층화 된 이미지 Z 순서 다이어그램")](icons-images-images/layered01.png#lightbox)

> [!IMPORTANT]
> **참고:** 겹친 이미지 응용 프로그램의 아이콘에 대 한 필요 하 고 다른 선택적 [포커스 항목](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection) (예: 위쪽 선반 이미지). 그러나 Apple 계층화 된 이미지를 사용 하 여 앱에 포커스를 얻을 수 있는 모든 이미지에 대 한 제안 합니다.




Apple에서는 계층화 된 이미지를 디자인 하기 위한 다음 방법을 수행 합니다.

- **배경 계층 불투명 확인** -배경 계층 (계층 1) **해야** 불투명 또는 Apple TV에 계층화 된 이미지를 사용 하려고 할 때 오류가 발생할 수 있습니다. 다른 모든 계층에는 여러 수준의 3 차원 효과 향상 시키기 위해 투명도 포함 될 수 있습니다.
- **전경, 중간 및 배경 요소 격리** -포그라운드에서 두드러진 요소 (예: 게임 자)를 배치할 보조 요소에 그림자 등에 대 한 중간을 사용 합니다. 마지막으로 프로그램 상위 계층에 대 한 단계를 제공 하는 중립 배경을 포함 됩니다.
- **텍스트 전경에 유지** -텍스트 상위 수준에 의해 가려질 수를 사용 하려는 경우가 아니면 일반적으로 것은 맨 위 계층에 있습니다.
- **간단한 계층화를 사용 하 여** -The 시차 효과 미묘한 설계 되었으므로 하므로 jarring, 비현실적 효과 방지 하기 위해 최소한의 대 한 레이어를 유지 합니다.
- **보호 영역을 포함** -시차 효과 중 상위 계층을 자를 수 있습니다, 때문에 각 계층에 보호 영역 테두리를 만들어야 합니다. 콘텐츠를 너무 가까이 레이어 가장자리 발생 하는 경우에서 잘리는 크기 될 수 있습니다. 상위 계층에서 더 많은 크기 조정 및 하위 계층 보다 자르기 발생 합니다. 참조는 [크기 조정 이미지 계층](#Sizing-Image-Layers) 아래 섹션.
- **보통 미리** -계층화 된 이미지에 원하는 3D 효과 발생 하는지 확인 하는 각 계층에 대해 콘텐츠 없음은 잘리지 빈도를 미리 볼 합니다. 예상 대로 렌더링 되도록 Apple TV의 실제 하드웨어에서 계층화 된 이미지를 미리 봐야 합니다.

가능 하면 항상 기본 제공을 사용 해야 `UIKit` 에 포커스 나올 때 시차 효과 자동으로 얻게 될 것으로 계층화 된 이미지를 표시 하는 컨트롤입니다.

<a name="Sizing-Image-Layers" />

### <a name="sizing-image-layers"></a>크기 조정 이미지 계층

반드시 포함 해야 하는 _보호 영역_ 테두리는 계층화 된 이미지를 구성 하는 각 계층에 있습니다. 개별 계층 크기가 조정 하 고 시차 효과 중 잘릴 수 있습니다, 때문에 계층의 내용은 수 잘려 레이어의 가장자리에 너무 가까워지면 경우:

[![](icons-images-images/layered02.png "35 픽셀 테두리")](icons-images-images/layered02.png#lightbox)

<a name="Creating-Layered-Images" />

### <a name="creating-layered-images"></a>계층화 된 이미지 만들기

다음 형식으로 tvOS 계층화 된 이미지와 함께 작동합니다.

- **자동차 파일** -Apple에서 만든 소유 자산 카탈로그 형식입니다. 자동차 파일을 직접 만들지 않으면, 모든 LSR 파일에서 컴파일 타임에 생성 및 앱 번들에 포함 합니다.
- **LSR 이미지** -Apple에서 만든 이미지 전용 형식입니다. 사용 하 여는 [시차 내보내기에서 Adobe Photoshop 플러그 인](https://itunespartner.apple.com/assets/downloads/ParallaxExporter_Apps.zip) 또는 [시차 미리 보기](http://itunespartner.apple.com/assets/downloads/Parallax%20Previewer.dmg) 를 만들고 LSR 형식으로 미리 보기 이미지 계층에 있습니다.
- **Assets.xcassets** -에서 두 개의 (2)를 5 개 표준 `.png` 자동차 또는 LSR로 컴파일되는 자산 카탈로그에 포함 된 형식이 지정 된 이미지 형식이 컴파일 타임에 계층화 된 이미지를 지정 합니다.
- **LCR 파일** -Apple에서 만든 고유 파일 형식입니다. LCR 파일 콘텐츠 서버 중 하나에서 다운로드 한 추가 내용으로 사용 하는 데 사용 됩니다. LCR 파일 앱 번들에 포함 하지 않을 합니다. 사용 하 여 LSR 또는 Photoshop 파일 LCR 파일이 생성 되도록는 `layerutil` Xcode에 포함 된 명령줄 도구입니다.

<a name="The-Parallax-Previewer" />

### <a name="the-parallax-previewer"></a>시차 미리 보기

생성 되는 Apple에서 [시차 미리 보기](http://itunespartner.apple.com/assets/downloads/Parallax%20Previewer.dmg) 미리 보기 및 응용 프로그램 아이콘 및 포커스를 받을 수 있는 선택 항목에 필요한 만든된 계층화 된 이미지를 합니다. 미리 보기는 복잡된 한 계층화 된 이미지를 구성 하는 모든 계층을 보여 줍니다.

[![](icons-images-images/layered03.png "시차 미리 보기")](icons-images-images/layered03.png#lightbox)

계층화 된 이미지를 미리 볼 때 이미지 회전를 시차 효과 미리 보려면 마우스를 사용할 수 있습니다. 사용 하 여는  **+**  (더하기) 및  **-**  (빼기) 단추를 추가 하 고 계층을 제거 합니다.

새 계층에 이미지를 만들 때 LSR 형식 하 고 앱의 번들에 포함 된 수 수 있습니다.

만들고 계층화 이미지 미리 보기에 대 한 자세한 내용은 Apple의를 참조 하십시오 [시차 아트 워크를 만드는](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/CreatingParallaxArtwork.html#//apple_ref/doc/uid/TP40015241-CH19-SW1) 의 섹션은 [tvOS에 대 한 응용 프로그램 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)합니다.

<a name="App-Icons" />

## <a name="app-icons"></a>응용 프로그램 아이콘

Xamarin.tvOS 앱 Apple TV 홈 화면에서 뿐만 아니라 앱 스토어에 대 한 아이콘 뿐만 아니라 응용 프로그램 아이콘 필요 합니다. 앱 아이콘은는 첫 번째 잠재적 사용자에 매우 깊은 인상을을 변경한 응용 프로그램의 용도 한 눈에 전달 해야 합니다.

[![](icons-images-images/icon01.png "앱 아이콘")](icons-images-images/icon01.png#lightbox)

모든 응용 프로그램은 작 및 해당 응용 프로그램 아이콘의 큰 버전을 모두 제공 해야 합니다. 작은 아이콘에는 앱을 설치 하는 경우 Apple TV 홈 화면에서 사용 됩니다. 큰 버전 앱 스토어에서 사용 됩니다. 큰 응용 프로그램 아이콘에는 작은 아이콘 버전의 모양과 느낌 유사 해야 합니다.

<table width="100%" border="1px">
<tr>
    <td colspan="2"><b>작은 아이콘</b></td>
    <td colspan="2"><b>큰 아이콘</b></td>
</tr>
<tr>
    <td><b>실제 크기</b></td>
    <td>400px x 240px</td>
    <td><b>Size</b></td>
    <td>1280px x 768px</td>
</tr>
<tr>
    <td><b>보호 영역 크기</b></td>
    <td>370px x 222px</td>
    <td></td>
    <td></td>
</tr>
<tr>
    <td><b>포커스가 없는 크기</b></td>
    <td>300px x 180px</td>
    <td></td>
    <td></td>
</tr>
<tr>
    <td><b>포커스가 지정 된 크기</b></td>
    <td>370px x 222px</td>
    <td></td>
    <td></td>
</tr>
</table>

> [!IMPORTANT]
> **참고:** 응용 프로그램 아이콘으로 제공 되어야 합니다 **계층화 이미지**합니다. 참조 하십시오는 [계층화 이미지](#Layered-Images) 자세한 내용은 섹션.




Apple 앱 아이콘을 만들기 위한 다음 제안 사항을 제공 합니다.

- **단일 포커스 지점 제공** – 단일 포커스 지점과 프로그램 아이콘 디자인 아이콘의 가운데에 직접 배치 합니다.
- **간단한 배경을 사용** – 상위 계층 구별 되도록 아이콘 배경을 간단 하 게 유지 합니다. 간단한 색 또는 미묘한 그라데이션을 사용 하는 것이 좋습니다.
- **텍스트의 양을 제한** – 사용자가을 선택 하면 아이콘 아래 응용 프로그램의 이름이 표시 됩니다, 이후 포함 해야 텍스트 아이콘의 디자인 해야 하는 경우.
- **스크린샷을 사용 하지 않는** – 스크린샷은 너무 복잡해 서 아이콘 및 사용자를 한 눈에 응용 프로그램의 용도 볼 수를 허용 하지 않습니다.
- **유지 아이콘 정사각형** – tvOS 약간 아이콘이 둥글게 하는 마스크를 자동으로 적용 됩니다. 사용자가 직접이 반올림을 포함 하지 마세요.
- **Your 레이어 신중 하 게 구분** – 대부분 계층, 중간 및 상위 사용자 계층을 허용 하는 중립 배경 보조 항목 텍스트의 상위에 있어야 합니다.
- **그라데이션 및 신중 하 게 그림자를 사용 하 여** – 신중 하 게 사용 해야 하므로 그라데이션과 그림자 시차 효과 함께 충돌 수입니다. 간단한 위쪽-아래쪽으로, 밝게-어둡게 그라데이션 스타일 가장 효과적입니다. 그림자 날카로운, 선명한 색조 기본적으로 정상적으로 작동 합니다.
- **계층 투명도 다** – 다양 한 3D 효과 높이기 위해 응용 프로그램 아이콘의 상위 수준에 대 한 투명도 수준을 사용 합니다. 배경 계층 불투명 이거나 오류가 발생 합니다.

<a name="Setting-the-App-Icons" />

### <a name="setting-the-app-icons"></a>앱 아이콘을 설정합니다.

TvOS 프로젝트에 필요한 앱 아이콘을 설정 하려면 다음을 수행 하십시오.

1. 에 **솔루션 탐색기**, 두 번 클릭 `Assets.xcassets` 열어 편집 하려면: 

    [![](icons-images-images/asset01.png "Assets.xcassets fileg")](icons-images-images/asset01.png#lightbox)
2. 에 **자산 편집기**를 확장 하 고는 `App Icon & Top Shelf Image` 자산: 

    [![](icons-images-images/asset04.png "위쪽 선반 이미지 자산 확장")](icons-images-images/asset04.png#lightbox)
3. 다음으로 확장 하 고는 `App Icon - Small` 자산: 

    [![](icons-images-images/asset05.png "응용 프로그램 아이콘-작은 자산 확장")](icons-images-images/asset05.png#lightbox)
4. 다음 확장은 `Back` 자산 고를 클릭은 `Contents` 항목: 

    [![](icons-images-images/asset06.png "다음 백 자산 확장")](icons-images-images/asset06.png#lightbox)
5. 클릭는 **1 Apple TV 항목 x** 이미지 파일을 선택 하 고 있습니다.
6. 에 대해 위의 단계를 반복 하는 `Front` 및 `Middle` 자산입니다.
7. 그런 다음 정의 하려면 같은 단계를 반복는 `App Icon - Large` 자산입니다.
4. 변경 내용을 저장합니다.

<a name="Top-Shelf-Image" />

## <a name="top-shelf-image"></a>위쪽 선반 이미지

사용자가을 배치 하 Xamarin.tvOS 앱 Apple TV 홈 화면에서 첫 행에는 사용자가 앱을 선택 하는 큰 위쪽 선반 이미지 표시 됩니다. 이 이미지는 응용 프로그램의 기능을 강조 표시 하거나 해당 내용에 대 한 직접 링크를 제공 해야 합니다.

[![](icons-images-images/topshelf01.png "위쪽 선반 이미지 예제")](icons-images-images/topshelf01.png#lightbox)

위쪽 선반 이미지 수를 단일 정적으로 제공 되거나 `.png` 또는 `.lsr` 파일 (참조 [계층화 된 이미지 만들기](#Creating-Layered-Images))를 동적으로 만들기 런타임에 포커스 항목의 단일 행으로 또는 (참조 [ 위쪽 선반 동적 콘텐츠](#Dynamic-Top-Shelf-Content) 아래).

<table width="100%" border="1px">
<tr>
    <td colspan="2"><b>위쪽 선반 이미지</b></td>
</tr>
<tr>
    <td><b>Size</b></td>
    <td>1920px x 720px

    Static `.png` or layered `.lsr` file</td>
</tr>
</table>

Apple 위쪽 선반 이미지를 만들기 위한 다음 제안 사항을 제공 합니다.

- **서식 있는 정적 이미지를 사용 하 여** – 응용 프로그램 동적 콘텐츠를 제공 하지 않는 위쪽 선반 이미지 포커스 됩니다. 이 이미지를 사용 하 여 응용 프로그램 또는 브랜드의 기능을 강조 표시 합니다.
- **응용 프로그램 콘텐츠를 링크** – 동적 위쪽 선반 레이아웃을 사용자 응용 프로그램에서 가장 중요 한 발견 하는 콘텐츠에 대 한 빠른 링크를 제공 합니다. 앱을 시작 하 즉시 이동 하려면 지정 된 내용에 대 한 빠른 링크를 제공 하려면이 영역을 사용 합니다.
- **최신 콘텐츠 전시** – 서식 있는 위쪽 선반 콘텐츠 앱에 사용자를 그릴 수 고를 더 사용할 수 있도록 설정 합니다. 영역으로이 가장 높은 이상인 최신 콘텐츠를 보여 주기 위해 사용 합니다.
- **콘텐츠를 개인 설정 된** –가 가장 자주 사용 되는 사용자 환경이 나 즐겨 찾는 앱의 홈 화면 위쪽 행에 있습니다. 위쪽 선반을 사용 하 여 가장 관심 것 콘텐츠를 표시 합니다.
- **광고를 사용할 수 없습니다** – 광고 위쪽 선반에 표시 되지 않도록 엄격히 금지 됩니다. 최신 있는 구입 가능한 콘텐츠를 표시할 수 있습니다 하지만 없는 가격 책정 정보를 표시 합니다.

### <a name="setting-the-top-shelf-image"></a>위쪽 선반 이미지 설정

TvOS 프로젝트에 대 한 필요한 위쪽 선반 이미지를 설정 하려면 다음을 수행 하십시오.

1. 에 **솔루션 탐색기**, 두 번 클릭 `Assets.xcassets` 열어 편집 하려면: 

    [![](icons-images-images/asset01.png "Assets.xcassets 파일")](icons-images-images/asset01.png#lightbox)
2. 에 **자산 편집기**를 확장 하 고는 `App Icon & Top Shelf Image` 자산: 

    [![](icons-images-images/asset04.png "위쪽 선반 이미지 자산 확장")](icons-images-images/asset04.png#lightbox)
3. 클릭는 `Top Shelf Image` 자산: 

    [![](icons-images-images/asset07.png "위쪽 선반 이미지 자산")](icons-images-images/asset07.png#lightbox)
5. 클릭는 **1 Apple TV 항목 x** 이미지 파일을 선택 하 고 있습니다.
6. 변경 내용을 저장합니다.

<a name="Dynamic-Top-Shelf-Content" />

### <a name="dynamic-top-shelf-content"></a>위쪽 선반 동적 콘텐츠

위쪽 선반 수의 동적 행을 포함 하는 데 정적 위쪽 선반 이미지를 표시 하는 대신 [포커스 항목](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection) 또는 배너 스크롤의 동적 집합입니다. 이러한 동적 스타일의 둘 다를 통해 응용 프로그램 또는 가장 사용 되는 해당 기능에 대 한 점프 하 여 제공 된 콘텐츠를 강조 표시할 수 있습니다.

<a name="Sectioned-Content-Row" />

#### <a name="sectioned-content-row"></a>구역이 구분 되어 있으며 콘텐츠 행

이 동적 위쪽 선반 콘텐츠 형식 스크롤, 필요에 따라 섹션으로 구분 하는 포커스 항목의 단일 행을 표시 합니다. 최근에 본 응용 프로그램 콘텐츠 또는 강조 즐겨 찾는 새로 만들기, 일반적으로 사용 될 합니다.

콘텐츠는 콘텐츠는 현재 데이터의 선택 된 콘텐츠 아래에 표시 되는 레이블 사용 (현재 포커스가) 단일, 가로 스크롤 목록으로 표시 됩니다. 사용자가 지정된 된 부분에 콘텐츠를 선택 하는 경우 앱을 시작할 및 해당 콘텐츠를 직접 수행 해야 합니다.

다음 콘텐츠 크기 해야 합니다.

<table width="100%" border="1px">
<tr>
    <td><b>&nbsp;</b></td>
    <td><b>포스터 (2:3)</b></td>
    <td><b>Square (1:1)</b></td>
    <td><b>HDTV (16:9)</b></td>
</tr>
<tr>
    <td><b>실제 크기</b></td>
    <td>404px x 608px</td>
    <td>608px x 608px</td>
    <td>908px x 512px</td>
</tr>
<tr>
    <td><b>보호 영역 크기</b></td>
    <td>380px x 570px</td>
    <td>570px x 570px</td>
    <td>852px x 479px</td>
</tr>
<tr>
    <td><b>포커스가 없는 크기</b></td>
    <td>333px x 500px</td>
    <td>500px x 500px</td>
    <td>782px x 440px</td>
</tr>
<tr>
    <td><b>포커스가 지정 된 크기</b></td>
    <td>380px x 570px</td>
    <td>570px x 570px</td>
    <td>852px x 479px</td>
</tr>
</table>

Apple 콘텐츠 단면화 행에 대 한 다음 제안 사항을 제공합니다.

- **행 완성** -화면의 전체 너비로 확장 하려면 충분 한 콘텐츠를 제공 해야 합니다.
- **혼합 이미지 크기 조정** – The 콘텐츠 단면화 행 설계 된 다양 한 이미지 크기 (위에 표시 된 목록)에서 보관 합니다. 그러나 이미지 크기를 혼합 수행 하는 경우에 콘텐츠 표시를 정규화 할 추가 크기 조정을 적용할 수는 주의 해야 합니다.

<a name="Scrolling-Inset-Banners" />

#### <a name="scrolling-inset-banners"></a>스크롤 Inset 배너

필요에 따라 Xamarin.tvOS 앱을 자동으로 스크롤 및 거의 전체 화면 배너의 컬렉션을 반복 위쪽 선반에서 해당 콘텐츠를 제공할 수 있습니다. 이 스타일은 새 TV 프로그램 같은 다양 하 고 새 콘텐츠를 보여 주기 위해 일반적으로 사용 됩니다.

자동 스크롤 하는 것 외에도 사용자는 배너를 제어 하 고 Siri 원격을 사용 하 여 어느 방향으로 스크롤할 수 있습니다. 소수에 수행할 배너 포커스가 있을 때 Siri 원격 순환 제스처는 해당 배너 시차 효과 정품 인증 됩니다.

<table width="100%" border="1px">
<tr>
    <td colspan="2"><b>배너 이미지 (추가 넓은)</b></td>
</tr>
<tr>
    <td><b>실제 크기</b></td>
    <td>1940px x 624px</td>
</tr>
<tr>
    <td><b>보호 영역 크기</b></td>
    <td>1740px x 620px</td>
</tr>
<tr>
    <td><b>포커스가 없는 크기</b></td>
    <td>1740px x 560px</td>
</tr>
<tr>
    <td><b>포커스가 지정 된 크기</b></td>
    <td>1740px x 620px</td>
</tr>
</table>

Inset 배너 스크롤 하나으로 제공 될 수는 정적 `.png` 계층화 또는 `.lsr` 파일입니다.

Apple 스크롤 Inset 배너에 대 한 다음 제안 사항을 제공합니다.

- **콘텐츠 크기** -자연 느껴야 하 스크롤에 대 한 최소 3 개의 배너를 제공 해야 합니다. 개 이하의 8 (8) 배너를 포함 해야 하거나 것으로 확인 될 탐색 하드 최종 사용자에 대 한 합니다.
- **텍스트 콘텐츠** -배너 배너 이미지에 텍스트를 포함할지 필요로 하는 경우. 계층화 된 이미지를 사용 하는 경우 텍스트는 맨 위 계층에 있어야 합니다.

Apple의를 참조 하십시오 [TVServices 프레임 워크 참조](https://developer.apple.com/library/prerelease/tvos/documentation/TVServices/Reference/TVServices_Ref/index.html#//apple_ref/doc/uid/TP40016412) 위쪽 선반 확장 추가 하는 동적 위쪽 선반 콘텐츠를 제공 하기 위해 앱에 대 한 자세한 내용은 합니다.

<a name="Game-Center-Images" />

## <a name="game-center-images"></a>게임 센터 이미지

Xamarin.tvOS 앱은 게임 게임 센터 지원을 포함 한 경우 몇 가지 더 많은 이미지 자산 해야 합니다.

- **성과 아이콘** -불투명 이미지는 자동으로 잘리는 원형으로 각 성과에 필요 합니다. 성과 포커스를 받을 수 없는 항목입니다.
- **대시보드 아트 워크** -제공 게임 센터 앱의 대시보드 위쪽에 표시 되는 선택적 이미지 될 수 있습니다. 이러한 이미지는 비-포커스를 받을 수 있습니다.
- **순위표 아트 워크** -에 제공 해야 간의 일 (1)을 3 개의 16:9 가로 세로 비율 이미지 앱에서 지 원하는 각 순위표 합니다. 정적 장치일 수 `.png` 계층화 또는 `.lsr` 파일입니다. 순위표 아트 워크 포커스를 받을 수입니다.

<table width="100%" border="1px">
<tr>
    <td><b>&nbsp;</b></td>
    <td><b>성과 아이콘</b></td>
    <td><b>대시보드 아트 워크</b></td>
    <td><b>순위표 아트 워크</b></td>
</tr>
<tr>
    <td><b>표시 되는 크기</b></td>
    <td>200px x 200px</td>
    <td>923px x 150px</td>
    <td>N/A</td>
</tr>
<tr>
    <td><b>실제 크기</b></td>
    <td>320px x 320px</td>
    <td>N/A</td>
    <td>659px x 371px</td>
</tr>
<tr>
    <td><b>보호 영역 크기</b></td>
    <td>N/A</td>
    <td>N/A</td>
    <td>618px x 348px</td>
</tr>
<tr>
    <td><b>포커스가 없는 크기</b></td>
    <td>N/A</td>
    <td>N/A</td>
    <td>548px x 309px</td>
</tr>
<tr>
    <td><b>포커스가 지정 된 크기</b></td>
    <td>N/A</td>
    <td>N/A</td>
    <td>618px x 348px</td>
</tr>
</table>

게임 센터 사용에 대 한 자세한 내용은 Apple의를 참조 하십시오 [게임 센터 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/NetworkingInternet/Conceptual/GameKit_Guide/Introduction/Introduction.html)합니다.

<a name="Working-with-Images" />

## <a name="working-with-images"></a>이미지 작업

9 tvOS iOS 9 포함 하 고 Xamarin.iOS 앱에서 이미지를 표시 합니다. 사용 되는 방법이의 하위 집합 이므로 Xamarin.tvOS 앱에 대해서도 작동 합니다. 참조 하십시오 우리의 [이미지 표시](~/ios/app-fundamentals/images-icons/displaying-an-image.md) 자세한 정보에 대 한 설명서입니다.

<a name="Setting-Xamarin.tvOS-Project-Images" />

## <a name="setting-xamarintvos-project-images"></a>Xamarin.tvOS 프로젝트 이미지 설정

모든 tvOS 응용 프로그램에서는 위에서 설명한 대로 [시작 이미지](#Launch-Image), 및 [앱 아이콘](#App-Icons)합니다. 이 섹션에서는 자산 카탈로그에 설정 된 후 Xamarin.tvOS 응용 프로그램 프로젝트에 대 한 시작 이미지 및 응용 프로그램 아이콘을 선택 합니다.

다음을 수행합니다.

1. 에 **솔루션 탐색기**를 두 번 클릭은 `Info.plist` 편집 하기 위해 열려는: 

    [![](icons-images-images/info01.png "Info.plist 파일")](icons-images-images/info01.png#lightbox)
2. 에 **Info.Plist 편집기**, 자산 카탈로그를 선택 (에서 위에 구성 된는 [앱 아이콘 설정](#Setting-the-App-Icons) 섹션)에 대 한는 **앱 아이콘**: 

    [![](icons-images-images/info02.png "Info.Plist 편집기")](icons-images-images/info02.png#lightbox)
3. 다음으로, 자산 카탈로그를 선택 (에서 위에 구성 된는 [시작 이미지 설정](#Setting-the-Launch-Image) 섹션)에 대 한는 **시작 이미지**합니다.
4. 변경 내용을 저장합니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서는 이미지 형식 및 Xamarin.tvOS 응용 프로그램에서 사용 된 크기의 모든 검사가 수행 합니다. 첫째, 이미지를 시작, 계층화 된 이미지, 응용 프로그램 아이콘, 위쪽 선반 이미지 및 게임 센터 이미지 다룹니다. 다음 이미지 Xamarin.tvOS 응용 프로그램에서 작업을를 다룹니다.

## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 응용 프로그램 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
