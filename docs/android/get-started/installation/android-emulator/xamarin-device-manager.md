---
title: Xamarin Android 장치 관리자
description: Xamarin Android 장치 관리자(현재 미리 보기 상태)는 Google의 레거시 장치 관리자를 대체합니다. 이 가이드에서는 Xamarin Android 장치 관리자를 사용하여 Android 장치를 에뮬레이트하는 AVD(Android 가상 장치)를 만들고 구성하는 방법을 설명합니다. 물리적 장치에 의존하지 않고도 이러한 가상 장치를 사용하여 앱을 실행하고 테스트할 수 있습니다.
ms.prod: xamarin
ms.assetid: ECB327F3-FF1C-45CC-9FA6-9C11032BD5EF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/26/2018
ms.openlocfilehash: 94f82c9f893e22074ba95c052b57ce6ff18eaa1e
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/27/2018
---
# <a name="xamarin-android-device-manager"></a>Xamarin Android 장치 관리자

_Xamarin Android 장치 관리자(현재 미리 보기 상태)는 Google의 레거시 장치 관리자를 대체합니다. 이 가이드에서는 Xamarin Android 장치 관리자를 사용하여 Android 장치를 에뮬레이트하는 AVD(Android 가상 장치)를 만들고 구성하는 방법을 설명합니다. 물리적 장치에 의존하지 않고도 이러한 가상 장치를 사용하여 앱을 실행하고 테스트할 수 있습니다._

![현재 미리 보기 상태](~/media/shared/preview.png)
 
## <a name="overview"></a>개요

([하드웨어 가속](~/android/get-started/installation/android-emulator/hardware-acceleration.md)에 설명된 대로) 하드웨어 가속이 활성화된 것을 확인한 후 다음 단계는 앱 테스트 및 디버깅에 사용할 가상 장치를 만드는 것입니다. Xamarin Android 장치 관리자를 사용하여 Android SDK 에뮬레이터에서 사용할 가상 장치를 만들 수 있습니다.

[Google 장치 관리자](~/android/get-started/installation/android-emulator/google-emulator-manager.md) 대신 Xamarin Android 장치 관리자를 사용해야 하는 이유는 무엇일까요?
Android SDK Tools 버전 26.0.1부터 Google은 새로운 CLI(명령줄 인터페이스) 도구를 위해 UI 기반 AVD 및 SDK 관리자에 대한 지원을 중단했습니다. 이러한 변경으로 인해 Android SDK Tools 26.0.1 이상(Android 8.0 Oreo 개발에 필요)으로 업데이트할 경우 [Xamarin SDK 관리자](~/android/get-started/installation/android-sdk.md) 및 Xamarin Android 장치 관리자를 사용해야 합니다.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

이 가이드에서는 Windows(또는 [Mac](?tabs=vsmac))용 Visual Studio를 위해 Xamarin Android 장치 관리자를 설치하고 사용하는 방법을 설명합니다.

