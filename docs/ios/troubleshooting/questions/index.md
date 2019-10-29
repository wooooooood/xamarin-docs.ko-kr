---
title: Xamarin.ios 질문과 대답
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 65E04188-185D-493D-BA3C-A89711CB6CAF
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: e79fca8c59ae49d27cd335106ca57945be106031
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031083"
---
# <a name="ios-frequently-asked-questions"></a>iOS faq (질문과 대답)

## <a name="general-questions"></a>일반적인 질문

### <a name="can-i-use-a-mac-vm-with-xamarinmac-vmmd"></a>[Xamarin에서 Mac VM을 사용할 수 있나요?](mac-vm.md)
예. 단, Mac 하드웨어 에서만 가능 합니다.

### <a name="how-can-i-downgrade-xcodedowngrade-xcodemd"></a>[Xcode를 다운그레이드하려면 어떻게 해야 하나요?](downgrade-xcode.md)
이 가이드에서는 이전 버전의 Xcode 및 최신 버전에 액세스 하기 위한 링크를 제공 합니다.

### <a name="where-can-i-set-my-ios-sdk-locationsios-sdkmd"></a>[iOS SDK 위치를 어디에 설정할 수 있나요?](ios-sdk.md)
대부분의 사용자에 게는 적절 한 위치로 자동으로 설정 됩니다. 이 가이드는 기본 SDK 위치 및 필요한 경우 변경 하는 방법을 나열 합니다.

### <a name="how-can-i-reenable-developer-options-after-updating-iosupdate-developer-optionsmd"></a>[IOS를 업데이트 한 후 개발자 옵션을 다시 활성화 하려면 어떻게 해야 하나요?](update-developer-options.md)
Ios 버그가 발생 하면 ios 버전을 업데이트 한 후 개발자 옵션이 사라질 수 있으며, iOS 4.x로 전환할 때이 문제가 관찰 되었습니다. 이 가이드에서는 옵션을 했다가 다시 설정할 하는 방법을 설명 합니다.

### <a name="user-location-not-working-in-ios-8ios8-user-locationmd"></a>[iOS 8에서 작동하지 않는 사용자 위치](ios8-user-location.md)
이 가이드에서는 iOS 8에서 사용자 위치를 사용 하도록 info.plist를 편집 하는 방법을 설명 합니다.

### <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logssymbolicate-ios-crashmd"></a>[iOS 크래시 로그를 기호로 표시하는 .dSYM 파일을 어디에서 찾을 수 있나요?](symbolicate-ios-crash.md)
이 가이드에서는 충돌을 진단 하는 데 도움이 되도록 iOS 크래시 로그를 symbolicating 하는 기본 단계에 대해 설명 합니다. 또한 고급 기호에 대 한 추가 리소스에 대 한 링크를 제공 하 여 iOS 크래시 로그 해석에 대 한 정보를 & 합니다.

### <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studioxs-mono-runtimemd"></a>[Xamarin Studio에서 iOS 프로젝트에 대한 Mono 런타임 환경 변수를 설정하려면 어떻게 해야 하나요?](xs-mono-runtime.md)
Mono에 대 한 런타임 환경 변수를 설정 해야 하는 경우 **프로젝트 옵션에서 > 일반 페이지 실행 >** 설정할 수 있습니다.

## <a name="publishing-questions"></a>질문 게시

### <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submissioninvalid-bundle-bitcodemd"></a>[앱 스토어에 제출 하는 동안 오류 발생: "잘못 된 번들에 포함 될 수 없는 옵션은 전송에서 검색 됩니다."](invalid-bundle-bitcode.md)

WatchOS 및 tvOS apps와 같이 bitcode를 _필요로_ 하는 앱을 제출 하려면 Xcode 9를 사용 하 여 작업을 수행 해야 합니다.

