---
title: 이전 버전의 Xcode 또는 Xamarin.iOS를 사용할 수 있습니까
description: 이 가이드에는 (현재 안정적인 릴리스) 보다 이전 버전의 Xcode 또는 Xamarin.iOS를 사용 하 여 문제를 간략하게 설명 합니다.
ms.prod: xamarin
ms.assetid: 27CF7EB7-9251-435F-BEA5-F20F8DD7DC17
ms.technology: xamarin-ios
author: chamons
ms.author: chhamo
ms.date: 04/16/2019
ms.openlocfilehash: 2a208d39454a33adc849bcccc66802361693e82e
ms.sourcegitcommit: 34819671c7910d29f018bdb394ddd4a4b0cd3a31
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59675895"
---
# <a name="can-i-use-an-older-version-of-xcode-or-xamarinios"></a>이전 버전의 Xcode 또는 Xamarin.iOS를 사용할 수 있습니까?

Xamarin 설명서 최신 Xamarin.iOS 및 권장 Xcode를 사용 하는 것으로 가정 합니다. 그러나 일부 고객은 오래 된 Xamarin.iOS 및/또는 Xcode를 사용 하려는 결과 대 한 세부 정보입니다.

다음 경고를 포함 하는 릴리스 정보:

> [!WARNING]
> **이전 Xcode 버전을 사용 하 여**
>
> 이전 Xcode 버전을 사용 하 여 (위의에 언급 된 것 보다 [요구 사항](https://docs.microsoft.com/xamarin/ios/release-notes/12/12.8#requirements)) 가능한 경우가 않지만 몇 가지 기능을 구할 수 있습니다. 또한 몇 가지 제한 사항이 해결 방법을 예를 들어 필요할 수 있습니다.
>
> - 정적 등록 기관에 최고의 응용 프로그램을 빌드하 Xcode 헤더 파일이 필요 [ `MT0091` ](https://docs.microsoft.com/xamarin/ios/troubleshooting/mtouch-errors#MT0091) 하거나 [ `MT4109` ](https://docs.microsoft.com/xamarin/ios/troubleshooting/mtouch-errors#MT4109) Api를 사용할 수 없는 경우 오류입니다. 대부분의 관리 되는 링커를 사용 하도록 설정 (API 제거) 하 여 도움이 됩니다.
> - Bitcode 빌드 (tvOS 및 watchOS)에 Xcode 9.0 이상 도구 체인 사용 되지 않은 경우 앱 스토어에 제출할을 실패할 수 있습니다.

## <a name="further-information"></a>추가 정보

최신 Xcode 및 개발 하 고 응용 프로그램을 제출 하는 경우 가장 최근의 Xamarin.iOS 릴리스를 사용 하는 것이 좋습니다. Apple 응용 프로그램을 제출 하는 경우 가장 최근의 Xcode를 사용 해야 합니다.

참고 최신 Xcode를 사용 하 여 응용 프로그램을 이전 iOS 버전 대상 지정을 방지 하지 않습니다. 지 원하는 iOS 버전을 기반으로 하 **Info.plist** 항목 및 Api 응용 프로그램에서 사용 합니다.

와 같은 다른 이름의 Xcode--나란히의 여러 버전을 설치 하는 것이 불가능 **Xcode101.app** 하 고 **Xcode102.app**합니다. 여러 버전을 사용 하는 경우 현재 Xcode 설정할 수 있는지 확인 [Mac 설정에 대 한 Visual Studio](~/ios/troubleshooting/questions/ios-sdk.md) 와 [ `xcode-select` ](https://developer.apple.com/library/archive/technotes/tn2339/_index.html#//apple_ref/doc/uid/DTS40014588-CH1-HOW_DO_I_SELECT_THE_DEFAULT_VERSION_OF_XCODE_TO_USE_FOR_MY_COMMAND_LINE_TOOLS_) [명령줄 도구](https://developer.apple.com/library/archive/technotes/tn2339/_index.html#//apple_ref/doc/uid/DTS40014588-CH1-HOW_DO_I_SELECT_THE_DEFAULT_VERSION_OF_XCODE_TO_USE_FOR_MY_COMMAND_LINE_TOOLS_)합니다.

그러나 드문 경우 이전 구성 요소를 사용을 해야 합니다. 이 문서에서는 발생할 때 일반적인 문제를 설명 하는 최신 보다 오래 된 버전을 사용 합니다.

그러나 Apple에서 각 릴리스는 고유 하며 여기에 문서화 되어 있지 않습니다. 다른 문제를 발견할 수 있습니다.

이러한 문제를 해결 하려면 가능한 최신 Xcode 및 최신 Xamarin.iOS의 지원 되는 구성에 맞도록 때마다이 특수 되기도 합니다.

## <a name="use-of-an-old-xamarinios-with-an-old-xcode"></a>이전 Xcode 사용 하 여 이전 xamarin.ios를 사용 하 여

Xamarin.iOS 및 Xcode를 업데이트 하지 가능 적어도 어느 정도의 시간에 대 한 합니다. 특정 시점에 Apple 해야 응용 프로그램을 제출 하는 Xcode의 최소 버전으로 제한이 됩니다. 이 시점에서 최신 버전 (또는 Xcode에 Apple 및 일치 하는 Xamarin.iOS 릴리스 필요한 새의 최소 버전)는 모든 구성 요소 (macOS, Xcode 및 Xamarin.iOS)을 업데이트 해야 합니다.

점진적으로 업데이트 하 고 작은 변경 내용을 따라갈 일반적으로 쉽습니다. 업데이트 수를 유지할 어려울 수 있는 대형 프로젝트 알려진된 작업 집합을 사용 하 여 유지 좋은 타협을 될 수 있습니다.

## <a name="use-of-new-xamarinios-with-older-xcode"></a>이전 Xcode 사용 하 여 새 Xamarin.iOS의 사용

Xamarin.iOS는 일반적 합리적으로 가능한 한 때마다 이전 Xcode 버전을 지원 합니다. 몇 가지 잠재적인 과제는 다음과 같습니다.

- 최신 Xamarin.iOS 일부 기능을 지원할 수 있습니다 하 고 선택한 Xcode에서 Api가 없음. 
- **정적 등록 기관** 이어지는 응용 프로그램을 빌드할 Xcode 헤더 파일이 필요 [ `MT0091` ](~/ios/troubleshooting/mtouch-errors.md#MT0091) 하거나 [ `MT4109` ](~/ios/troubleshooting/mtouch-errors.md#MT4109)' Api를 사용할 수 없는 경우 오류.
  - 대부분의 관리 되는 링커는 데 도움이 됩니다 (새 API에 대 한 관리 되는 바인딩을 제거) 하 여 사용 하지 않는 경우.
- Bitcode 빌드 (tvOS 및 watchOS)에 Xcode 9.0 이상 도구 체인 사용 되지 않은 경우 앱 스토어에 제출할을 실패할 수 있습니다.

## <a name="use-of-new-xcode-with-older-xamarinios"></a>오래 된 Xamarin.iOS 사용 하 여 새 Xcode 사용

이 사용 사례에 한층 어려운은 Xamarin.iOS 새 Xcode의 변경 요구 사항을 예측할 수 있습니다. MacOS의 업데이트 문제를 야기할 수 있습니다 하 고 호환성 패치 없이 Xamarin.iOS의 많은 부분을 영향을 받을 수 있습니다. 

잠재적인 영역 수 문제가 발생할 경우 포함 하 여 여러 가지

- 와 호환 되지 않는 `mlaunch`:
  - 제대로 (또는 모든) 시뮬레이터 지원 작동 하지 않을 수 없습니다.
  - 장치 지원에는 제대로 (또는 모든) 작동 하지 않을 수 없습니다.
- 알 수 없는 지원 `mtouch` 
  - 새 프레임 워크에 대해 지원 되지 않습니다
  - 새 자격에 대 한 지원 되지 않습니다
  - 새롭거나 업데이트 된 도구에 대 한 지원 되지 않습니다
    - 코드 서명에 영향을 줄 수 있습니다.

### <a name="new-appstore-submission-rules"></a>새 앱 스토어 제출 규칙

Apple 언제 든 지 앱 스토어 제출 규칙에 대 한 업데이트를 예약합니다. 이러한 규칙 변경 내용은 미리 공지 가끔씩만 됩니다. 이러한 변경 사항 중 일부를 지원 하기 위해 Xamarin.iOS 구성 요소를 업데이트 해야 하는 변경 내용을 도구 필요 합니다.

규칙 변경 외에도 Apple는 종종 제출 된 앱에 추가 유효성 검사를 추가 하 하거나 기존은 강화 합니다. 그 중 일부 도구 (예: 새 블랙 리스트에 올린된 기호를) 변경 해야 합니다. 이러한 많은 최초로 발생할을 제출 하는 고객이 없는 알림 (또는 목록) 규칙 없기 때문에 합니다.

## <a name="summary"></a>요약

가능 하면 안전을 다음 Apple의 지침 및 개발에 제출 하 여 앱 스토어에 릴리스된 최신 Xcode를 사용 하 여 합니다.

따라서 릴리스된 최신 Xamarin.iOS를 사용 합니다. 이 제출 된 응용 프로그램에 영향을 줄 수 있습니다 및 가장 최근의 규칙 변경 내용을 준수 하는 최신 수정 프로그램 추적.

이것이 가능 하지 않은 경우에 일치 하는 이전 Xcode 및 Xamarin.iOS 릴리스를 사용 하는 것이 좋습니다. 한 번에 작동할 수 있습니다 하지만 Apple 시점에서 최신 도구 요구 됩니다 적절 하 게 계획 합니다.
