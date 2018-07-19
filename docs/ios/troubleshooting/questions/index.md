---
title: 질문과 대답
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 65E04188-185D-493D-BA3C-A89711CB6CAF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: f8264c48b3b4679d45d5603b5637df0016cd3d17
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30780237"
---
# <a name="frequently-asked-questions"></a>질문과 대답

## <a name="general-questions"></a>일반적인 질문

### <a name="can-i-use-a-mac-vm-with-xamarinmac-vmmd"></a>[Xamarin에서 Mac VM을 사용할 수 있나요?](mac-vm.md)
예, Mac 하드웨어에만 합니다.

### <a name="how-can-i-downgrade-xcodedowngrade-xcodemd"></a>[Xcode를 다운그레이드하려면 어떻게 해야 하나요?](downgrade-xcode.md)
이 가이드는 이전 버전의 Xcode 뿐만 아니라 최신 버전에 액세스 하는 링크를 제공 합니다.

### <a name="where-can-i-set-my-ios-sdk-locationsios-sdkmd"></a>[iOS SDK 위치를 어디에 설정할 수 있나요?](ios-sdk.md)
대부분의 사용자에 대 한 이러한 적절 한 위치에 자동으로 설정 됩니다. 이 가이드에는 기본 SDK 위치 및 필요에 따라 변경 하는 방법을 보여 줍니다.

### <a name="how-can-i-reenable-developer-options-after-updating-iosupdate-developer-optionsmd"></a>[IOS를 업데이트 한 후 개발자 옵션을 다시 수는 방법](update-developer-options.md)
IOS 버그 개발자 옵션 iOS 버전을 업데이트 한 후 사라질 때까지 발생할 수 있습니다,이 바이러스 iOS로 전환할 때 8.x 합니다. 이 가이드에서는 옵션을 했다가 다시 설정할 수 방법을 설명 합니다.

### <a name="user-location-not-working-in-ios-8ios8-user-locationmd"></a>[iOS 8에서 작동하지 않는 사용자 위치](ios8-user-location.md)
이 가이드에서는 iOS 8에서에서 사용자 위치를 사용 하도록 설정 하려면을 편집 하는 방법을 알려줍니다.

### <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logssymbolicate-ios-crashmd"></a>[iOS 크래시 로그를 기호로 표시하는 .dSYM 파일을 어디에서 찾을 수 있나요?](symbolicate-ios-crash.md)
이 가이드에서는 iOS 크래시 로그 충돌 진단에 도움이 되도록 symbolicating 하기 위한 기본 단계를 설명 합니다. 또한 고급 symbolication 기술 및 iOS 크래시 로그를 해석에 대 한 정보에 대 한 추가 리소스에 연결 합니다.


### <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studioxs-mono-runtimemd"></a>[Xamarin Studio에서 iOS 프로젝트에 대한 Mono 런타임 환경 변수를 설정하려면 어떻게 해야 하나요?](xs-mono-runtime.md)
모노에 대 한 모든 런타임 환경 변수를 설정 해야 할 경우 설정할 수 있기는 **프로젝트 옵션 > 실행 > 일반** 페이지.

## <a name="publishing-questions"></a>게시 관련 질문

### <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submissioninvalid-bundle-bitcodemd"></a>[앱 스토어에 제출 하는 동안 오류가 발생 했습니다: "잘못 된 번들-bitcode에 포함 될 수 없는 옵션에서에서 발견 되 면 전송"](invalid-bundle-bitcode.md)

앱을 제출 하는 _필요_ Xcode 9로 bitcode watchOS 및 tvOS 응용 프로그램 등을 수행 해야 합니다.

### <a name="can-i-change-the-output-path-of-the-ipa-fileipa-output-pathmd"></a>[IPA 파일의 출력 경로 변경할 수 있나요?](ipa-output-path.md)
Xamarin 주기 7을 기준으로이 작업을 수행할 사용자 지정 된 MSBuild 대상을 사용할 수 있습니다.

### <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folderipa-tfsmd"></a>[TFS 저장 폴더에 IPA 출력 파일을 복사할 수는 방법](ipa-tfs.md)
이 가이드에서는 설명 하는 예, 방법입니다.

### <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studiomodify-ipamd"></a>[파일을 추가 하거나 Visual Studio에서 빌드한 후 IPA 파일에서 파일을 제거할 수 있습니까?](modify-ipa.md)
예, 수도 있지만 다시 서명 하는 일반적으로 필요는 `.app` 변경을 수행한 후 번들입니다. 해당 수정 참고는 `.ipa` 파일이 일반적인 사용 시에 필요 하지 않습니다. 이 문서는 정보 제공 용도로 제공 됩니다.

### <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studiocreate-xcarchivemd"></a>[Visual Studio에서.xcarchive 보관을 만들 수는?](create-xcarchive.md)
Xamarin 4부터 이제 수는 만들기는 `.xcarchive` 설정 하 여 Windows에서의 `ArchiveOnBuild` 속성을 `true`합니다.

### <a name="why-does-my-app-submission-fail-with-disallowed-paths--itunesmetadataplist--found-at--itunesmetadata-disallowed-pathsmd"></a>[“Disallowed paths ( "iTunesMetadata.plist" ) found at ...”(허용되지 않은 경로(“iTunesMetadata.plist”)가 발견되었습니다...) 오류와 함께 앱 제출에 실패하는 이유는 무엇인가요?](itunesmetadata-disallowed-paths.md)
이 오류에는 Apple 앱 스토어 확인 프로세스에 변경의 결과입니다. 이 특정 오류는 _하지_ Xamarin이 설치 되어 있는의 특정 버전과 관련 된, 따라서 다운 그레이드 됩니다 _하지_ 도움말입니다. 문제를 해결 하는 방법에 대 한 자세한 내용은이 가이드 연결 되어 있습니다.


