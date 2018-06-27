---
title: 에뮬레이터 설정 문제 해결
description: 이 문서에서는 Android Device Manager를 사용할 때 발생할 수 있는 문제를 진단하고 해결하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 4F053CC9-9378-47CB-8002-978A6558C4D0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/01/2018
ms.openlocfilehash: 4cbd7d7626d114856d6c68fe7bc9feb7c2a5abc2
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34733832"
---
# <a name="troubleshooting-emulator-setup-problems"></a>에뮬레이터 설정 문제 해결

_이 문서에서는 Android Device Manager를 사용하여 Android Emulator를 구성할 때 발생할 수 있는 문제를 진단하고 해결하는 방법을 설명합니다. Android Emulator를 실행하는 동안 문제 해결에 대한 정보는 [Google Android Emulator 문제 해결](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md)을 참조하세요._

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="android-sdk-in-non-standard-location"></a>표준이 아닌 위치의 Android SDK

일반적으로 Android SDK는 다음 위치에 설치됩니다.

**C:\\Program Files (x86)\\Android\\android-sdk**

이 위치에 SDK가 설치되지 않은 경우 Android Device Manager를 실행할 때 이 오류가 나타날 수 있습니다.

![Android SDK 인스턴스 오류](troubleshooting-images/win/01-sdk-error.png)

이 문제를 해결하려면 다음을 수행합니다.

1. Windows 바탕 화면에서 **C:\\Users\\*username*\\AppData\\Roaming\\XamarinDeviceManager**로 이동합니다.

    ![Android Device Manager 로그 파일 위치](troubleshooting-images/win/02-log-files.png)

2. 로그 파일 중 하나를 두 번 클릭하여 열고 **구성 파일 경로**를 찾습니다. 예:

    [![로그 파일의 구성 파일 경로](troubleshooting-images/win/03-config-file-path-sml.png)](troubleshooting-images/win/03-config-file-path.png#lightbox)

3. 이 위치로 이동하고 **user.config**를 두 번 클릭하여 엽니다. 

4. **user.config**에서 **&lt;UserSettings&gt;** 요소를 찾아 **AndroidSdkPath** 특성을 추가합니다. 컴퓨터에서 Android SDK를 설치한 경로에 이 특성을 저장하고 파일을 저장합니다. 예를 들어 Android SDK가 **C:\\Programs\\Android\\SDK**에 설치된 경우 **&lt;UserSettings&gt;** 는 다음과 같습니다.
        
    ```xml
    <UserSettings SdkLibLastWriteTimeUtcTicks="636409365200000000" AndroidSdkPath="C:\Programs\Android\SDK" />
    ```

**user.config**를 이와 같이 변경한 후에는 Android Device Manager를 실행할 수 있어야 합니다.

## <a name="snapshot-disables-wifi-on-android-oreo"></a>스냅숏이 Android Oreo에서 WiFi를 사용하지 않음

시뮬레이션된 Wi-Fi 액세스를 통해 Android Oreo용 AVD가 구성되어 있는 경우 스냅숏을 만든 후 AVD를 다시 시작하면 Wi-Fi 액세스가 비활성화될 수 있습니다.

이 문제를 해결하려면 다음과 같이 합니다.

1. Android Device Manager에서 AVD를 선택합니다.

2. 추가 옵션 메뉴에서 **탐색기에 표시**를 클릭합니다.

3. **스냅숏 > default_boot**로 이동합니다.

4. **snapshot.pb** 파일을 삭제합니다.

    ![snapshot.pb 파일의 위치](troubleshooting-images/win/05-delete-snapshot.png)

5. AVD를 다시 시작합니다. 

이러한 변경 사항이 적용되면 Wi-Fi를 다시 작동하도록 하는 상태로 AVD가 다시 시작됩니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="snapshot-disables-wifi-on-android-oreo"></a>스냅숏이 Android Oreo에서 WiFi를 사용하지 않음

시뮬레이션된 Wi-Fi 액세스를 통해 Android Oreo용 AVD가 구성되어 있는 경우 스냅숏을 만든 후 AVD를 다시 시작하면 Wi-Fi 액세스가 비활성화될 수 있습니다.

이 문제를 해결하려면 다음과 같이 합니다.

1. Android Device Manager에서 AVD를 선택합니다.

2. 추가 옵션 메뉴에서 **Finder에 표시**를 클릭합니다.

3. **스냅숏 > default_boot**로 이동합니다.

4. **snapshot.pb** 파일을 삭제합니다.

    [![snapshot.pb 파일의 위치](troubleshooting-images/mac/02-delete-snapshot-sml.png)](troubleshooting-images/mac/02-delete-snapshot.png#lightbox)

5. AVD를 다시 시작합니다. 

이러한 변경 사항이 적용되면 Wi-Fi를 다시 작동하도록 하는 상태로 AVD가 다시 시작됩니다.


-----

## <a name="generating-a-bug-report"></a>버그 보고서 생성

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

위의 문제 해결 팁을 사용하여 해결할 수 없는 Android Device Manager 관련 문제를 발견할 경우 제목 표시줄을 마우스 오른쪽 단추로 클릭하고 **버그 보고서 생성**을 선택하여 버그 보고서를 제출하세요.

[![버그 보고서를 제출하는 데 사용되는 메뉴 항목의 위치](troubleshooting-images/win/04-bug-report-sml.png)](troubleshooting-images/win/04-bug-report.png#lightbox)


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

위의 문제 해결 팁을 사용하여 해결할 수 없는 Android Device Manager 관련 문제를 발견할 경우 **도움말 > 버그 보고서 생성**을 클릭하여 버그 보고서를 제출하세요.

[![버그 보고서를 제출하는 데 사용되는 메뉴 항목의 위치](troubleshooting-images/mac/01-bug-report-sml.png)](troubleshooting-images/mac/01-bug-report.png#lightbox)

-----
