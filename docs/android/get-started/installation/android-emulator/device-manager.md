---
title: Android Device Manager를 사용하여 가상 장치 관리
description: 이 문서에서는 Android Device Manager를 사용하여 물리적 Android 장치를 에뮬레이트하는 AVD(Android 가상 장치)를 만들고 구성하는 방법을 설명합니다. 물리적 장치에 의존하지 않고도 이러한 가상 장치를 사용하여 앱을 실행하고 테스트할 수 있습니다.
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: ECB327F3-FF1C-45CC-9FA6-9C11032BD5EF
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/06/2018
ms.openlocfilehash: 8cf11056881af6fd622cae901d518fe27f61d08f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115395"
---
# <a name="managing-virtual-devices-with-the-android-device-manager"></a>Android Device Manager를 사용하여 가상 장치 관리

_이 문서에서는 Android Device Manager를 사용하여 물리적 Android 장치를 에뮬레이트하는 AVD(Android 가상 장치)를 만들고 구성하는 방법을 설명합니다. 물리적 장치에 의존하지 않고도 이러한 가상 장치를 사용하여 앱을 실행하고 테스트할 수 있습니다._

(에뮬레이터 성능에 대한 [하드웨어 가속](~/android/get-started/installation/android-emulator/hardware-acceleration.md)에 설명된 대로) 하드웨어 가속이 활성화된 것을 확인한 후에 다음 단계는 _Android Device Manager_(_Xamarin Android Device Manager_라고도 함)를 사용하여 앱을 테스트하고 디버깅하는 데 사용할 수 있는 가상 장치를 만드는 것입니다.

::: zone pivot="windows"

## <a name="android-device-manager-on-windows"></a>Windows의 Android Device Manager

이 문서에서는 Android Device Manager를 사용하여 Android 가상 장치를 만들고, 복제하고, 사용자 지정하고, 실행하는 방법을 설명합니다.

