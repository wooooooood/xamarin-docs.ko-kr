---
title: Android 애플리케이션 패키지에 서명
description: 게시할 APK(Android 애플리케이션 패키지)에 서명하는 방법
ms.prod: xamarin
ms.assetid: 8E3EFBB2-F8AD-C126-5F32-7FD140791E53
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/02/2018
ms.openlocfilehash: 5bdd95409e71955b4f1549eece42b15cee38131a
ms.sourcegitcommit: a7170494e1975f0f1be547a45444752fd8e57819
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58506995"
---
# <a name="signing-the-android-application-package"></a>Android 애플리케이션 패키지에 서명

[릴리스용 앱 준비](~/android/deploy-test/release-prep/index.md)에서는 **보관 관리자**를 사용하여 앱을 빌드하고, 서명 및 게시를 위해 아카이브에 저장했습니다. 이 섹션에서는 Android 서명 ID를 만들고, Android 애플리케이션에 대한 새 서명 인증서를 만들고, 보관된 앱 *ad hoc*을 디스크에 게시하는 방법을 설명합니다. 앱 스토어를 통하지 않고 Android 디바이스에 생성된 APK를 사이드로드할 수 있습니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[게시를 위해 보관](~/android/deploy-test/release-prep/index.md#archive)에서 **배포 채널** 대화 상자에는 두 가지 배포 옵션이 제공됩니다. **임시**를 선택합니다.

[![배포 채널 대화 상자](images/vs/01-distribution-channel-sml.png)](images/vs/01-distribution-channel.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[게시를 위해 보관](~/android/deploy-test/release-prep/index.md#archive)에서 **서명 및 배포...** 대화 상자에는 두 가지 배포 옵션이 제공됩니다. **임시**를 선택하고 **다음**을 클릭합니다.

[![서명 및 배포 대화 상자](images/xs/01-select-ad-hoc-sml.png)](images/xs/01-select-ad-hoc.png#lightbox)

-----

<a name="newcertvs" />
<a name="newcert" />
<a name="newcertxs" />

## <a name="create-a-new-certificate"></a>새 인증서 만들기

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

**임시**를 선택하면 Visual Studio에서 다음 스크린샷에 표시된 것과 같은 대화 상자의 **서명 ID** 페이지가 열립니다. .APK를 게시하려면 먼저 (인증서라고도 하는) 서명 키로 서명해야 합니다.

**가져오기** 단추를 클릭한 후 [APK 서명](#sign-the-apk)으로 이동하면 기존 인증서를 사용할 수 있습니다. 또는 **+** 단추를 클릭하여 새 인증서를 만들 수 있습니다.

[![임시 서명 ID](images/vs/02-ad-hoc-signing-identity-vs-sml.png)](images/vs/02-ad-hoc-signing-identity-vs.png#lightbox)

**Android 키 저장소 만들기** 대화 상자가 표시됩니다. 이 대화 상자를 사용하여 Android 애플리케이션에 서명하는 데 사용할 수 있는 새로운 서명 인증서를 만들 수 있습니다. 이 대화 상자에 표시된 것과 같이 필요한 정보(빨간색으로 윤곽선 처리)를 입력합니다.

[![Android 키 저장소 만들기 대화 상자](images/vs/03-create-android-key-store-vs-sml.png)](images/vs/03-create-android-key-store-vs.png#lightbox)

다음 예제에서는 제공해야 하는 정보의 종류를 보여 줍니다. **만들기**를 클릭하여 새 인증서를 만듭니다.

[![새 인증서 만들기](images/vs/04-key-store-example-vs-sml.png)](images/vs/04-key-store-example-vs.png#lightbox)

결과 키 저장소는 다음 위치에 있습니다.

**C:\\Users\\*USERNAME*\\AppData\\Local\\Xamarin\\Mono for Android\\Keystore\\*ALIAS*\\*ALIAS*.keystore**

예를 들어 위의 단계는 **chimp**를 별명으로 사용하여 다음 위치에 새 서명 키를 만듭니다.

**C:\\Users\\*USERNAME*\\AppData\\Local\\Xamarin\\Mono for Android\\Keystore\\chimp\\chimp.keystore**

> [!NOTE]
> 결과 키 저장소 파일 및 암호는 솔루션에 포함되지 않으므로 안전한 장소 &ndash;에 백업해야 합니다. 다른 컴퓨터로 이동하거나 Windows를 다시 설치한 이유 등으로 키 저장소 파일을 잃어버린 경우 이전 버전과 동일한 인증서로 앱에 서명할 수 없게 됩니다.

키 저장소에 대한 자세한 내용은 [키 저장소의 MD5 또는 SHA1 서명 찾기](~/android/deploy-test/signing/keystore-signature.md)를 참조하세요.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

**임시**를 클릭하면 Mac용 Visual Studio에서 다음 스크린샷에 표시된 것과 같은 **Android 서명 ID** 대화 상자가 열립니다. .APK를 게시하려면 먼저 (인증서라고도 하는) 서명 키로 서명해야 합니다. 인증서가 이미 있는 경우 **기존 키 가져오기** 단추를 클릭하여 가져온 후 [APK 서명](#sign-the-apk)을 진행합니다. 그렇지 않을 경우 **새 키 만들기** 단추를 클릭하여 새 인증서를 만듭니다.

[![Android 서명 ID 대화 상자](images/xs/02-android-signing-identity-sml.png)](images/xs/02-android-signing-identity.png#lightbox)

Android 애플리케이션에 서명하는 데 사용할 수 있는 새로운 서명 인증서를 만들 때는 **새 인증서 만들기** 대화 상자가 사용됩니다. 필요한 정보를 입력한 후 **확인**을 클릭하세요.

[![새 인증서 만들기 대화 상자](images/xs/03-create-new-certificate-sml.png)](images/xs/03-create-new-certificate.png#lightbox)

결과 키 저장소는 다음 위치에 있습니다.

**~/Library/Developer/Xamarin/Keystore/alias/alias.keystore**

예를 들어 위의 단계는 다음 위치에 새 서명 키를 만들 수 있습니다.

**~/Library/Developer/Xamarin/Keystore/chimp/chimp.keystore**


> [!NOTE]
> 결과 키 저장소 파일 및 암호는 솔루션에 포함되지 않으므로 안전한 장소 &ndash;에 백업해야 합니다. 다른 컴퓨터로 이동하거나 macOS를 다시 설치한 이유 등으로 키 저장소 파일을 잃어버린 경우 이전 버전과 동일한 인증서로 앱에 서명할 수 없게 됩니다.

키 저장소에 대한 자세한 내용은 [키 저장소의 MD5 또는 SHA1 서명 찾기](~/android/deploy-test/signing/keystore-signature.md)를 참조하세요.

-----

## <a name="sign-the-apk"></a>APK 서명

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

**만들기**를 클릭하면 다음 스크린샷에 표시된 것처럼 **서명 ID** 아래에 새 키 저장소(새 인증서 포함)가 저장되고 나열됩니다. Google Play에서 앱을 게시하려면 **취소**를 클릭하고 [Google Play에 게시](~/android/deploy-test/publishing/publishing-to-google-play/index.md)로 이동합니다.
*ad-hoc*을 게시하려면 서명에 사용할 서명 ID를 선택하고 **다른 이름으로 저장**을 클릭하여 앱을 개별 배포용으로 게시합니다. 예를 들어 이 스크린샷에서는 (이전에 만든) **chimp** 서명 ID가 선택되었습니다.

[![서명 ID 예제](images/vs/05-save-as-vs-sml.png)](images/vs/05-save-as-vs.png#lightbox)

다음으로 **보관 관리자**가 게시 진행률을 표시합니다. 게시 프로세스가 완료되면 생성된 .APK 파일을 저장할 위치를 묻는 **다른 이름으로 저장** 대화 상자가 열립니다.

[![다른 이름으로 저장 대화 상자](images/vs/06-save-as-dialog-vs-sml.png)](images/vs/06-save-as-dialog-vs.png#lightbox)

원하는 위치로 이동하고 **저장**을 클릭합니다. 키 암호를 알 수 없는 경우에는 선택한 인증서에 대한 암호를 묻는 **서명 암호** 대화 상자가 열립니다.

[![서명 암호 대화 상자](images/vs/07-signing-password-vs-sml.png)](images/vs/07-signing-password-vs.png#lightbox)

서명 프로세스가 완료되면 **배포 열기**를 클릭합니다.

[![배포 열기 단추](images/vs/08-open-distribution-sml.png)](images/vs/08-open-distribution.png#lightbox)

이렇게 하면 Windows 탐색기에 생성된 APK 파일을 포함하는 폴더가 열립니다. 이때 Visual Studio는 Xamarin.Android 애플리케이션을 배포 준비가 된 APK로 컴파일했습니다.
다음 스크린샷에서는 게시 준비가 된 응용 프로그램의 예 **MyApp.MyApp.apk**를 보여 줍니다.

[![Windows 탐색기에 표시된 APK](images/vs/09-generated-app-vs-sml.png)](images/vs/09-generated-app-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)


여기 표시된 것처럼 새 인증서가 키 저장소에 추가되었습니다. Google Play에서 앱을 게시하려면 **취소**를 클릭하고 [Google Play에 게시](~/android/deploy-test/publishing/publishing-to-google-play/index.md)로 이동합니다.
그렇지 않으면 이 예에 표시된 것처럼 **다음**을 클릭하여 *ad-hoc* 앱을 (개별 배포용으로) 게시합니다.

[![서명 및 배포 대화 상자](images/xs/04-select-identity-sml.png)](images/xs/04-select-identity.png#lightbox)

서명된 앱이 게시되기 전에 **임시로 게시** 대화 상자에 해당 앱에 대한 요약이 제공됩니다. 이 정보가 올바르면 **게시**를 클릭합니다.

[![임시로 게시 대화 상자](images/xs/05-publish-ad-hoc-sml.png)](images/xs/05-publish-ad-hoc.png#lightbox)

**출력 APK 파일** 대화 상자가 APK를 지정된 경로에 저장합니다. **저장**을 클릭합니다.

![출력 APK 파일 대화 상자](images/xs/06-output-apk-file.png)

다음으로, 인증서 암호(**새 인증서 만들기** 대화 상자에서 사용된 암호)를 입력하고 **확인**을 클릭합니다.

![인증서 암호 입력](images/xs/07-signing-certificate.png)

APK가 인증서로 서명되어 지정된 위치에 저장됩니다. **Finder에 표시**를 클릭합니다.

[![게시 성공 대화 상자](images/xs/08-app-is-ready-sml.png)](images/xs/08-app-is-ready.png#lightbox)

Finder에 서명된 APK 파일의 위치가 열립니다.

[![찾기에 표시된 APK](images/xs/09-show-in-finder-sml.png)](images/xs/09-show-in-finder.png#lightbox)

APK가 Finder에서 복사하여 최종 대상으로 보낼 수 있습니다. Android 디바이스에 APK를 설치하고 배포하기 전에 사용해 보는 것이 좋습니다. *임시* APK를 게시하는 방법에 대한 자세한 내용은 [독립적으로 게시](~/android/deploy-test/publishing/publishing-independently.md)를 참조하세요.

-----



## <a name="next-steps"></a>다음 단계

릴리스를 위해 서명한 애플리케이션 패키지를 게시해야 합니다. 다음 섹션에서는 애플리케이션을 게시하는 여러 가지 방법을 설명합니다.
