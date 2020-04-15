---
title: 변경 내용 적용
description: Xamarin.Android 프로젝트에서 변경 내용 적용을 사용하여 시작하는 방법입니다.
ms.assetid: 38950B0C-8880-448F-B12E-08D5BFE87D0D
author: JonDouglas
ms.author: jodou
ms.date: 03/09/2020
ms.openlocfilehash: 03bace97908a53ac6112cd2600b20d429fe2f521
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "80215315"
---
# <a name="apply-changes"></a>변경 내용 적용

변경 내용 적용을 사용하면 앱을 다시 시작하지 않고도 실행 중인 앱에 리소스 변경 내용을 푸시할 수 있습니다. 이렇게 하면 디바이스 또는 에뮬레이터의 현재 상태를 유지하면서 소규모 증분 변경 내용을 배포하고 테스트하려는 경우 다시 시작되는 앱의 양을 제어할 수 있습니다.

변경 내용 적용은 Android 8.0(API 레벨 26) 이상을 실행하는 디바이스나 에뮬레이터에서 지원되는 [Android JVMTI 구현](https://docs.oracle.com/javase/8/docs/platform/jvmti/jvmti.html#bci)의 기능을 사용합니다.

## <a name="requirements"></a>요구 사항

다음 목록은 변경 내용 적용을 사용하기 위한 요구 사항입니다.

- **Visual Studio** - Windows에서 Visual Studio 2019 버전 16.5 이상으로 업데이트합니다. macOS에서 Mac용 Visual Studio 2019 버전 8.5 이상으로 업데이트합니다.
- **Xamarin.Android** - Xamarin.Android 10.2 이상은 Visual Studio와 함께 설치되어야 합니다(Xamarin.Android는 Windows에서 **.NET을 이용한 모바일 개발** 워크로드의 일부로 자동 설치되고 **Mac용 Visual Studio 설치 관리자**의 일부로 설치됨).
- **Android SDK** - Android API 28 이상은 Android SDK 관리자를 통해 설치해야 합니다.
- **대상 디바이스 또는 에뮬레이터** - 디바이스 또는 에뮬레이터에서 Android 8.0(API 레벨 26) 이상을 실행해야 합니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

## <a name="get-started"></a>시작

변경 내용 적용을 시작하려면 디바이스 또는 에뮬레이터에서 Android 8.0(API 레벨 26) 이상을 실행하고 있는지 확인해야 합니다. 그런 다음 디버깅을 사용하거나 사용하지 않고 Android 애플리케이션을 실행합니다.

그러면 다음과 같은 방법으로 변경 내용 적용을 조작할 수 있습니다.

1. **도구 모음 아이콘.** 변경 내용 적용 도구 모음 아이콘을 클릭하여 대상 디바이스 또는 에뮬레이터에 변경 내용을 적용할 수 있습니다.

    [![변경 내용 적용 - 도구 모음 아이콘](apply-changes-images/Apply-Changes-Toolbar.png)](apply-changes-images/Apply-Changes-Toolbar.png#lightbox)

2. **바로 가기 키.** 바로 가기 키 **Shift + Alt + F5**를 사용하여 대상 디바이스나 에뮬레이터에 변경 내용을 적용할 수 있습니다.
3. **디버그 메뉴.** **디버그 > 변경 내용 적용** 메뉴 항목을 사용하여 대상 디바이스 또는 에뮬레이터에 변경 내용을 적용할 수 있습니다.

    [![변경 내용 적용 - 디버그 메뉴](apply-changes-images/Apply-Changes-Debug-Menu.png)](apply-changes-images/Apply-Changes-Debug-Menu.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

## <a name="get-started"></a>시작

변경 내용 적용을 시작하려면 디바이스 또는 에뮬레이터에서 Android 8.0(API 레벨 26) 이상을 실행하고 있는지 확인해야 합니다. 그런 다음 디버깅을 사용하거나 사용하지 않고 Android 애플리케이션을 실행합니다.

그러면 다음과 같은 방법으로 변경 내용 적용을 조작할 수 있습니다.

1. **바로 가기 키.** 바로 가기 키 **⌥ + ⇧ + R**을 사용하여 대상 디바이스나 에뮬레이터에 변경 내용을 적용할 수 있습니다.
2. **실행 메뉴.** **실행 > 변경 내용 적용** 메뉴 항목을 사용하여 대상 디바이스 또는 에뮬레이터에 변경 내용을 적용할 수 있습니다.

    [![변경 내용 적용 - 디버그 메뉴](apply-changes-images/Apply-Changes-Debug-Menu-Mac.png)](apply-changes-images/Apply-Changes-Debug-Menu-Mac.png#lightbox)

-----

## <a name="limitations"></a>제한 사항

다음 변경 내용을 적용하려면 애플리케이션을 다시 시작해야 합니다.

- C# 코드 변경
- 리소스 추가 또는 제거
- AndroidManifest.xml 변경
- 네이티브 라이브러리(.so 파일) 변경

## <a name="related-links"></a>관련 링크

- [변경 내용 적용](https://developer.android.com/studio/run#apply-changes)
