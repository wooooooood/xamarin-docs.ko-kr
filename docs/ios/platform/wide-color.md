---
title: 광범위 한 색
description: 이 문서에서는 광범위 한 색 및 Xamarin.iOS 또는 Xamarin.Mac 앱에 사용할 수 있습니다 어떻게 다룹니다.
ms.prod: xamarin
ms.assetid: 576E978A-F182-489A-83E4-D8CDC6890B24
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 5f56b396715159cbc1539ae9a7f30cc7ad2236bf
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="wide-color"></a>광범위 한 색

_이 문서에서는 광범위 한 색 및 Xamarin.iOS 또는 Xamarin.Mac 앱에 사용할 수 있습니다 어떻게 다룹니다._

iOS 10와 macOS 시에라 확장 범위 픽셀 형식 및 코어 그래픽, Core 이미지, 금속 및 AVFoundation 등의 프레임 워크를 비롯 하 여 시스템 전체에서 광범위 한 색상 공간에 대 한 지원을 향상 시킵니다. 전체 전체 그래픽 스택에서이 동작을 제공 하 여 광범위 한 색 표시를 사용 하 여 장치에 대 한 지원은 쉽게 할 추가 합니다.

## <a name="about-wide-color"></a>광범위 한 색에 대 한

위에서 설명한 대로 iOS 10와 macOS 시에라 확장 범위 픽셀 형식 및 코어 그래픽, Core 이미지, 금속 및 AVFoundation 등의 프레임 워크를 포함 하 여 시스템 전체의 광범위 한 색상 공간에 대 한 지원을 향상 시킵니다. 전체 전체 그래픽 스택에서이 동작을 제공 하 여 광범위 한 색 표시를 사용 하 여 장치에 대 한 지원은 쉽게 할 추가 합니다.

90's Apple mac에 대 한 처리 색상 처리 ColorSync 만들었습니다. 이러한 작업 만들고 일련의 컴퓨터 하드웨어에서 색을 처리 하기 위한 표준 승격 하 International Color Consortium (ICC)를 찾을 수 있습니다. Apple의 사용은 ICC ColorSync에 포함 된 및 OS X (이제 macOS 라고 함)의 핵심에 기본 제공 된 합니다.

레 티 나 표시, 새 P3 표시 및 (2015에서는 iMac에 릴리스됨) 표시 P3 색 공간 등의 하드웨어 디스플레이 기술의 선두 되었습니다 Apple 및 iPad, iPhone 7 장점과 7 iPhone에 표시 됩니다는 TrueTone 플러스 합니다.

시에라 macOS에서 광범위 한 색 및 10 iOS와 Apple macOS와 iOS에는이 새로운 디스플레이 기술을 최대한 활용 하는 색을 처리 하는 방법이 변경 됩니다.

## <a name="core-color-concepts"></a>색의 핵심 개념

다음 핵심 개념을 색 보다 자세히 살펴볼 iOS와 macOS에서 색을 수행 하기 전에 적용 해야 합니다.

### <a name="color-space"></a>색 공간

색 공간은 환경은 색 표시 하 고 비교할 수 있습니다. 강도 색상 구성 요소에서 정의한 1 ~ 4 개의 차원 공간 수 있습니다. 

