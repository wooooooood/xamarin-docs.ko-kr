---
title: Visual Studio에서 빌드한 후 파일을 IPA 파일에 추가 하거나 파일을 제거할 수 있나요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6C3082FB-C3F1-4661-BE45-64570E56DE7C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: f63cc544b251ca284a8d087e09f9cbc4dfb3f769
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030952"
---
# <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studio"></a>Visual Studio에서 빌드한 후 파일을 IPA 파일에 추가 하거나 파일을 제거할 수 있나요?

예, 가능 하지만 변경을 수행한 후에는 `.app` 번들에 다시 서명 해야 합니다.

일반적인 사용에서는 `.ipa` 파일을 수정할 필요가 없습니다. 이 문서는 참고용 으로만 제공 됩니다.

## <a name="example-removing-a-file-from-a-ipa-archive"></a>예: `.ipa` archive에서 파일 제거

이 예제에서는 Xamarin.ios 프로젝트의 이름이 `iPhoneApp1` 되 고 `generated session id`을 `cc530d20d6b19da63f6f1c6f67a0a254` 하는 것으로 가정 합니다.

1. Visual Studio에서 정상적으로 `.ipa` 파일을 빌드합니다.

2. Mac 빌드 호스트로 전환 합니다.

3. `~/Library/Caches/Xamarin/mtbs/builds` 폴더에서 빌드를 찾습니다. 이 경로를 **finder > 이동 > 폴더로 이동** 하 여 finder에서 폴더를 찾아볼 수 있습니다. 프로젝트 이름과 일치 하는 폴더를 찾습니다. 해당 폴더 내에서 빌드의 `generated session id`와 일치 하는 폴더를 찾습니다. 이는 가장 최근의 수정 시간이 있는 하위 폴더가 될 가능성이 높습니다.

4. 새 `Terminal.app` 창을 엽니다.

5. `cd`를 Terminal. 앱 창에 입력 한 다음 `generated session id` 폴더를 `Terminal.app` 창으로 끌어 놓습니다 & 합니다.

    ![](modify-ipa-images/session-id-folder.png "Locating the generated session id folder in Finder")

6. 반환 키를 입력 하 여 디렉터리를 `generated session id` 폴더로 변경 합니다.

7. 다음 명령을 사용 하 여 임시 `old/` 폴더로 `.ipa` 파일의 압축을 풉니다. 특정 프로젝트에 필요한 `Ad-Hoc` 및 `iPhoneApp1` 이름을 조정 합니다.

    > ditto-xk bin/iPhone/Ad-Hoc/iPhoneApp1-1.0. ipa old/

8. `Terminal.app` 창을 계속 열어 둡니다.

9. `.ipa`에서 원하는 파일을 삭제 합니다. Finder를 사용 하 여 휴지통으로 이동 하거나 `Terminal.app`를 사용 하 여 명령줄에서 삭제할 수 있습니다. Finder에서 `Payload/iPhone` 파일의 내용을 보려면 파일을 제어 하 고 **패키지 내용 표시**를 선택 합니다.

10. 3 단계에서와 동일한 일반적인 방법을 사용 하 여 프로젝트 이름과 `generated session id`이 이름에 있는 `~/Library/Logs/Xamarin/MonoTouchVS/`에서 로그 파일을 찾습니다.![](modify-ipa-images/build-log.png "Finder에서 프로젝트 빌드 로그 찾기")

11. 예를 들어 10 단계에서 빌드 로그를 두 번 클릭 하 여 엽니다.

12. `tool /usr/bin/codesign execution started with arguments: -v --force --sign`포함 된 줄을 찾습니다.

13. 8 단계의 Terminal. 앱 창에 `/usr/bin/codesign`을 입력 합니다.

14. `-v`로 시작 하는 모든 인수를 12 단계 줄에서 복사 하 여 Terminal. 앱 창에 붙여넣습니다.

15. 마지막 인수를 `old/Payload/` 폴더에 있는 `.app` 번들로 변경한 다음 명령을 실행 합니다.

    ```bash
    /usr/bin/codesign -v --force --sign SOME_LONG_STRING in/iPhone/Ad-Hoc/iPhoneApp1.app/ResourceRules.plist --entitlements obj/iPhone/Ad-Hoc/Entitlements.xcent old/Payload/iPhoneApp1.app
    ```

16. 터미널에서 `old/` 디렉터리로 변경 합니다.

    ```bash
    cd old
    ```

17. `zip` 명령을 사용 하 여 디렉터리의 내용을 새 `.ipa` 파일로 압축 합니다. `"$HOME/Desktop/iPhoneApp1-1.0.ipa"` 인수를 변경 하 여 원하는 위치에 `.ipa` 파일을 출력할 수 있습니다.

    ```bash
    zip -yr "$HOME/Desktop/iPhoneApp1-1.0.ipa" *
    ```

## <a name="common-error-messages"></a>일반적인 오류 메시지

`Invalid Signature. A sealed resource is missing or invalid.`표시 되는 경우이는 일반적으로 `.app` 번들 내에서 변경 된 것을 의미 하 고 나중에 `.app` 번들이 올바르게 다시 서명 되지 않았음을 의미 합니다. 또한 배포 프로필을 사용 하 여 `.ipa`를 만들려는 경우에는 배포 프로필을 사용 하 여 원래 `.ipa`을 빌드해야 _합니다_ . 그렇지 않으면 `Entitlements.xcent` 잘못 됩니다.

이 오류가 발생 하는 방법에 대 한 구체적인 예를 제공 하기 위해 9 단계 후 터미널 창에서 다음 `codesign --verify` 명령을 실행 하면 오류의 정확한 원인과 함께 오류가 표시 됩니다.

```bash
$ codesign -dvvv --no-strict --verify old/Payload/iPhoneApp1.app
old/Payload/iPhoneApp1.app: a sealed resource is missing or invalid
file missing: /Users/macuser/Library/Caches/Xamarin/mtbs/builds/iPhoneApp1/cc530d20d6b19da63f6f1c6f67a0a254/old/Payload/iPhoneApp1.app/MyFile.png
```

그리고 앱 스토어 확인 프로세스에서 유사한 오류 메시지를 보고 합니다.

> 오류 ITMS-90035: "서명이 잘못 되었습니다. 봉인 리소스가 없거나 잘못 되었습니다. [IPhoneApp1/iPhoneApp1] 경로의 이진 파일에 잘못 된 서명이 포함 되어 있습니다. 임시 인증서 나 개발 인증서가 아닌 배포 인증서를 사용 하 여 응용 프로그램에 서명 했는지 확인 합니다. Xcode의 코드 서명 설정이 대상 수준에서 올바른지 확인 합니다 (프로젝트 수준의 모든 값 재정의). 또한 업로드할 번들이 시뮬레이터 대상이 아닌 Xcode의 릴리스 대상을 사용 하 여 빌드 되었는지 확인 합니다. 코드 서명 설정이 올바른지 확실 한 경우 Xcode에서 "모두 정리"를 선택 하 고 Finder에서 "build" 디렉터리를 삭제 한 다음 릴리스 대상을 다시 빌드합니다. 자세한 내용은 [https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html](https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html)"을 참조 하세요.
