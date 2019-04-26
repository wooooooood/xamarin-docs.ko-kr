---
title: Xamarin.iOS에 대 한 질문과 대답
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 65E04188-185D-493D-BA3C-A89711CB6CAF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 8b90b76cad5277fe76fc476a0bcd6f600e91b256
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61421830"
---
# <a name="ios-frequently-asked-questions"></a>Frequently Asked Questions iOS

## <a name="general-questions"></a>일반적인 질문

### <a name="can-i-use-a-mac-vm-with-xamarinmac-vmmd"></a>[Xamarin에서 Mac VM을 사용할 수 있나요?](mac-vm.md)
예, Mac 하드웨어에만 있습니다.

### <a name="how-can-i-downgrade-xcodedowngrade-xcodemd"></a>[Xcode를 다운그레이드하려면 어떻게 해야 하나요?](downgrade-xcode.md)
이 가이드는 이전 버전의 Xcode와 최신 버전에 액세스 하는 링크를 제공 합니다.

### <a name="where-can-i-set-my-ios-sdk-locationsios-sdkmd"></a>[iOS SDK 위치를 어디에 설정할 수 있나요?](ios-sdk.md)
대부분의 사용자에 대 한 다음 적절 한 위치에 자동으로 설정 됩니다. 이 가이드는 기본 SDK 위치 및 필요에 따라 변경 하는 방법을 나열 합니다.

### <a name="how-can-i-reenable-developer-options-after-updating-iosupdate-developer-optionsmd"></a>[IOS를 업데이트 한 후 개발자 옵션 다시 수는 방법](update-developer-options.md)
iOS 버그가 발생 사라질 iOS 버전을 업데이트 한 후 개발자 옵션을이 관찰 한 적이 iOS로 전환할 때 8.x 합니다. 이 가이드에서는 옵션을 다시 활성화할 수 있습니다 하는 방법을 설명 합니다.

### <a name="user-location-not-working-in-ios-8ios8-user-locationmd"></a>[iOS 8에서 작동하지 않는 사용자 위치](ios8-user-location.md)
이 가이드에서는 iOS 8에서에서 사용자 위치를 사용 하도록 설정 하려면 info.plist를 편집 하는 방법을 알려줍니다.

### <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logssymbolicate-ios-crashmd"></a>[iOS 크래시 로그를 기호로 표시하는 .dSYM 파일을 어디에서 찾을 수 있나요?](symbolicate-ios-crash.md)
이 가이드에서는 symbolicating iOS 크래시 로그 충돌 진단에 도움이 되는 기본 단계를 설명 합니다. 또한 보다 고급 기호화 기술 및 iOS 크래시 로그를 해석에 대 한 정보에 대 한 추가 리소스에 연결 됩니다.


### <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studioxs-mono-runtimemd"></a>[Xamarin Studio에서 iOS 프로젝트에 대한 Mono 런타임 환경 변수를 설정하려면 어떻게 해야 하나요?](xs-mono-runtime.md)
Mono에 대 한 모든 런타임 환경 변수를 설정 해야 할 경우 설정할 수 있습니다 합니다 **프로젝트 옵션 > 실행 > 일반** 페이지입니다.

## <a name="publishing-questions"></a>질문 게시

### <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submissioninvalid-bundle-bitcodemd"></a>[앱 스토어에 제출 하는 동안 오류가 발생 했습니다. "잘못 된 번들-bitcode에 포함 될 수 없는 옵션이 제출에서 감지 됩니다."](invalid-bundle-bitcode.md)

앱을 제출 하는 _필요_ watchOS 및 tvOS 앱과 같은 bitcode Xcode 9를 사용 하 여 수행 해야 합니다.

### <a name="can-i-change-the-output-path-of-the-ipa-fileipa-output-pathmd"></a>[IPA 파일의 출력 경로 변경할 수 있나요?](ipa-output-path.md)
Xamarin 주기 7 일부 터이 달성 하기 위해 사용자 지정된 MSBuild 대상으로 사용할 수 있습니다.

### <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folderipa-tfsmd"></a>[TFS 저장 폴더에 IPA 출력 파일을 복사할 수는 방법](ipa-tfs.md)
예,이 가이드에서는 설명 하는 방법입니다.

### <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studiomodify-ipamd"></a>[파일을 추가 하거나 Visual Studio에서 빌드한 후 IPA 파일에서 파일을 제거 수 있나요?](modify-ipa.md)
수는 있지만 일반적으로 해야 하 고 다시 서명 하는 예는 `.app` 변경을 수행한 후 번들입니다. 해당 수정 확인을 `.ipa` 파일이 일반적인 사용의 필요 하지 않습니다. 이 문서는 정보 제공을 위해서만 제공 됩니다.

### <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studiocreate-xcarchivemd"></a>[Visual Studio에서.xcarchive 아카이브를 만들 수는?](create-xcarchive.md)
Xamarin 4부터 아니며 이제 만들 수는 `.xcarchive` 설정 하 여 Windows에서의 `ArchiveOnBuild` 속성을 `true`입니다.

### <a name="why-does-my-app-submission-fail-with-disallowed-paths--itunesmetadataplist--found-at--itunesmetadata-disallowed-pathsmd"></a>[다음 메시지가 표시되며 내 앱 제출에 실패한 이유는 무엇인가요? "...에서 허용되지 않는 경로("iTunesMetadata.plist")를 발견했습니다."](itunesmetadata-disallowed-paths.md)
이 오류는 Apple 앱 스토어 확인 프로세스에 변경의 결과입니다. 이 특정 오류는 _되지_ 에 관련 된 특정 버전의 Xamarin이 설치 되어 있는 것 이므로 다운 그레이드 됩니다 _하지_ 도움말입니다. 이 가이드는 문제를 해결 하는 방법에 대 한 자세한 정보에 연결 됩니다.


