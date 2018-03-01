---
title: "Android에서 Xamarin 통합 문제 해결"
ms.topic: article
ms.prod: xamarin
ms.assetid: F1BD293B-4EB7-4C18-A699-718AB2844DFB
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: eb188abb3e757f6f66af7758ced311ae1236d3ce
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="troubleshooting-xamarin-workbooks-on-android"></a>Android에서 Xamarin 통합 문제 해결

## <a name="emulator-support"></a>에뮬레이터 지원

Android는 통합 문서를 실행 하려면 Android 에뮬레이터를 사용 하기 위해 제공 해야 합니다. Android 장치를 물리적 지원 되지 않습니다.

컴퓨터에서 지 원하는 경우 HAXM 함께 Google의 에뮬레이터를 좋습니다.
Hyper-v 시스템에서 사용 하도록 설정 해야 할 경우 대신 Visual Studio Android 에뮬레이터와 함께 이동 합니다.

5.0 이상 Android를 실행 하는 에뮬레이터가 있어야 합니다. ARM 에뮬레이터 지원 되지 않습니다. 사용 하 여 `x86` 또는 `x86_64` 장치에만 해당 합니다.

읽으십시오 [Android 에뮬레이터를 설정에 대 한 우리의 설명서] [ android-emu] 는 프로세스에 잘 모를 경우.

**참고:** 1.1 및 이전 버전 통합 문서는 시도 및 실패!을 ARM 에뮬레이터를 사용 하 여 사용 가능한 경우. 해결 하려면이 시작 x86 에뮬레이터 열거나 Android는 통합 문서를 만들기 전에 선택한 합니다. 통합 문서를 호환으로 실행 중인 에뮬레이터에 연결 하는 데 항상 선호 됩니다.

## <a name="workbooks-wont-load"></a>통합 문서를 로드 하지 않습니다.

### <a name="workbook-window-spins-forever-never-loads-windows"></a>회전 영원히 통합 문서 창이 되지 로드 (Windows)

첫째, 에뮬레이터에서 에뮬레이터의 웹 브라우저에서 모든 웹 사이트를 테스트 하 여 완벽 하 게 작업 네트워크 액세스할 수 있는지 확인 합니다.

### <a name="visual-studio-android-emulator-cannot-connect-to-internet"></a>Visual Studio의 Android 에뮬레이터는 인터넷에 연결할 수 없습니다.

에뮬레이터에 대 한 네트워크 액세스 하는 경우 Hyper-v 네트워크 스위치를 해결 하려면 다음이 단계를 수행 해야 합니다. Wi-fi 네트워크에 자주 간을 전환 하면이 작업을 정기적으로 반복 해야 할 수 있습니다.

0. **모든 중요 한 네트워크 작업이 완료 되 면 인터넷에서 Windows을 일시적으로 끊을 수 있습니다이 처럼 있는지 확인 합니다.**
1. 에뮬레이터를 닫습니다.
2. `Hyper-V Manager`를 엽니다.
3. 아래 `Actions`개방형 `Virtual Switch Manager...`합니다.
4. 가상 스위치를 모두 삭제 합니다.
5. `OK`을 클릭합니다.
6. VS Android 에뮬레이터를 시작 합니다. 아마도 가상 네트워크 스위치를 다시 만들 하 라는 메시지가 표시 됩니다.
7. VS Android 에뮬레이터 브라우저 인터넷에 액세스할 수 있는지 테스트 합니다.

[android-emu]: https://developer.xamarin.com/guides/android/deployment,_testing,_and_metrics/debug-on-emulator/


## <a name="related-links"></a>관련 링크

- [버그를 보고](~/tools/workbooks/install.md#reporting-bugs)
