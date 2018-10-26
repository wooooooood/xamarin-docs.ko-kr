---
title: Xamarin.iOS에서 광범위 한 색
description: 이 문서는 광범위 한 색 및 어떻게 Xamarin.iOS 또는 Xamarin.Mac 앱에서 사용할 수를 설명 합니다. 또한 색 공간, 채널 및 주 데이터베이스와 같은 중요 한 많은 색상 관련 개념의 대략적인 개요를 제공합니다.
ms.prod: xamarin
ms.assetid: 576E978A-F182-489A-83E4-D8CDC6890B24
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: f139bcceda12752e43a3a8330fa0a0e038e539f9
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121310"
---
# <a name="wide-color-in-xamarinios"></a>Xamarin.iOS에서 광범위 한 색

_이 문서에서는 광범위 한 색 및 어떻게 Xamarin.iOS 또는 Xamarin.Mac 앱에서 사용할 수 있습니다를 다룹니다._

iOS 10과 macOS Sierra 확장 범위 픽셀 형식 및 AVFoundation 핵심 그래픽, Core 이미지, 금속 등 프레임 워크를 비롯 한 시스템 전체에서 광범위 한 색상 공간에 대 한 지원을 향상 시킵니다. 와이드 컬러 디스플레이 사용 하 여 장치에 대 한 지원은 전체 그래픽 스택 전체에서이 동작을 제공 하 여 용이 추가 됩니다.

## <a name="about-wide-color"></a>광범위 한 색에 대 한

위에서 설명한 대로 iOS 10과 macOS Sierra 확장 범위 픽셀 형식 및 AVFoundation 핵심 그래픽, Core 이미지, 금속 등 프레임 워크를 포함 하 여 시스템 전체에서 광범위 한 색상 공간에 대 한 지원을 향상 시킵니다. 와이드 컬러 디스플레이 사용 하 여 장치에 대 한 지원은 전체 그래픽 스택 전체에서이 동작을 제공 하 여 용이 추가 됩니다.

90's Apple ColorSync 핸들 색 mac에 대 한 처리를 만들었습니다. 또한 이러한을 만들고 컴퓨터 하드웨어의 색을 처리 하기 위한 표준 집합인 승격 International 색 Consortium (ICC)를 찾을 수 있었습니다. Apple의 사용 된 ICC ColorSync에 포함 된 및 OS X (이제 macOS 라고 함)의 핵심에 내장 되었습니다.

레 티 나 표시, 새 P3 표시 및 (2015에서는 iMac 릴리스) 표시 P3 색 공간 등의 하드웨어를 사용 하 여 디스플레이 기술 최우선 되었습니다 Apple 및 iPad, iPhone 7 장점과 iPhone 7에 표시 된 TrueTone Plus입니다.

MacOS Sierra에서에서 광범위 한 색 및 iOS 10 사용 하 여 Apple macOS 및 iOS에는 이러한 새로운 표시 기술의 활용 하기 위해 색을 처리 하는 방식을 변경 됩니다.

## <a name="core-color-concepts"></a>색의 핵심 개념

다음 핵심 색 개념 macOS 및 iOS에서 색을 면밀히 검토를 수행 하기 전에 설명 해야 합니다.

### <a name="color-space"></a>색 공간

색 공간을는 있는 색 표현 하 고 비교할 수는 환경입니다. 해당 색 구성의 강도 따라 정의한 1 ~ 4 개의 차원 공간을 수 있습니다. 

