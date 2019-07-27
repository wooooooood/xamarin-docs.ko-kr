---
title: Android에서 Xamarin Workbooks 문제 해결
description: 이 문서에서는 Android에서 Xamarin Workbooks를 사용 하기 위한 문제 해결 팁을 제공 합니다. 에뮬레이터 지원, 로드 되지 않는 통합 문서 및 기타 항목에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: F1BD293B-4EB7-4C18-A699-718AB2844DFB
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: 0d04b42a8d9f230c48bb09059296eb3740336dc6
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68511840"
---
# <a name="troubleshooting-xamarin-workbooks-on-android"></a>Android에서 Xamarin Workbooks 문제 해결

## <a name="emulator-support"></a>에뮬레이터 지원

Android 통합 문서를 실행 하려면 Android 에뮬레이터를 사용할 수 있어야 합니다. 물리적 Android 장치는 지원 되지 않습니다.

컴퓨터에서 지 원하는 경우 Google의 에뮬레이터가 HAXM와 함께 사용 하는 것이 좋습니다.
시스템에서 Hyper-v를 사용 하도록 설정 해야 하는 경우 대신 Visual Studio Android Emulator로 이동 합니다.

Android 5.0 이상을 실행 하는 에뮬레이터가 있어야 합니다. ARM 에뮬레이터는 지원 되지 않습니다. `x86` 또는`x86_64` 장치만 사용 합니다.

프로세스에 익숙하지 않은 경우 Android 에뮬레이터를 설정 하는 방법 [에 대 한 설명서][android-emu] 를 참조 하세요.

> [!NOTE]
> 통합 문서 1.1 및 이전 버전은 사용 가능한 경우 ARM 에뮬레이터를 사용 하 여 (및 실패!) 시도 합니다. 이 문제를 해결 하려면 Android 통합 문서를 열거나 만들기 전에 선택한 x86 에뮬레이터를 시작 합니다. 통합 문서는 항상 호환 되는 동안 실행 중인 에뮬레이터에 연결 하는 것을 선호 합니다.

## <a name="workbooks-wont-load"></a>통합 문서가 로드 되지 않음

### <a name="workbook-window-spins-forever-never-loads-windows"></a>통합 문서 창이 영원히 회전 하 고 로드 되지 않음 (Windows)

먼저 에뮬레이터의 웹 브라우저에서 웹 사이트를 테스트 하 여 에뮬레이터에 완벽 하 게 작동 하는 네트워크 액세스가 있는지 확인 합니다.

### <a name="visual-studio-android-emulator-cannot-connect-to-the-internet"></a>Visual Studio Android Emulator 인터넷에 연결할 수 없습니다.

에뮬레이터에 네트워크 액세스 권한이 없는 경우 다음 단계를 수행 하 여 Hyper-v 네트워크 스위치를 수정 해야 할 수 있습니다. Wi-fi 네트워크 간을 자주 전환 하는 경우이 작업을 주기적으로 반복 해야 할 수 있습니다.

1. **인터넷에서 Windows의 연결을 일시적으로 끊을 수 있으므로 중요 한 네트워크 작업이 완료 되었는지 확인 합니다.**
1. 에뮬레이터를 닫습니다.
1. `Hyper-V Manager`를 엽니다.
1. 에서 `Actions`를 엽니다 `Virtual Switch Manager...`.
1. 모든 가상 스위치를 삭제 합니다.
1. `OK`을 클릭합니다.
1. VS Android Emulator를 시작 합니다. 가상 네트워크 스위치를 다시 만들라는 메시지가 표시 될 것입니다.
1. VS Android Emulator의 브라우저에서 인터넷에 액세스할 수 있는지 테스트 합니다.

[android-emu]: ~/android/deploy-test/debugging/debug-on-emulator.md

## <a name="related-links"></a>관련 링크

- [버그 보고](~/tools/workbooks/install.md#reporting-bugs)
