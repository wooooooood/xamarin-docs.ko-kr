---
title: Google 에뮬레이터 관리자
description: Google 에뮬레이터 관리자를 사용하여 Android 가상 장치를 만들고 관리하는 방법
ms.prod: xamarin
ms.assetid: 0C0BBEC0-C84A-4558-B905-4EF81FCD62F9
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: a399aa1c314f1e93377a7831b430e563d9fd1b13
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="google-emulator-manager"></a>Google 에뮬레이터 관리자

([Android 에뮬레이터 하드웨어 가속](~/android/get-started/installation/android-emulator/hardware-acceleration.md)에 설명된 대로) 하드웨어 가속이 활성화된 것을 확인한 후 다음 단계는 앱 테스트 및 디버깅에 사용할 가상 장치를 만드는 것입니다. 레거시 Google 에뮬레이터 관리자(*AVD(Android 가상 장치) 관리자*라고도 함)를 사용하여 Android SDK 에뮬레이터에서 사용할 가상 장치를 만들 수 있습니다.

> [!NOTE]
> Android 8.0 Oreo를 대상으로 하는 경우에는 [Xamarin Android 장치 관리자](~/android/get-started/installation/android-emulator/xamarin-device-manager.md)를 사용하여 가상 장치를 만들고 구성해야 합니다.


## <a name="installing-system-images"></a>시스템 이미지 설치

대상으로 할 Android API 수준에 따라 Android SDK 에뮬레이터에서 사용하는 API 수준별 시스템 이미지를 다운로드하고 설치해야 합니다. 각 Android API 수준에는 가상 장치를 만들기 위해 다운로드하고 설치해야 하는 일련의 **x86** 시스템 이미지가 있습니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

필요한 시스템 이미지를 설치하려면 Android SDK Manager를 시작하고(**도구 > Android > Android SDK Manager**) 지원하려는 API 수준으로 스크롤합니다. 각 API 수준에서 다음 시스템 이미지 옆의 확인 표시를 활성화합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

필요한 시스템 이미지를 설치하려면 Android SDK Manager를 시작하고(**도구 > SDK Manager**) 지원하려는 API 수준으로 스크롤합니다. 각 API 수준에서 다음 시스템 이미지 옆의 확인 표시를 활성화합니다.

-----

-   **Intel x86 Atom 시스템 이미지**
-   **Google API Intel x86 Atom 시스템 이미지**

후자의 시스템 이미지는 가상 장치에 Google API(예: Google Maps API)를 추가합니다. 