[![장치 탭에 있는 Xamarin Android 장치 관리자의 스크린샷](xamarin-device-manager-images/win/01-devices-dialog-sml.png)](xamarin-device-manager-images/win/01-devices-dialog.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

이 가이드에서는 Mac(또는 [Windows](?tabs=vswin))용 Visual Studio를 위해 Xamarin Android 장치 관리자를 설치하고 사용하는 방법을 설명합니다.

[![장치 탭에 있는 Xamarin Android 장치 관리자의 스크린샷](xamarin-device-manager-images/mac/01-devices-dialog-sml.png)](xamarin-device-manager-images/mac/01-devices-dialog.png#lightbox)

> [!NOTE]
> 이 가이드는 Mac용 Visual Studio에만 적용됩니다.
Xamarin Studio는 Xamarin Android 장치 관리자와 호환되지 않습니다.

-----

[Android SDK 에뮬레이터](~/android/deploy-test/debugging/android-sdk-emulator/index.md)에서 실행되는 AVD(*Android 가상 장치*)를 만들고 구성할 때는 Xamarin Android 장치 관리자를 사용합니다.
각 AVD는 실제 Android 장치를 시뮬레이션하는 에뮬레이터 구성입니다. 이를 통해 여러 실제 Android 장치를 시뮬레이션하는 다양한 구성에서 앱을 실행하고 테스트할 수 있습니다. Xamarin Android 장치 관리자는 Google의 독립 실행형 AVD 관리자(사용되지 않음)를 대체합니다.

이 가이드에서는 Android 장치 관리자를 설치하고 시작하는 방법을 알아봅니다. 가상 장치를 만들고, 복제하고, 사용자 지정하고, 실행하는 방법을 알아봅니다. 이 가이드에서는 또한 각 가상 장치의 속성(예: API 수준, CPU, 메모리 및 해상도)을 구성하고, 가속도계, GPS, 방향 및 조명 센서 같은 시뮬레이션된 센서를 활성화/비활성화하고, 가상 장치에서 사용하는 하드웨어 가속의 유형을 구성하는 방법도 설명합니다.

## <a name="requirements"></a>요구 사항

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin Android 장치 관리자를 사용하려면 다음이 필요합니다.

- Visual Studio 2017 버전 15.5 이상이 필요합니다. Visual Studio Community 버전 이상이 지원됩니다.

- Visual Studio용 Xamarin 버전 4.8 이상. Xamarin을 업데이트하는 방법에 대한 자세한 내용은 [업데이트 채널 변경](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/)을 참조하세요.

- 최신 버전의 Windows용 [Xamarin 장치 관리자 설치 관리자](https://go.microsoft.com/fwlink/?linkid=865528).

- **Android SDK** &ndash; Android SDK를 설치해야 하고([Android SDK 설치](~/android/get-started/installation/android-sdk.md) 참조), 다음 섹션에 설명된 대로 SDK Tools 버전 26.0을 설치해야 합니다. (아직 설치되지 않은 경우) **C:\\Program Files (x86)\\Android\\android-sdk**에 Android SDK를 설치해야 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

- Mac용 Visual Studio 7.4 이상.

- 최신 버전의 macOS용 [Xamarin 장치 관리자 설치 관리자](https://go.microsoft.com/fwlink/?linkid=865527).

- **Android SDK** &ndash; SDK 관리자를 통해 Android SDK 8.0(API 26) 이상을 설치해야 합니다.

-----

## <a name="installing-the-device-manager"></a>장치 관리자 설치

Xamarin Android 장치 관리자를 설치하려면 다음 단계를 따릅니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Windows용 [Xamarin 장치 관리자 설치 관리자](https://go.microsoft.com/fwlink/?linkid=865528)를 다운로드합니다.

2. **Xamarin.DeviceManager.msi**를 두 번 클릭하고 설치 지침을 따릅니다. 

    ![Xamarin Android 장치 관리자 설치 마법사](xamarin-device-manager-images/win/30-installer.png)


> [!NOTE]
> [Visual Studio 2017 Preview 5](https://www.visualstudio.com/vs/preview/)부터 Android 장치 관리자가 VS2017 설치 관리자의 일부로 배포됩니다. Visual Studio 2017 Preview 5와 함께 Xamarin Android 장치 관리자를 가져오기 위해 별도의 설치 관리자를 다운로드할 필요가 없습니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. macOS용 [Xamarin 장치 관리자 설치 관리자](https://go.microsoft.com/fwlink/?linkid=865527)를 다운로드합니다.

2. **AndroidDevices.pkg**를 두 번 클릭하고 설치 지침을 따릅니다. 

    [![Xamarin Android 장치 관리자 설치 마법사](xamarin-device-manager-images/mac/30-installer-sml.png)](xamarin-device-manager-images/mac/30-installer.png#lightbox)

-----
## <a name="launching-the-device-manager"></a>장치 관리자 실행

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio 15.6 미리 보기 3 이상에서는 **도구** 메뉴에서 Xamarin Android 장치 관리자를 실행할 수 있습니다. Visual Studio 15.6 미리 보기 3 이상을 사용 중인 경우에는 **도구 > Android Emulator 관리자**를 클릭하여 장치 관리자를 시작합니다.

[![도구 메뉴에서 실행](xamarin-device-manager-images/win/04-tools-menu-sml.png)](xamarin-device-manager-images/win/04-tools-menu.png#lightbox)

이전 버전의 Visual Studio를 사용 중인 경우 Windows **시작** 메뉴에서 Xamarin Android 장치 관리자를 실행해야 합니다.

![시작 메뉴의 Xamarin Android 장치 관리자](xamarin-device-manager-images/win/31-start-menu.png)

**Xamarin Android 장치 관리자**를 마우스 오른쪽 단추로 클릭하고 **더 보기 > 관리자 권한으로 실행**을 선택합니다. 실행 시 다음과 같은 오류 대화 상자가 표시될 경우 [문제 해결](#troubleshooting) 섹션에서 해결책 설명을 참조하세요.

![Android SDK 인스턴스 오류](xamarin-device-manager-images/win/32-sdk-error.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac용 Visual Studio 7.6 미리 보기 3(현재는 알파 채널에 있음) 이상에서는 **도구 > 에뮬레이터 관리자**를 선택하여 Xamarin Android 장치 관리자를 실행할 수 있습니다.

[![도구 메뉴에서 실행](xamarin-device-manager-images/mac/16-tools-menu-sml.png)](xamarin-device-manager-images/mac/16-tools-menu.png#lightbox)

이전 버전의 Mac용 Visual Studio를 사용 중인 경우 Xamarin Android 장치 관리자를 독립적으로 실행해야 합니다. **응용 프로그램** 폴더에서 **Android 장치**를 찾아 두 번 클릭하여 실행합니다.

[![Finder에서 Xamarin Android 장치 관리자의 위치](xamarin-device-manager-images/mac/31-location-in-finder-sml.png)](xamarin-device-manager-images/mac/31-location-in-finder.png#lightbox)

-----

Android 장치 관리자를 사용하려면 Android SDK Tools 버전 26.0.0 이상을 설치해야 합니다. Android SDK Tools 26.0.0 이상이 설치되어 있지 않으면 실행 시 이러한 오류 대화 상자가 표시됩니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Android SDK 인스턴스 오류 대화 상자](xamarin-device-manager-images/win/02-sdk-instance-error.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Android SDK 인스턴스 오류 대화 상자](xamarin-device-manager-images/mac/02-sdk-instance-error.png)

-----

이 오류 대화 상자가 나타나면 **확인**을 클릭하여 Android SDK Manager를 엽니다. Android SDK Manager에서 **도구** 탭을 클릭하여 **Android SDK Tools 26.0.2** 이상, **Android SDK Platform-Tools 26.0.0** 이상, **Android SDK Build-Tools 26.0.0**(이상)을 설치합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Android SDK Tools 26.0 설치](xamarin-device-manager-images/win/03-sdk-tools-sml.png)](xamarin-device-manager-images/win/03-sdk-tools.png#lightbox)

이러한 패키지를 설치한 후 SDK Manager를 닫고 Android 장치 관리자를 다시 시작할 수 있습니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Android SDK Tools 26.0 설치](xamarin-device-manager-images/mac/03-sdk-tools-sml.png)](xamarin-device-manager-images/mac/03-sdk-tools.png#lightbox)

-----

## <a name="main-screen"></a>주 화면

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Android 장치 관리자를 처음 실행하면 현재 구성된 모든 가상 장치가 화면에 표시됩니다. 각 장치의 **이름**, **운영 체제**(Android API 수준), **CPU**, **메모리** 크기 및 화면 해상도가 표시됩니다.

[![설치된 장치의 목록 및 매개 변수](xamarin-device-manager-images/win/05-installed-list-sml.png)](xamarin-device-manager-images/win/05-installed-list.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Android 장치 관리자를 처음 실행하면 현재 구성된 모든 가상 장치가 화면에 표시됩니다. 각 장치의 **이름**, **시스템 이미지**(Android API 수준), **CPU**, **메모리** 크기 및 화면 해상도가 표시됩니다.

[![설치된 장치의 목록 및 매개 변수](xamarin-device-manager-images/mac/05-devices-list-sml.png)](xamarin-device-manager-images/mac/05-devices-list.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

목록에서 장치를 클릭하면 **시작** 단추가 오른쪽에 나타납니다. **시작** 단추를 클릭하면 이 가상 장치로 에뮬레이터를 시작할 수 있습니다.

[![장치 이미지에 대한 시작 단추](xamarin-device-manager-images/win/06-start-button-sml.png)](xamarin-device-manager-images/win/06-start-button.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

**재생** 단추를 클릭하여 원하는 가상 장치로 에뮬레이터를 실행합니다.

[![장치 이미지에 대한 시작 단추](xamarin-device-manager-images/mac/06-start-button-sml.png)](xamarin-device-manager-images/mac/06-start-button.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

선택한 가상 장치로 에뮬레이터가 시작되면 **시작** 단추가 에뮬레이터를 중지하는 데 사용할 수 있는 **중지** 단추로 변경됩니다.

[![실행 중인 장치에 대한 중지 단추](xamarin-device-manager-images/win/07-stop-button-sml.png)](xamarin-device-manager-images/win/07-stop-button.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

선택한 가상 장치로 에뮬레이터가 시작되면 **재생** 단추가 에뮬레이터를 중지하는 데 사용할 수 있는 **중지** 단추로 변경됩니다.

[![실행 중인 장치에 대한 중지 단추](xamarin-device-manager-images/mac/07-stop-button-sml.png)](xamarin-device-manager-images/mac/07-stop-button.png#lightbox)

-----

### <a name="new-device"></a>새 장치

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

새 장치를 만들려면 **새로 만들기** 단추(화면의 오른쪽 상단에 있음)를 클릭합니다.

[![새 장치를 만드는 데 사용되는 새로 만들기 단추](xamarin-device-manager-images/win/08-new-button-sml.png)](xamarin-device-manager-images/win/08-new-button.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

새 장치를 만들려면 **새 장치** 단추(화면의 오른쪽 상단에 있음)를 클릭합니다.

[![새 장치를 만드는 데 사용되는 새로 만들기 단추](xamarin-device-manager-images/mac/08-new-button-sml.png)](xamarin-device-manager-images/mac/08-new-button.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

**새로 만들기**를 클릭하면 **새 장치** 화면이 열립니다.

[![장치 관리자의 새 장치 화면](xamarin-device-manager-images/win/09-new-device-editor-sml.png)](xamarin-device-manager-images/win/09-new-device-editor.png#lightbox)

**새 장치** 화면에서 새 장치를 구성하려면 다음 단계를 따르세요.

1. **장치** 풀 다운 메뉴를 클릭하여 에뮬레이트할 실제 장치를 선택합니다.

    [![장치 풀 다운 메뉴](xamarin-device-manager-images/win/10-device-menu-sml.png)](xamarin-device-manager-images/win/10-device-menu.png#lightbox)

2. **시스템 이미지** 풀 다운 메뉴를 클릭하여 이 가상 장치와 함께 사용할 시스템 이미지를 선택합니다. 이 메뉴는 **설치됨** 아래에 설치된 시스템 이미지를 나열합니다. **다운로드** 섹션에는 개발 컴퓨터에서 현재 사용할 수 없지만 자동으로 설치될 수 있는 시스템 이미지가 나열됩니다.

    [![시스템 이미지 풀 다운 메뉴](xamarin-device-manager-images/win/11-system-image-menu-sml.png)](xamarin-device-manager-images/win/11-system-image-menu.png#lightbox)

3. 장치에 새 이름을 지정합니다. 다음 예제에서는 새 장치에 **Nexus 5 API 25**라는 이름을 지정합니다.

    [![새 장치 이름 지정](xamarin-device-manager-images/win/12-device-name-sml.png)](xamarin-device-manager-images/win/12-device-name.png#lightbox)

4. 수정해야 하는 모든 속성을 편집합니다. 속성을 변경하려면 이 가이드의 뒷부분에 나오는 [프로필 속성](#properties)을 참조하세요.

5. 명시적으로 설정해야 하는 추가 속성을 추가합니다. **새 장치** 화면에는 가장 자주 수정되는 속성만 나열되지만 (왼쪽 하단에 있는) **속성 추가** 풀 다운 메뉴를 클릭하여 추가 속성을 추가할 수 있습니다. 다음 예제에서는 `hw.lcd.backlight` 속성이 추가됩니다.

    [![속성 추가 풀 다운 메뉴](xamarin-device-manager-images/win/13-add-property-menu-sml.png)](xamarin-device-manager-images/win/13-add-property-menu.png#lightbox)

6. **만들기** 단추(오른쪽 하단)를 클릭하여 새 장치를 만듭니다.

    ![만들기 단추](xamarin-device-manager-images/win/14-create-button.png)

7. **라이선스 승인** 화면이 표시될 수 있습니다. 사용 조건에 동의하면 **동의**를 클릭합니다.

    ![라이선스 승인 화면](xamarin-device-manager-images/win/15-license-acceptance.png)

8. Android 장치 관리자는 장치를 만들 때 **만드는 중** 진행률 표시기와 함께 설치된 가상 장치 목록에 새 장치를 추가합니다.

    [![만들기 진행률 표시기](xamarin-device-manager-images/win/16-creating-the-device-sml.png)](xamarin-device-manager-images/win/16-creating-the-device.png#lightbox)

9. 만들기 프로세스가 완료되면 설치된 가상 장치 목록에 실행할 준비가 된 새 장치와 **시작** 단추가 표시됩니다.

   [![실행할 준비가 된 새로 생성된 장치](xamarin-device-manager-images/win/17-created-device-sml.png)](xamarin-device-manager-images/win/17-created-device.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

**새 장치**를 클릭하면 **새 장치** 화면이 열립니다.

[![장치 관리자의 새 장치 화면](xamarin-device-manager-images/mac/09-new-device-editor-sml.png)](xamarin-device-manager-images/mac/09-new-device-editor.png#lightbox)

**새 장치** 화면에서 새 장치를 구성하려면 다음 단계를 따르세요.

1. **장치** 풀 다운 메뉴를 클릭하여 에뮬레이트할 실제 장치를 선택합니다.

    [![장치 풀 다운 메뉴](xamarin-device-manager-images/mac/10-device-menu-sml.png)](xamarin-device-manager-images/mac/10-device-menu.png#lightbox)

2. **시스템 이미지** 풀 다운 메뉴를 클릭하여 이 가상 장치와 함께 사용할 시스템 이미지를 선택합니다. 이 메뉴는 **설치됨** 아래에 설치된 시스템 이미지를 나열합니다. **다운로드** 섹션(표시될 경우)에는 개발 컴퓨터에서 현재 사용할 수 없지만 자동으로 설치될 수 있는 시스템 이미지가 나열됩니다.

    [![시스템 이미지 풀 다운 메뉴](xamarin-device-manager-images/mac/11-system-image-menu-sml.png)](xamarin-device-manager-images/mac/11-system-image-menu.png#lightbox)

3. 장치에 새 이름을 지정합니다. 다음 예제에서는 새 장치에 **Nexus 5X API 25**라는 이름을 지정합니다.

    [![새 장치 이름 지정](xamarin-device-manager-images/mac/12-device-name-sml.png)](xamarin-device-manager-images/mac/12-device-name.png#lightbox)

4. 수정해야 하는 모든 속성을 편집합니다. 속성을 변경하려면 이 가이드의 뒷부분에 나오는 [프로필 속성](#properties)을 참조하세요.

5. 명시적으로 설정해야 하는 추가 속성을 추가합니다. **새 장치** 화면에는 가장 자주 수정되는 속성만 나열되지만 (왼쪽 하단에 있는) **속성 추가** 풀 다운 메뉴를 클릭하여 추가 속성을 추가할 수 있습니다.

    [![속성 추가 풀 다운 메뉴](xamarin-device-manager-images/mac/13-add-property-menu-sml.png)](xamarin-device-manager-images/mac/13-add-property-menu.png#lightbox)

6. **사용자 지정**을 클릭하여 장치에 대한 새 속성을 정의할 수도 있습니다.

    ![만들기 단추](xamarin-device-manager-images/mac/14-custom-button.png)

7. **만들기** 단추(오른쪽 하단)를 클릭하여 새 장치를 만듭니다.

    ![만들기 단추](xamarin-device-manager-images/mac/15-create-button.png)

8. **라이선스 승인** 화면이 표시될 수 있습니다. 사용 조건에 동의하면 **동의**를 클릭합니다.

9. Android 장치 관리자는 장치를 만들 때 **만드는 중** 진행률 표시기와 함께 장치 목록에 새 장치를 추가합니다.

    [![만들기 진행률 표시기](xamarin-device-manager-images/mac/17-creating-the-device-sml.png)](xamarin-device-manager-images/mac/17-creating-the-device.png#lightbox)

10. 만들기 프로세스가 완료되면 장치 목록에 실행할 준비가 된 새 장치와 **재생** 단추가 표시됩니다.

   [![실행할 준비가 된 새로 생성된 장치](xamarin-device-manager-images/mac/18-created-device-sml.png)](xamarin-device-manager-images/mac/18-created-device.png#lightbox)

-----


<a name="device-edit" />


### <a name="edit-device"></a>장치 편집

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

기존 가상 장치를 편집하려면 장치를 선택하고 (화면 오른쪽 상단에 있는) **편집** 단추를 클릭합니다.

[![새 장치를 수정하기 위한 편집 단추](xamarin-device-manager-images/win/19-edit-button-sml.png)](xamarin-device-manager-images/win/19-edit-button.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

기존 가상 장치를 편집하려면 **추가 옵션** 풀 다운 메뉴(기어 아이콘)을 선택하고 **편집**을 선택합니다.
 
[![새 장치를 수정하기 위한 편집 메뉴 선택](xamarin-device-manager-images/mac/19-edit-button-sml.png)](xamarin-device-manager-images/mac/19-edit-button.png#lightbox)
 
-----

**편집**을 클릭하면 선택된 가상 장치에 대한 장치 편집기가 실행됩니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![장치 편집기 화면](xamarin-device-manager-images/win/20-device-editor-sml.png)](xamarin-device-manager-images/win/20-device-editor.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
 
[![장치 편집기 화면](xamarin-device-manager-images/mac/20-device-editor-sml.png)](xamarin-device-manager-images/mac/20-device-editor.png#lightbox)
 
-----

**장치 편집기** 화면에서 첫 번째 열에는 가상 장치 속성이 나열되고, 두 번째 열에는 각 속성의 해당 값이 나열됩니다. 속성을 선택하면 속성에 대한 자세한 설명이 오른쪽에 표시됩니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

예를 들어 다음 스크린샷에서 `hw.lcd.density` 속성은 **420**에서 **240**으로 변경됩니다.

[![장치 편집 예제](xamarin-device-manager-images/win/21-device-editing-sml.png)](xamarin-device-manager-images/win/21-device-editing.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

예를 들어 다음 스크린샷에서 `hw.lcd.density` 속성은 **320**에서 **240**으로 변경되고, `hw.ramSize` 속성은 **768**로 변경됩니다.
 
[![장치 편집 예제](xamarin-device-manager-images/mac/21-device-editing-sml.png)](xamarin-device-manager-images/mac/21-device-editing.png#lightbox)
 
-----

필요한 구성을 변경했으면 **저장** 단추를 클릭합니다.
가상 장치 속성을 변경하는 방법에 대한 자세한 내용은 이 가이드의 뒷부분에 나오는 [프로필 속성](#properties)을 참조하세요.


 
### <a name="additional-options"></a>추가 옵션

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

장치를 사용하기 위한 추가 옵션은 오른쪽 위에 있는 &hellip; 메뉴에서 사용할 수 있습니다.

[![추가 옵션 메뉴의 위치](xamarin-device-manager-images/win/22-overflow-menu-sml.png)](xamarin-device-manager-images/win/22-overflow-menu.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

장치를 사용하기 위한 추가 옵션은 **재생** 단추의 왼쪽에 있는 풀 다운 메뉴에서 사용할 수 있습니다.

[![추가 옵션 메뉴의 위치](xamarin-device-manager-images/mac/22-overflow-menu-sml.png)](xamarin-device-manager-images/mac/22-overflow-menu.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

추가 옵션 메뉴에는 다음 항목이 포함되어 있습니다.

-   **복제 및 편집** &ndash; 현재 선택된 장치를 복제하고 다른 고유한 이름을 사용하여 **새 장치** 화면에서 엽니다. 예를 들어 **VisualStudio_android-23_x86_phone**을 선택하고 **복제 및 편집**을 클릭하면 이름에 카운터가 추가됩니다.

    [![복제 및 편집 화면](xamarin-device-manager-images/win/23-dupe-and-edit-sml.png)](xamarin-device-manager-images/win/23-dupe-and-edit.png#lightbox)

-   **탐색기에 표시** &ndash; Windows 탐색기 창에 가상 장치에 대한 파일이 들어 있는 폴더가 열립니다. 예를 들어 **Nexus 5X API 25**를 선택하고 **탐색기에 표시**를 클릭하면 다음과 같은 창이 열립니다.

    [![탐색기에 표시 클릭 후 결과](xamarin-device-manager-images/win/24-reveal-in-explorer-sml.png)](xamarin-device-manager-images/win/24-reveal-in-explorer.png#lightbox)

-   **공장 재설정** &ndash; 선택된 장치를 기본 설정으로 재설정하여 장치가 실행 중일 때 사용자가 변경한 장치의 내부 상태에 대해 모든 내용을 지웁니다. 생성 및 편집 중에 가상 장치에서 수정된 내용은 이러한 변경의 영향을 받지 않습니다. 이러한 재설정을 수행할 수 없음을 알리는 대화 상자가 표시됩니다. **사용자 데이터 지우기**를 클릭하여 재설정을 확인합니다.

-   **삭제** &ndash; 선택된 가상 장치를 영구적으로 삭제합니다.
    장치 삭제는 실행 취소할 수 없음을 알리는 대화 상자가 표시됩니다. 장치를 삭제하려는 것이 확실한 경우 **삭제**를 클릭합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

추가 옵션 메뉴에는 다음 항목이 포함되어 있습니다.

-   **편집** &ndash; 앞서 [장치 편집](#device-edit)에서 설명한 대로 현재 선택된 장치를 장치 편집기에서 엽니다.

-   **복제 및 편집** &ndash; 현재 선택된 장치를 복제하고 다른 고유한 이름을 사용하여 **새 장치** 화면에서 엽니다.
    예를 들어 **Nexus 5X API 25**를 선택하고 **복제 및 편집**을 클릭하면 이름에 카운터가 추가됩니다.

    [![복제 및 편집 화면](xamarin-device-manager-images/mac/23-dupe-and-edit-sml.png)](xamarin-device-manager-images/mac/23-dupe-and-edit.png#lightbox)

-   **Finder에 표시** &ndash; macOS Finder 창에 가상 장치에 대한 파일이 들어 있는 폴더가 열립니다. 예를 들어 **Nexus 5X API 25**를 선택하고 **Finder에 표시**를 클릭하면 다음과 같은 창이 열립니다.

    [![탐색기에 표시 클릭 후 결과](xamarin-device-manager-images/mac/24-reveal-in-finder-sml.png)](xamarin-device-manager-images/mac/24-reveal-in-finder.png#lightbox)

-   **공장 재설정** &ndash; 선택된 장치를 기본 설정으로 재설정하여 장치가 실행 중일 때 사용자가 변경한 장치의 내부 상태에 대해 모든 내용을 지웁니다. 생성 및 편집 중에 가상 장치에서 수정된 내용은 이러한 변경의 영향을 받지 않습니다. 이러한 재설정을 수행할 수 없음을 알리는 대화 상자가 표시됩니다. **사용자 데이터 지우기**를 클릭하여 재설정을 확인합니다.

-   **삭제** &ndash; 선택된 가상 장치를 영구적으로 삭제합니다.
    장치 삭제는 실행 취소할 수 없음을 알리는 대화 상자가 표시됩니다. 장치를 삭제하려는 것이 확실한 경우 **삭제**를 클릭합니다.

-----

<a name="properties" />
 

## <a name="profile-properties"></a>프로필 속성

**새 장치** 및 **장치 편집기** 화면에서 첫 번째 열에는 가상 장치 속성이 나열되고, 두 번째 열에는 각 속성의 해당 값이 나열됩니다. 속성을 선택하면 속성에 대한 자세한 설명이 오른쪽에 표시됩니다. *하드웨어 프로필 속성* 및 해당 *AVD 속성*을 수정할 수 있습니다.
하드웨어 프로필 속성(예: `hw.ramSize` 및 `hw.accelerometer`)은 에뮬레이트된 장치의 물리적 특성을 나타냅니다. 이러한 특성에는 화면 크기, 사용 가능한 RAM의 양, 가속도계의 유무가 포함됩니다. AVD 속성은 실행되는 AVD의 작업을 나타냅니다. 예를 들어 AVD 속성을 구성하여 AVD에서 개발 컴퓨터의 그래픽 카드를 렌더링에 사용하는 방식을 지정할 수 있습니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

다음 지침에 따라 속성을 변경할 수 있습니다.

-   부울 속성을 변경하려면 부울 속성의 오른쪽에 있는 확인 표시를 클릭합니다.

    ![부울 속성 변경](xamarin-device-manager-images/win/25-boolean-value.png)

-   *열거형*(enumerated) 속성을 변경하려면 속성의 오른쪽에 있는 아래쪽 화살표를 클릭하고 새 값을 선택합니다.

    ![열거형 속성 변경](xamarin-device-manager-images/win/27-enum-value.png)

-   정수 또는 문자열 속성을 변경하려면 값 열에서 현재 문자열 또는 정수 설정을 두 번 클릭하고 새 값을 입력합니다.

    ![정수 속성 변경](xamarin-device-manager-images/win/26-integer-value.png)


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

다음 지침에 따라 속성을 변경할 수 있습니다.

-   부울 속성을 변경하려면 부울 속성의 오른쪽에 있는 확인 표시를 클릭합니다.

    ![부울 속성 변경](xamarin-device-manager-images/mac/25-boolean-value.png)

-   *열거형*(enumerated) 속성을 변경하려면 속성의 오른쪽에 있는 풀 다운 메뉴를 클릭하고 새 값을 선택합니다.

    ![열거형 속성 변경](xamarin-device-manager-images/mac/27-enum-value.png)

-   정수 또는 문자열 속성을 변경하려면 값 열에서 현재 문자열 또는 정수 설정을 두 번 클릭하고 새 값을 입력합니다.

    ![정수 속성 변경](xamarin-device-manager-images/mac/26-integer-value.png)


-----

다음 표에는 **새 장치** 및 **장치 편집기** 화면에 나오는 속성에 대한 자세한 설명이 나와 있습니다.

[!include[](~/android/includes/emulator-properties.md)]

이러한 속성에 대한 자세한 내용은 [하드웨어 프로필 속성](https://developer.android.com/studio/run/managing-avds.html#hpproperties)을 참조하세요.


<a name="troubleshooting" />

## <a name="troubleshooting"></a>문제 해결

다음은 일반적인 Xamarin Android 장치 관리자 문제 및 해결법에 대한 설명입니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

### <a name="android-sdk-in-non-standard-location"></a>표준이 아닌 위치의 Android SDK

일반적으로 Android SDK는 다음 위치에 설치됩니다.

**C:\\Program Files (x86)\\Android\\android-sdk**

이 위치에 SDK가 설치되지 않은 경우 실행 시 이 오류가 나타날 수 있습니다.

![Android SDK 인스턴스 오류](xamarin-device-manager-images/win/32-sdk-error.png)

이 문제를 해결하려면 다음을 수행합니다.

1. Windows 바탕 화면에서 **C:\\Users\\*username*\\AppData\\Roaming\\XamarinDeviceManager**로 이동합니다.

    ![Xamarin 장치 관리자 로그 파일 위치](xamarin-device-manager-images/win/33-log-files.png)

2. 로그 파일 중 하나를 두 번 클릭하여 열고 **구성 파일 경로**를 찾습니다. 예:

    [![로그 파일의 구성 파일 경로](xamarin-device-manager-images/win/34-config-file-path-sml.png)](xamarin-device-manager-images/win/34-config-file-path.png#lightbox)

3. 이 위치로 이동하고 **user.config**를 두 번 클릭하여 엽니다. 

4. **user.config**에서 **&lt;UserSettings&gt;** 요소를 찾아 **AndroidSdkPath** 특성을 추가합니다. 컴퓨터에서 Android SDK를 설치한 경로에 이 특성을 저장하고 파일을 저장합니다. 예를 들어 Android SDK가 **C:\\Programs\\Android\\SDK**에 설치된 경우 **&lt;UserSettings&gt;** 는 다음과 같습니다.
        
    ```xml
    <UserSettings SdkLibLastWriteTimeUtcTicks="636409365200000000" AndroidSdkPath="C:\Programs\Android\SDK" />
    ```

**user.config**를 이와 같이 변경한 후에는 Xamarin Android 장치 관리자를 실행할 수 있어야 합니다.

### <a name="snapshot-disables-wifi-on-android-oreo"></a>스냅숏이 Android Oreo에서 WiFi를 사용하지 않음

시뮬레이션된 Wi-Fi 액세스를 통해 Android Oreo용 AVD가 구성되어 있는 경우 스냅숏을 만든 후 AVD를 다시 시작하면 Wi-Fi 액세스가 비활성화될 수 있습니다.

이 문제를 해결하려면 다음과 같이 합니다.

1. Xamarin 장치 관리자에서 AVD를 선택합니다.

2. 추가 옵션 메뉴에서 **탐색기에 표시**를 클릭합니다.

3. **스냅숏 > default_boot**로 이동합니다.

4. **snapshot.pb** 파일을 삭제합니다.

    [![snapshot.pb 파일의 위치](xamarin-device-manager-images/win/36-delete-snapshot-sml.png)](xamarin-device-manager-images/win/36-delete-snapshot.png#lightbox)

5. AVD를 다시 시작합니다. 

이러한 변경 사항이 적용되면 Wi-Fi를 다시 작동하도록 하는 상태로 AVD가 다시 시작됩니다.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

### <a name="snapshot-disables-wifi-on-android-oreo"></a>스냅숏이 Android Oreo에서 WiFi를 사용하지 않음

시뮬레이션된 Wi-Fi 액세스를 통해 Android Oreo용 AVD가 구성되어 있는 경우 스냅숏을 만든 후 AVD를 다시 시작하면 Wi-Fi 액세스가 비활성화될 수 있습니다.

이 문제를 해결하려면 다음과 같이 합니다.

1. Xamarin 장치 관리자에서 AVD를 선택합니다.

2. 추가 옵션 메뉴에서 **Finder에 표시**를 클릭합니다.

3. **스냅숏 > default_boot**로 이동합니다.

4. **snapshot.pb** 파일을 삭제합니다.

    [![snapshot.pb 파일의 위치](xamarin-device-manager-images/mac/36-delete-snapshot-sml.png)](xamarin-device-manager-images/mac/36-delete-snapshot.png#lightbox)

5. AVD를 다시 시작합니다. 

이러한 변경 사항이 적용되면 Wi-Fi를 다시 작동하도록 하는 상태로 AVD가 다시 시작됩니다.

-----


### <a name="generating-a-bug-report"></a>버그 보고서 생성

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

위의 문제 해결 팁을 사용하여 해결할 수 없는 Xamarin Android 장치 관리자 관련 문제를 발견할 경우 제목 표시줄을 마우스 오른쪽 단추로 클릭하고 **버그 보고서 생성**을 선택하여 버그 보고서를 제출하세요.

![버그 보고서를 제출하는 데 사용되는 메뉴 항목의 위치](xamarin-device-manager-images/win/35-bug-report.png)


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

위의 문제 해결 팁을 사용하여 해결할 수 없는 Xamarin Android 장치 관리자 관련 문제를 발견할 경우 **도움말 > 버그 보고서 생성**을 클릭하여 버그 보고서를 제출하세요.

![버그 보고서를 제출하는 데 사용되는 메뉴 항목의 위치](xamarin-device-manager-images/mac/35-bug-report.png)

-----

 
 
## <a name="summary"></a>요약

이 가이드에서는 Mac용 Visual Studio 및 Visual Studio용 Xamarin에서 사용할 수 있는 Xamarin Android 장치 관리자를 소개했습니다. Android 에뮬레이터를 시작 및 중지하고, 실행할 AVD(Android 가상 장치)를 선택하고, 새 가상 장치를 만드는 기능과 같은 필수 기능과 가상 장치를 편집하는 방법을 설명했습니다. 또한 추가 사용자 지정을 위해 프로필 하드웨어 속성을 편집하는 방법을 설명 했습니다.


## <a name="related-links"></a>관련 링크

- [Android SDK Tools에 대한 변경 내용](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Android SDK 에뮬레이터를 사용하여 디버깅](~/android/deploy-test/debugging/android-sdk-emulator/index.md)
- [SDK Tools 릴리스 정보(Google)](https://developer.android.com/studiohttps://developer.xamarin.com/releases/sdk-tools.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
