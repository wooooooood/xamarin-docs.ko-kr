---
title: Xamarin Hot Restart
description: 이 문서에서는 Xamarin Hot Restart를 설정해 iOS 앱 디버깅에 사용하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 6BC62A88-9368-41BB-8494-760F2A4805DB
ms.technology: xamarin-forms
author: jimmgarrido
ms.author: jigarrid
ms.date: 01/14/2020
ms.openlocfilehash: 1f87fffe99656cdc0d0bf0f0178413740a20aa75
ms.sourcegitcommit: e9d88587aafc912124b87732d81c3910247ad811
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78337278"
---
# <a name="xamarin-hot-restart-preview"></a>Xamarin Hot Restart(미리 보기)

![미리 보기 기능](~/media/shared/preview.png)

Xamarin Hot Restart는 다중 파일 코드 편집, 리소스 및 참조 등의 개발 과정에서 앱의 변경 사항을 신속히 테스트하는 데 도움이 됩니다. 디버그 대상의 기존 앱 번들로 새 변경 사항을 푸시하여 빌드 및 배포 주기에 속도를 한층 더하게 됩니다.

> [!IMPORTANT]
> Xamarin 핫 다시 시작은 현재 Visual Studio 2019 버전과 16.5 미리 보기 버전에서 사용할 수 있으며 Xamarin.Forms를 이용 중인 iOS 앱을 지원합니다. Mac용 Visual Studio 및 비Xamarin.Forms 앱에 대한 지원은 현재 로드맵에 포함되어 있습니다.

## <a name="requirements"></a>요구 사항

- Visual Studio 2019 버전 16.5 미리 보기 3
- iTunes(64비트)
- Apple 개발자 계정


## <a name="initial-setup"></a>초기 설정

> [!NOTE]
> Xamarin Hot Restart는 미리 보기로 제공되는 동안 기본적으로 비활성화되어 있습니다. **도구 > 옵션 > 환경 > 미리 보기 기능 > Xamarin Hot Restart 사용**에서 이를 사용하도록 설정할 수 있습니다.

1. iOS 프로젝트가 시작 프로젝트로 설정되어 있고 빌드 구성이 **Debug|iPhone**으로 설정되어 있는지 확인합니다.

   1. 이것이 기존 프로젝트일 경우엔 **빌드 > 구성 관리자…** 로 이동합니다. 그리고 **배포**가 iOS 프로젝트에 대해 활성화되어 있는지 확인합니다.

2. 도구 모음에서 **로컬 디바이스**를 선택한 후 클릭하여 설치 마법사를 시작합니다.

    [![](hot-restart-images/toolbar.png "Screenshot of the Visual Studio toolbar with local device set as the debug target.")](hot-restart-images/toolbar.png)

3. iTunes가 설치되지 않았다면 **iTunes 다운로드**를 클릭하여 설치 관리자를 다운로드합니다. iTunes 설치가 완료되면 **다음**을 클릭합니다.

4. iOS 디바이스를 컴퓨터에 연결합니다. 디바이스가 감지되면 마법사에 그 이름이 표시됩니다. **다음**을 클릭합니다.

5. Apple 개발자 계정 자격 증명을 입력하고 **다음**을 클릭합니다.

6. 프로젝트에서 [자동 프로비저닝](~/ios/get-started/installation/device-provisioning/automatic-provisioning.md)을 사용하도록 설정하려면 드롭다운 메뉴를 사용하여 개발 팀을 선택합니다. **마침**을 클릭합니다.

> [!NOTE]
> 자동 프로비저닝은 iOS 기기의 배포 구성이 간편해질 수 있다는 장점으로 사용이 권장됩니다. 물론 적합한 프로비저닝 프로필이 있다면 이를 사용하지 않고 수동 프로비저닝을 계속 사용할 수도 있습니다.

## <a name="use-xamarin-hot-restart"></a>Xamarin Hot Restart 사용
초기 설치 후에는 연결된 디바이스가 디버그 대상 드롭다운 메뉴에 표시됩니다. 앱을 디버그하려면 드롭다운에서 디바이스를 선택하고 **실행** 단추를 클릭합니다. 디버그 세션을 시작하기 위해 디바이스에서 앱을 수동으로 시작하라는 메시지가 Visual Studio에 표시될 수 있습니다.

디버깅하는 동안 코드 파일을 편집한 다음 디버그 도구 모음에서 **다시 시작** 단추를 누르거나 **Ctrl+Shift+F5**를 눌러 새 변경 사항이 적용된 상태로 디버그 세션을 다시 시작할 수 있습니다.

[![](hot-restart-images/restart.png "Screenshot of the debug toolbar with the restart button highlighted.")](hot-restart-images/toolbar.png)

`HOTRESTART` 전처리기 기호를 사용하여 Xamarin Hot Restart로 디버깅할 때 특정 코드가 실행되지 않도록 할 수도 있습니다.

## <a name="limitations"></a>제한 사항
- 현재는 Xamarin.Forms 및 iOS 기기로 빌드된 iOS 앱만 지원됩니다.
- 스토리보드 및 XIB 파일은 지원되지 않으며 런타임에 로드하려고 하면 앱이 충돌할 수도 있습니다. `HOTRESTART` 전처리기 기호를 사용하여 이 코드가 실행되지 않도록 합니다.
- 정적 iOS 라이브러리 및 프레임워크는 지원되지 않으며 앱에서 이러한 라이브러리 및 프레임워크를 로드하려고 하면 런타임 오류가 발생하거나 충돌이 발생할 수 있습니다. `HOTRESTART` 전처리기 기호를 사용하여 이 코드가 실행되지 않도록 합니다. 동적 iOS 라이브러리가 지원됩니다.
- Xamarin Hot Restart는 게시용 앱 번들을 만드는 데에 사용할 수 없습니다. 프로덕션으로의 애플리케이션 전체 컴파일, 서명 및 배포를 위해서는 Mac 컴퓨터가 계속 필요합니다.

## <a name="troubleshoot"></a>문제 해결
- iTunes는 Microsoft Store를 통해 설치된 경우 설치 마법사에 감지되지 않습니다. 이 버전을 먼저 제거한 다음 [Apple의 설치 관리자](https://go.microsoft.com/fwlink/?linkid=2101014)를 다운로드해야 합니다.
- 디바이스별 빌드를 사용하도록 설정하면 앱이 디버그 모드로 전환하지 못하는 알려진 문제가 있습니다. 해결하려면 **속성 > iOS 빌드**에서 이 빌드를 사용하지 않도록 설정하고 디버깅을 다시 시도해야 합니다. 이 문제는 향후 릴리스에서 수정됩니다.
- 앱이 디바이스에 이미 있다면 `AMDeviceStartHouseArrestService` 오류로 Hot Restart 배포 시도가 실패할 수 있습니다. 해결 방법은 디바이스에서 앱을 제거한 후 다시 배포하는 것입니다.
- Apple 개발자 프로그램에 속하지 않는 Apple ID를 입력하면 `Authentication Error. Xcode 7.3 or later is required to continue developing with your Apple ID` 오류가 발생합니다. iOS 디바이스에서 Xamarin Hot Restart를 사용하려면 유효한 Apple 개발자 계정이 있어야 합니다. 

그 외의 문제를 추가로 보고하려면 [도움말 > 의견 보내기 > 문제 보고](/visualstudio/ide/feedback-options?view=vs-2019#report-a-problem)로 이동하여 피드백 도구를 사용하시기 바랍니다.
