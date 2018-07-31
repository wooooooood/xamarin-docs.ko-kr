---
title: 파일을 추가 하거나 Visual Studio에서 빌드한 후 IPA 파일에서 파일을 제거 수 있나요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6C3082FB-C3F1-4661-BE45-64570E56DE7C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 366308774a7302e54b0d47753256638e89d97b82
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350984"
---
# <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studio"></a>파일을 추가 하거나 Visual Studio에서 빌드한 후 IPA 파일에서 파일을 제거 수 있나요?

수는 있지만 일반적으로 해야 하 고 다시 서명 하는 예는 `.app` 변경을 수행한 후 번들입니다.

해당 수정 확인을 `.ipa` 파일이 일반적인 사용의 필요 하지 않습니다. 이 문서는 정보 제공을 위해서만 제공 됩니다.

## <a name="example-removing-a-file-from-a-ipa-archive"></a>예:에서 파일 제거는 `.ipa` 보관

이 예에서는 가정 Xamarin.iOS 프로젝트의 이름을 `iPhoneApp1` 하며 `generated session id` 는 `cc530d20d6b19da63f6f1c6f67a0a254`

1.  빌드는 `.ipa` Visual Studio에서 정상적으로 파일입니다.

2.  Mac 빌드 호스트를 전환 합니다.

3.  빌드 찾기는 `~/Library/Caches/Xamarin/mtbs/builds` 폴더입니다. 에이 경로 붙여 넣을 수 있습니다 **Finder > 이동 > 폴더로 이동** 파인더에서 폴더를 찾아볼 수 있습니다. 프로젝트 이름과 일치 하는 폴더를 찾습니다. 해당 폴더 내의 일치 하는 폴더를 찾습니다는 `generated session id` 빌드. 가장 최근에 수정 시간에는 하위 폴더 가능성이 됩니다.

4.  새 `Terminal.app` 창입니다.

5.  형식 `cd ` 는 Terminal.app 창 및 다음 끌어서 놓기에 `generated session id` 폴더를 `Terminal.app` 창:

    ![](modify-ipa-images/session-id-folder.png "Finder에서 생성 된 세션 id 폴더 찾기")

6.  디렉터리를 변경 하려면 return 키를 입력 합니다 `generated session id` 폴더입니다.

7.  압축을 풉니다 합니다 `.ipa` 파일을 임시 `old/` 다음 명령을 사용 하 여 폴더입니다. 조정 된 `Ad-Hoc` 및 `iPhoneApp1` 특정 프로젝트에 필요한 대로 이름을 지정 합니다.

    > 이전-xk bin/iPhone/Ad-Hoc/iPhoneApp1-1.0.ipa 같음 /

8.  유지 된 `Terminal.app` 창이 열려 있습니다.

9.  원하는 파일을 삭제 합니다 `.ipa`합니다. Finder를 사용 하 여 휴지통으로 이동 하거나 사용 하 여 명령줄에서 삭제할 `Terminal.app`합니다. 내용을 보려고 합니다 `Payload/iPhone` 파일을 컨트롤 클릭 Finder에서 파일을 선택 **패키지 내용 표시**.

10.  3 단계와 같이 동일한 일반적인 접근 방식을 사용 하 여 아래에 있는 로그 파일을 찾을 `~/Library/Logs/Xamarin/MonoTouchVS/` 프로젝트 이름을 가진 및 `generated session id` 이름에서: ![ ] (modify-ipa-images/build-log.png "파인더에서 프로젝트 빌드 로그를 찾으려면")

11.  두 번 클릭 하 여 10 단계에서 빌드 로그를 예를 들어 엽니다.

12.  포함 하는 줄을 찾아서 `tool /usr/bin/codesign execution started with arguments: -v --force --sign`합니다.

13.  형식 `/usr/bin/codesign ` 8 단계의 Terminal.app 창에 있습니다.

14.  시작 인수를 모두 복사 `-v` 12 단계에서 줄에서 Terminal.app 창에 붙여넣습니다.

15.  마지막 인수를 변경 합니다 `.app` 번들 내에 있는 `old/Payload/` 폴더 및 다음 명령 실행 합니다.

```bash
/usr/bin/codesign -v --force --sign SOME_LONG_STRING in/iPhone/Ad-Hoc/iPhoneApp1.app/ResourceRules.plist --entitlements obj/iPhone/Ad-Hoc/Entitlements.xcent old/Payload/iPhoneApp1.app
```

16.  변경 된 `old/` 디렉터리 터미널에서:

```bash
cd old
```

17.  새 디렉터리의 내용을 zip `.ipa` 를 사용 하 여 파일을 `zip` 명령입니다. 변경할 수 있습니다 합니다 `"$HOME/Desktop/iPhoneApp1-1.0.ipa"` 출력 인수를 `.ipa` 원하는 어디서 나 파일:

```bash
zip -yr "$HOME/Desktop/iPhoneApp1-1.0.ipa" *
```

## <a name="common-error-messages"></a>일반적인 오류 메시지

표시 되 면 `Invalid Signature. A sealed resource is missing or invalid.`, 일반적으로 즉, 내에서 변경 된 것을 `.app` 번들 및는 `.app` 번들 서명 되지 않았습니다 올바르게 다시 나중에 합니다. 참고를 만들려는 경우는 `.ipa` 배포 프로필을 사용 하 여 있습니다 _해야_ 원래 빌드 `.ipa` 배포 프로필을 사용 하 여 합니다. 그렇지 않은 경우는 `Entitlements.xcent` 올바르지 않을 수 있습니다.

다음을 실행 하는 경우이 오류가 발생할 수 있는 방법의 구체적인 예를 제공 하려면 `codesign --verify` 명령 9 단계를 수행한 후 터미널 창에서 발생 한 오류의 정확한 원인 함께 오류가 표시 됩니다.

```bash
$ codesign -dvvv --no-strict --verify old/Payload/iPhoneApp1.app
old/Payload/iPhoneApp1.app: a sealed resource is missing or invalid
file missing: /Users/macuser/Library/Caches/Xamarin/mtbs/builds/iPhoneApp1/cc530d20d6b19da63f6f1c6f67a0a254/old/Payload/iPhoneApp1.app/MyFile.png
```

및 앱 스토어 인증 프로세스와 유사한 오류 메시지를 보고 합니다.

> 오류 ITMS-90035: "잘못 된 서명입니다. 봉인 된 리소스 누락 되었거나 잘못 되었습니다. 이진 경로 [iPhoneApp1.app/iPhoneApp1]에 잘못 된 서명이 있습니다. 배포 인증서, 없습니다 임시 인증서를 또는 개발 인증서를 사용 하 여 응용 프로그램을 체결 해야 합니다. 수준 대상 (프로젝트 수준에서 모든 값을 재정의) Xcode에서 코드 서명 설정이 올바른지 확인 합니다. 또한 업로드 하는 번들 릴리스 대상 시뮬레이터 대상 없습니다 Xcode에서 사용 하 여 빌드된 해야 합니다. 인 경우 코드 서명 설정이 잘못 된 특정 Xcode에서 정리 "모두"를 선택, 파인더에서 "빌드" 디렉터리를 삭제 및 릴리스 대상을 다시 작성 합니다. 자세한 내용은 참조 하세요 [ https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html ](https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html)"