## <a name="diagnosing-specific-error-messages"></a>특정 오류 메시지 진단

### <a name="ios-designer-error-with-registerserviceporterror-registerserviceportmd"></a>[RegisterServicePort에 발생하는 iOS Designer 오류](error-registerserviceport.md)
사용 하 여 오류 `RegisterServicePort` 위에 같은 비슷한 오류 메시지가 문제 스파이웨어/컴퓨터에서 맬웨어를 일반적으로 및 합니다. 이 가이드를 세부 정보 진단 및 스파이웨어/맬웨어를 제거 하는 방법에 대 한 정보를 확인 합니다.

### <a name="why-does-my-ios-build-fail-with-no-valid-iphone-code-signing-keys-found-in-keychainno-codesigning-keysmd"></a>[키 집합에 유효한 iPhone 코드 서명 키가 없다는 오류와 함께 iOS 빌드에 실패하는 이유는 무엇인가요?](no-codesigning-keys.md)
이 오류 메시지에는 찾을 수 없는 하지만 문제의 프로젝트에서 올바른 코드 서명 자격 증명을 찾고 때 발생 합니다. 코드 서명 하는 것은; 실제 iOS 장치에 배포에 필요 뿐만 아니라 임시 및 응용 프로그램 빌드를 저장 합니다.

### <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-objectexception-marshal-obj-cmd"></a>[Objective-C 개체를 마샬링하지 못하는 System.Exception 오류와 함께 iOS 9 앱에 실패하는 이유는 무엇인가요?](exception-marshal-obj-c.md)
IOS 9에서에서 API 변경 예상 이제 기본 API로 비관리 코드를 호출할 때 콜백 생성자를 사용 하도록 해야 합니다.

### <a name="runtime-error-the-assembly-mscorlibdll-was-not-found-or-could-not-be-loadederror-mscorlib-not-foundmd"></a>[런타임 오류: 어셈블리 mscorlib.dll을 찾을 수 없거나 로드할 수 없습니다.](error-mscorlib-not-found.md)
이 문제는 발생 때는 *숨겨진* `.monotouch-32` 및 `.monotouch-64` 에서 누락 된 폴더는 `.xcarchive` 서명에 / IPA 만들기, 런타임 오류를 발생 시키는 합니다.

## <a name="deprecated"></a>사용 되지 않음

> [!IMPORTANT]
> 아래 문서는 최신 버전의 Xamarin에 해결 된 문제에 적용 됩니다. 그러나 최신 버전의 소프트웨어에 문제가 발생 하면 보관는 [새 버그](~/cross-platform/troubleshooting/questions/howto-file-bug.md) 정보 및 전체 빌드 로그 출력 전체 프로그램 버전 관리를 사용 합니다.



### <a name="ipa-file-is-0-bytesipa-zero-bytesmd"></a>[IPA 파일이 0바이트입니다.](ipa-zero-bytes.md)
이전 버전의 Xamarin IPA 파일 windows 0 바이트 수를 일으킬 수 있는 몇 가지 알려진된 문제가 있었습니다.

### <a name="ibtool-error-the-operation-couldnt-be-completederror-ibtoolmd"></a>[IBTool 오류: 작업을 완료할 수 없습니다.](error-ibtool.md)
Apple [고정](https://developer.apple.com/library/ios/releasenotes/DeveloperTools/RN-Xcode/Chapters/xc6_release_notes.html) 이 `ibtool` Xcode 6.1.1에 따라서 Xcode 6.1.1 업그레이드 이상에 버그는 가장 쉬운 해결 합니다.

### <a name="error-mt1009-could-not-copy-the-assemblyerror-mt1009md"></a>[오류 MT1009: 어셈블리를 복사할 수 없습니다.](error-mt1009.md)
Xamarin.iOS 7.2.6를 실행 하는 사용자가 결정 됩니다. 이 문제는 개발자의 주요 계정을 다음 파일 사용 권한을 다른 사용자 계정으로 Xamarin.iOS 설치 될 때 더 높은 권한이 필요한 때문입니다.

### <a name="systemexception-amdevicenotificationsubscribe-returned-exception-amddevicenotificationsubscribemd"></a>[System.Exception AMDeviceNotificationSubscribe가 반환되었습니다...](exception-amddevicenotificationsubscribe.md)
또는, Mac 용 Visual Studio를 처음 시작 오류 대화 상자에이 메시지가 나타날 수는 `mtbserver.log` 파일입니다. 일반적이 지 않은 문제 인지 note 합니다. Visual Studio는 Mac 빌드 호스트 연결을 문제가 많은 경우에 발생할 가능성이 높은 다른 오류는 `mtbserver.log` 파일입니다.

### <a name="mdocarchivetomsxdocconverterexe-not-found-rverbasecommandonrequestmdocarchivetomsxdocconverter-not-foundmd"></a>[MDocArchiveToMsxDocConverter.exe not found rver.BaseCommand.OnRequest](mdocarchivetomsxdocconverter-not-found.md)
이 오류에 나타날 수는 `Mac Server Log` Visual Studio에서.