### <a name="can-i-change-the-output-path-of-the-ipa-fileipa-output-pathmd"></a>[IPA 파일의 출력 경로를 변경할 수 있나요?](ipa-output-path.md)
Xamarin 주기 7에서는 사용자 지정 된 MSBuild 대상을 사용 하 여이를 달성할 수 있습니다.

### <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folderipa-tfsmd"></a>[IPA 출력 파일을 TFS drop 폴더로 복사 하려면 어떻게 해야 하나요?](ipa-tfs.md)
예,이 가이드는 방법에 대해 설명 합니다.

### <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studiomodify-ipamd"></a>[Visual Studio에서 빌드한 후 파일을 IPA 파일에 추가 하거나 파일을 제거할 수 있나요?](modify-ipa.md)
예, 가능 하지만 변경을 수행한 후에는 `.app` 번들에 다시 서명 해야 합니다. 일반적인 사용에서는 `.ipa` 파일을 수정할 필요가 없습니다. 이 문서는 참고용 으로만 제공 됩니다.

### <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studiocreate-xcarchivemd"></a>[Visual Studio에서 .xcarchive 보관 파일을 만들 수 있나요?](create-xcarchive.md)
Xamarin 4를 기준으로 이제 `ArchiveOnBuild` 속성을 `true`로 설정 하 여 Windows에서 `.xcarchive`를 만들 수 있습니다.

### <a name="why-does-my-app-submission-fail-with-disallowed-paths--itunesmetadataplist--found-at--itunesmetadata-disallowed-pathsmd"></a>[“Disallowed paths ( "iTunesMetadata.plist" ) found at ...”(허용되지 않은 경로(“iTunesMetadata.plist”)가 발견되었습니다...) 오류와 함께 앱 제출에 실패하는 이유는 무엇인가요?](itunesmetadata-disallowed-paths.md)
이 오류는 Apple 앱 스토어 확인 프로세스의 변경에 대 한 결과입니다. 이 특정 오류는 설치한 Xamarin의 특정 _버전과 관련이 없으므로 다운 그레이드는 도움이_ _되지_ 않습니다. 이 가이드는 문제를 해결 하는 방법에 대 한 자세한 정보로 연결 됩니다.

## <a name="diagnosing-specific-error-messages"></a>특정 오류 메시지 진단

### <a name="ios-designer-error-with-registerserviceporterror-registerserviceportmd"></a>[RegisterServicePort에 발생하는 iOS Designer 오류](error-registerserviceport.md)
위와 같은 `RegisterServicePort` 및 유사한 오류 메시지와 함께 발생 하는 오류는 일반적으로 컴퓨터의 스파이웨어/맬웨어에 문제가 있는 경우에 발생 합니다. 이 가이드에서는 스파이웨어/맬웨어 제거에 대 한 진단 및 정보를 확인 하는 방법에 대해 자세히 설명 합니다.

### <a name="why-does-my-ios-build-fail-with-no-valid-iphone-code-signing-keys-found-in-keychainno-codesigning-keysmd"></a>[키 집합에 유효한 iPhone 코드 서명 키가 없다는 오류와 함께 iOS 빌드에 실패하는 이유는 무엇인가요?](no-codesigning-keys.md)
이 오류 메시지는 해당 프로젝트가 유효한 코드 서명 자격 증명을 찾고 있지만 찾을 수 없는 경우에 발생 합니다. 물리적 iOS 장치에서 테스트 및 배포에는 코드 서명이 필요 합니다. 임시 & App store 빌드도 있습니다.

### <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-objectexception-marshal-obj-cmd"></a>[Objective-C 개체를 마샬링하지 못하는 System.Exception 오류와 함께 iOS 9 앱에 실패하는 이유는 무엇인가요?](exception-marshal-obj-c.md)
IOS 9의 API 변경에는 이제 기본 API에서 예상 하는 것 처럼 비관리 코드를 호출할 때 콜백 생성자를 사용 해야 합니다.