[![](wide-color-images/color00.png "색 공간")](wide-color-images/color00.png#lightbox)

### <a name="color-channels"></a>색 채널

또한 색 구성 요소도 색 채널을 참조할 수 있습니다. 몇 가지 친숙 한 표현을 RGB 공간, 회색 공간, CMYK 공백이 나 장치 독립적인 공간 것입니다. 

[![](wide-color-images/color02.png "색 구성 요소 수 수 라고도 색 채널")](wide-color-images/color02.png#lightbox)

### <a name="color-primaries"></a>원색

원색을 비교 하 고 색을 계산 하는 데 사용 되는 좌표계를 제공 합니다. 일반적으로 원색 색 채널 내에서 생성 될 수 있는 지정된 된 색의 가장 강한 버전을 대체 합니다.

[![](wide-color-images/color01.png "원색을 비교 하 고 색을 계산 하는 데 사용 되는 좌표계를 제공 합니다.")](wide-color-images/color01.png#lightbox)

위에 표시 되는 RGB 색 공간에서는 경우 색 주 복제본의 위치를 `1.0` 좌표는 고정 (같은 `[1.0, 0.0, 0.0]` red에 대 한).

### <a name="color-gamut"></a>색 범위

색 범위는 모든 주어진 색 공간 내에서 개별 색 채널을 조합으로 정의 될 수 있는 색 가리킵니다.

[![](wide-color-images/color03.png "색 색상 영역 예제")](wide-color-images/color03.png#lightbox)

## <a name="what-is-wide-color"></a>와이드 컬러 란

광범위 한 색의 항목을 다루기 전에 현재 업계 표준 RGB 색 공간 (sRGB) 색에 대 한 표준에 대 한 논의 했습니다 해야 합니다. 현재 컴퓨팅 환경에서 가장 널리 사용 되는 색 공간 이며 iOS 및 macOS에 대 한 기본 색 공간입니다.

SRGB 색 공간에는 다음 속성이 있습니다.

- Itu-r BT.709 표준에 기반 합니다.
- 여기에 대략적인 감마 2.2 있습니다.
- 일반적인 조명 조건을 (d 65)를 나타냅니다.

SRGB 널리 업계에서 사용, 개발자 수 몇 가지 가정을 하는 때문에 표시 되는 모든 장치에서 지정 된 색을 충실히 표시 됩니다. 그러나 이렇게 하지 않을 경우. 또한 sRGB 색 공간에 맞지 않고 따라서를 표현할 수 없는 것을 여러 색 있습니다.

예를 들어, 많은 섬유는 많은 잉크 및 잉크나 염료 sRGB 외부 대체를 사용 하 여 스레드를 사용 하 여 설계 되었습니다. 또한 사용자는 일상 생활에서 발생 하는 여러 개의 제품 범위 sRGB 색 공간에 속하지 않는 밝은, 선명한 색을 사용 하 여 생성 됩니다. SRGB 표현할 수 없는 색의 대표적인 예 중 일부는 노을, autumn 리프, 특이 한 flowers 열 대 곧바로 등 본질적으로 작업 합니다.

사용자가 디지털 이미지를 원시 형식으로 캡처 되었습니다 sRGB 색 공간에서에서 적절 하 게 표현할 수 없습니다 하 고 따라서 없습니다 제대로 화면에 표시 되는 경우에 모든 색 데이터를 포함 하는 장치의 이미지를 있을 수 있습니다.

### <a name="the-display-p3-color-space"></a>디스플레이 P3 색 공간

2015 년 Apple sRGB 색 공간을 만들어 문제를 처리 하는 새 디스플레이 P3 색 공간을 제공 하는 새 제품 (iMac 및 iPad Pro 9.7 인치)를 해제 합니다.

[![](wide-color-images/color04.png "새 디스플레이 P3 색 공간")](wide-color-images/color04.png#lightbox)

디스플레이 P3 색 공간에는 다음 속성이 있습니다.

- 최신 하드웨어 플랫폼에 대 한 광범위 한 색 공간을 지원합니다.
- SMPTE DCI-P3 표준 기반으로 합니다. DCI P3 디지털 프로젝터 용 디자인 되었지만 모니터링을 지원 하기 위해 Apple에서 수정 되었습니다.
- 여기에 대략적인 감마 2.2 있습니다.
- 일반적인 조명 조건을 (d 65)를 나타냅니다.

Apple에 따라 사용자를 이동 하는 해당 워크플로 모바일 플랫폼에 해당 합니다. SRGB 이상의 와이드 컬러 디스플레이 포함 하는 데 필요한 전문 모바일 장치 (iPad 전문가)에 제공한 있는 색 프레젠테이션 문제를 해결 합니다. 솔루션 중 하나 개별 장치는 공장 인지 확인 하는 장치에 컬러 디스플레이가 정확 하 고 일관 된에서 보정 되어 있도록 팩터리 보정을 업그레이드 하는 것 이었습니다.

다른 솔루션은 Apple iOS 10과 macOS Sierra에 빌드되어 있는 완전 한 시스템 색 관리 포함 합니다. 

### <a name="the-extended-range-srgb-color-space"></a>확장 범위 sRGB 색 공간

새 iOS 10 시스템 색 관리 만들어지고 sRGB 미세 조정 하는 기존 iOS 앱의 모든 계정에 있습니다. 이러한 기존 앱의 두 색 표현 또는 앱 성능 영향을 하지 않은 되도록 설계 되었습니다.

이 상황을 처리 하려면 Apple가 iOS 10에서에서 확장 범위 sRGB 색 공간 (및 macOS Sierra도)에 포함 됩니다.

확장 범위 sRGB 색 공간에는 다음 속성이 있습니다.

- 모든 동일한 sRGB 주 복제본에 있습니다.
- 여기에 대략적인 감마 2.2 있습니다.
- 일반적인 조명 조건을 (d 65)를 나타냅니다.
- 음수 값을 지원 하 고 값 1 (1)을 초과 합니다.

0 보다 작은 값에 대 한 허용 및 1 보다 큰 색 공간을 사용 하면 기존 앱을 표시 하지 않고 성능 적중 횟수 또는 왜곡 하는 sRGB 색에 대 한 확장 범위 sRGB 수를 표시 하는 내부 색을 나타내는 색 공간 스펙트럼입니다. 이 모든 작업은 여전히 동일한 앵커 지점의 srgb 색 공간을 유지 하는 동안 수행 됩니다.

### <a name="extended-range-srgb-in-action"></a>실행 중인 확장된 범위 sRGB

확장 범위 sRGB 색 공간에서에서 값 0과 1 외부에서 작동 하는 방법을 보려면, 다음 예제에서는 수행 된 디스플레이 P3 색 공간에서 사용 가능한 가장 높은 채도 빨간색의:

[![](wide-color-images/color05.png "확장 범위 sRGB 색 공간에서에서 값 0과 1 외부에서 작동 하는 방법")](wide-color-images/color05.png#lightbox)

디스플레이 P3이이 색으로 나타낼 수는 `[1.0, 0.0, 0.0]` 것이 확장 범위 sRGB 및 `[1.358, -0.074, -0.012]`합니다. SRGB 값 가득 찼기 때문에 디스플레이 P3 내에서 포함 된 및 디스플레이 P3 값 레이아웃 "외부" sRGB 범위입니다.

극단적인 긍정 극단적인 음수 값으로 이동할 픽셀 값을 허용 하는 실제 하드웨어에 대 한 가시 스펙트럼에서 사용할 수 있는 색을 표시할 수 있습니다 및 확장 범위 sRGB 색 공간에서에서 이러한 값을 나타낼 수 있습니다.

### <a name="device-pixel-formats"></a>장치 픽셀 형식 

색 공간을 크게 되었습니다 sRGB 색 채널당 8 비트 대부분 이므로 8 비트 픽셀 형식으로 표준화 sRGB 색에 설명 하는 데 충분 합니다. 이 변경은 완벽 한 아니라 적당 하 고 이미지를 표시 하는 메모리 및 프로세서 사용 간의 적절 한 균형을 제공 합니다.

디스플레이 P3 sRGB 색 공간 밖에 색 좌표를 나타내는 16 비트를 올바르게 확장 범위 sRGB 색 공간을 사용 하 여 색을 나타내는 색 채널당 필요 합니다.

## <a name="system-wide-wide-color-support"></a>시스템 차원의 와이드 컬러 지원

광범위 한 색 및 iOS 10 내 멋지고 macOS Sierra 완벽 하 게 지원 하려면 Apple에 확장 범위 sRGB 색 공간을 완전히 활용 및 디스플레이 P3 되려면 다음 프레임 워크를 확장 합니다.

- UIKit (예: iOS만 해당)
- SceneKit
- 핵심 그래픽
- ImageIO
- Core 이미지
- WebKit
- SpriteKit
- 코어 애니메이션
- (MacOS만 해당)에 대 한 AppKit

또한 확장 범위 sRGB 색 공간에 대 한 지원이 향상 되었습니다 레 티 나 디스플레이 및 디스플레이 P3 표시 합니다.

광범위 한 색은 지원 및 다음 응용 프로그램 콘텐츠 형식에서 사용할 수 있습니다.

- 앱 번들에 포함 된 정적 이미지 리소스입니다.
- 문서 및 네트워크 기반 이미지 리소스입니다.
- Live 사진 또는 원시 형식의 이미지와 같은 고급 미디어입니다.
- 3D 그래픽 셰이더 텍스처 이미지입니다.

## <a name="solving-the-color-problem"></a>색 문제 해결

앱에서 표시 되는 콘텐츠는 다양 한 색을 갖춘 소스에서에서 가져올 수 있습니다. 또한 자신의 다양 한 색 표시 기능을 사용 하 여 각 장치의 광범위 한 범위에서이 콘텐츠를 표시할 수 있습니다.

IOS 10 앱의 차이 새 기본 제공 되는 시스템 차원의 색 관리 시스템을 사용 하 여 이러한 두 가지 문제를 연결 합니다. 이 시스템 이미지에서 인코딩된 이미지 색 공간에 관계 없이 모든 iOS 장치에서 동일한 표시 되는지 확인 합니다.

색 관리 해당된 색 공간 (또는 색 프로필)에 있는 모든 이미지를 사용 하 여 시작 합니다. 이 정보를 사용 합니다 _색 일치 하는 프로세스_ 출력 장치에서 색을 원본 이미지의 색 일치 하는 합니다. 이미지의 모든 픽셀 색 일치 되도록 해야, 하므로 시간이 오래 걸릴 수 고 수 장치의 CPU에 부담을 배치 합니다.

특성으로 인해 합니다 _색 일치 하는 프로세스_,이 변환은 손실 될 수 있는"" 출력 장치에 있는 경우 소스 이미지 보다 더 작은 영역 수 있습니다.

계산에 포함 된 다행 스럽게도 합니다 _색 일치 하는 프로세스_ 쉽게 하드웨어 속도가 빨라질 수 있습니다 (중 하나에 CPU 또는 GPU) 및 Apple 지원 Quartz 2D와 같은 기본 시스템에 구축 하 여 자동으로 작동 하는지 확인 ColorSync 및 핵심 애니메이션 합니다. 올바르게 태그가 지정 된 콘텐츠에 대 한 코딩 해야 이러한 기능을 활용 합니다.

색 관리는 다음과 같이 각 플랫폼에서 지원 됩니다에:

- **macOS** -macOS 이래로 관리 되는 색 되었습니다.
- **iOS** -iOS는 iOS 9.3 이상 이후 자동 색 관리를 지원 합니다.

### <a name="designing-for-wide-gamut"></a>멋지고 디자인

Apple에 디자인에 대 한 다음 제안 및 너비를 사용 하 여 색, iOS 및 macOS 앱의 멋지고 이미지 콘텐츠:

- 만 멋지고 콘텐츠를 사용 하 여 확인에서 위치는 자동으로 사용할 수 없습니다 어디서 나 앱에서 감지 합니다.
- 만 사용 하 여 멋지고 콘텐츠 선명한 색 사용자 환경을 향상 시키는 됩니다.
- 기존 앱에 대 한 P3에 모든 콘텐츠를 변경 하는 데 필요한 아닙니다.

Apple의 도구 체인을 사용 하면 와이드 컬러 앱에서 지원 되지 않습니다는 이것 아니면 저것 인 하므로 점진적으로 옵트인, 멋지고 이미지 콘텐츠에 대 한 지원.

### <a name="upgrading-existing-content-to-wide-color"></a>기존 콘텐츠 광범위 한 색으로 업그레이드

Apple에 광범위 한 색을 기존 이미지 콘텐츠를 업그레이드 하기 위한 다음 제안에 있습니다.

- 방금 "에 할당 안 함" P3 프로필 이미지 편집 앱의에서 콘텐츠입니다. 이렇게 기존 색 색 이미지를 변경 하므로 새 공간에 맞게 늘이기 같은 예기치 않은 결과 함께 새 색 공간에 콘텐츠 다시 매핑할 하기만 하면 됩니다.
- 이미지 콘텐츠를 앱을 편집 하는 이미지를 사용 하 여 디스플레이 P3 프로필 "변환" 되지는 해야 합니다.

### <a name="file-formats-and-color-profiles"></a>파일 형식 및 색 프로필

Apple 파일 형식 및 앱의 와이드 컬러 이미지 콘텐츠에 사용 되는 색 프로필에 대 한 다음 제안에 있습니다.

- RGB 작업 공간에 대 한 "디스플레이 P3" 색 프로필을 사용 합니다.
- 색 채널 모드 당 16 비트를 사용 합니다.
- 늦게 2015 iMac을 사용 하 여 (또는 이상)를 정확 하 게 이미지 콘텐츠를 미리 보려면.
- 포함 된 "디스플레이 P3" ICC 프로필을 사용 하 여 16 비트 PNG 파일로 이미지 자산을 내보냅니다.

> [!IMPORTANT]
> 사용 하는 **웹에 대 한 저장** 또는 **내보내기 자산** 소프트웨어를 편집 하는 가장 인기 있는 이미지에 기능을 찾을 _것입니다_ 되지 않은이 기능이 있으므로 와이드 컬러 이미지에 대 한 작업 아직 필요한 파일 형식 사양을 지원 하도록 업데이트 합니다.

### <a name="supporting-wide-color-with-asset-catalogs"></a>자산 카탈로그를 사용 하 여 와이드 컬러 지원

IOS 10과 macOS Sierra에서 Apple 와이드 컬러를 지원 하기 위해 자산 카탈로그를 포함 하 고 앱의 번들에 정적 이미지 콘텐츠를 분류 하는 데 확장 되었습니다.

사용 하 여 자산 카탈로그 앱에 다음과 같은 이점을 제공 합니다.

- 정적 이미지 자산에 대 한 가장 좋은 배포 옵션을 제공합니다.
- 자동 색 보정을 지원합니다.
- 이러한 자동 픽셀 형식 최적화를 지원 합니다.
- 앱 및 앱 관련 get 된 콘텐츠만 최종 사용자의 장치에 배달 되도록 보장 하는 축소 지원 합니다.

Apple 와이드 컬러 지원에 대 한 자산 카탈로그에 다음과 같은 향상 기능이 했습니다.

- 16 비트 색 채널당 원본 콘텐츠를 지원합니다.
- 카탈로그 콘텐츠가 표시 영역으로 지원합니다. 콘텐츠는 sRGB 또는 디스플레이 P3 색상에 대 한 태그를 지정할 수 있습니다.

개발자가 자신의 앱에서 지원 되는 광범위 한 색 내용에 대 한 세 가지 옵션:

1. **아무 작업도 수행** -이 요구 사항은 외부에서 내용으로 두어야 와이드 컬러 콘텐츠 앱 선명한 색 (여기서는 사용자의 환경을 개선)를 표시 해야 하는 경우에만 사용 해야, 하므로-됩니다. 모든 하드웨어 상황에서 올바르게 렌더링 되어야 하는 것이 계속 됩니다.
2. **디스플레이 P3를 업그레이드 하는 기존 콘텐츠** -개발자가 자산 카탈로그의 기존 이미지 콘텐츠를 P3 형식으로 업그레이드 된 새 파일을 바꾸고 그러한 자산 편집기에서 태그가 필요 합니다. 빌드 시 sRGB 파생 이미지를 이러한 자산에서 생성 됩니다.
3. **자산 콘텐츠 액세스에 최적화 된 제공** -이 경우 개발자는 8 비트 sRGB 및 16 비트 디스플레이 P3 비전 (제대로 자산 편집기에서 태그 지정)은 자산 카탈로그의 각 이미지의 제공 됩니다.

### <a name="asset-catalog-deployment"></a>자산 카탈로그 배포

다음은 개발자가 와이드 컬러 이미지 콘텐츠를 포함 하는 자산 카탈로그를 사용 하 여 앱 스토어에 앱을 제출 하는 경우 발생 합니다.

- 최종 사용자에 게 앱을 배포 하면 앱 조각화 하면 사용자의 장치에 적절 한 콘텐츠 variant를 배달 합니다.
- 와이드 컬러를 지원 하지 않는 장치에서 비용은 없습니다 페이로드 와이드 컬러 콘텐츠 포함에 대 한 대로 장치에 제공 되지 않습니다.
- `NSImage` macos Sierra (이상) 하드웨어의 표시에 대 한 최상의 콘텐츠 표현을 자동으로 선택 됩니다.
- 표시 된 장치 하드웨어 디스플레이 특성 변경 하는 경우 자동으로 새로 고칠 수 됩니다.

### <a name="asset-catalog-storage"></a>자산 카탈로그 저장소

자산 카탈로그 저장소에는 다음 속성 및 앱에 미치는 영향에 있습니다.

- 빌드 시 Apple를 통해 고품질 이미지 변환은 이미지 콘텐츠 저장소를 최적화 하려고 합니다.
- 16 비트 와이드 컬러 콘텐츠에 대 한 색 채널당 사용 됩니다.
- 콘텐츠를 배달할 콘텐츠 크기를 줄이는 종속 이미지 압축을 사용 합니다. 새 "손실" 압축 콘텐츠 크기를 최적화 하기 위해 추가 되었습니다.

## <a name="colors-in-user-interfaces"></a>사용자 인터페이스의 색

사용자 인터페이스의 색을 사용 하는 경우 대부분 화면의 픽셀의 단색의 경우 또한 대부분 이러한 픽셀의 이미지 자산에서 제공 되지 않지만 앱에서 직접 (또는 앱을 대신 하 여 OS) 그려집니다.

다양 한 색상 UI 수준에서 작업 하는 경우 여러 가지 문제를 제공할 수 있습니다.:

- **색 통신** -디자이너와 개발자는 일반적으로 간 색에 대해 이야기 하는 경우는 _가정_ sRGB 색 공간 관련 됩니다. 색으로 전달할 수 있도록 `rgb(128, 45, 56)` 또는 `#FF0456`합니다. 멋지고 디자인에서는 이러한 표현이 정확 하 게 지정된 된 색을 나타내는 데 충분 한 정보를 제공 하지 않습니다, 그리고 색 공간 작업 포함 되어야 합니다. Apple에서 사용 하 여 제안 `P3(128, 45, 56)` 고 `P3#FF0456` 대신 합니다. 
- **색 선택** -대부분의 해당 기본 제공 색 선택기를 사용 하는 경우 동일한 제약이 sRGB 색 공간에서 저하 되는 소프트웨어 디자인 하 고 인기 있는 이미지 편집 합니다. 디자이너가 지 색 선택에서 "디스플레이 P3" 색 공간에서 광범위 한 색 디자인을 사용 하 여 작업 하는 경우 확인 해야 합니다.
- **색 코딩** -두 `NSColor` (macOS) 및 `UIColor` (iOS 및 tvOS) P3 색을 직접 생성 하는 것에 대 한 새 편의 메서드 있고 둘 다는 확장 범위 sRGB 색 공간 뿐만에서 색을 지원 하도록 확장 되었습니다.
- **색 저장** -앱의 문서에 저장 멋지고 색 때 주의 기울여야 합니다. 여기서 sRGB 색 공간에 대 한 정상적으로 작동 하는 색 채널당 8 비트, 16 비트 색 채널당 와이드 색에 사용할 해야 합니다. 개발자는 또한 감시 해야 인스턴스의 맡은 색 공간 (되었으므로 모든 기존의 sRGB만).

## <a name="colors-on-the-web"></a>웹에서 색

웹 페이지 및 광범위 한 색 표시를 지 원하는 장치에서 광범위 한 색을 사용 하 여 작업 하는 경우 주의 해야 합니다. 모든 웹 사이트에 포함 된 이미지 콘텐츠가 적절 하 게 태그가 지정 된, 하는 경우 iOS 및 macOS 자동으로 일치 색를 올바르게 표시 합니다.

미디어 쿼리에 P3 및 sRGB 지원 장치 간에 자산을 해결 하는 데 사용할 수도 있습니다.

```xml
<picture>
    <source srcset="monkey-p3.jpg" media="(color-gamut: p3)">
    <source srcset="monkey-rpg.jpg">
</picture>
```

Apple에 CSS를 맡은 sRGB 공간 외에 다른 색 공간에서는 지정할 수 있도록 WebKit 제안이 있습니다.

## <a name="rendering-off-screen-images-in-app"></a>앱에서 화면 외부 이미지를 렌더링합니다.

만들려는 앱의 유형에 따라 해당 이미지 콘텐츠는 인터넷에서 수집한 만들거나 (예: 벡터 앱을 예를 들어 드로잉) 앱 내에서 직접 이미지 콘텐츠를 포함 하도록 사용자를 수 있습니다.

두이 경우 모두 앱 렌더링할 수는 필요한 이미지 iOS 및 macOS에 추가 된 향상 된 기능을 사용 하 여 광범위 한 색에서 화면 밖에.

### <a name="drawing-wide-color-in-ios"></a>IOS에서 광범위 한 색을 그리기

IOS 10에서에서 와이드 컬러 이미지를 올바르게 그리는 방법에 설명 하기 전에 다음 일반적인 iOS 그리기 코드를 살펴보십시오.

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

해결 해야 하는 표준 코드를 사용 하 여 문제가 _하기 전에_ 와이드 컬러 이미지를 그릴 수 있습니다. `UIGraphics.BeginImageContext (size)` iOS 이미지 그리기를 시작 하는 데 사용 되는 메서드는 다음과 같은 제한이 있습니다.

- 이미지 컨텍스트가 색 채널당 8 비트 이상을 사용 하 여 만들 수 없습니다.
- 확장 범위 sRGB 색 공간에서에서 색을 나타낼 수 없습니다.
- 백그라운드에서 호출 되는 하위 수준 C 루틴으로 인해 비 sRGB 색 공간에서 컨텍스트를 만들기에 대 한 인터페이스를 제공할 수가 없습니다.

이러한 한계를 처리 하 고 iOS 10에서에서 와이드 컬러 이미지를 그리기를 하려면 다음 코드를 대신 사용 합니다.

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

새 `UIGraphicsImageRenderer` 클래스 확장 범위 sRGB 색 공간을 처리할 수 있는 새 이미지 컨텍스트를 만들고 다음 기능이 포함 되어 있습니다.

- 기본적으로 관리 되는 색을 완벽 하 게 합니다.
- 기본적으로 확장 범위 sRGB 색 공간을 지원합니다.
- 지능적으로 sRGB 또는 확장 범위 sRGB 색 공간을 앱에서 실행 되는 iOS 장치 기능에 따라 렌더링 해야 하는 경우를 결정 합니다.
- 완벽 하 게 하 고 자동으로 관리 이미지 컨텍스트 (`CGContext`) 수명 개발자 호출에 걱정 하지 않아도 되므로 시작 및 상황에 맞는 명령을 종료 합니다.
- 호환 되는 `UIGraphics.GetCurrentContext()` 메서드.

합니다 `CreateImage` 메서드는 `UIGraphicsImageRenderer` 클래스 와이드 컬러 이미지를 만드는 호출 되 고 완료 처리기에 그릴 이미지 컨텍스트를 사용 하 여 전달 합니다. 다음과 같이이 완료 처리기 내에서 수행 됩니다 모든 그리기.

- `UIColor.FromDisplayP3` 메서드 새 채도가 빨간색 멋지고 디스플레이 P3 색 공간에서 만들고 사각형의 처음 절반을 그리는 데 사용 됩니다. 
- 두 번째 사각형의 절반을 그리기 일반 sRGB 완벽 하 게 포화 비교에 대 한 빨간색으로 표시 합니다.

### <a name="drawing-wide-color-in-macos"></a>MacOS의 광범위 한 색을 그리기

`NSImage` 클래스에서 macOS Sierra 와이드 컬러 이미지의 그리기를 지원 하도록 확장 되었습니다. 예를 들어:

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

## <a name="rendering-on-screen-images-in-app"></a>앱에서 이미지 렌더링 화면

와이드 컬러 이미지를 화면의 렌더링 하려면 프로세스 위에 표시 되는 iOS 및 macOS에 대 한 오프 스크린 와이드 컬러 이미지를 그릴 비슷하게 작동 합니다.

### <a name="rendering-on-screen-in-ios"></a>IOS에서 화면의 렌더링

앱을 iOS에서 화면의 와이드 컬러에서 이미지를 렌더링 해야 하는 경우 재정의 `Draw` 메서드는 `UIView` 평소 대로 해당 합니다. 예를 들어:

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

IOS 10와 마찬가지로 합니다 `UIGraphicsImageRenderer` 지능적으로 sRGB 또는 확장 범위 sRGB 색 공간 앱이 실행 하는 경우에 iOS 장치 기능에 따라 렌더링 해야 하는 경우 결정 위에 표시 된 클래스는 `Draw` 메서드가 호출 됩니다. 또한는 `UIImageView` 색도 iOS 9.3 이후 관리 되었습니다.

앱에서 렌더링이 수행 되는 방법을 알아야 할 경우는 `UIView` 또는 `UIViewController`, 새 확인할 수 있습니다 `DisplayGamut` 의 속성을 `UITraitCollection` 클래스입니다. 이 값은 한 `UIDisplayGamut` 열거형 중:

- P3
- Srgb
- 지정되지 않음

앱에서 색 공간 이미지를 그리는 데 사용 되는 제어 하려는 경우 새 사용할 수 있습니다 `ContentsFormat` 의 속성을 `CALayer` 원하는 색 공간을 지정 합니다. 이 값은 한 `CAContentsFormat` 열거형 중:

- Gray8Uint
- Rgba16Float
- Rgba8Uint

### <a name="rendering-on-screen-in-macos"></a>MacOS에서 화면 렌더링

앱을 macOS에서 화면의 광범위 한 색에서 이미지를 렌더링 해야 하는 경우 재정의 `DrawRect` 메서드는 `NSView` 평소 대로 해당 합니다. 예를 들어:

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

마찬가지로 지능적으로 결정 sRGB 또는 확장 범위 sRGB 색 공간 앱이 실행 하는 경우에 Mac 하드웨어의 기능에 따라 렌더링 해야 하는 경우는 `DrawRect` 메서드가 호출 됩니다.

앱에서 색 공간 이미지를 그리는 데 사용 되는 제어 하려는 경우 새 사용할 수 있습니다 `DepthLimit` 의 속성을 `NSWindow` 원하는 색 공간을 지정 하는 클래스입니다. 이 값은 한 `NSWindowDepth` 열거형 중:

- OneHundredTwentyEightBitRgb
- SixtyfourBitRgb
- TwentyfourBitRgb

## <a name="summary"></a>요약

있습니다 수 구현를 Xamarin.iOS 또는 Xamarin.Mac 앱 내에서 사용 되는 방법 및 광범위 한 색이 문서이에서는 설명 했습니다.



## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://developer.xamarin.com/samples/ios/iOS10/)