다음 스크린샷에서는 Android 6.0을 실행하는 가상 장치를 만들 수 있도록 **Intel x86 Atom** 이미지가 설치됩니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Android 에뮬레이터용 Android 6.0 x86 시스템 이미지 선택](google-emulator-manager-images/win/03-select-x86-images-sml.png)](google-emulator-manager-images/win/03-select-x86-images.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Android 에뮬레이터용 Android 6.0 x86 시스템 이미지 선택](google-emulator-manager-images/mac/02-select-x86-images-sml.png)](google-emulator-manager-images/mac/02-select-x86-images.png#lightbox)

-----

64비트 앱을 개발하는 경우 다음과 같은 시스템 이미지를 대신 설치합니다.

-   **Intel x86 Atom_64 시스템 이미지**
-   **Google API Intel x86 Atom_64 시스템 이미지**

이러한 64비트 이미지를 사용하여 32비트 앱을 실행할 수 있지만 32비트 **Intel x86 Atom 시스템 이미지**는 Android SDK 에뮬레이터에서 약간 더 빠르게 실행됩니다.

Android Wear용 앱을 개발하는 경우 다음과 같은 시스템 이미지를 설치하세요.

-   **Android Wear Intel x86 Atom 시스템 이미지**
-   **Google API Intel x86 Atom 시스템 이미지**

이러한 시스템 이미지를 설치한 후 가상 장치 구성 중에 적절한 API 수준 및 CPU/ABI 옵션을 선택하여 **x86** 기반 Android 가상 장치를 만들 수 있습니다(이 내용은 다음에 설명).


## <a name="configuring-virtual-devices"></a>가상 장치 구성

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

**Android Emulator 관리자**(_Android 가상 장치 관리자_ 또는 _AVD 관리자_라고도 함)를 통해 가상 장치를 구성할 수 있습니다. Visual Studio에서 Android Emulator 관리자를 시작하려면 도구 모음에서 **Android Emulator 관리자** 아이콘을 클릭합니다.

[![AVD 아이콘 위치](google-emulator-manager-images/win/04-avd-icon-sml.png)](google-emulator-manager-images/win/04-avd-icon.png#lightbox)

또한 메뉴 모음에서 **도구 > Android > Android Emulator 관리자**를 선택하여 Android Emulator 관리자를 시작할 수도 있습니다.

[![Android Emulator 관리자 메뉴 항목 위치](google-emulator-manager-images/win/05-avd-manager-menu-item-sml.png)](google-emulator-manager-images/win/05-avd-manager-menu-item.png#lightbox)

**AVD(Android 가상 장치) 관리자** 대화 상자에는 기존 Android 가상 장치 목록이 표시됩니다.

[![Android 가상 장치 관리자](google-emulator-manager-images/win/06-virtual-device-manager-sml.png)](google-emulator-manager-images/win/06-virtual-device-manager.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

**Android Emulator 관리자**(_Android 가상 장치 관리자_ 또는 _AVD 관리자_라고도 함)를 통해 가상 장치를 구성할 수 있습니다. 

메뉴 모음에서 **도구 > Google 에뮬레이터 관리자**를 선택하여 Android Emulator 관리자를 시작할 수 있습니다.

[![Android Emulator 관리자 메뉴 항목 위치](google-emulator-manager-images/mac/03-avd-manager-menu-item-sml.png)](google-emulator-manager-images/mac/03-avd-manager-menu-item.png#lightbox)

**AVD(Android 가상 장치) 관리자** 대화 상자에는 기존 Android 가상 장치 목록이 표시됩니다.

[![Android 가상 장치 관리자](google-emulator-manager-images/mac/05-virtual-device-manager-sml.png)](google-emulator-manager-images/mac/05-virtual-device-manager.png#lightbox)

-----

여러 장치 특성 및 API 수준을 사용하여 새 가상 장치 이미지를 만들 수 있습니다. 다음 섹션에서는 사용자 지정 장치 정의 및 가상 장치를 만드는 방법을 설명합니다.


### <a name="creating-a-custom-device-definition"></a>사용자 지정 장치 정의 만들기

사용자 지정 장치 정의를 만들려면 **AVD(Android 가상 장치) 관리자**에서 **만들기...**를 클릭합니다. 그러면 **새 AVD(Android 가상 장치) 만들기** 대화 상자가 열립니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Nexus 6을 기반으로 하는 사용자 지정 장치 정의](google-emulator-manager-images/win/07-custom-device-sml.png)](google-emulator-manager-images/win/07-custom-device.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Nexus 6을 기반으로 하는 사용자 지정 장치 정의](google-emulator-manager-images/mac/06-custom-device-sml.png)](google-emulator-manager-images/mac/06-custom-device.png#lightbox)

-----

이 대화 상자에서 다음 옵션을 구성합니다.

-   **AVD 이름** &ndash; 장치 정의의 고유한 이름입니다. 위의 스크린샷 예제에서 이 이름은 **MyNexus**로 설정되었습니다. AVD 이름은 공백을 포함할 수 없습니다. AVD 이름에 공백을 사용하려고 하면 **확인** 단추가 비활성화됩니다.

-   **장치** &ndash; 에뮬레이트하려는 하드웨어 프로필을 선택합니다(예: **Nexus 5** 또는 **Nexus 6**).

-   **대상** &ndash; 가상 장치의 Android API 수준을 선택합니다. 이 설정은 앱의 최소 Android 버전보다 크거나 같아야 합니다.

-   **CPU/ABI** &ndash; 장치 정의에서 Google API를 사용할 수 있도록 **Google API Intel Atom(x86)**을 선택합니다.

-   **스킨** &ndash; 가상 장치의 모양을 선택합니다. 위의 스크린샷 예제에서는 **HVGA** 스킨이 선택되었습니다(이 아티클의 끝에 있는 에뮬레이터 스크린샷은 **HVGA** 스킨의 한 예입니다).

-   **메모리 옵션** &ndash; 일반적으로 기본 RAM 설정은 너무 높으며, Windows에서 **768M보다 큰 RAM의 에뮬레이트는 실패할 수 있습니다** 경고를 유발합니다. 대부분의 사용자는 (위의 스크린샷에 나와 있는 것처럼) RAM을 768MB로 설정하는 것이 좋습니다. RAM 값이 크면 에뮬레이터가 느려질 수 있습니다.

-   **호스트 GPU 사용** &ndash; 이 옵션은 에뮬레이터가 호스트 컴퓨터의 GPU(그래픽 처리 장치)를 사용하여 그래픽 작업을 수행하도록 합니다. 에뮬레이터의 성능을 더 높이려면 이 옵션을 사용하도록 설정하는 것이 좋습니다. **에뮬레이션 옵션** 섹션에 대한 자세한 내용은 [스냅숏과 호스트 GPU 사용 에뮬레이션 옵션의 용도는?](https://android.stackexchange.com/questions/51739/what-is-snapshot-and-use-host-gpu-emulation-options-for)을 참조하세요.


나머지 옵션은 기본 설정으로 유지할 수 있습니다. 준비되면 **확인**을 클릭하여 새 가상 장치를 만듭니다. 다음 대화 상자에 새 가상 장치 구성 결과가 자세히 표시됩니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![새 AVD를 만든 후 결과 대화 상자](google-emulator-manager-images/win/08-create-results.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![새 AVD를 만든 후 결과 대화 상자](google-emulator-manager-images/mac/07-create-results.png)

-----

이 대화 상자에 나열된 구성 속성에 대한 자세한 내용은 [하드웨어 프로필 속성](https://developer.android.com/studio/run/managing-avds.html#hpproperties)을 참조하세요.
**확인**을 클릭하면 기존 Android 가상 장치 목록에 새 장치 구성이 표시됩니다. 다음 스크린샷에서는 **MyNexus**가 목록에 추가되었습니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![장치 목록에 추가된 MyNexus](google-emulator-manager-images/win/09-added-to-list-sml.png)](google-emulator-manager-images/win/09-added-to-list.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![장치 목록에 추가된 MyNexus](google-emulator-manager-images/mac/08-added-to-list-sml.png)](google-emulator-manager-images/mac/08-added-to-list.png#lightbox)

-----

새 사용자 지정 가상 장치는 장치 풀 다운 메뉴에도 추가됩니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![장치 풀 다운 메뉴에 추가된 MyNexus](google-emulator-manager-images/win/10-available-custom-device-sml.png)](google-emulator-manager-images/win/10-available-custom-device.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![장치 풀 다운 메뉴에 추가된 MyNexus](google-emulator-manager-images/mac/09-available-custom-device-sml.png)](google-emulator-manager-images/mac/09-available-custom-device.png#lightbox)

-----



### <a name="cloning-a-device-definition"></a>장치 정의 복제

기존 장치 정의를 선택하고 *복제*하여 새 사용자 지정 장치 정의를 만들 수 있습니다. 기존 장치 정의에서 몇 가지 사소한 부분만 조정하면 요구 사항을 맞출 수 있는 경우에 사용하기 좋은 전략입니다. **AVD(Android 가상 장치) 관리자**의 **장치 정의** 탭에는 사용 가능한 모든 장치 정의가 나열됩니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![사용 가능한 장치 정의 목록](google-emulator-manager-images/win/11-device-definitions-sml.png)](google-emulator-manager-images/win/11-device-definitions.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![사용 가능한 장치 정의 목록](google-emulator-manager-images/mac/10-device-definitions-sml.png)](google-emulator-manager-images/mac/10-device-definitions.png#lightbox)

-----

이 목록에서 미리 구성된 장치는 수정할 수 없습니다. 사용자가 만든 가상 장치만 편집할 수 있습니다. 장치 정의를 선택하고 **복제**를 클릭하면 미리 구성된 장치 정의에서 새 장치 정의를 도출할 수 있습니다. 예를 들어 **Nexus 5** 정의를 선택하고 **복제**를 클릭하면 다음과 같은 대화 상자가 표시됩니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![장치 복제 대화 상자](google-emulator-manager-images/win/12-clone-device-sml.png)](google-emulator-manager-images/win/12-clone-device.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![장치 복제 대화 상자](google-emulator-manager-images/mac/11-clone-device-sml.png)](google-emulator-manager-images/mac/11-clone-device.png#lightbox)

-----

다음 스크린샷에서는 새 사용자 지정 장치 정의를 만들기 위해 이름을 **Nexus 5 사용자 지정**으로 변경하고, 장치 매개 변수를 수정했습니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![사용자 지정 Nexus 5 AVD](google-emulator-manager-images/win/13-custom-nexus.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![사용자 지정 Nexus 5 AVD](google-emulator-manager-images/mac/12-custom-nexus-sml.png)](google-emulator-manager-images/mac/12-custom-nexus.png#lightbox)

-----

**장치 복제**를 클릭하면 새 장치 정의가 생성되고 **장치 정의** 목록에 표시됩니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Nexus 5가 새 사용자 장치 정의로 표시됨](google-emulator-manager-images/win/14-new-definition-sml.png)](google-emulator-manager-images/win/14-new-definition.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Nexus 5가 새 사용자 장치 정의로 표시됨](google-emulator-manager-images/mac/13-new-definition-sml.png)](google-emulator-manager-images/mac/13-new-definition.png#lightbox)

-----

사용자가 만든 각 장치 정의는 위에 표시된 것처럼 녹색 아이콘과 함께 표시됩니다. 이 정의를 선택하고 **AVD 만들기**를 클릭하면 이 새 장치 정의 정의를 사용하여 새 AVD를 만들 수 있습니다. 그러면 **새 AVD(Android 가상 장치) 만들기** 대화 상자가 표시됩니다. 다음 예제에서는 새 가상 장치에 대해 **AVD\_for\_Nexus\_5\_Custom**이라는 이름이 자동으로 생성되었습니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Nexus 5 사용자 지정 사용자 장치 정의로 AVD 만들기](google-emulator-manager-images/win/15-create-avd-sml.png)](google-emulator-manager-images/win/15-create-avd.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Nexus 5 사용자 지정 사용자 장치 정의로 AVD 만들기](google-emulator-manager-images/mac/14-create-avd-sml.png)](google-emulator-manager-images/mac/14-create-avd.png#lightbox)

-----

**확인**을 클릭하면 기존 Android 가상 장치 목록에 사용자 지정 장치 구성이 표시됩니다. 도한 장치 풀 다운 메뉴에도 추가됩니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![장치 드롭다운 메뉴에 추가된 새 사용자 지정 AVD](google-emulator-manager-images/win/16-new-avd-sml.png)](google-emulator-manager-images/win/16-new-avd.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![장치 드롭다운 메뉴에 추가된 새 사용자 지정 AVD](google-emulator-manager-images/mac/15-new-avd-sml.png)](google-emulator-manager-images/mac/15-new-avd.png#lightbox)

-----