[![](wide-color-images/color00.png "색 공간")](wide-color-images/color00.png#lightbox)

### <a name="color-channels"></a>색 채널

또한 색상 구성 색 채널로 참조할 수 있습니다. 일부 친숙 한 표현을 RGB 공백, 회색 공간, CMYK 공백 또는 장치 독립적인 공간 것입니다. 

[![](wide-color-images/color02.png "색의 구성 요소 수를 구분 색 채널으로")](wide-color-images/color02.png#lightbox)

### <a name="color-primaries"></a>기본 색

색 됨으로 비교 하 고 색을 계산 하는 데 사용 되는 좌표계를 제공 합니다. 색 됨으로 대개 색 채널 내에서 생성 될 수 있는 지정된 된 색의 가장 강한 버전을 포함 합니다.

[![](wide-color-images/color01.png "색 됨으로 비교 하 고 색을 계산 하는 데 사용 되는 좌표계를 제공 합니다.")](wide-color-images/color01.png#lightbox)

RGB 색 공간 위에 표시 되는 경우 색 됨으로 위치는 `1.0` 좌표 고정 됩니다 (같은 `[1.0, 0.0, 0.0]` 빨강)입니다.

### <a name="color-gamut"></a>색 영역

모든 제공 색 공간 내에서 개별 색 채널의 조합으로 정의할 수 있는 색 색 영역 참조 합니다.

[![](wide-color-images/color03.png "색 색상 영역 예제")](wide-color-images/color03.png#lightbox)

## <a name="what-is-wide-color"></a>광범위 한 색 무엇입니까

광범위 한 색의 주제를 다루기 전에 현재 업계 표준 RGB 색 공간 (sRGB) 색에 대 한 표준에 대 한 토론이 되어야 합니다. 현재 컴퓨팅 환경에서 가장 널리 사용 되는 색 공간 이며 macOS 및 iOS에 대 한 기본 색 공간입니다.

색 공간 sRGB에 다음과 같은 속성이 있습니다.

- Itu-r BT.709 표준에 기반 합니다.
- 2.2는 대략적인 감마를 있습니다.
- 일반적인 조명 조건을 (d 65)를 나타냅니다.

SRGB 널리 업계에서 사용 하면 개발자 몇 가지 사항을 가정 프로그램으로 지정할 수는 지정 된 색이 충실히에 표시 되는 모든 장치에서 표현 됩니다. 그러나이 항상 아닐 경우. 또한는 몇 가지 색을 색 공간 sRGB에 적합 하지 않은 하 고, 따라서에 나타낼 수 없습니다.

예를 들어 많은 텍 많은 잉크 및 잉크나 염료 sRGB 외부 떨어질 때 사용 하 여 스레드를 가진 설계 되었습니다. 또한 사용자는 자신의 일상 업무에 경험 하는 많은 제품 색 공간 sRGB 범위에 속하지 않는 밝은, 선명한 색으로 만들어집니다. SRGB에서 표현할 수 없는 색의 가장 큰 예 일부가 sunsets가 차원 리프, 삼 꽃 열 대 물 같은 본질적으로 작업 합니다.

사용자가 있는 원시 형태로 디지털 이미지 캡처 되었습니다 sRGB 색 공간에서에서 적절 하 게 표현할 수 없는 하 고 따라서 없습니다 제대로 화면에 표시 하는 경우에 모든이 색 데이터를 포함 하는 장치 이미지 될 수 있습니다.

### <a name="the-display-p3-color-space"></a>디스플레이 P3 색 공간

2015, 사과 출시 sRGB 색 공간에서 만든 문제를 처리 하는 새 디스플레이 P3 색 공간을 제공 하는 새 제품 (iMac 및 iPad Pro 9.7").

[![](wide-color-images/color04.png "새 표시 P3 색 공간")](wide-color-images/color04.png#lightbox)

디스플레이 P3 색 공간에는 다음과 같은 속성이 있습니다.

- 최신 하드웨어 플랫폼에 대 한 광범위 한 색 공간을 지원합니다.
- SMPTE DCI P3 표준을 기반으로 합니다. DCI P3은 디지털 프로젝터 용 모니터를 지원 하 여 수정한 것입니다.
- 2.2는 대략적인 감마를 있습니다.
- 일반적인 조명 조건을 (d 65)를 나타냅니다.

Apple에 따라 사용자를 이동 하는 워크플로 해당 모바일 플랫폼입니다. SRGB 보다 더 광범위 한 색 표시를 포함 하 여 필요한 전문 모바일 장치 (iPad 전문가)에서 설명 하는 색 프레젠테이션 문제를 해결 합니다. 이러한 솔루션 중 하나가 된 팩터리 인지 확인 하는 장치를 컬러 디스플레이가 정확 하 고 일관에서 보정 되어 각 개별 장치 공장 보정 업그레이드 하는 것 이었습니다.

다른 솔루션은 전체, 시스템 수준 색 관리 Apple iOS 10와 macOS 시에라에 구축에 포함 합니다. 

### <a name="the-extended-range-srgb-color-space"></a>확장 범위 sRGB 색 공간

새로운 iOS 10 색 시스템 수준 관리는 모두 기존 iOS 앱 빌드 및 sRGB에 대 한 미세을 고려 합니다. 이러한 기존 앱의 어느 색 표현 또는 응용 프로그램 성능 영향을 하지 않은 되도록 설계 되었습니다.

이 상황을 처리 하려면 Apple가 iOS 10에서에서 범위 확장 sRGB 색 공간 (및 macOS 시에라도)를 포함 합니다.

확장 범위 sRGB 색 공간에는 다음과 같은 속성이 있습니다.

- 에 모든 동일한 sRGB 보관 됩니다.
- 2.2는 대략적인 감마를 있습니다.
- 일반적인 조명 조건을 (d 65)를 나타냅니다.
- 음수 값을 지원 하며 (1) 1 보다 큰 값.

으로 0 보다 작은 값에 대 한 허용 하 고, 1 보다 큰 색 공간 뿐만 아니라 성능 문제가 또는 왜곡 하지만 없이 sRGB에 현재 색에 기존 앱에 대 한 허용 범위 확장 sRGB 허용 안에 표시 되는 색을 나타내는 색 공간 스펙트럼 합니다. 이 모든 작업은 동일한 앵커 지점을 색 공간 sRGB로 그대로 유지 하면서 수행 됩니다.

### <a name="extended-range-srgb-in-action"></a>동작에서 확장 된 범위 sRGB

확장 범위 sRGB 색 공간에서에서 외부 0과 1의 값이 어떻게 작동을 보려면 수행의 다음 예제는 디스플레이 P3 색 공간에서 사용할 수 있는 가장 높은 채도 빨간색의:

[![](wide-color-images/color05.png "확장 범위 sRGB 색 공간에서에서 값 0과 1 외부에서 작동 하는 방법")](wide-color-images/color05.png#lightbox)

디스플레이 P3이이 색으로 나타낼 수는 `[1.0, 0.0, 0.0]` 및 것 확장 범위 sRGB `[1.358, -0.074, -0.012]`합니다. SRGB 값 가득 찼기 때문에 표시 P3 값과 표시 P3 안에 포함 된 배치 "외부" sRGB 범위의 합니다.

음수 값이 높은로 극단적인 양수에서 픽셀 값을 허용 하는 실제 하드웨어에 대 한 표시 스펙트럼에서 사용할 수 있는 색을 표시할 수 있습니다 및 확장 범위 sRGB 색 공간에서에서 이러한 값을 나타낼 수 있습니다.

### <a name="device-pixel-formats"></a>장치 픽셀 형식입니다. 

대부분의 색 채널당 8 비트는 이후 8 비트 픽셀 형식을 사용 하 여 색 공간을 크게 되었습니다 sRGB 표준화 sRGB의 색을 설명 하기 위해 충분 합니다. 이 변경은 완벽 한 아니라 적당 하 고 이미지를 표시 하는 메모리 및 프로세서 사용을 좋은 절충을 제공 합니다.

디스플레이 P3 sRGB 색 공간 외부에서 색 좌표를 나타낼 수 있으므로 / 색 채널을 올바르게 확장 범위 sRGB 색 공간으로 색을 나타내는 16 비트 필요 합니다.

## <a name="system-wide-wide-color-support"></a>시스템 차원의 광범위 한 색 지원

광범위 한 색 및 iOS 10 내부 넓은 범위와 macOS 시에라를 완벽 하 게 지원 하려면 Apple에 확장 범위 sRGB 색 공간을 완전히 활용 및 P3 디스플레이 사용 하도록 다음 프레임 워크 확장:

- UIKit (iOS만 해당)에 대 한
- SceneKit
- 코어 그래픽
- ImageIO
- Core 이미지
- WebKit
- SpriteKit
- 코어 애니메이션
- AppKit (용 macOS에만 해당)

또한 확장 범위 sRGB 색 공간에 대 한 지원이 향상 되었습니다 레 티 나 디스플레이 및 디스플레이 P3 표시 합니다.

광범위 한 색은 지원 및 응용 프로그램 콘텐츠 형식에 사용할 수 있습니다.

- 앱 번들에 포함 된 정적 이미지 리소스입니다.
- 문서 및 네트워크 이미지 리소스를 기반으로 합니다.
- 라이브 사진이 나 원시 형식의 이미지와 같은 고급 미디어입니다.
- 3D 그래픽 셰이더 질감 이미지입니다.

## <a name="solving-the-color-problem"></a>색 문제 해결

응용 프로그램에 표시 되는 콘텐츠는 다양 한 색 서식 있는 소스에서에서 가져올 수 있습니다. 또한 자신의 범위의 색 표시 기능을 가진 각 광범위 한 장치 범위에이 콘텐츠를 표시할 수 있습니다.

10 iOS 앱의 차이이 두 가지 문제는 새 기본 제공 되는 시스템 차원의 색 관리 시스템을 사용 하 여 연결 합니다. 이 시스템에는 이미지 필드가 표시 동일한 이미지에서 인코딩된 어떤 색 공간에 관계 없이 모든 iOS 장치에서 되도록 합니다.

연결 된 색 공간 (또는 색 프로필) 모든 이미지와 색 관리 시작 합니다. 이 정보에 사용 되는 _색 일치 과정_ 출력 장치에 색에 원본 이미지의 색 일치 위치 합니다. 이미지의 모든 픽셀 색 일치 될 해야, 하므로 수 시간이 오래 걸릴 수 하 고 장치 CPU 시 과도 합니다.

특성으로 인해는 _색 일치 과정_,이 변환은 손실 될 수 있는"" 출력 장치에는 더 작은 영역 보다 소스 이미지 될 수 있습니다.

로 이동 하는 계산 다행히는 _색 일치 과정_ 쉽게 하드웨어 속도가 빨라질 수 있습니다 (중 하나에 CPU 또는 GPU) 및 Apple 지원 Quartz 2D 같은 기본 시스템을 구축 하 여 자동으로 작동 하는지 확인 ColorSync 및 코어 애니메이션 합니다. 올바르게 태그가 지정 된 콘텐츠에 대 한 코딩이 필요 이러한 기능을 사용 합니다.

색 관리 다음과 같이 각 플랫폼에서 지원 되는:

- **macOS** -macOS 이래로 관리 되는 색 되었습니다.
- **iOS** -iOS는 iOS 9.3 (이상)부터 자동 색 관리를 지원 합니다.

### <a name="designing-for-wide-gamut"></a>넓은 영역을 위한 디자인

Apple에 디자인에 대 한 다음 제안 하 고 너비를 사용 하 여 색, macOS 및 iOS 앱에 넓은 영역 이미지 콘텐츠:

- 만 확인에서 위치 넓은 영역 콘텐츠를 사용 하지 자동으로 사용 해야 everywhere 앱에서 다음을 감지 합니다.
- 만 사용 하 여 넓은 영역 콘텐츠 선명한 색 사용자 환경을 향상 시키는 됩니다.
- 에 필요 하지 않으므로 P3를 기존 응용 프로그램에 대 한 모든 콘텐츠를 변경 하려면

Apple의 도구 체인을 사용 하면 완전히 상황은 응용 프로그램의 광범위 한 색을 지 원하는 점진적으로 옵트인, 넓은 영역 이미지 콘텐츠에 대 한 지원 있습니다.

### <a name="upgrading-existing-content-to-wide-color"></a>기존 콘텐츠 광범위 한 색으로 업그레이드

Apple에 기존 이미지 콘텐츠 광범위 한 색으로 업그레이드 하기 위한 다음 제안 사항이 있습니다.

- 방금 "에 할당 안 함" P3 프로필 이미지 편집 응용 프로그램의에서 콘텐츠입니다. 이렇게 기존 색 색 따라서 이미지를 변경 하는 새 공간에 맞게 늘이기 같은 예기치 않은 결과 사용 하 여 새 색 공간에 콘텐츠를 다시 매핑할 하기만 하면 됩니다.
- 변환할 "" 응용 프로그램을 편집 하는 이미지를 사용 하 여 디스플레이 P3 프로필 이미지 콘텐츠를 할 수 있습니다.

### <a name="file-formats-and-color-profiles"></a>파일 형식 및 색 프로필

Apple에 다음 파일 형식 및 응용 프로그램의 광범위 한 색 이미지 내용에 사용 되는 색 프로필에 대 한 제안 사항:

- RGB 작업 공간에 대 한 "디스플레이 P3" 색 프로필을 사용 합니다.
- 색 채널 모드 당 16 비트를 사용 합니다.
- 지연 2015 iMac를 사용 하 여 (또는 이상) 이미지 콘텐츠를 미리 보려면 정확 하 게 합니다.
- 이미지 자산에 포함 된 "디스플레이 P3" ICC 프로 파일 16 비트 PNG 파일로 내보냅니다.

> [!IMPORTANT]
> 사용 하는 **웹에 대 한 저장** 또는 **내보내기 자산** 편집 소프트웨어 가장 인기 있는 이미지에 기능을 찾을 _되지 것입니다_ 이러한 기능이 적용 되지 않은 이후 광범위 한 색 이미지에 대 한 작업 아직 필요한 파일 형식 사양을 지원 하도록 업데이트 합니다.

### <a name="supporting-wide-color-with-asset-catalogs"></a>광범위 한 색 자산 카탈로그 지원

IOS 10와 macOS 시에라 Apple에 확장 되었고, 광범위 한 색을 지원 하기 위해 자산 카탈로그를 포함 하 고 앱의 번들에 정적 이미지 콘텐츠를 분류 하는 데 사용 합니다.

사용 하 여 자산 카탈로그에는 앱에 다음과 같은 이점을 제공 합니다.

- 정적 이미지 자산에 대 한 최상의 배포 옵션을 제공합니다.
- 지 원하는 자동 색 수정.
- 이러한 자동 픽셀 형식 최적화를 지원 합니다.
- 지 원하는 응용 프로그램 및 응용 프로그램은 관련 get 콘텐츠만 최종 사용자의 장치에 전달 되도록 보장 하는 감소 합니다.

Apple 자산 카탈로그 다음과 같은 향상 기능이 광범위 한 색 지원에 대 한 적용 했습니다.

- 두 소스 16 비트 (채널당 색) 콘텐츠를 지원합니다.
- 디스플레이 영역에서 카탈로그 콘텐츠를 지원 합니다. 콘텐츠는 sRGB 또는 디스플레이 P3 색상에 대 한 태그 수 있습니다.

개발자가 자신의 응용 프로그램에서 광범위 한 색 콘텐츠를 지원 하기 위한 세 가지 옵션:

1. **아무 작업도 수행 하지** -이 요구 사항 외 기간에 콘텐츠를로 두어야 광범위 한 색 콘텐츠 응용 프로그램 (여기서은 돋보이게 할 사용자의 경험) 선명한 색을 표시 해야 하는 경우에만 사용 해야, 이후-됩니다. 모든 하드웨어 상황에서 제대로 렌더링 계속 됩니다.
2. **기존 콘텐츠 표시 P3로 업그레이드** -이렇게 하려면 개발자가 자신의 자산 카탈로그에 기존 이미지 콘텐츠 P3 형식으로 업그레이드 된 새로운 파일을 바꾸고 그러한 자산 편집기에서 태그 수입니다. 빌드 시 sRGB 파생 이미지를 이러한 자산에서 생성 됩니다.
3. **자산 콘텐츠 액세스에 최적화 된 제공** -이 경우 개발자는 8 비트 sRGB 및 자산 카탈로그 (제대로 자산 편집기에서 태그 있음)에 각 이미지의 16 비트 디스플레이 P3 비전 모두 제공 합니다.

### <a name="asset-catalog-deployment"></a>자산 카탈로그 배포

다음은 개발자가 광범위 한 색 이미지 콘텐츠를 포함 하는 자산 카탈로그와 함께 앱 스토어에 앱을 제출 하는 경우 발생 합니다.

- 최종 사용자에 게 앱을 배포 하는 앱에서 분리 하면 적절 한 콘텐츠 variant 사용자의 장치에 배달 됩니다.
- 광범위 한 색을 지원 하지 않는 장치에서 페이로드 무료로 광범위 한 색 콘텐츠를 포함 하는 데에 장치에 함께 제공 되지 않습니다.
- `NSImage` 시에라 macOS에서 (이상)가 자동으로 하드웨어의 표시에 대 한 콘텐츠를 가장 잘 표시를 선택 합니다.
- 표시 된 콘텐츠가 변경 하거나 장치 하드웨어 디스플레이 특성 변경 하는 경우 자동으로 새로 고칠 수 됩니다.

### <a name="asset-catalog-storage"></a>자산 카탈로그 저장소

자산 카탈로그 저장소에는 다음과 같은 속성 및 응용 프로그램에 미치는 영향:

- 빌드 시 Apple 고품질 이미지 변환을 통해 이미지 콘텐츠 저장을 최적화 하려고 시도 합니다.
- 16 비트 광범위 한 색 콘텐츠 / 색 채널 사용 됩니다.
- 콘텐츠 종속 이미지 압축 결과물 콘텐츠 크기를 줄이려면 하는 데 사용 됩니다. 새 "손실" 압축 콘텐츠 크기를 최적화 하기 위해 추가 되었습니다.

## <a name="colors-in-user-interfaces"></a>사용자 인터페이스의 색

사용자 인터페이스의 색을 사용 하는 경우 대부분의 화면에 있는 픽셀은 단색입니다. 또한 대부분 이러한 픽셀의 이미지 자산 중에서 오지 있지만 응용 프로그램에서 직접 (또는 응용 프로그램을 대신 하 여 운영 체제에서) 그려집니다.

다양 한 색상 UI 수준에서 작업할 때 몇 가지 문제를 제공 합니다.

- **색 통신** -디자이너와 개발자는 일반적으로 사이의 색에 대해 이야기 하는 경우는 _가정_ sRGB 색 공간을 포함 합니다. 색으로 전달 될 수 있도록 `rgb(128, 45, 56)` 또는 `#FF0456`합니다. 넓은 영역 디자인에서 이러한 표현은 정확 하 게 지정 된 색을 나타내는 데 충분 한 정보를 제공 하지 않습니다, 그리고 색 공간 작업 포함 해야 합니다. 사용 하 여 Apple 제안 `P3(128, 45, 56)` 및 `P3#FF0456` 대신 합니다. 
- **색 선택** -대부분의 인기 있는 이미지 편집와 디자인의 기본 제공 색 선택기를 사용 하는 경우 소프트웨어 색 공간 sRGB과 동일한 제한 사항이에서 발생 합니다. 디자이너는 이들이 색상 선택기에는 "디스플레이 P3" 색 공간에 광범위 한 색 디자인으로 작업할 때 확인 해야 합니다.
- **색상 코딩** 두- `NSColor` (macOS) 및 `UIColor` (iOS 및 tvOS) P3 색을 직접 생성 하기 위한 새 편의 메서드 있고 둘 다는 확장 범위 sRGB 색 공간도에서 색을 지원 하도록 확장 되었습니다.
- **색 저장** -앱의 문서에 저장 넓은 영역 색을 지정 하는 경우 주의 해야 합니다. 여기서 sRGB 색 공간에 대 한 없었지만 색 채널당 8 비트, 넓은 색에 16 비트 / 색 채널을 사용 해야 합니다. 개발자는 또한 감시 해야 맡은 색 공간 (이후 모든 항목은 일반적으로 sRGB만)의 인스턴스.

## <a name="colors-on-the-web"></a>웹에서 색

웹 페이지에서, 광범위 한 색 표시를 지 원하는 장치에서 광범위 한 색을 작업할 때 주의 해야 합니다. 경우 모든 웹 사이트에 포함 된 이미지 콘텐츠 적절 하 게 태그가 지정 되었습니다, iOS 및 macOS 자동으로 일치 하는 색를 정확 하 게 표시할 합니다.

미디어 쿼리 P3 및 sRGB 가능 장치 간의 자산을 해결 하기 위해 사용할 수도 있습니다.

```xml
<picture>
    <source srcset="monkey-p3.jpg" media="(color-gamut: p3)">
    <source srcset="monkey-rpg.jpg">
</picture>
```

Apple에 CSS를 맡은 sRGB 공간 외에도 다른 색상 공간에 지정할 수 있도록 WebKit 제안을 있습니다.

## <a name="rendering-off-screen-images-in-app"></a>응용 프로그램에서 화면에서 벗어난 이미지 렌더링

만들려는 앱의 형식에 따라 것은 인터넷에서 수집한 이미지 내용을 만들거나 (예: 벡터 응용 프로그램을 예를 들어 드로잉) 응용 프로그램 내에 직접 이미지 콘텐츠를 포함 하도록 사용자를 통합할 수 있습니다.

두이 경우 모두 응용 프로그램 렌더링할 수 있습니다 필요한 이미지 iOS 및 macOS에 추가 된 향상 된 기능을 사용 하 여 광범위 한 색에서 화면에서 벗어난 합니다.

### <a name="drawing-wide-color-in-ios"></a>IOS에 광범위 한 색을 그리기

IOS 10에서에서 광범위 한 색 이미지를 올바르게 그리는 방법을 다루기 전에 다음과 같은 일반적인 iOS 그리기 코드를 살펴보겠습니다.

```csharp
public UIImage DrawWideColorImage ()
{
    var size = new CGSize (250, 250);
    UIGraphics.BeginImageContext (size);

    ...
    
    UIGraphics.EndImageContext ();
    return image = UIGraphics.GetImageFromCurrentImageContext ();
}
```

해결 해야 하는 표준 코드에는 문제가 _전에_ 광범위 한 색 이미지를 그리는 데 사용할 수 있습니다. `UIGraphics.BeginImageContext (size)` iOS 이미지 그리기를 시작 하는 데 사용 하는 메서드에 다음과 같은 제한이 있습니다.

- 이미지 컨텍스트 색 채널당 8 비트 이상 만들 수 없습니다.
- 확장 범위 sRGB 색 공간에서에서 색을 나타낼 수 없습니다.
- 백그라운드에서 호출 되 고 하위 수준 C 루틴으로 인해 비 sRGB 색 공간에서 컨텍스트를 만들에 대 한 인터페이스를 제공 하는 기능을 않아도 됩니다.

이러한 제한은 처리 하 고 iOS 10에서에서 광범위 한 색 이미지를 그리기를 하려면 다음 코드를 대신 사용:

```csharp
public UIImage DrawWideColorImage ()
{
    var size = new CGSize (250, 250);
    var render = new UIGraphicsImageRenderer (size);

    var image = render.CreateImage ((UIGraphicsImageRendererContext context) => {
        var bounds = context.Format.Bounds;
        CGRect slice;
        CGRect remainder;
        bounds.Divide (bounds.Width / 2, CGRectEdge.MinXEdge, out slice, out remainder);

        var redP3 = UIColor.FromDisplayP3 (1.0F, 0.0F, 0.0F, 1.0F);
        redP3.SetColor ();
        context.FillRect (slice);

        var redRGB = UIColor.Red;
        redRGB.SetColor ();
        context.FillRect (remainder);
    });

    // Return results
    return image;
}
```

새 `UIGraphicsImageRenderer` 다음 기능이 및 클래스 확장 범위 sRGB 색 공간을 처리할 수 있는 새 이미지 컨텍스트를 만듭니다.

- 기본적으로 관리 되는 색을 완벽 하 게 합니다.
- 기본적으로 확장 범위 sRGB 색 공간을 지원합니다.
- 지능적으로 sRGB 또는 Extended 범위 sRGB 응용 프로그램에서 실행 되는 iOS 장치 기능에 따라 색 공간에서 렌더링 해야 하는 경우를 결정 합니다.
- 완벽 하 게 하 고 자동으로 관리 하는 이미지 컨텍스트 (`CGContext`) 수명 개발자 호출에 대해서는 걱정 하지 않아도 되므로 시작 되 고 상황에 맞는 명령을 종료 합니다.
- 와 호환 되는 `UIGraphics.GetCurrentContext()` 메서드.

`CreateImage` 의 메서드는 `UIGraphicsImageRenderer` 클래스 광범위 한 색 이미지를 만드는 데 호출 되 고 완료 처리기에 이미지 컨텍스트를 전달 합니다. 모든 그리기 작업 수행 됩니다이 완료 처리기 안에서 다음과 같습니다.

- `UIColor.FromDisplayP3` 메서드 새 채도가 빨간색 표시 P3 색 공간 넓은 영역에서 만들고 사각형의 첫 번째 부분을 그리는 데 사용 됩니다. 
- 두 번째 완벽 하 게 정상적인 sRGB에 그릴 사각형의 절반 포화 비교에 대 한 빨간색입니다.

### <a name="drawing-wide-color-in-macos"></a>광범위 한 색 macOS에서 그리기

`NSImage` 광범위 한 색 이미지 그리기를 지원 하기 위해 시에라 macOS에서 클래스 확장 되었습니다. 예를 들어:

```csharp
var size = CGSize(250,250);
var wideColorImage = new NSImage(size, false, (drawRect) =>{
    var rects = drawRect.Divide(drawRect.Size.Width/2,CGRect.MinXEdge);
    
    var color = new NSColor(1.0, 0.0, 0.0, 1.0).Set();
    var path = new NSBezierPath(rects.Slice).Fill();
    
    color = new NSColor(1.0, 0.0, 0.0, 1.0).Set();
    path = new NSBezierPath(rects.Remainder).Fill();
    
    // Return modified
    return true;
});
```

## <a name="rendering-on-screen-images-in-app"></a>응용 프로그램에서 이미지 화면 렌더링

이미지를 렌더링할 광범위 한 색 화면, 프로세스 작동 macOS 및 위에 iOS에 대 한 광범위 한 색 화면에서 벗어난 이미지 그리기 비슷합니다.

### <a name="rendering-on-screen-in-ios"></a>IOS의 화면 렌더링

응용 프로그램을 광범위 한 색 iOS에서 화면에 이미지를 렌더링 해야 하는 경우 재정의 `Draw` 의 메서드는 `UIView` 일반적인 방법 대로 의심 합니다. 예를 들어:

```csharp
using System;
using UIKit;
using CoreGraphics;

namespace MonkeyTalk
{
    public class MonkeyView : UIView
    {
        public MonkeyView ()
        {
        }

        public override void Draw (CGRect rect)
        {
            base.Draw (rect);

            // Draw the image to screen
            ...
        }
    }
}
``` 

IOS 10와 마찬가지로 `UIGraphicsImageRenderer` 지능적으로 sRGB 또는 Extended 범위 sRGB 응용 프로그램이 실행 될 때 iOS 장치 기능에 따라 색 공간에서 렌더링 해야 하는 경우 결정 위에 표시 된 클래스는 `Draw` 메서드를 호출 합니다. 또한는 `UIImageView` 도 iOS 9.3 이후 관리 컬러 되었습니다.

응용 프로그램에 렌더링이 수행 되는 방법을 알고 있어야 하는 경우는 `UIView` 또는 `UIViewController`, 새 확인할 수 `DisplayGamut` 의 속성은 `UITraitCollection` 클래스. 이 값이 됩니다는 `UIDisplayGamut` 다음의 열거 합니다.

- P3
- Srgb
- 지정되지 않음

응용 프로그램에서 색 공간 이미지를 그리는 데 사용 되는 제어 하려는 경우 새 사용할 수 `ContentsFormat` 의 속성은 `CALayer` 원하는 색 공간을 지정 합니다. 이 값이 될 수 있습니다는 `CAContentsFormat` 다음의 열거 합니다.

- Gray8Uint
- Rgba16Float
- Rgba8Uint

### <a name="rendering-on-screen-in-macos"></a>MacOS에서 화면 렌더링

응용 프로그램을 광범위 한 색 macOS에서 화면에 이미지를 렌더링 해야 하는 경우 재정의 `DrawRect` 의 메서드는 `NSView` 일반적인 방법 대로 의심 합니다. 예를 들어:

```csharp
using System;
using AppKit;
using CoreGraphics;
using CoreAnimation;

namespace MonkeyTalkMac
{
    public class MonkeyView : NSView
    {
        public MonkeyView ()
        {
        }

        public override void DrawRect (CGRect dirtyRect)
        {
            base.DrawRect (dirtyRect);

            // Draw graphics into the view
            ...
        }
    }
}
```

다시 지능적으로 결정 하는 경우에 sRGB 또는 Extended 범위 sRGB 될 때 앱을 실행 하는 Mac 하드웨어의 기능에 따라 색 공간에서 렌더링 해야 하는 `DrawRect` 메서드를 호출 합니다.

응용 프로그램에서 색 공간 이미지를 그리는 데 사용 되는 제어 하려는 경우 새 사용할 수 `DepthLimit` 의 속성은 `NSWindow` 원하는 색 공간을 지정 하는 클래스입니다. 이 값이 될 수 있습니다는 `NSWindowDepth` 다음의 열거 합니다.

- OneHundredTwentyEightBitRgb
- SixtyfourBitRgb
- TwentyfourBitRgb

## <a name="summary"></a>요약

이 문서 광범위 한 색와 수 있습니다 수 구현 하는 Xamarin.iOS 또는 Xamarin.Mac 앱 내에서 사용 되는 방식 검사가 수행 됩니다.



## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://developer.xamarin.com/samples/ios/iOS10/)
