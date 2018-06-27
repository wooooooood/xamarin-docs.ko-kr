---
title: Android Device Manager를 사용하여 가상 장치 관리
description: 이 문서에서는 Android Device Manager를 사용하여 물리적 Android 장치를 에뮬레이트하는 AVD(Android 가상 장치)를 만들고 구성하는 방법을 설명합니다. 물리적 장치에 의존하지 않고도 이러한 가상 장치를 사용하여 앱을 실행하고 테스트할 수 있습니다.
ms.prod: xamarin
ms.assetid: ECB327F3-FF1C-45CC-9FA6-9C11032BD5EF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/02/2018
ms.openlocfilehash: 888f126d3e58b0300ba7ce3ad1cb5a8001fc545a
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34733777"
---
# <a name="managing-virtual-devices-with-the-android-device-manager"></a>Android Device Manager를 사용하여 가상 장치 관리

_이 문서에서는 Android Device Manager를 사용하여 물리적 Android 장치를 에뮬레이트하는 AVD(Android 가상 장치)를 만들고 구성하는 방법을 설명합니다. 물리적 장치에 의존하지 않고도 이러한 가상 장치를 사용하여 앱을 실행하고 테스트할 수 있습니다._

## <a name="overview"></a>개요

(에뮬레이터 성능에 대한 [하드웨어 가속](~/android/get-started/installation/android-emulator/hardware-acceleration.md)에 설명된 대로) 하드웨어 가속이 활성화된 것을 확인한 후에 다음 단계는 _Android Device Manager_(_Xamarin Android Device Manager_라고도 함)를 사용하여 앱을 테스트하고 디버깅하는 데 사용할 수 있는 가상 장치를 만드는 것입니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

이 문서에서는 Android Device Manager를 사용하여 Android 가상 장치를 만들고, 복제하고, 사용자 지정하고, 실행하는 방법을 설명합니다.

