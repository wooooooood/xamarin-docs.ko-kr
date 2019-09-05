---
title: Visual Studio에서 빌드한 후 파일을 IPA 파일에 추가 하거나 파일을 제거할 수 있나요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6C3082FB-C3F1-4661-BE45-64570E56DE7C
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 04/03/2018
ms.openlocfilehash: d1546b83304d8c66f7433bd77c5ebecc9dc95aaa
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70278479"
---
# <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studio"></a>Visual Studio에서 빌드한 후 파일을 IPA 파일에 추가 하거나 파일을 제거할 수 있나요?

예, 가능 하지만 변경을 수행한 후 번들에 `.app` 다시 서명 해야 합니다.

일반적인 사용에서는 파일 `.ipa` 을 수정할 필요가 없습니다. 이 문서는 참고용 으로만 제공 됩니다.

## <a name="example-removing-a-file-from-a-ipa-archive"></a>예: `.ipa` 보관 파일에서 파일 제거

이 예제에서는 xamarin.ios 프로젝트의 이름이이 `iPhoneApp1` `generated session id` 고가 인 것으로 가정 합니다.`cc530d20d6b19da63f6f1c6f67a0a254`

1. Visual Studio에서 평소와 같이 파일을빌드합니다.`.ipa`

2. Mac 빌드 호스트로 전환 합니다.

3. `~/Library/Caches/Xamarin/mtbs/builds` 폴더에서 빌드를 찾습니다. 이 경로를 **finder > 이동 > 폴더로 이동** 하 여 finder에서 폴더를 찾아볼 수 있습니다. 프로젝트 이름과 일치 하는 폴더를 찾습니다. 해당 폴더 내에서 빌드의와 일치 `generated session id` 하는 폴더를 찾습니다. 이는 가장 최근의 수정 시간이 있는 하위 폴더가 될 가능성이 높습니다.

4. 새 `Terminal.app` 창을 엽니다.

5. 를 `cd` 클릭 하 여 Terminal. 앱 창에 입력 한 다음 폴더를 `generated session id` `Terminal.app` 창으로 끌어 & 놓습니다.

    ![](modify-ipa-images/session-id-folder.png "Finder에 생성 된 세션 id 폴더 찾기")

6. 디렉터리 `generated session id` 를 폴더로 변경 하려면 반환 키를 입력 합니다.

7. 다음 명령을 `.ipa` 사용 하 여 임시 `old/` 폴더에 파일의 압축을 풉니다. 특정 프로젝트에 `iPhoneApp1` 필요한 대로 및이름을조정합니다.`Ad-Hoc`

    > ditto-xk bin/iPhone/Ad-Hoc/iPhoneApp1-1.0. ipa old/

8. `Terminal.app` 창을 계속 열어 둡니다.

9. 에서 원하는 파일을 삭제 `.ipa`합니다. Finder를 사용 하 여 휴지통으로 이동 하거나를 사용 하 여 `Terminal.app`명령줄에서 삭제할 수 있습니다. Finder에서 `Payload/iPhone` 파일의 내용을 보려면 파일을 제어 하 고 **패키지 내용 표시**를 선택 합니다.

10. 3 단계에서와 동일한 일반적인 방법을 사용 하 여에서 `~/Library/Logs/Xamarin/MonoTouchVS/` 프로젝트 이름과 `generated session id` 이름을 모두 포함 하는 로그 파일을 찾습니다. ![](modify-ipa-images/build-log.png "Finder에서 프로젝트 빌드 로그 찾기")

11. 예를 들어 10 단계에서 빌드 로그를 두 번 클릭 하 여 엽니다.

12. 을 포함 `tool /usr/bin/codesign execution started with arguments: -v --force --sign`하는 줄을 찾습니다.

13. 8 `/usr/bin/codesign` 단계에서 시작 하 여 터미널. 앱 창에 입력 합니다.

14. 로 `-v` 시작 하는 모든 인수를 12 단계 줄에서 복사 하 여 Terminal. 앱 창에 붙여넣습니다.

15. 마지막 인수를 `old/Payload/` 폴더 내에 있는 `.app` 번들로 변경한 다음 명령을 실행 합니다.

    ```bash
    /usr/bin/codesign -v --force --sign SOME_LONG_STRING in/iPhone/Ad-Hoc/iPhoneApp1.app/ResourceRules.plist --entitlements obj/iPhone/Ad-Hoc/Entitlements.xcent old/Payload/iPhoneApp1.app
    ```

16. 터미널에서 디렉터리로 변경 합니다. `old/`

    ```bash
    cd old
    ```

17. 명령을 사용 하 여 디렉터리의 내용을 새 `.ipa` 파일로 압축 합니다. `zip` 인수를 `"$HOME/Desktop/iPhoneApp1-1.0.ipa"` 변경 하 여 원하는 위치에 `.ipa` 파일을 출력할 수 있습니다.

    ```bash
    zip -yr "$HOME/Desktop/iPhoneApp1-1.0.ipa" *
    ```

## <a name="common-error-messages"></a>일반적인 오류 메시지

이 표시 `Invalid Signature. A sealed resource is missing or invalid.`되 면 일반적으로 `.app` `.app` 번들 내에서 항목이 변경 되었으며 번들이 나중에 올바르게 다시 서명 되지 않았음을 의미 합니다. 또한 배포 프로필을 `.ipa` 사용 하 여을 만들려면 배포 프로필을 사용 하 여 원본을 `.ipa` 빌드해야 _합니다_ . 그렇지 않으면가 올바르지 않습니다.`Entitlements.xcent`

이 오류가 발생 하는 방법에 대 한 구체적인 예를 제공 하기 위해 9 단계 `codesign --verify` 이후에 터미널 창에서 다음 명령을 실행 하면 오류에 대 한 정확한 원인과 함께 오류가 표시 됩니다.

```bash
$ codesign -dvvv --no-strict --verify old/Payload/iPhoneApp1.app
old/Payload/iPhoneApp1.app: a sealed resource is missing or invalid
file missing: /Users/macuser/Library/Caches/Xamarin/mtbs/builds/iPhoneApp1/cc530d20d6b19da63f6f1c6f67a0a254/old/Payload/iPhoneApp1.app/MyFile.png
```

그리고 앱 스토어 확인 프로세스에서 유사한 오류 메시지를 보고 합니다.

> ERROR ITMS-90035: "서명이 잘못 되었습니다. 봉인 리소스가 없거나 잘못 되었습니다. [IPhoneApp1/iPhoneApp1] 경로의 이진 파일에 잘못 된 서명이 포함 되어 있습니다. 임시 인증서 나 개발 인증서가 아닌 배포 인증서를 사용 하 여 응용 프로그램에 서명 했는지 확인 합니다. Xcode의 코드 서명 설정이 대상 수준에서 올바른지 확인 합니다 (프로젝트 수준의 모든 값 재정의). 또한 업로드할 번들이 시뮬레이터 대상이 아닌 Xcode의 릴리스 대상을 사용 하 여 빌드 되었는지 확인 합니다. 코드 서명 설정이 올바른지 확실 한 경우 Xcode에서 "모두 정리"를 선택 하 고 Finder에서 "build" 디렉터리를 삭제 한 다음 릴리스 대상을 다시 빌드합니다. 자세한 내용은 다음을 참조 [https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html](https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html)하세요.