[![장치 탭에 있는 Android Device Manager의 스크린샷](device-manager-images/win/01-devices-dialog-sml.png)](device-manager-images/win/01-devices-dialog.png#lightbox)

Android Device Manager를 사용하여 [Android Emulator](~/android/deploy-test/debugging/debug-on-emulator.md)에서 실행되는 AVD(_Android 가상 장치_)를 만들고 구성합니다.
각 AVD는 실제 Android 장치를 시뮬레이션하는 에뮬레이터 구성입니다. 이를 통해 여러 실제 Android 장치를 시뮬레이션하는 다양한 구성에서 앱을 실행하고 테스트할 수 있습니다.

## <a name="requirements"></a>요구 사항

Android Device Manager를 사용하려면 다음 항목이 필요합니다.

- Visual Studio 2017 버전 15.8 이상이 필요합니다. Visual Studio Community, Professional 및 Enterprise 버전이 지원됩니다.

- Visual Studio Tools for Xamarin 버전 4.9 이상

- Android SDK를 설치해야 합니다([Xamarin.Android에 대한 Android SDK 설정](~/android/get-started/installation/android-sdk.md) 참조).
  아직 설치되지 않은 경우 **C:\\Program Files(x86)\\Android\\android-sdk**의 기본 위치에 Android SDK를 설치해야 합니다.

- [Android SDK Manager](~/android/get-started/installation/android-sdk.md)를 통해 다음 패키지를 설치해야 합니다. 
    - **Android SDK Tools 버전 26.1.1** 이상
    - **Android SDK 플랫폼 도구 27.0.1** 이상
    - **Android SDK 빌드 도구 27.0.3** 이상 
    - **Android Emulator 27.2.7** 이상. 

  이러한 패키지는 다음 스크린샷에 표시된 대로 **설치됨** 상태로 표시됩니다.

  [![Android SDK Tools 설치](device-manager-images/win/02-sdk-tools-sml.png)](device-manager-images/win/02-sdk-tools.png#lightbox)


## <a name="launching-the-device-manager"></a>장치 관리자 실행

**도구 > Android > Android Device Manager**를 클릭하여 **도구** 메뉴에서 Android Device Manager를 시작합니다.

[![도구 메뉴에서 장치 관리자 시작](device-manager-images/win/03-tools-menu-sml.png)](device-manager-images/win/03-tools-menu.png#lightbox)

시작 시 다음 오류 대화 상자가 표시되면 [문제 해결](#troubleshooting) 섹션에서 해결 방법 지침을 참조하세요.

![Android SDK 인스턴스 오류 대화 상자](device-manager-images/win/04-sdk-error.png)


## <a name="main-screen"></a>주 화면

Android 장치 관리자를 처음 실행하면 현재 구성된 모든 가상 장치가 화면에 표시됩니다. 각 가상 장치에 대해 **이름**, **OS**(Android 버전), **프로세서**, **메모리** 크기 및 화면 **해상도**가 표시됩니다.

[![설치된 장치의 목록 및 매개 변수](device-manager-images/win/05-installed-list-sml.png)](device-manager-images/win/05-installed-list.png#lightbox)

목록에서 장치를 선택하면 **시작** 단추가 오른쪽에 나타납니다. **시작** 단추를 클릭하면 이 가상 장치로 에뮬레이터를 시작할 수 있습니다.

[![장치 이미지에 대한 시작 단추](device-manager-images/win/06-start-button-sml.png)](device-manager-images/win/06-start-button.png#lightbox)

선택한 가상 장치로 에뮬레이터가 시작되면 **시작** 단추가 에뮬레이터를 중지하는 데 사용할 수 있는 **중지** 단추로 변경됩니다.

[![실행 중인 장치에 대한 중지 단추](device-manager-images/win/07-stop-button-sml.png)](device-manager-images/win/07-stop-button.png#lightbox)

### <a name="new-device"></a>새 장치

새 장치를 만들려면 **새로 만들기** 단추(화면의 오른쪽 상단에 있음)를 클릭합니다.

[![새 장치를 만드는 데 사용되는 새로 만들기 단추](device-manager-images/win/08-new-button-sml.png)](device-manager-images/win/08-new-button.png#lightbox)

**새로 만들기**를 클릭하면 **새 장치** 화면이 열립니다.

[![장치 관리자의 새 장치 화면](device-manager-images/win/09-new-device-editor-sml.png)](device-manager-images/win/09-new-device-editor.png#lightbox)

**새 장치** 화면에서 새 장치를 구성하려면 다음 단계를 따르세요.

1. 장치에 새 이름을 지정합니다. 다음 예제에서는 새 장치에 **Pixel_API_27**이라는 이름을 지정합니다.

   [![새 장치 이름 지정](device-manager-images/win/10-device-name-sml.png)](device-manager-images/win/10-device-name.png#lightbox)

2. **기본 장치** 풀 다운 메뉴를 클릭하여 에뮬레이트할 실제 장치를 선택합니다.

   [![에뮬레이트할 실제 장치 선택](device-manager-images/win/11-device-menu-sml.png)](device-manager-images/win/11-device-menu.png#lightbox)

3. **프로세서** 풀 다운 메뉴를 클릭하여 이 가상 장치의 프로세서 유형을 선택합니다. **x86**을 선택하면 에뮬레이터가 [하드웨어 가속](~/android/get-started/installation/android-emulator/hardware-acceleration.md)을 활용할 수 있으므로 최상의 성능을 제공합니다.
   **x86_64** 옵션도 하드웨어 가속을 사용하지만 **x86**보다 약간 느리게 실행됩니다(**x86_64**는 일반적으로 64비트 응용 프로그램 테스트를 위해 사용).

   [![프로세서 유형 선택](device-manager-images/win/12-processor-type-menu-sml.png)](device-manager-images/win/12-processor-type-menu.png#lightbox)

4. **OS** 풀 다운 메뉴를 클릭하여 Android 버전(API 수준)을 선택합니다. 예를 들어 **Oreo 8.1 - API 27**을 선택하여 API 레벨 27에 대한 가상 장치를 만듭니다.

   [![Android 버전 선택](device-manager-images/win/13-android-version-w158-sml.png)](device-manager-images/win/13-android-version-w158.png#lightbox)

   아직 설치되지 않은 Android API 수준을 선택하면 Device Manager에 &ndash; 화면의 아래쪽의 **새 장치가 다운로드 됨** 메시지가 표시됩니다. 새 가상 장치를 만들 때 필요한 파일을 다운로드하고 설치합니다.

   ![새 장치 이미지가 다운로드됩니다.](device-manager-images/win/14-automatic-download-w158.png)

5. 가상 장치에 Google Play Services API를 포함하려면 **Google API** 옵션을 사용하도록 설정합니다. Google Play 스토어 앱을 포함하려면 **Google Play 스토어** 옵션을 사용하도록 설정합니다.

   [![Google Play 서비스 및 Google Play 스토어 선택](device-manager-images/win/15-google-play-services-sml.png)](device-manager-images/win/15-google-play-services.png#lightbox)

   Google Play 스토어 이미지는 픽셀, 픽셀 2, Nexus 5 및 Nexus 5X와 같은 몇 가지 기본 장치 유형에만 사용할 수 있습니다.

6. 수정해야 하는 모든 속성을 편집합니다. 속성을 변경하려면 [Android 가상 장치 속성 편집](~/android/get-started/installation/android-emulator/device-properties.md)을 참조하세요.

7. 명시적으로 설정해야 하는 추가 속성을 추가합니다. **새 장치** 화면에는 가장 일반적으로 수정되는 속성만 나열되지만 **속성 추가** 풀 다운 메뉴(하단에서)를 클릭하여 추가 속성을 추가할 수 있습니다.

   [![속성 추가 풀 다운 메뉴](device-manager-images/win/16-add-property-menu-sml.png)](device-manager-images/win/16-add-property-menu.png#lightbox)

    속성 목록 맨 위에 있는 **사용자 지정...** 을 선택하여 사용자 지정 속성을 정의할 수도 있습니다.

8. **만들기** 단추(오른쪽 하단)를 클릭하여 새 장치를 만듭니다.

   [![만들기 단추](device-manager-images/win/17-create-button-sml.png)](device-manager-images/win/17-create-button.png#lightbox)

9. **라이선스 승인** 화면이 표시될 수 있습니다. 사용 조건에 동의하면 **동의**를 클릭합니다.

   [![라이선스 승인 화면](device-manager-images/win/18-license-acceptance-sml.png)](device-manager-images/win/18-license-acceptance.png#lightbox)

10. Android Device Manager는 장치 생성 중 **만드는 중** 진행률 표시기를 표시하는 동안 설치된 가상 장치 목록에 새 장치를 추가합니다.

    [![만들기 진행률 표시기](device-manager-images/win/19-creating-the-device-sml.png)](device-manager-images/win/19-creating-the-device.png#lightbox)

11. 만들기 프로세스가 완료되면 설치된 가상 장치 목록에 실행할 준비가 된 새 장치와 **시작** 단추가 표시됩니다.

    [![실행할 준비가 된 새로 생성된 장치](device-manager-images/win/20-created-device-sml.png)](device-manager-images/win/20-created-device.png#lightbox)


### <a name="edit-device"></a>장치 편집

기존 가상 장치를 편집하려면 장치를 선택하고 (화면 오른쪽 상단에 있는) **편집** 단추를 클릭합니다.

[![장치를 수정하기 위한 편집 단추](device-manager-images/win/21-edit-button-sml.png)](device-manager-images/win/21-edit-button.png#lightbox)

**편집**을 클릭하면 선택된 가상 장치에 대한 장치 편집기가 실행됩니다.

[![장치 편집기 화면](device-manager-images/win/22-device-editor-sml.png)](device-manager-images/win/22-device-editor.png#lightbox)

**장치 편집기** 화면에는 가상 장치의 속성이 **값** 열에 각 속성의 해당 값과 함께 **속성** 열 아래의 나열됩니다. 속성을 선택하면 속성에 대한 자세한 설명이 오른쪽에 표시됩니다.

속성을 변경하려면 **값** 열에서 해당 값을 편집합니다.
예를 들어 다음 스크린샷에서 `hw.lcd.density` 속성은 **480**에서 **240**으로 변경됩니다.

[![장치 편집 예제](device-manager-images/win/23-device-editing-sml.png)](device-manager-images/win/23-device-editing.png#lightbox)

필요한 구성을 변경했으면 **저장** 단추를 클릭합니다.
가상 장치 속성을 변경하는 방법에 대한 자세한 내용은 [Android 가상 장치 속성 편집](~/android/get-started/installation/android-emulator/device-properties.md)을 참조하세요.


### <a name="additional-options"></a>추가 옵션

장치를 사용하기 위한 추가 옵션은 오른쪽 위에 있는 **추가 옵션**(&hellip;) 풀 다운 메뉴에서 사용할 수 있습니다.

[![추가 옵션 메뉴의 위치](device-manager-images/win/24-overflow-menu-sml.png)](device-manager-images/win/24-overflow-menu.png#lightbox)

추가 옵션 메뉴에는 다음 항목이 포함되어 있습니다.

- **복제 및 편집** &ndash; 현재 선택된 장치를 복제하고 다른 고유한 이름을 사용하여 **새 장치** 화면에서 엽니다. 예를 들어 **Pixel_API_27**을 선택하고 **복제 및 편집**을 클릭하면 이름에 카운터가 추가됩니다.

  [![복제 및 편집 화면](device-manager-images/win/25-dupe-and-edit-sml.png)](device-manager-images/win/25-dupe-and-edit.png#lightbox)

- **탐색기에 표시** &ndash; Windows 탐색기 창에 가상 장치에 대한 파일이 들어 있는 폴더가 열립니다. 예를 들어 **Pixel_API_27**을 선택하고 **탐색기에 표시**를 클릭하면 다음 예제와 같은 창이 열립니다.

  [![탐색기에 표시 클릭 후 결과](device-manager-images/win/26-reveal-in-explorer-sml.png)](device-manager-images/win/26-reveal-in-explorer.png#lightbox)

- **공장 재설정** &ndash; 선택된 장치를 기본 설정으로 재설정하여 장치가 실행 중일 때 사용자가 변경한 장치의 내부 상태에 대해 모든 내용을 지웁니다(있는 경우 현재 [빠른 부팅](~/android/deploy-test/debugging/debug-on-emulator.md#quick-boot) 스냅숏도 지웁니다). 생성 및 편집 중에 가상 장치에서 수정된 내용은 이러한 변경의 영향을 받지 않습니다. 이러한 재설정을 수행할 수 없음을 알리는 대화 상자가 표시됩니다. **공장 재설정**을 클릭하여 재설정을 확인합니다.

  ![공장 재설정 대화 상자](device-manager-images/win/27-factory-reset.png)

- **삭제** &ndash; 선택된 가상 장치를 영구적으로 삭제합니다. 장치 삭제는 실행 취소할 수 없음을 알리는 대화 상자가 표시됩니다. 장치를 삭제하려는 것이 확실한 경우 **삭제**를 클릭합니다.

  ![장치 삭제 대화 상자](device-manager-images/win/28-delete-device-w158.png)


::: zone-end
::: zone pivot="macos"

## <a name="android-device-manager-on-macos"></a>macOS의 Android Device Manager

이 문서에서는 Android Device Manager를 사용하여 Android 가상 장치를 만들고, 복제하고, 사용자 지정하고, 실행하는 방법을 설명합니다.

[![장치 탭에 있는 Android Device Manager의 스크린샷](device-manager-images/mac/01-devices-dialog-sml.png)](device-manager-images/mac/01-devices-dialog.png#lightbox)

> [!NOTE]
> 이 가이드는 Mac용 Visual Studio에만 적용됩니다.
Xamarin Studio는 Android Device Manager와 호환되지 않습니다.

Android Device Manager를 사용하여 [Android Emulator](~/android/deploy-test/debugging/debug-on-emulator.md)에서 실행되는 AVD(*Android 가상 장치*)를 만들고 구성합니다.
각 AVD는 실제 Android 장치를 시뮬레이션하는 에뮬레이터 구성입니다. 이를 통해 여러 실제 Android 장치를 시뮬레이션하는 다양한 구성에서 앱을 실행하고 테스트할 수 있습니다.

## <a name="requirements"></a>요구 사항

Android Device Manager를 사용하려면 다음 항목이 필요합니다.

- Mac용 Visual Studio 7.6 이상.

- Android SDK를 설치해야 합니다([Xamarin.Android에 대한 Android SDK 설정](~/android/get-started/installation/android-sdk.md) 참조).

- [Android SDK Manager](~/android/get-started/installation/android-sdk.md)를 통해 다음 패키지를 설치해야 합니다. 
    -  **SDK 도구 버전 26.1.1** 이상
    -  **Android SDK 플랫폼 도구 28.0.1** 이상 
    -  **Android SDK 빌드 도구 26.0.3** 이상

  이러한 패키지는 다음 스크린샷에 표시된 대로 **설치됨** 상태로 표시됩니다.

  [![Android SDK Tools 설치](device-manager-images/mac/02-sdk-tools-sml.png)](device-manager-images/mac/02-sdk-tools.png#lightbox)


## <a name="launching-the-device-manager"></a>장치 관리자 실행

**도구 > 장치 관리자**를 클릭하여 Android Device Manager를 시작합니다.

[![도구 메뉴에서 장치 관리자 시작](device-manager-images/mac/03-tools-menu-sml.png)](device-manager-images/mac/03-tools-menu.png#lightbox)

시작 시 다음 오류 대화 상자가 표시되면 [문제 해결](#troubleshooting) 섹션에서 해결 방법 지침을 참조하세요.

![Android SDK 인스턴스 오류 대화 상자](device-manager-images/mac/04-sdk-instance-error.png)


## <a name="main-screen"></a>주 화면

Android 장치 관리자를 처음 실행하면 현재 구성된 모든 가상 장치가 화면에 표시됩니다. 각 가상 장치에 대해 **이름**, **OS**(Android 버전), **프로세서**, **메모리** 크기 및 화면 **해상도**가 표시됩니다.

[![설치된 장치의 목록 및 매개 변수](device-manager-images/mac/05-devices-list-sml.png)](device-manager-images/mac/05-devices-list.png#lightbox)

목록에서 장치를 선택하면 **재생** 단추가 오른쪽에 나타납니다. **재생** 단추를 클릭하면 이 가상 장치로 에뮬레이터를 시작할 수 있습니다.

[![장치 이미지에 대한 재생 단추](device-manager-images/mac/06-start-button-sml.png)](device-manager-images/mac/06-start-button.png#lightbox)

선택한 가상 장치로 에뮬레이터가 시작되면 **재생** 단추가 에뮬레이터를 중지하는 데 사용할 수 있는 **중지** 단추로 변경됩니다.

[![실행 중인 장치에 대한 중지 단추](device-manager-images/mac/07-stop-button-sml.png)](device-manager-images/mac/07-stop-button.png#lightbox)

에뮬레이터를 중지하면 다음 빠른 부팅을 위해 현재 상태를 저장할지 묻는 메시지가 표시될 수 있습니다.

![빠른 부팅 대화 상자의 현재 상태 저장](device-manager-images/mac/08-save-for-quick-boot-m76.png)

현재 상태를 저장하면 이 가상 장치를 다시 시작할 때 에뮬레이터가 더 빨리 부팅됩니다. 빠른 부팅에 대한 자세한 내용은 [빠른 부팅](~/android/deploy-test/debugging/debug-on-emulator.md#quick-boot)을 참조하세요.

### <a name="new-device"></a>새 장치

새 장치를 만들려면 **새 장치** 단추(화면의 왼쪽 상단에 있음)를 클릭합니다.

[![새 장치를 만드는 데 사용되는 새로 만들기 단추](device-manager-images/mac/09-new-button-sml.png)](device-manager-images/mac/09-new-button.png#lightbox)

**새 장치**를 클릭하면 **새 장치** 화면이 열립니다.

[![장치 관리자의 새 장치 화면](device-manager-images/mac/10-new-device-editor-sml.png)](device-manager-images/mac/10-new-device-editor.png#lightbox)

**새 장치** 화면에서 새 장치를 구성하려면 다음 단계를 따르세요.

1. 장치에 새 이름을 지정합니다. 다음 예제에서는 새 장치에 **Pixel_API_27**이라는 이름을 지정합니다.

   [![새 장치 이름 지정](device-manager-images/mac/11-device-name-m76-sml.png)](device-manager-images/mac/11-device-name-m76.png#lightbox)

2. **기본 장치** 풀 다운 메뉴를 클릭하여 에뮬레이트할 실제 장치를 선택합니다.

   [![에뮬레이트할 실제 장치 선택](device-manager-images/mac/12-device-menu-m76-sml.png)](device-manager-images/mac/12-device-menu-m76.png#lightbox)

3. **프로세서** 풀 다운 메뉴를 클릭하여 이 가상 장치의 프로세서 유형을 선택합니다. **x86**을 선택하면 에뮬레이터가 [하드웨어 가속](~/android/get-started/installation/android-emulator/hardware-acceleration.md)을 활용할 수 있으므로 최상의 성능을 제공합니다.
   **x86_64** 옵션도 하드웨어 가속을 사용하지만 **x86**보다 약간 느리게 실행됩니다(**x86_64**는 일반적으로 64비트 응용 프로그램 테스트를 위해 사용).

   [![프로세서 유형 선택](device-manager-images/mac/13-processor-type-menu-m76-sml.png)](device-manager-images/mac/13-processor-type-menu-m76.png#lightbox)

4. **OS** 풀 다운 메뉴를 클릭하여 Android 버전(API 수준)을 선택합니다. 예를 들어 **Oreo 8.1 - API 27**을 선택하여 API 레벨 27에 대한 가상 장치를 만듭니다.

   [![Android 버전 선택](device-manager-images/mac/14-android-screenshot-m76-sml.png)](device-manager-images/mac/14-android-screenshot-m76.png#lightbox)

   아직 설치되지 않은 Android API 수준을 선택하면 Device Manager에 &ndash; 화면의 아래쪽의 **새 장치가 다운로드 됨** 메시지가 표시됩니다. 새 가상 장치를 만들 때 필요한 파일을 다운로드하고 설치합니다.

   ![새 장치 이미지가 다운로드됩니다.](device-manager-images/mac/15-automatic-download-m76.png)

5. 가상 장치에 Google Play Services API를 포함하려면 **Google API** 옵션을 사용하도록 설정합니다. Google Play 스토어 앱을 포함하려면 **Google Play 스토어** 옵션을 사용하도록 설정합니다.

   [![Google Play 서비스 및 Google Play 스토어 선택](device-manager-images/mac/16-google-play-services-m76-sml.png)](device-manager-images/mac/16-google-play-services-m76.png#lightbox)

   Google Play 스토어 이미지는 픽셀, 픽셀 2, Nexus 5 및 Nexus 5X와 같은 몇 가지 기본 장치 유형에만 사용할 수 있습니다.

6. 수정해야 하는 모든 속성을 편집합니다. 속성을 변경하려면 [Android 가상 장치 속성 편집](~/android/get-started/installation/android-emulator/device-properties.md)을 참조하세요.

7. 명시적으로 설정해야 하는 추가 속성을 추가합니다. **새 장치** 화면에는 가장 일반적으로 수정되는 속성만 나열되지만 **속성 추가** 풀 다운 메뉴(하단에서)를 클릭하여 추가 속성을 추가할 수 있습니다.

   [![속성 추가 풀 다운 메뉴](device-manager-images/mac/17-add-property-menu-m76-sml.png)](device-manager-images/mac/17-add-property-menu-m76.png#lightbox)

   이 속성 목록 맨 위에 있는 **사용자 지정...** 을 클릭하여 사용자 지정 속성을 정의할 수도 있습니다.

8. **만들기** 단추(오른쪽 하단)를 클릭하여 새 장치를 만듭니다.

   ![만들기 단추](device-manager-images/mac/18-create-button-m76.png)

9. Android Device Manager는 장치 생성 중 **만드는 중** 진행률 표시기를 표시하는 동안 설치된 가상 장치 목록에 새 장치를 추가합니다.

   [![만들기 진행률 indictator](device-manager-images/mac/19-creating-the-device-m76-sml.png)](device-manager-images/mac/19-creating-the-device-m76.png#lightbox)

10. 만들기 프로세스가 완료되면 설치된 가상 장치 목록에 실행할 준비가 된 새 장치와 **시작** 단추가 표시됩니다.

    [![실행할 준비가 된 새로 생성된 장치](device-manager-images/mac/20-created-device-m76-sml.png)](device-manager-images/mac/20-created-device-m76.png#lightbox)


### <a name="edit-device"></a>장치 편집

기존 가상 장치를 편집하려면 **추가 옵션** 풀 다운 메뉴(기어 아이콘)을 선택하고 **편집**을 선택합니다.

[![새 장치를 수정하기 위한 편집 메뉴 선택](device-manager-images/mac/21-edit-button-m76-sml.png)](device-manager-images/mac/21-edit-button-m76.png#lightbox)

**편집**을 클릭하면 선택된 가상 장치에 대한 장치 편집기가 실행됩니다.

[![장치 편집기 화면](device-manager-images/mac/22-device-editor-sml.png)](device-manager-images/mac/22-device-editor.png#lightbox)

**장치 편집기** 화면에는 가상 장치의 속성이 **값** 열에 각 속성의 해당 값과 함께 **속성** 열 아래의 나열됩니다. 속성을 선택하면 속성에 대한 자세한 설명이 오른쪽에 표시됩니다.

속성을 변경하려면 **값** 열에서 해당 값을 편집합니다.
예를 들어 다음 스크린샷에서 `hw.lcd.density` 속성은 **480**에서 **240**으로 변경됩니다.

[![장치 편집 예제](device-manager-images/mac/23-device-editing-sml.png)](device-manager-images/mac/23-device-editing.png#lightbox)

필요한 구성을 변경했으면 **저장** 단추를 클릭합니다.
가상 장치 속성을 변경하는 방법에 대한 자세한 내용은 [Android 가상 장치 속성 편집](~/android/get-started/installation/android-emulator/device-properties.md)을 참조하세요.


### <a name="additional-options"></a>추가 옵션

장치를 사용하기 위한 추가 옵션은 **재생** 단추의 왼쪽에 있는 풀 다운 메뉴에서 사용할 수 있습니다.

[![추가 옵션 메뉴의 위치](device-manager-images/mac/24-overflow-menu-sml.png)](device-manager-images/mac/24-overflow-menu.png#lightbox)

추가 옵션 메뉴에는 다음 항목이 포함되어 있습니다.

- **편집** &ndash; 앞에서 설명한 대로 현재 선택된 장치를 장치 편집기에서 엽니다.

- **복제 및 편집** &ndash; 현재 선택된 장치를 복제하고 다른 고유한 이름을 사용하여 **새 장치** 화면에서 엽니다. 예를 들어 **Pixel 2 API 28**을 선택하고 **복제 및 편집**을 클릭하면 이름에 카운터가 추가됩니다.

  [![복제 및 편집 화면](device-manager-images/mac/25-dupe-and-edit-sml.png)](device-manager-images/mac/25-dupe-and-edit.png#lightbox)

- **Finder에 표시** &ndash; macOS Finder 창에 가상 장치에 대한 파일이 들어 있는 폴더가 열립니다. 예를 들어 **Pixel 2 API 28**을 선택하고 **Finder에 표시**를 클릭하면 다음 예제와 같은 창이 열립니다.

  [![Finder에 표시 클릭 후 결과](device-manager-images/mac/26-reveal-in-finder-sml.png)](device-manager-images/mac/26-reveal-in-finder.png#lightbox)

- **공장 재설정** &ndash; 선택된 장치를 기본 설정으로 재설정하여 장치가 실행 중일 때 사용자가 변경한 장치의 내부 상태에 대해 모든 내용을 지웁니다(있는 경우 현재 [빠른 부팅](~/android/deploy-test/debugging/debug-on-emulator.md#quick-boot) 스냅숏도 지웁니다). 생성 및 편집 중에 가상 장치에서 수정된 내용은 이러한 변경의 영향을 받지 않습니다. 이러한 재설정을 수행할 수 없음을 알리는 대화 상자가 표시됩니다. **공장 재설정**을 클릭하여 재설정을 확인합니다.

  ![공장 재설정 대화 상자](device-manager-images/mac/27-factory-reset-m76.png)

- **삭제** &ndash; 선택된 가상 장치를 영구적으로 삭제합니다. 장치 삭제는 실행 취소할 수 없음을 알리는 대화 상자가 표시됩니다. 장치를 삭제하려는 것이 확실한 경우 **삭제**를 클릭합니다.

  ![장치 삭제 대화 상자](device-manager-images/mac/28-delete-device-m76.png)

-----


<a name="troubleshooting" />

## <a name="troubleshooting"></a>문제 해결

다음 섹션에서는 Android Device Manager를 사용하여 가상 장치를 구성할 때 발생할 수 있는 문제를 진단하고 해결하는 방법을 설명합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

### <a name="android-sdk-in-non-standard-location"></a>표준이 아닌 위치의 Android SDK

일반적으로 Android SDK는 다음 위치에 설치됩니다.

**C:\\Program Files (x86)\\Android\\android-sdk**

이 위치에 SDK가 설치되지 않은 경우 Android Device Manager를 실행할 때 이 오류가 나타날 수 있습니다.

![Android SDK 인스턴스 오류](device-manager-images/win/29-sdk-error.png)

이 문제를 해결하려면 다음 단계를 수행합니다.

1. Windows 바탕 화면에서 **C:\\Users\\*username*\\AppData\\Roaming\\XamarinDeviceManager**로 이동합니다.

   ![Android Device Manager 로그 파일 위치](device-manager-images/win/30-log-files.png)

2. 로그 파일 중 하나를 두 번 클릭하여 열고 **구성 파일 경로**를 찾습니다. 예:

   [![로그 파일의 구성 파일 경로](device-manager-images/win/31-config-file-path-sml.png)](device-manager-images/win/31-config-file-path.png#lightbox)

3. 이 위치로 이동하고 **user.config**를 두 번 클릭하여 엽니다.

4. **user.config**에서 `<UserSettings>` 요소를 찾아 **AndroidSdkPath** 특성을 추가합니다. 컴퓨터에서 Android SDK를 설치한 경로에 이 특성을 저장하고 파일을 저장합니다. 예를 들어 Android SDK가 **C:\\Programs\\Android\\SDK**에 설치된 경우 `<UserSettings>`는 다음과 같이 표시됩니다.

   ```xml
   <UserSettings SdkLibLastWriteTimeUtcTicks="636409365200000000" AndroidSdkPath="C:ProgramsAndroidSDK" />
   ```

**user.config**를 이와 같이 변경한 후에는 Android Device Manager를 실행할 수 있어야 합니다.


### <a name="wrong-version-of-android-sdk-tools"></a>Android SDK Tools의 잘못된 버전

Android SDK Tools 26.1.1 이상이 설치되어 있지 않으면 실행 시 이러한 오류 대화 상자가 표시됩니다.

![Android SDK 인스턴스 오류 대화 상자](device-manager-images/win/32-sdk-instance-error.png)

이 오류 대화 상자가 나타나면 **SDK Manager 열기**를 클릭하여 Android SDK Manager를 엽니다. Android SDK Manager에서 **도구** 탭을 클릭하고 다음 패키지를 설치합니다.

- **Android SDK Tools 26.1.1** 이상
- **Android SDK 플랫폼 도구 27.0.1** 이상
- **Android SDK 빌드 도구 27.0.3** 이상


### <a name="snapshot-disables-wifi-on-android-oreo"></a>스냅숏이 Android Oreo에서 WiFi를 사용하지 않음

시뮬레이션된 Wi-Fi 액세스를 통해 Android Oreo용 AVD가 구성되어 있는 경우 스냅숏을 만든 후 AVD를 다시 시작하면 Wi-Fi 액세스가 비활성화될 수 있습니다.

이 문제를 해결하려면 다음과 같이 합니다.

1. Android Device Manager에서 AVD를 선택합니다.

2. 추가 옵션 메뉴에서 **탐색기에 표시**를 클릭합니다.

3. **스냅숏 > default_boot**로 이동합니다.

4. **snapshot.pb** 파일을 삭제합니다.

   ![snapshot.pb 파일의 위치](device-manager-images/win/33-delete-snapshot.png)

5. AVD를 다시 시작합니다.

이러한 변경 사항이 적용되면 Wi-Fi를 다시 작동하도록 하는 상태로 AVD가 다시 시작됩니다.


# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

### <a name="wrong-version-of-android-sdk-tools"></a>Android SDK Tools의 잘못된 버전

Android SDK Tools 26.1.1 이상이 설치되어 있지 않으면 실행 시 이러한 오류 대화 상자가 표시됩니다.

![Android SDK 인스턴스 오류 대화 상자](device-manager-images/mac/29-sdk-instance-error.png)

이 오류 대화 상자가 나타나면 **확인**을 클릭하여 Android SDK Manager를 엽니다. Android SDK Manager에서 **도구** 탭을 클릭하고 다음 패키지를 설치합니다.

- **Android SDK Tools 26.1.1** 이상
- **Android SDK 플랫폼 도구 28.0.1** 이상
- **Android SDK 빌드 도구 26.0.3** 이상

### <a name="snapshot-disables-wifi-on-android-oreo"></a>스냅숏이 Android Oreo에서 WiFi를 사용하지 않음

시뮬레이션된 Wi-Fi 액세스를 통해 Android Oreo용 AVD가 구성되어 있는 경우 스냅숏을 만든 후 AVD를 다시 시작하면 Wi-Fi 액세스가 비활성화될 수 있습니다.

이 문제를 해결하려면 다음과 같이 합니다.

1. Android Device Manager에서 AVD를 선택합니다.

2. 추가 옵션 메뉴에서 **Finder에 표시**를 클릭합니다.

3. **스냅숏 > default_boot**로 이동합니다.

4. **snapshot.pb** 파일을 삭제합니다.

   [![snapshot.pb 파일의 위치](device-manager-images/mac/30-delete-snapshot-sml.png)](device-manager-images/mac/30-delete-snapshot.png#lightbox)

5. AVD를 다시 시작합니다.

이러한 변경 사항이 적용되면 Wi-Fi를 다시 작동하도록 하는 상태로 AVD가 다시 시작됩니다.

-----

### <a name="generating-a-bug-report"></a>버그 보고서 생성

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

위의 문제 해결 팁을 사용하여 해결할 수 없는 Android Device Manager 관련 문제를 발견할 경우 제목 표시줄을 마우스 오른쪽 단추로 클릭하고 **버그 보고서 생성**을 선택하여 버그 보고서를 제출하세요.

[![버그 보고서를 제출하는 데 사용되는 메뉴 항목의 위치](device-manager-images/win/34-bug-report-sml.png)](device-manager-images/win/34-bug-report.png#lightbox)


# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

위의 문제 해결 팁을 사용하여 해결할 수 없는 Android Device Manager 관련 문제를 발견할 경우 **도움말 > 문제 보고**를 클릭하여 버그 보고서를 제출하세요.

[![버그 보고서를 제출하는 데 사용되는 메뉴 항목의 위치](device-manager-images/mac/31-bug-report-sml.png)](device-manager-images/mac/31-bug-report.png#lightbox)

::: zone-end

## <a name="summary"></a>요약

이 가이드에서는 Xamarin용 Visual Studio Tools 및 Mac용 Visual Studio에서 사용할 수 있는 Android Device Manager를 소개했습니다. Android 에뮬레이터를 시작 및 중지하고, 실행할 AVD(Android 가상 장치)를 선택하고, 새 가상 장치를 만드는 기능과 같은 필수 기능과 가상 장치를 편집하는 방법을 설명했습니다. 추가 사용자 지정에 대한 프로필 하드웨어 속성을 편집하는 방법을 설명하고 일반적인 문제에 대한 문제 해결 팁을 제공했습니다.


## <a name="related-links"></a>관련 링크

- [Android SDK Tools에 대한 변경 내용](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Android Emulator에서 디버깅](~/android/deploy-test/debugging/debug-on-emulator.md)
- [SDK Tools 릴리스 정보(Google)](https://developer.android.com/studio/releases/sdk-tools)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