[![장치 탭에 있는 Android Device Manager의 스크린샷](device-manager-images/win/01-devices-dialog-sml.png)](device-manager-images/win/01-devices-dialog.png#lightbox)

Android Device Manager를 사용하여 [Google Android Emulator](~/android/deploy-test/debugging/android-sdk-emulator/index.md)에서 실행되는 AVD(_Android 가상 장치_)를 만들고 구성합니다.
각 AVD는 실제 Android 장치를 시뮬레이션하는 에뮬레이터 구성입니다. 이를 통해 여러 실제 Android 장치를 시뮬레이션하는 다양한 구성에서 앱을 실행하고 테스트할 수 있습니다.

## <a name="requirements"></a>요구 사항

Android Device Manager를 사용하려면 다음이 필요합니다.

-   Visual Studio 2017 버전 15.7 이상이 필요합니다. Visual Studio Community, Professional 및 Enterprise 버전이 지원됩니다.

-   Visual Studio Tools for Xamarin 버전 4.9 이상

-   Android SDK를 설치해야 하고([Xamarin.Android에 대한 Android SDK 설정](~/android/get-started/installation/android-sdk.md) 참조), 다음 섹션에 설명된 대로 SDK Tools 버전 26.1.1 이상을 설치해야 합니다. (아직 설치되지 않은 경우) **C:\\Program Files (x86)\\Android\\android-sdk**에 Android SDK를 설치해야 합니다.


## <a name="launching-the-device-manager"></a>장치 관리자 실행

**도구 > Android > Android Device Manager**를 클릭하여 **도구** 메뉴에서 Android Device Manager를 시작합니다.

[![도구 메뉴에서 실행](device-manager-images/win/04-tools-menu-sml.png)](device-manager-images/win/04-tools-menu.png#lightbox)

실행 시 다음과 같은 오류 대화 상자가 표시될 경우 해결 방법 지침은 [에뮬레이터 설정 문제 해결](~/android/get-started/installation/android-emulator/troubleshooting.md)을 참조하세요.

![Android SDK 인스턴스 오류](device-manager-images/win/32-sdk-error.png)

Android Device Manager를 사용하려면 Android SDK Tools 버전 26.1.1 이상을 설치해야 합니다. Android SDK Tools 26.1.1 이상이 설치되어 있지 않으면 실행 시 이러한 오류 대화 상자가 표시됩니다.

![Android SDK 인스턴스 오류 대화 상자](device-manager-images/win/02-sdk-instance-error.png)

이 오류 대화 상자가 나타나면 **SDK Manager 열기**를 클릭하여 Android SDK Manager를 엽니다. Android SDK Manager에서 **도구** 탭을 클릭하고 다음을 설치합니다.

-   **Android SDK Tools 26.1.1** 이상 
-   **Android SDK 플랫폼 도구 27.0.1** 이상  
-   **Android SDK 빌드 도구 27.0.3** 이상

이러한 패키지는 다음 스크린샷에 표시된 대로 **설치됨** 상태로 표시됩니다.

[![Android SDK Tools 27.0 설치](device-manager-images/win/03-sdk-tools-sml.png)](device-manager-images/win/03-sdk-tools.png#lightbox)

이러한 패키지를 설치한 후 SDK Manager를 닫고 Android 장치 관리자를 다시 시작할 수 있습니다.

## <a name="main-screen"></a>주 화면

Android 장치 관리자를 처음 실행하면 현재 구성된 모든 가상 장치가 화면에 표시됩니다. 각 장치의 **이름**, **운영 체제**(Android API 수준), **CPU**, **메모리** 크기 및 화면 해상도가 표시됩니다.

[![설치된 장치의 목록 및 매개 변수](device-manager-images/win/05-installed-list-sml.png)](device-manager-images/win/05-installed-list.png#lightbox)

목록에서 장치를 클릭하면 **시작** 단추가 오른쪽에 나타납니다. **시작** 단추를 클릭하면 이 가상 장치로 에뮬레이터를 시작할 수 있습니다.

[![장치 이미지에 대한 시작 단추](device-manager-images/win/06-start-button-sml.png)](device-manager-images/win/06-start-button.png#lightbox)

선택한 가상 장치로 에뮬레이터가 시작되면 **시작** 단추가 에뮬레이터를 중지하는 데 사용할 수 있는 **중지** 단추로 변경됩니다.

[![실행 중인 장치에 대한 중지 단추](device-manager-images/win/07-stop-button-sml.png)](device-manager-images/win/07-stop-button.png#lightbox)

### <a name="new-device"></a>새 장치

새 장치를 만들려면 **새로 만들기** 단추(화면의 오른쪽 상단에 있음)를 클릭합니다.

[![새 장치를 만드는 데 사용되는 새로 만들기 단추](device-manager-images/win/08-new-button-sml.png)](device-manager-images/win/08-new-button.png#lightbox)

**새로 만들기**를 클릭하면 **새 장치** 화면이 열립니다.

[![장치 관리자의 새 장치 화면](device-manager-images/win/09-new-device-editor-sml.png)](device-manager-images/win/09-new-device-editor.png#lightbox)

**새 장치** 화면에서 새 장치를 구성하려면 다음 단계를 따르세요.

1. **장치** 풀 다운 메뉴를 클릭하여 에뮬레이트할 실제 장치를 선택합니다.

    [![장치 풀 다운 메뉴](device-manager-images/win/10-device-menu-sml.png)](device-manager-images/win/10-device-menu.png#lightbox)

2. **시스템 이미지** 풀 다운 메뉴를 클릭하여 이 가상 장치와 함께 사용할 시스템 이미지를 선택합니다. 이 메뉴는 **설치됨** 아래에 설치된 시스템 장치 관리자 이미지를 나열합니다. **다운로드** 섹션에는 개발 컴퓨터에서 현재 사용할 수 없지만 자동으로 설치될 수 있는 시스템 장치 관리자 이미지가 나열됩니다.

    [![시스템 이미지 풀 다운 메뉴](device-manager-images/win/11-system-image-menu-sml.png)](device-manager-images/win/11-system-image-menu.png#lightbox)

3. 장치에 새 이름을 지정합니다. 다음 예제에서는 새 장치에 **Nexus 5 API 25**라는 이름을 지정합니다.

    [![새 장치 이름 지정](device-manager-images/win/12-device-name-sml.png)](device-manager-images/win/12-device-name.png#lightbox)

4. 수정해야 하는 모든 속성을 편집합니다. 속성을 변경하려면 [Android 가상 장치 속성 편집](~/android/get-started/installation/android-emulator/device-properties.md)을 참조하세요.

5. 명시적으로 설정해야 하는 추가 속성을 추가합니다. **새 장치** 화면에는 가장 자주 수정되는 속성만 나열되지만 (왼쪽 하단에 있는) **속성 추가** 풀 다운 메뉴를 클릭하여 추가 속성을 추가할 수 있습니다. 다음 예제에서는 `hw.lcd.backlight` 속성이 추가됩니다.

    [![속성 추가 풀 다운 메뉴](device-manager-images/win/13-add-property-menu-sml.png)](device-manager-images/win/13-add-property-menu.png#lightbox)

6. **만들기** 단추(오른쪽 하단)를 클릭하여 새 장치를 만듭니다.

    ![만들기 단추](device-manager-images/win/14-create-button.png)

7. **라이선스 승인** 화면이 표시될 수 있습니다. 사용 조건에 동의하면 **동의**를 클릭합니다.

    ![라이선스 승인 화면](device-manager-images/win/15-license-acceptance.png)

8. Android Device Manager는 장치 생성 중 **만드는 중** 진행률 표시기를 표시하는 동안 설치된 가상 장치 목록에 새 장치를 추가합니다.

    [![만들기 진행률 표시기](device-manager-images/win/16-creating-the-device-sml.png)](device-manager-images/win/16-creating-the-device.png#lightbox)

9. 만들기 프로세스가 완료되면 설치된 가상 장치 목록에 실행할 준비가 된 새 장치와 **시작** 단추가 표시됩니다.

   [![실행할 준비가 된 새로 생성된 장치](device-manager-images/win/17-created-device-sml.png)](device-manager-images/win/17-created-device.png#lightbox)


### <a name="edit-device"></a>장치 편집

기존 가상 장치를 편집하려면 장치를 선택하고 (화면 오른쪽 상단에 있는) **편집** 단추를 클릭합니다.

[![새 장치를 수정하기 위한 편집 단추](device-manager-images/win/19-edit-button-sml.png)](device-manager-images/win/19-edit-button.png#lightbox)

**편집**을 클릭하면 선택된 가상 장치에 대한 장치 편집기가 실행됩니다.

[![장치 편집기 화면](device-manager-images/win/20-device-editor-sml.png)](device-manager-images/win/20-device-editor.png#lightbox)

**장치 편집기** 화면에서 첫 번째 열에는 가상 장치 속성이 나열되고, 두 번째 열에는 각 속성의 해당 값이 나열됩니다. 속성을 선택하면 속성에 대한 자세한 설명이 오른쪽에 표시됩니다.

예를 들어 다음 스크린샷에서 `hw.lcd.density` 속성은 **420**에서 **240**으로 변경됩니다.

[![장치 편집 예제](device-manager-images/win/21-device-editing-sml.png)](device-manager-images/win/21-device-editing.png#lightbox)

필요한 구성을 변경했으면 **저장** 단추를 클릭합니다.
가상 장치 속성을 변경하는 방법에 대한 자세한 내용은 [Android 가상 장치 속성 편집](~/android/get-started/installation/android-emulator/device-properties.md)을 참조하세요.

 
### <a name="additional-options"></a>추가 옵션

장치를 사용하기 위한 추가 옵션은 오른쪽 위에 있는 &hellip; 메뉴에서 사용할 수 있습니다.

[![추가 옵션 메뉴의 위치](device-manager-images/win/22-overflow-menu-sml.png)](device-manager-images/win/22-overflow-menu.png#lightbox)

추가 옵션 메뉴에는 다음 항목이 포함되어 있습니다.

-   **복제 및 편집** &ndash; 현재 선택된 장치를 복제하고 다른 고유한 이름을 사용하여 **새 장치** 화면에서 엽니다. 예를 들어 **VisualStudio_android-23_x86_phone**을 선택하고 **복제 및 편집**을 클릭하면 이름에 카운터가 추가됩니다.

    [![복제 및 편집 화면](device-manager-images/win/23-dupe-and-edit-sml.png)](device-manager-images/win/23-dupe-and-edit.png#lightbox)

-   **탐색기에 표시** &ndash; Windows 탐색기 창에 가상 장치에 대한 파일이 들어 있는 폴더가 열립니다. 예를 들어 **Nexus 5X API 25**를 선택하고 **탐색기에 표시**를 클릭하면 다음과 같은 창이 열립니다.

    [![탐색기에 표시 클릭 후 결과](device-manager-images/win/24-reveal-in-explorer-sml.png)](device-manager-images/win/24-reveal-in-explorer.png#lightbox)

-   **공장 재설정** &ndash; 선택된 장치를 기본 설정으로 재설정하여 장치가 실행 중일 때 사용자가 변경한 장치의 내부 상태에 대해 모든 내용을 지웁니다(있는 경우 현재 [빠른 부팅](~/android/deploy-test/debugging/android-sdk-emulator/running-the-emulator.md#quick-boot) 스냅숏도 지웁니다). 생성 및 편집 중에 가상 장치에서 수정된 내용은 이러한 변경의 영향을 받지 않습니다. 이러한 재설정을 수행할 수 없음을 알리는 대화 상자가 표시됩니다. **사용자 데이터 지우기**를 클릭하여 재설정을 확인합니다.

-   **삭제** &ndash; 선택된 가상 장치를 영구적으로 삭제합니다.
    장치 삭제는 실행 취소할 수 없음을 알리는 대화 상자가 표시됩니다. 장치를 삭제하려는 것이 확실한 경우 **삭제**를 클릭합니다.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

이 문서에서는 Android Device Manager를 사용하여 Android 가상 장치를 만들고, 복제하고, 사용자 지정하고, 실행하는 방법을 설명합니다.

[![장치 탭에 있는 Android Device Manager의 스크린샷](device-manager-images/mac/01-devices-dialog-sml.png)](device-manager-images/mac/01-devices-dialog.png#lightbox)

> [!NOTE]
> 이 가이드는 Mac용 Visual Studio에만 적용됩니다.
Xamarin Studio는 Android Device Manager와 호환되지 않습니다.

Android Device Manager를 사용하여 [Google Android Emulator](~/android/deploy-test/debugging/android-sdk-emulator/index.md)에서 실행되는 AVD(*Android 가상 장치*)를 만들고 구성합니다.
각 AVD는 실제 Android 장치를 시뮬레이션하는 에뮬레이터 구성입니다. 이를 통해 여러 실제 Android 장치를 시뮬레이션하는 다양한 구성에서 앱을 실행하고 테스트할 수 있습니다.

## <a name="requirements"></a>요구 사항

- Mac용 Visual Studio 7.5 이상

- Android SDK Manager를 통해 Android SDK 8.0(API 26) 이상을 설치해야 합니다.


## <a name="launching-the-device-manager"></a>장치 관리자 실행

**도구 > 장치 관리자**를 클릭하여 Android Device Manager를 시작합니다.

[![도구 메뉴에서 실행](device-manager-images/mac/16-tools-menu-sml.png)](device-manager-images/mac/16-tools-menu.png#lightbox)

Android Device Manager를 사용하려면 Android SDK Tools 버전 26.0.2 이상을 설치해야 합니다. Android SDK Tools 26.0.2 이상이 설치되어 있지 않으면 실행 시 이러한 오류 대화 상자가 표시됩니다.

![Android SDK 인스턴스 오류 대화 상자](device-manager-images/mac/02-sdk-instance-error.png)

이 오류 대화 상자가 나타나면 **확인**을 클릭하여 Android SDK Manager를 엽니다. Android SDK Manager에서 **도구** 탭을 클릭하고 다음을 설치합니다.

-   **Android SDK Tools 26.0.2** 이상 
-   **Android SDK 플랫폼 도구 26.0.0** 이상 
-   **Android SDK 빌드 도구 26.0.0** 이상

이러한 패키지는 다음 스크린샷에 표시된 대로 **설치됨** 상태로 표시됩니다.

[![Android SDK Tools 26.0 설치](device-manager-images/mac/03-sdk-tools-sml.png)](device-manager-images/mac/03-sdk-tools.png#lightbox)

## <a name="main-screen"></a>주 화면

Android 장치 관리자를 처음 실행하면 현재 구성된 모든 가상 장치가 화면에 표시됩니다. 각 장치의 **이름**, **시스템 이미지**(Android API 수준), **CPU**, **메모리** 크기 및 화면 해상도가 표시됩니다.

[![설치된 장치의 목록 및 매개 변수](device-manager-images/mac/05-devices-list-sml.png)](device-manager-images/mac/05-devices-list.png#lightbox)

**재생** 단추를 클릭하여 원하는 가상 장치로 에뮬레이터를 실행합니다.

[![장치 이미지에 대한 시작 단추](device-manager-images/mac/06-start-button-sml.png)](device-manager-images/mac/06-start-button.png#lightbox)

선택한 가상 장치로 에뮬레이터가 시작되면 **재생** 단추가 에뮬레이터를 중지하는 데 사용할 수 있는 **중지** 단추로 변경됩니다.

[![실행 중인 장치에 대한 중지 단추](device-manager-images/mac/07-stop-button-sml.png)](device-manager-images/mac/07-stop-button.png#lightbox)

### <a name="new-device"></a>새 장치

새 장치를 만들려면 **새 장치** 단추(화면의 오른쪽 상단에 있음)를 클릭합니다.

[![새 장치를 만드는 데 사용되는 새로 만들기 단추](device-manager-images/mac/08-new-button-sml.png)](device-manager-images/mac/08-new-button.png#lightbox)

**새 장치**를 클릭하면 **새 장치** 화면이 열립니다.

[![장치 관리자의 새 장치 화면](device-manager-images/mac/09-new-device-editor-sml.png)](device-manager-images/mac/09-new-device-editor.png#lightbox)

**새 장치** 화면에서 새 장치를 구성하려면 다음 단계를 따르세요.

1. **장치** 풀 다운 메뉴를 클릭하여 에뮬레이트할 실제 장치를 선택합니다.

    [![장치 풀 다운 메뉴](device-manager-images/mac/10-device-menu-sml.png)](device-manager-images/mac/10-device-menu.png#lightbox)

2. **시스템 이미지** 풀 다운 메뉴를 클릭하여 이 가상 장치와 함께 사용할 시스템 이미지를 선택합니다. 이 메뉴는 **설치됨** 아래에 설치된 시스템 장치 관리자 이미지를 나열합니다. **다운로드** 섹션에는(표시되는 경우) 개발 컴퓨터에서 현재 사용할 수 없지만 자동으로 설치될 수 있는 시스템 장치 관리자 이미지가 나열됩니다.

    [![시스템 이미지 풀 다운 메뉴](device-manager-images/mac/11-system-image-menu-sml.png)](device-manager-images/mac/11-system-image-menu.png#lightbox)

3. 장치에 새 이름을 지정합니다. 다음 예제에서는 새 장치에 **Nexus 5X API 25**라는 이름을 지정합니다.

    [![새 장치 이름 지정](device-manager-images/mac/12-device-name-sml.png)](device-manager-images/mac/12-device-name.png#lightbox)

4. 수정해야 하는 모든 속성을 편집합니다. 속성을 변경하려면 [Android 가상 장치 속성 편집](~/android/get-started/installation/android-emulator/device-properties.md)을 참조하세요.

5. 명시적으로 설정해야 하는 추가 속성을 추가합니다. **새 장치** 화면에는 가장 자주 수정되는 속성만 나열되지만 (왼쪽 하단에 있는) **속성 추가** 풀 다운 메뉴를 클릭하여 추가 속성을 추가할 수 있습니다.

    [![속성 추가 풀 다운 메뉴](device-manager-images/mac/13-add-property-menu-sml.png)](device-manager-images/mac/13-add-property-menu.png#lightbox)

6. **사용자 지정**을 클릭하여 장치에 대한 새 속성을 정의할 수도 있습니다.

    ![만들기 단추](device-manager-images/mac/14-custom-button.png)

7. **만들기** 단추(오른쪽 하단)를 클릭하여 새 장치를 만듭니다.

    ![만들기 단추](device-manager-images/mac/15-create-button.png)

8. **라이선스 승인** 화면이 표시될 수 있습니다. 사용 조건에 동의하면 **동의**를 클릭합니다.

9. Android Device Manager는 장치 생성 중 **만드는 중** 진행률 표시기를 표시하는 동안 설치된 가상 장치 목록에 새 장치를 추가합니다.

    [![만들기 진행률 표시기](device-manager-images/mac/17-creating-the-device-sml.png)](device-manager-images/mac/17-creating-the-device.png#lightbox)

10. 만들기 프로세스가 완료되면 장치 목록에 실행할 준비가 된 새 장치와 **재생** 단추가 표시됩니다.

   [![실행할 준비가 된 새로 생성된 장치](device-manager-images/mac/18-created-device-sml.png)](device-manager-images/mac/18-created-device.png#lightbox)


### <a name="edit-device"></a>장치 편집

기존 가상 장치를 편집하려면 **추가 옵션** 풀 다운 메뉴(기어 아이콘)을 선택하고 **편집**을 선택합니다.
 
[![새 장치를 수정하기 위한 편집 메뉴 선택](device-manager-images/mac/19-edit-button-sml.png)](device-manager-images/mac/19-edit-button.png#lightbox)

**편집**을 클릭하면 선택된 가상 장치에 대한 장치 편집기가 실행됩니다.
 
[![장치 편집기 화면](device-manager-images/mac/20-device-editor-sml.png)](device-manager-images/mac/20-device-editor.png#lightbox)

**장치 편집기** 화면에서 첫 번째 열에는 가상 장치 속성이 나열되고, 두 번째 열에는 각 속성의 해당 값이 나열됩니다. 속성을 선택하면 속성에 대한 자세한 설명이 오른쪽에 표시됩니다.

예를 들어 다음 스크린샷에서 `hw.lcd.density` 속성은 **320**에서 **240**으로 변경되고, `hw.ramSize` 속성은 **768**로 변경됩니다.
 
[![장치 편집 예제](device-manager-images/mac/21-device-editing-sml.png)](device-manager-images/mac/21-device-editing.png#lightbox)

필요한 구성을 변경했으면 **저장** 단추를 클릭합니다.
가상 장치 속성을 변경하는 방법에 대한 자세한 내용은 [Android 가상 장치 속성 편집](~/android/get-started/installation/android-emulator/device-properties.md)을 참조하세요.

 
### <a name="additional-options"></a>추가 옵션

장치를 사용하기 위한 추가 옵션은 **재생** 단추의 왼쪽에 있는 풀 다운 메뉴에서 사용할 수 있습니다.

[![추가 옵션 메뉴의 위치](device-manager-images/mac/22-overflow-menu-sml.png)](device-manager-images/mac/22-overflow-menu.png#lightbox)

추가 옵션 메뉴에는 다음 항목이 포함되어 있습니다.

-   **편집** &ndash; 앞에서 설명한 대로 현재 선택된 장치를 장치 편집기에서 엽니다.

-   **복제 및 편집** &ndash; 현재 선택된 장치를 복제하고 다른 고유한 이름을 사용하여 **새 장치** 화면에서 엽니다.
    예를 들어 **Nexus 5X API 25**를 선택하고 **복제 및 편집**을 클릭하면 이름에 카운터가 추가됩니다.

    [![복제 및 편집 화면](device-manager-images/mac/23-dupe-and-edit-sml.png)](device-manager-images/mac/23-dupe-and-edit.png#lightbox)

-   **Finder에 표시** &ndash; macOS Finder 창에 가상 장치에 대한 파일이 들어 있는 폴더가 열립니다. 예를 들어 **Nexus 5X API 25**를 선택하고 **Finder에 표시**를 클릭하면 다음과 같은 창이 열립니다.

    [![탐색기에 표시 클릭 후 결과](device-manager-images/mac/24-reveal-in-finder-sml.png)](device-manager-images/mac/24-reveal-in-finder.png#lightbox)

-   **공장 재설정** &ndash; 선택된 장치를 기본 설정으로 재설정하여 장치가 실행 중일 때 사용자가 변경한 장치의 내부 상태에 대해 모든 내용을 지웁니다. 생성 및 편집 중에 가상 장치에서 수정된 내용은 이러한 변경의 영향을 받지 않습니다. 이러한 재설정을 수행할 수 없음을 알리는 대화 상자가 표시됩니다. **사용자 데이터 지우기**를 클릭하여 재설정을 확인합니다.

-   **삭제** &ndash; 선택된 가상 장치를 영구적으로 삭제합니다.
    장치 삭제는 실행 취소할 수 없음을 알리는 대화 상자가 표시됩니다. 장치를 삭제하려는 것이 확실한 경우 **삭제**를 클릭합니다.

-----

## <a name="summary"></a>요약

이 가이드에서는 Mac용 Visual Studio 및 Visual Studio Tools for Xamarin에서 사용할 수 있는 Android Device Manager를 소개했습니다. Android 에뮬레이터를 시작 및 중지하고, 실행할 AVD(Android 가상 장치)를 선택하고, 새 가상 장치를 만드는 기능과 같은 필수 기능과 가상 장치를 편집하는 방법을 설명했습니다. 또한 추가 사용자 지정을 위해 프로필 하드웨어 속성을 편집하는 방법을 설명 했습니다.


## <a name="related-links"></a>관련 링크

- [Android SDK Tools에 대한 변경 내용](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Android SDK 에뮬레이터를 사용하여 디버깅](~/android/deploy-test/debugging/android-sdk-emulator/index.md)
- [SDK Tools 릴리스 정보(Google)](https://developer.android.com/studio/releases/sdk-tools)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
