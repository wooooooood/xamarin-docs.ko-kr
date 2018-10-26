---
title: Android의 Xamarin Workbooks 문제 해결
description: 이 문서에서는 Android의 Xamarin Workbooks를 사용 하 여 작업에 대 한 문제 해결 팁을 제공 합니다. 에뮬레이터 지원, 로드 되지 않습니다는 통합 문서 및 기타 항목에 설명 합니다.
ms.prod: xamarin
ms.assetid: F1BD293B-4EB7-4C18-A699-718AB2844DFB
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: a93288829ff99027a4b33e7720a7f849df37e9b1
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112613"
---
# <a name="troubleshooting-xamarin-workbooks-on-android"></a>Android의 Xamarin Workbooks 문제 해결

## <a name="emulator-support"></a>에뮬레이터 지원

Android는 통합 문서를 실행 하려면 Android 에뮬레이터를 사용 하 여 사용할 수 있어야 합니다. 물리적 Android 장치는 지원 되지 않습니다.

컴퓨터에서 지 원하는 경우 Google 에뮬레이터를 HAXM 함께 좋습니다.
Hyper-v 시스템에서 사용 하도록 설정 해야 하는 경우 대신 Visual Studio Android Emulator를 사용 하 여 이동 합니다.

5.0 이상을 Android를 실행 하는 에뮬레이터가 있어야 합니다. ARM 에뮬레이터 지원 되지 않습니다. 사용 하 여 `x86` 또는 `x86_64` 장치에만 해당 합니다.

읽어보세요 [Android 에뮬레이터 설정 설명서] [ android-emu] 프로세스에 익숙해 없는 경우.

> [!NOTE]
> 1.1 및 이전 버전의 통합 문서는 시도 실패! ARM 에뮬레이터를 사용 하 여 사용할 수 있는 경우. 해결 하려면 열거나 Android 통합 문서를 만들기 전에 선택한이, 시작 x86 에뮬레이터. 통합 문서를 호환 되는 것으로 실행 중인 에뮬레이터에 연결할 항상 선호 됩니다.

## <a name="workbooks-wont-load"></a>통합 문서를 로드 하지 않습니다.

### <a name="workbook-window-spins-forever-never-loads-windows"></a>통합 문서 창을 회전 하지 않습니다 (Windows) 로드 영원히

먼저, 에뮬레이터에서 에뮬레이터의 웹 브라우저에서 웹 사이트를 테스트 하 여 완벽 하 게 작업 네트워크 액세스할 수 있는지 확인 합니다.

### <a name="visual-studio-android-emulator-cannot-connect-to-the-internet"></a>Visual Studio Android Emulator는 인터넷에 연결할 수 없습니다.

에뮬레이터에 대 한 네트워크 액세스에 없는 경우 Hyper-v 네트워크 스위치를 해결 하려면 다음이 단계를 수행 해야 합니다. Wi-fi 네트워크에 자주 간을 전환 하면이 작업을 정기적으로 반복 해야 할 수 있습니다.

0. **모든 중요 한 네트워크 작업이 완료 되 면 인터넷에서 Windows를 일시적으로 분리 될 수 있습니다이 대로 있는지 확인 합니다.**
1. 에뮬레이터를 닫습니다.
2. `Hyper-V Manager`를 엽니다.
3. 아래 `Actions`열고 `Virtual Switch Manager...`합니다.
4. 가상 스위치를 모두 삭제 합니다.
5. `OK`을 클릭합니다.
6. VS Android 에뮬레이터를 시작 합니다. 아마도 가상 네트워크 스위치를 다시 하 라는 메시지가 표시 됩니다.
7. VS Android Emulator 브라우저 인터넷에 액세스할 수 있는지 테스트 합니다.

[android-emu]: https://developer.xamarin.com/guides/android/deployment,_testing,_and_metrics/debug-on-emulator/


## <a name="related-links"></a>관련 링크

- [버그 보고](~/tools/workbooks/install.md#reporting-bugs)