### <a name="runtime-error-the-assembly-mscorlibdll-was-not-found-or-could-not-be-loadederror-mscorlib-not-foundmd"></a>[런타임 오류: 어셈블리 mscorlib.dll을 찾을 수 없거나 로드할 수 없습니다.](error-mscorlib-not-found.md)
이 문제는 서명/IPA 만들기 `.xcarchive`에 *숨겨진* `.monotouch-32` 및 `.monotouch-64` 폴더가 없는 경우에 발생 합니다 .이는 런타임 오류를 트리거합니다.

### <a name="compile-error-can-not-encode-offset-x-in-resulting-scattered-relocationerror-encode-offset-scattered-relocationmd"></a>[컴파일 오류: 분산 재배치에서 오프셋 X를 인코딩할 수 없습니다.](error-encode-offset-scattered-relocation.md)
이 문제는 ARMv7와 같은 32 비트 아키텍처에 대해 빌드할 때 최종 바이너리가 네이티브 도구 체인에 비해 너무 클 경우 발생 합니다.

## <a name="deprecated"></a>Mapi

> [!IMPORTANT]
> 다음 문서는 최신 버전의 Xamarin에서 해결 된 문제에 적용 됩니다. 그러나 최신 버전의 소프트웨어에서 문제가 발생 하는 경우 전체 버전 정보 및 전체 빌드 로그 출력을 사용 하 여 [새 버그](~/cross-platform/troubleshooting/questions/howto-file-bug.md) 를 작성 하세요.

### <a name="ipa-file-is-0-bytesipa-zero-bytesmd"></a>[IPA 파일이 0바이트입니다.](ipa-zero-bytes.md)
이전 버전의 Xamarin에서는 Windows의 IPA 파일을 0 바이트로 만드는 몇 가지 알려진 문제가 있었습니다.

### <a name="ibtool-error-the-operation-couldnt-be-completederror-ibtoolmd"></a>[IBTool 오류: 작업을 완료할 수 없습니다.](error-ibtool.md)
Apple 은 Xcode 6.1.1에서 이 `ibtool`버그를 [수정](https://developer.apple.com/library/ios/releasenotes/DeveloperTools/RN-Xcode/Chapters/xc6_release_notes.html) 했으므로 Xcode 6.1.1 이상으로 업그레이드 하는 것이 가장 쉬운 픽스입니다.

### <a name="error-mt1009-could-not-copy-the-assemblyerror-mt1009md"></a>[오류 MT1009: 어셈블리를 복사할 수 없습니다.](error-mt1009.md)
이는 Xamarin.ios 7.2.6를 실행 하는 사용자에 게 영향을 줍니다. 이 문제는 Xamarin.ios가 다른 사용자 계정으로 설치 된 후 개발자의 기본 계정을 사용 하 여 더 높은 권한을 필요로 하는 파일 사용 권한 때문에 발생 합니다.

### <a name="systemexception-amdevicenotificationsubscribe-returned-exception-amddevicenotificationsubscribemd"></a>[System.Exception AMDeviceNotificationSubscribe가 반환되었습니다...](exception-amddevicenotificationsubscribe.md)
이 메시지는 Mac용 Visual Studio를 처음 시작 하거나 `mtbserver.log` 파일에서 오류 대화 상자에 표시 될 수 있습니다. 일반적이 지 않은 문제입니다. Mac 빌드 호스트에 연결 하는 데 문제가 있는 경우 `mtbserver.log` 파일에 표시 될 가능성이 높은 다른 오류가 있습니다.

### <a name="mdocarchivetomsxdocconverterexe-not-found-rverbasecommandonrequestmdocarchivetomsxdocconverter-not-foundmd"></a>[MDocArchiveToMsxDocConverter.exe not found rver.BaseCommand.OnRequest](mdocarchivetomsxdocconverter-not-found.md)
이 오류는 Visual Studio의 `Mac Server Log`에 나타날 수 있습니다.