## <a name="diagnosing-specific-error-messages"></a>특정 오류 메시지 진단

### <a name="ios-designer-error-with-registerserviceporterror-registerserviceportmd"></a>[RegisterServicePort에 발생하는 iOS Designer 오류](error-registerserviceport.md)
사용 하 여 오류 `RegisterServicePort` 위에 같은 유사한 오류 메시지는 일반적으로 스파이웨어/컴퓨터의 맬웨어 문제가 및 합니다. 이 가이드를 자세히 설명 진단과 스파이웨어/맬웨어 제거에 대 한 정보를 확인 합니다.

### <a name="why-does-my-ios-build-fail-with-no-valid-iphone-code-signing-keys-found-in-keychainno-codesigning-keysmd"></a>[키 집합에 유효한 iPhone 코드 서명 키가 없다는 오류와 함께 iOS 빌드에 실패하는 이유는 무엇인가요?](no-codesigning-keys.md)
이 오류 메시지 때 해당 프로젝트를 유효한 코드 서명 자격 증명을 찾고 발생 되지만 찾을 수 없습니다. 테스트 및 실제 iOS 장치에 배포에 반드시 코드 서명 뿐만 아니라 임시 및 앱 빌드를 저장 합니다.

### <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-objectexception-marshal-obj-cmd"></a>[다음 메시지를 표시하며 내 iOS 9 앱이 실패한 이유는 무엇인가요? System.Exception: Objective-C 개체를 마샬링하지 못했습니다.](exception-marshal-obj-c.md)
IOS 9에서에서 API 변경 필요 이제 기본 API로 관리 되지 않는 코드를 호출 하는 경우 콜백 생성자를 사용는 필요 합니다.

### <a name="runtime-error-the-assembly-mscorlibdll-was-not-found-or-could-not-be-loadederror-mscorlib-not-foundmd"></a>[런타임 오류: 어셈블리 mscorlib.dll을 찾을 수 없거나 로드할 수 없습니다.](error-mscorlib-not-found.md)
이런 경우는 *숨겨진* `.monotouch-32` 및 `.monotouch-64` 폴더에서 누락 된를 `.xcarchive` 서명에 / IPA 만들기, 런타임 오류를 발생 시키는.

## <a name="deprecated"></a>사용되지 않음

> [!IMPORTANT]
> 아래 문서는 Xamarin의 최신 버전에서 해결 된 문제에 적용 됩니다. 그러나 소프트웨어의 최신 버전에 문제가 발생 하면 제출 하세요를 [새 버그](~/cross-platform/troubleshooting/questions/howto-file-bug.md) 정보 및 전체 빌드 로그 출력 전체 버전을 사용 하 여 합니다.



### <a name="ipa-file-is-0-bytesipa-zero-bytesmd"></a>[IPA 파일이 0바이트입니다.](ipa-zero-bytes.md)
이전 버전 Windows 0 바이트에 IPA 파일을 일으킬 수 있는 Xamarin의 몇 가지 알려진된 문제가 있었습니다.

### <a name="ibtool-error-the-operation-couldnt-be-completederror-ibtoolmd"></a>[IBTool 오류: 작업을 완료할 수 없습니다.](error-ibtool.md)
Apple [고정](https://developer.apple.com/library/ios/releasenotes/DeveloperTools/RN-Xcode/Chapters/xc6_release_notes.html) 이 `ibtool` 6.1.1 않으므로 Xcode 6.1.1로 업그레이드 하거나 더 높은 Xcode에서 버그는 가장 손쉬운 해결 합니다.

### <a name="error-mt1009-could-not-copy-the-assemblyerror-mt1009md"></a>[오류 MT1009: 어셈블리를 복사할 수 없습니다.](error-mt1009.md)
이 7.2.6 Xamarin.iOS를 실행 하는 사용자를 영향을 줍니다. 이 문제는 다른 사용자 계정을 사용 하 여 Xamarin.iOS 설치 될 때 더 높은 권한이 필요로 하는 파일 권한으로 인해 다음 개발자의 기본 계정입니다.

### <a name="systemexception-amdevicenotificationsubscribe-returned-exception-amddevicenotificationsubscribemd"></a>[System.Exception AMDeviceNotificationSubscribe가 반환되었습니다...](exception-amddevicenotificationsubscribe.md)
또는 Mac 용 Visual Studio를 처음 시작할 때 오류 대화 상자에이 메시지가 나타날 수는 `mtbserver.log` 파일입니다. 이 일반적이 지 않은 문제는 note 합니다. 에 표시 될 가능성이 있는 다른 오류가 있는 Visual Studio는 Mac 빌드 호스트에 연결 하는 데 문제가 있는 경우는 `mtbserver.log` 파일입니다.

### <a name="mdocarchivetomsxdocconverterexe-not-found-rverbasecommandonrequestmdocarchivetomsxdocconverter-not-foundmd"></a>[MDocArchiveToMsxDocConverter.exe not found rver.BaseCommand.OnRequest](mdocarchivetomsxdocconverter-not-found.md)
이 오류가 나타날 수는 `Mac Server Log` Visual Studio에서.
