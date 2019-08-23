---
title: Xamarin.ios 오류
description: 이 문서에서는 Xamarin.ios 응용 프로그램을 묶는 데 사용 되는 도구인 mtouch에서 생성 되는 다양 한 오류에 대해 설명 합니다. 오류는 코드 별로 나열 되며 전체 설명이 제공 됩니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9F76162B-D622-45DA-996B-2FBF8017E208
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/06/2018
ms.openlocfilehash: 588c46274aa0b4d77742d004bf1fbe91e56a42c6
ms.sourcegitcommit: 0df727caf941f1fa0aca680ec871bfe7a9089e7c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69620602"
---
# <a name="xamarinios-errors"></a>Xamarin.ios 오류

## <a name="mt0xxx-mtouch-error-messages"></a>MT0xxx: mtouch 오류 메시지

예를 들어 매개 변수, 환경, 누락 된 도구입니다.

<!--
 MT0xxx mtouch itself, e.g. parameters, environment (e.g. missing tools)
 https://github.com/xamarin/xamarin-macios/blob/master/tools/mtouch/error.cs
  -->

<a name="MT0000" />

### <a name="mt0000-unexpected-error---please-fill-a-bug-report-at-httpsgithubcomxamarinxamarin-maciosissuesnew"></a>MT0000: 예기치 않은 오류입니다. 버그 보고서를 작성해 주세요. https://github.com/xamarin/xamarin-macios/issues/new

예기치 않은 오류 조건이 발생 했습니다. 다음과 같이 최대한 많은 정보를 사용 하 여 [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) 에서 새 문제를 해결 하세요.

* 최대 세부 정보 표시 (예: `-v -v -v -v` **추가 mtouch 인수**)를 사용 하는 전체 빌드 로그
* 오류를 재현 하는 최소 테스트 사례입니다. 하거나
* 모든 버전 정보 롤업

정확한 버전 정보를 얻는 가장 쉬운 방법은 **Mac용 Visual Studio** 메뉴, Mac용 Visual Studio 항목에 **대 한** 자세한 **정보 표시** 단추를 사용 하 고 버전 정보 롤업를 복사/붙여 넣는 것입니다 ( **정보 복사** 단추를 사용할 수 있음). .

<a name="MT0001" />

### <a name="mt0001--devname-was-provided-without-any-device-specific-action"></a>MT0001: 장치 관련 작업 없이 '-devname '이 제공 되었습니다.

이 경고는 장치 관련 작업 (-logdev/-installdev/-killdev/-launchdev/-listapps)이 요청 되지 않은 경우-devname이 mtouch에 전달 될 때 발생 하는 경고입니다.

<a name="MT0002" />

### <a name="mt0002-could-not-parse-the-environment-variable-"></a>MT0002: 환경 변수 *을 (를) 구문 분석할 수 없습니다.

이 오류는 잘못 된 환경 키 = 값 변수 쌍을 설정 하려고 할 때 발생 합니다. 올바른 형식은 다음과 같습니다.`mtouch --setenv=VARIABLE=VALUE`

<a name="MT0003" />

### <a name="mt0003-application-name-exe-conflicts-with-an-sdk-or-product-assembly-dll-name"></a>MT0003: 응용 프로그램 이름 ' * .exe '가 SDK 또는 제품 어셈블리 (.dll) 이름과 충돌 합니다.

실행 파일 어셈블리의 이름과 응용 프로그램 이름은 앱에 있는 모든 dll의 이름과 일치할 수 없습니다. 실행 파일의 이름을 수정 하십시오.

<a name="MT0004" />

### <a name="mt0004-new-refcounting-logic-requires-sgen-to-be-enabled-too"></a>MT0004: 새 refcounting 논리를 사용 하려면 SGen를 사용 하도록 설정 해야 합니다.

Refcounting 확장을 사용 하도록 설정 하는 경우 프로젝트의 iOS 빌드 옵션 (고급 탭) 에서도 SGen 가비지 수집기를 사용 하도록 설정 해야 합니다.

7\.2.1부터이 요구 사항이 리프트 된 후 Boehm 및 SGen 가비지 수집기를 사용 하 여 새 refcounting 논리를 사용 하도록 설정할 수 있습니다.

<a name="MT0005" />

### <a name="mt0005-the-output-directory--does-not-exist"></a>MT0005: 출력 디렉터리 *가 없습니다.

디렉터리를 만드세요.

이 오류는 더 이상 생성 되지 않습니다. mtouch가 존재 하지 않는 경우 디렉터리를 자동으로 만듭니다.

<a name="MT0006" />

### <a name="mt0006-there-is-no-devel-platform-at--use---platformplat-to-specify-the-sdk"></a>MT0006: *에 개발자 플랫폼이 없습니다.--platform = cross-plat을 사용 하 여 SDK를 지정 합니다.

Xamarin.ios는 오류 메시지에 언급 된 위치에서 SDK 디렉터리를 찾을 수 없습니다. 경로가 올바른지 확인 하십시오.

<a name="MT0007" />

### <a name="mt0007-the-root-assembly--does-not-exist"></a>MT0007: 루트 어셈블리 *가 없습니다.

Xamarin.ios는 오류 메시지에 언급 된 위치에서 어셈블리를 찾을 수 없습니다. 경로가 올바른지 확인 하십시오.

<a name="MT0008" />

### <a name="mt0008-you-should-provide-one-root-assembly-only-found--assemblies-"></a>MT0008: 하나의 루트 어셈블리만 제공 하 고 # assemblies: *를 찾았습니다.

Mtouch에 두 개 이상의 루트 어셈블리가 전달 되었지만 하나의 루트 어셈블리만 있을 수 있습니다.

<a name="MT0009" />

### <a name="mt0009-error-while-loading-assemblies-"></a>MT0009: 어셈블리를 로드 하는 동안 오류가 발생 했습니다. *.

루트 어셈블리 참조에서 어셈블리를 로드 하는 동안 오류가 발생 했습니다. 빌드 출력에 추가 정보가 제공 될 수 있습니다.

<a name="MT0010" />

### <a name="mt0010-could-not-parse-the-command-line-arguments-"></a>MT0010: 명령줄 인수를 구문 분석할 수 없습니다. *.

명령줄 인수를 구문 분석 하는 동안 오류가 발생 했습니다. 모두 올바른지 확인 하세요.

<a name="MT0011" />

### <a name="mt0011--was-built-against-a-more-recent-runtime--than-monotouch-supports"></a>MT0011: *가 Monotouch.dialog에서 지 원하는 보다 최신 런타임 (*)에 대해 빌드 되었습니다.

이 경고는 일반적으로 Xamarin.ios BCL을 사용 하 여 빌드되지 않은 클래스 라이브러리에 대 한 참조가 프로젝트에 있기 때문에 보고 됩니다.

.Net 4.0 SDK를 사용 하는 앱이 .NET 2.0을 지 원하는 시스템에서 작동 하지 않을 수 있는 것과 동일한 방식으로 .NET 4.0을 사용 하 여 빌드된 라이브러리는 Xamarin.ios에서 작동 하지 않을 수 있습니다. Xamarin.ios에는 없는 API를 사용할 수 있습니다.

일반적인 솔루션은 Xamarin.ios 클래스 라이브러리로 라이브러리를 빌드하는 것입니다. 새 Xamarin.ios 클래스 라이브러리 프로젝트를 만들고 여기에 모든 소스 파일을 추가 하 여이를 수행할 수 있습니다. 라이브러리에 대 한 소스 코드가 없는 경우 공급 업체에 문의 하 여 해당 라이브러리의 Xamarin.ios 호환 버전을 제공 하도록 요청 해야 합니다.

<a name="MT0012" />

### <a name="mt0012-incomplete-data-is-provided-to-complete-"></a>MT0012: 완료를 위해 불완전 한 데이터가 제공 됩니다 *.

이 오류는 최신 버전의 Xamarin.ios에서 더 이상 보고 되지 않습니다.

<a name="MT0013" />

### <a name="mt0013-profiling-support-requires-sgen-to-be-enabled-too"></a>MT0013: 프로 파일링을 지원 하려면 sgen를 사용 하도록 설정 해야 합니다.

프로 파일링 (--프로 파일링)을 사용 하는 경우 SGen (--SGen)를 사용 하도록 설정 해야 합니다.

<a name="MT0014" />

### <a name="mt0014-the-ios--sdk-does-not-support-building-applications-targeting-"></a>MT0014: IOS * SDK는 *를 대상으로 하는 응용 프로그램 빌드를 지원 하지 않습니다.

이 문제는 다음과 같은 경우에 발생할 수 있습니다.

* ARMv6를 사용 하도록 설정 하 고 Xcode 4.5 이상을 설치 합니다.
* Armv7s 용 thumb-2를 사용 하도록 설정 하 고 Xcode 4.4 또는 이전 버전을 설치 합니다.

설치 된 Xcode 버전에서 선택한 아키텍처를 지원 하는지 확인 하세요.

<a name="MT0015" />

### <a name="mt0015-invalid-abi--supported-abis-are-i386-x86_64--armv7-armv7llvm-armv7llvmthumb2-armv7s-armv7sllvm-armv7sllvmthumb2-arm64-and-arm64llvm"></a>MT0015: ABI: *이 잘못 되었습니다. 지원 되는 ABIs: i386, x86_64, armv7, armv7 + llvm, armv7 + llvm + thumb2, armv7s 용 thumb-2, armv7s 용 thumb-2 + llvm, armv7s 용 thumb-2 + llvm + thumb2, arm64 및 arm64 + llvm.

Mtouch에 잘못 된 ABI를 전달 했습니다. 올바른 ABI를 지정 하세요.

<a name="MT0016" />

### <a name="mt0016-the-option--has-been-deprecated"></a>MT0016: * 옵션은 더 이상 사용 되지 않습니다.

언급 된 mtouch 옵션은 더 이상 사용 되지 않으며 무시 됩니다.

<a name="MT0017" />

### <a name="mt0017-you-should-provide-a-root-assembly"></a>MT0017: 루트 어셈블리를 제공 해야 합니다.

앱을 빌드할 때 루트 어셈블리 (일반적으로 주 실행 파일)를 지정 해야 합니다.

<a name="MT0018" />

### <a name="mt0018-unknown-command-line-argument-"></a>MT0018: 알 수 없는 명령줄 인수입니다. *.

Mtouch는 오류 메시지에 언급 된 명령줄 인수를 인식 하지 못합니다.

<a name="MT0019" />

### <a name="mt0019-only-one---loginstallkilllaunchdev-or---launchdebugsim-option-can-be-used"></a>MT0019: 단일--[log | install | kill | launch] dev 또는--[launch | debug] sim 옵션을 사용할 수 있습니다.

동시에 사용할 수 없는 mtouch에 대 한 몇 가지 옵션이 있습니다.

- --logdev
- --installdev
- --killdev
- --launchdev
- --launchdebug
- --launchsim

<a name="MT0020" />

### <a name="mt0020-the-valid-options-for--are-"></a>MT0020: '\*'에 대 한 올바른 옵션은\*' '입니다.

<a name="MT0021" />

### <a name="mt0021-cannot-compile-using-gccg---use-gcc-when-using-the-static-registrar-this-is-the-default-when-compiling-for-device-either-remove-the---use-gcc-flag-or-use-the-dynamic-registrar---registrardynamic"></a>MT0021: 정적 등록자를 사용할 때 gcc/g + + (--사용-gcc)를 사용 하 여 컴파일할 수 없습니다 (장치에 대해 컴파일할 때의 기본값). --Gcc 플래그를 제거 하거나 동적 등록자 (--등록자: 동적)를 사용 합니다.

<a name="MT0022" />

### <a name="mt0022-the-options---unsupported--enable-generics-in-registrar-and---registrar-are-not-compatible"></a>MT0022: 지원 되지 않는---------------------------등록자 ' 옵션은 호환 되지 않습니다

`--unsupported--enable-generics-in-registrar` 옵션과`--registrar`를 모두 제거 합니다. Xamarin.ios 7.2.1부터 기본 등록자는 제네릭을 지원 합니다.

이 오류는 더 이상 표시 되지 않습니다 (mtouch에서 명령줄 `--unsupported--enable-generics-in-registrar` 인수가 제거 됨).

<a name="MT0023" />

### <a name="mt0023-application-name-exe-conflicts-with-another-user-assembly"></a>MT0023: 응용 프로그램 이름 ' * .exe '이 (가) 다른 사용자 어셈블리와 충돌 합니다.

실행 파일 어셈블리의 이름과 응용 프로그램 이름은 앱에 있는 모든 dll의 이름과 일치할 수 없습니다. 실행 파일의 이름을 수정 하십시오.

<a name="MT0024" />

### <a name="mt0024-could-not-find-required-file-"></a>MT0024: ' * ' 필수 파일을 찾을 수 없습니다.

<a name="MT0025" />

### <a name="mt0025-no-sdk-version-was-provided-please-add---sdkxy-to-specify-which-ios-sdk-should-be-used-to-build-your-application"></a>MT0025: SDK 버전이 제공 되지 않았습니다. 응용 프로그램 `--sdk=X.Y` 을 빌드하는 데 사용할 iOS SDK를 지정 하려면를 추가 하세요.

<a name="MT0026" />

### <a name="mt0026-could-not-parse-the-command-line-argument--"></a>MT0026: 명령줄 인수 ' * '를 구문 분석할 수 없습니다. *

<a name="MT0027" />

### <a name="mt0027-the-options--and--are-not-compatible"></a>MT0027: 옵션 '\*' 및 '\*'이 (가) 호환 되지 않습니다.

<a name="MT0028" />

### <a name="mt0028-cannot-enable-pie--pie-when-targeting-ios-41-or-earlier-please-disable-pie--piefalse-or-set-the-deployment-target-to-at-least-ios-42"></a>MT0028: IOS 4.1 이전 버전을 대상으로 하는 경우 원형 (-원형)을 사용할 수 없습니다. 원형 (-원형: false)을 사용 하지 않도록 설정 하거나 배포 대상을 iOS 4.2 이상으로 설정 하세요.

<a name="MT0029" />

### <a name="mt0029-repl---enable-repl-is-only-supported-in-the-simulator---sim"></a>MT0029: REPL (--enable-repl)은 시뮬레이터 에서만 지원 됩니다 (--sim).

시뮬레이터에 대해 빌드하는 경우에만 REPL이 지원 됩니다. 즉, mtouch에 전달 `--enable-repl` 하는 경우도 전달 `--sim`해야 합니다.

<a name="MT0030" />

### <a name="mt0030-the-executable-name--and-the-app-name--are-different-this-may-prevent-crash-logs-from-getting-symbolicated-properly"></a>MT0030: 실행 파일 이름 (\*)과 앱 이름 (\*)이 서로 다르므로 충돌 로그가 기호화 된 크래시을 제대로 얻지 못할 수 있습니다.

Xcode symbolicates (메모리 주소를 함수 이름 및 파일/줄 번호)로 변환 하는 경우 실행 파일과 응용 프로그램의 이름이 다른 경우 (확장명 없음) 프로세스가 실패할 수 있습니다.

이 문제를 해결 하려면 프로젝트의 Build/iOS 응용 프로그램 옵션에서 ' 응용 프로그램 이름 '을 변경 하거나 프로젝트의 빌드/출력 옵션에서 ' 어셈블리 이름 '을 변경 합니다.

<a name="MT0031" />

### <a name="mt0031-the-command-line-arguments---enable-background-fetch-and---launch-for-background-fetch-require---launchsim-too"></a>MT0031: 명령줄 인수 '--enable-background-fetch ' 및 '--background-fetch '에는 '--launchsim '이 필요 합니다.

<a name="MT0032" />

### <a name="mt0032-the-option---debugtrack-is-ignored-unless---debug-is-also-specified"></a>MT0032: '--Debug '도 지정 하지 않으면 '--debugtrack ' 옵션이 무시 됩니다.

<a name="MT0033" />

### <a name="mt0033-a-xamarinios-project-must-reference-either-monotouchdll-or-xamariniosdll"></a>MT0033: Xamarin.ios 프로젝트는 monotouch.dialog 또는 Xamarin.ios .dll을 참조 해야 합니다.

<a name="MT0034" />

### <a name="mt0034-cannot-include-both-monotouchdll-and-xamariniosdll-in-the-same-xamarinios-project----is-referenced-explicitly-while--is-referenced-by-"></a>MT0034: 동일한 xamarin.ios 프로젝트\*에 ' monotouch.dialog '와 ' xamarin.ios '를 모두 포함할 수 없습니다. ' '은 (\*는) 명시적으로 참조 되지만 ' '은 ' * '에서 참조 됩니다.

<!-- MT0035 unused -->

<a name="MT0036" />

### <a name="mt0036-cannot-launch-a--simulator-for-a--app-please-enable-the-correct-architectures-in-your-projects-ios-build-options-advanced-page"></a>MT0036: * 앱에 대 한 * 시뮬레이터를 시작할 수 없습니다. 프로젝트의 iOS 빌드 옵션 (고급 페이지)에서 올바른 아키텍처를 사용 하도록 설정 하세요.

<a name="MT0037" />

### <a name="mt0037-monotouchdll-is-not-64-bit-compatible-either-reference-xamariniosdll-or-do-not-build-for-a-64-bit-architecture-arm64-andor-x86_64"></a>MT0037: monotouch.dialog는 64 비트와 호환 되지 않습니다. Xamarin.ios를 참조 하거나 64 비트 아키텍처 (ARM64 및/또는 x86_64)에 대해 빌드하지 않습니다.

<a name="MT0038" />

### <a name="mt0038-the-old-registrars---registraroldstaticolddynamic-are-not-supported-when-referencing-xamariniosdll"></a>MT0038: Xamarin.ios를 참조할 때 이전 등록 기관 (--등록자: oldstatic | oldstatic)은 지원 되지 않습니다.

<a name="MT0039" />

### <a name="mt0039-applications-targeting-armv6-cannot-reference-xamariniosdll"></a>MT0039: ARMv6를 대상으로 하는 응용 프로그램은 Xamarin.ios를 참조할 수 없습니다.

<a name="MT0040" />

### <a name="mt0040-could-not-find-the-assembly--referenced-by-"></a>MT0040: ' '이 (가)\*\*참조 하는 ' ' 어셈블리를 찾을 수 없습니다.

<a name="MT0041" />

### <a name="mt0041-cannot-reference-both-monotouchdll-and-xamariniosdll"></a>MT0041: ' Monotouch.dialog '와 ' Xamarin.ios '를 모두 참조할 수는 없습니다.

<a name="MT0042" />

### <a name="mt0042-no-reference-to-either-monotouchdll-or-xamariniosdll-was-found-a-reference-to-monotouchdll-will-be-added"></a>MT0042: Monotouch.dialog 또는 Xamarin.ios에 대 한 참조를 찾을 수 없습니다. Monotouch.dialog에 대 한 참조가 추가 됩니다.

<a name="MT0043" />

### <a name="mt0043-the-boehm-garbage-collector-is-currently-not-supported-when-referencing-xamariniosdll-the-sgen-garbage-collector-has-been-selected-instead"></a>MT0043: Boehm 가비지 수집기는 현재 ' Xamarin.ios '를 참조할 때 지원 되지 않습니다. 대신 SGen 가비지 수집기를 선택 했습니다.

SGen 가비지 수집기만 통합 프로젝트에서 지원 됩니다. Boehm를 가비지 수집기로 지정 하는 추가 mtouch 플래그가 없는지 확인 합니다.

<a name="MT0044" />

### <a name="mt0044---listsim-is-only-supported-with-xcode-60-or-later"></a>MT0044:--listsim은 Xcode 6.0 이상 에서만 지원 됩니다.

최신 Xcode 버전을 설치 합니다.

<a name="MT0045" />

### <a name="mt0045---extension-is-only-supported-when-using-the-ios-80-or-later-sdk"></a>MT0045:--extension은 iOS 8.0 이상 SDK를 사용 하는 경우에만 지원 됩니다.

<!-- MT0046 is not reported anymore -->

<a name="MT0047" />

### <a name="mt0047-the-minimum-deployment-target-for-unified-applications-is-511-the-current-deployment-target-is--please-select-a-newer-deployment-target-in-your-projects-ios-application-options"></a>MT0047: 통합 응용 프로그램의 최소 배포 대상은 5.1.1, 현재 배포 대상은 ' * '입니다. 프로젝트의 iOS 응용 프로그램 옵션에서 최신 배포 대상을 선택 하세요.

<!-- MT0048 is not reported anymore -->

<a name="MT0049" />

### <a name="mt0049-framework-is-supported-only-if-deployment-target-is-80-or-later--features-might-not-work-correctly"></a>MT0049: *. framework는 배포 대상이 8.0 이상인 경우에만 지원 됩니다. * 기능이 제대로 작동 하지 않을 수 있습니다.

지정 된 프레임 워크는 배포 대상에서 참조 하는 iOS 버전에서 지원 되지 않습니다. 배포 대상을 최신 iOS 버전으로 업데이트 하거나 앱에서 지정 된 프레임 워크의 사용을 제거 합니다.

<!-- MT0050 is not reported anymore -->

<a name="MT0051" />

### <a name="mt0051-xamarinios--requires-xcode-50-or-later-the-current-xcode-version-found-in--is-"></a>MT0051: Xamarin.ios *에는 Xcode 5.0 이상이 필요 합니다. 현재 Xcode 버전 (*에 있음)은 *입니다.

최신 Xcode를 설치 합니다.

<a name="MT0052" />

### <a name="mt0052-no-command-specified"></a>MT0052: 명령을 지정 하지 않았습니다.

Mtouch에 대 한 작업이 지정 되지 않았습니다.

<!-- 0053 is used by mmp -->

<a name="MT0054" />

### <a name="mt0054-unable-to-canonicalize-the-path--"></a>MT0054: ' * ' 경로를 정규화 할 수 없습니다. *

이것은 내부 오류입니다. 이 오류가 표시 되 면 [github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요.

<a name="MT0055" />

### <a name="mt0055-the-xcode-path--does-not-exist"></a>MT0055: Xcode 경로 ' * '가 없습니다.

을 사용 하 여 `--sdkroot` 전달 된 Xcode 경로가 없습니다. 올바른 경로를 지정 하십시오.

<a name="MT0056" />

### <a name="mt0056-cannot-find-xcode-in-the-default-location-applicationsxcodeapp-please-install-xcode-or-pass-a-custom-path-using---sdkroot-path"></a>MT0056: 기본 위치에서 Xcode를 찾을 수 없습니다 (/Sups/xcode.xml 앱). Xcode를 설치 하거나--sdkroot \<path >를 사용 하 여 사용자 지정 경로를 전달 하세요.

<a name="MT0057" />

### <a name="mt0057-cannot-determine-the-path-to-xcodeapp-from-the-sdk-root--please-specify-the-full-path-to-the-xcodeapp-bundle"></a>MT0057: Sdk 루트 ' * '에서 Xcode에 대 한 경로를 확인할 수 없습니다. Xcode 번들에 대 한 전체 경로를 지정 하십시오.

을 사용 하 여 `--sdkroot` 전달 된 경로가 유효한 Xcode 앱을 지정 하지 않습니다. Xcode 앱의 경로를 지정 하세요.

<a name="MT0058" />

### <a name="mt0058-the-xcodeapp--is-invalid-the-file--does-not-exist"></a>MT0058: Xcode '\*'이 (가) 잘못 되었습니다. 파일 '\*'이 (가) 없습니다.

을 사용 하 여 `--sdkroot` 전달 된 경로가 유효한 Xcode 앱을 지정 하지 않습니다. Xcode 앱의 경로를 지정 하세요.

<a name="MT0059" />

### <a name="mt0059-could-not-find-the-currently-selected-xcode-on-the-system-"></a>MT0059: 시스템에서 현재 선택 된 Xcode을 찾을 수 없습니다. *

<a name="MT0060" />

### <a name="mt0060-could-not-find-the-currently-selected-xcode-on-the-system-xcode-select---print-path-returned--but-that-directory-does-not-exist"></a>MT0060: 시스템에서 현재 선택한 Xcode를 찾을 수 없습니다. ' xcode '가 ' * '를 반환 했지만 해당 디렉터리가 없습니다.

<a name="MT0061" />

### <a name="mt0061-no-xcodeapp-specified-using---sdkroot-using-the-system-xcode-as-reported-by-xcode-select---print-path-"></a>MT0061: Xcode가 지정 되지 않았습니다. (--sdkroot 사용), ' Xcode에서 보고 한 시스템 Xcode 사용: *

이 경고는 사용 되는 Xcode를 설명 하는 정보를 제공 하기 위한 것입니다.

<a name="MT0062" />

### <a name="mt0062-no-xcodeapp-specified-using---sdkroot-or-xcode-select---print-path-using-the-default-xcode-instead-"></a>MT0062: Xcode가 지정 되지 않았습니다 (--sdkroot 또는 ' Xcode ' 사용). 대신 기본 Xcode를 사용 합니다. *

이 경고는 사용 되는 Xcode를 설명 하는 정보를 제공 하기 위한 것입니다.

<a name="MT0063" />

### <a name="mt0063-cannot-find-the-executable-in-the-extension--no-cfbundleexecutable-entry-in-its-infoplist"></a>MT0063: 확장에서 실행 파일을 찾을 수 없습니다. * (info.plist에서 CFBundleExecutable 항목 없음)

모든 info.plist에는 실행 파일이 있어야 합니다 (CFBundleExecutable 항목 사용). 그러나 빌드하는 동안 항목이 자동으로 생성 되어야 합니다.

이는 일반적으로 Xamarin.ios의 버그를 나타냅니다. 테스트 사례를 사용 하 여 [github](https://github.com/xamarin/xamarin-macios/issues/new) 에서 새 문제를 해결 하세요.

<a name="MT0064" />

### <a name="mt0064-xamarinios-only-supports-embedded-frameworks-with-unified-projects"></a>MT0064: Xamarin.ios는 통합 프로젝트와 함께 포함 된 프레임 워크도 지원 합니다.

Xamarin.ios는 Unified API를 사용할 때만 포함 된 프레임 워크를 지원 합니다. Unified API를 사용 하려면 프로젝트를 업데이트 하세요.

<a name="MT0065" />

### <a name="mt0065-xamarinios-only-supports-embedded-frameworks-when-deployment-target-is-at-least-80-current-deployment-target--embedded-frameworks-"></a>MT0065: Xamarin.ios는 배포 대상이 8.0 (현재 배포 대상: * 포함 프레임 워크: *) 이상인 경우에만 포함 된 프레임 워크를 지원 합니다.

Xamarin.ios는 배포 대상이 8.0 이상인 경우에만 포함 된 프레임 워크를 지원 합니다. 이전 버전의 iOS는 포함 된 프레임 워크를 지원 하지 않기 때문입니다.

프로젝트 info.plist의 배포 대상을 8.0 이상으로 업데이트 하십시오.

<a name="MT0066" />

### <a name="mt0066-invalid-build-registrar-assembly-"></a>MT0066: 잘못 된 빌드 등록자 어셈블리: *

이는 일반적으로 Xamarin.ios의 버그를 나타냅니다. 테스트 사례를 사용 하 여 [github](https://github.com/xamarin/xamarin-macios/issues/new) 에서 새 문제를 해결 하세요.

<a name="MT0067" />

### <a name="mt0067-invalid-registrar-"></a>MT0067: 잘못 된 등록자: *

이는 일반적으로 Xamarin.ios의 버그를 나타냅니다. 테스트 사례를 사용 하 여 [github](https://github.com/xamarin/xamarin-macios/issues/new) 에서 새 문제를 해결 하세요.

<a name="MT0068" />

### <a name="mt0068-invalid-value-for-target-framework-"></a>MT0068: 대상 프레임 워크에 대 한 값이 잘못 되었습니다. *.

--Target 프레임 워크 인수를 사용 하 여 잘못 된 대상 프레임 워크가 전달 되었습니다. 올바른 대상 프레임 워크를 지정 하십시오.

<a name="MT0069" />

<!--### MT0069: currently unused -->

<a name="MT0070" />

### <a name="mt0070-invalid-target-framework--valid-target-frameworks-are-"></a>MT0070: 잘못 된 대상 프레임 워크: *. 올바른 대상 프레임 워크는 *입니다.

--Target 프레임 워크 인수를 사용 하 여 잘못 된 대상 프레임 워크가 전달 되었습니다. 올바른 대상 프레임 워크를 지정 하십시오.

<a name="MT0071" />

### <a name="mt0071-unknown-platform--this-usually-indicates-a-bug-in-xamarinios-please-file-a-bug-report-at-httpbugzillaxamarincom-with-a-test-case"></a>MT0071: 알 수 없는 플랫폼: *. 이는 일반적으로 Xamarin.ios의 버그를 나타냅니다. 테스트 사례를 사용 하 여 http://bugzilla.xamarin.com 에서 버그 보고서를 파일에 입력 하세요.

이는 일반적으로 Xamarin.ios의 버그를 나타냅니다. 테스트 사례를 사용 하 여 [github](https://github.com/xamarin/xamarin-macios/issues/new) 에서 새 문제를 해결 하세요.

<a name="MT0072" />

### <a name="mt0072-extensions-are-not-supported-for-the-platform-"></a>MT0072: ' * ' 플랫폼에 대해 확장이 지원 되지 않습니다.

이는 일반적으로 Xamarin.ios의 버그를 나타냅니다. 테스트 사례를 사용 하 여 [github](https://github.com/xamarin/xamarin-macios/issues/new) 에서 새 문제를 해결 하세요.

<a name="MT0073" />

### <a name="mt0073-xamarinios--does-not-support-a-deployment-target-of--the-minimum-is--please-select-a-newer-deployment-target-in-your-projects-infoplist"></a>MT0073: Xamarin.ios *는 * (최소 is *)의 배포 대상을 지원 하지 않습니다. 프로젝트의 info.plist에서 최신 배포 대상을 선택 하세요.

최소 배포 대상은 오류 메시지에 지정 된 대상입니다. 프로젝트의 info.plist에서 최신 배포 대상을 선택 하세요.

배포 대상 업데이트를 수행할 수 없는 경우 이전 버전의 Xamarin.ios를 사용 하세요.

<a name="MT0074" />

### <a name="mt0074-xamarinios--does-not-support-a-minimum-deployment-target-of--the-maximum-is--please-select-an-older-deployment-target-in-your-projects-infoplist-or-upgrade-to-a-newer-version-of-xamarinios"></a>MT0074: Xamarin.ios *는 *의 최소 배포 대상 (최대 *)을 지원 하지 않습니다. 프로젝트의 info.plist에서 이전 배포 대상을 선택 하거나 최신 버전의 Xamarin.ios로 업그레이드 하세요.

Xamarin.ios는이 특정 버전의 Xamarin.ios가 빌드된 버전 보다 높은 버전으로 최소 배포 대상을 설정 하는 것을 지원 하지 않습니다.

프로젝트의 info.plist에서 이전 최소 배포 대상을 선택 하거나 최신 버전의 Xamarin.ios로 업그레이드 하세요.

<a name="MT0075" />

### <a name="mt0075-invalid-architecture--for--projects-valid-architectures-are-"></a>MT0075: * 프로젝트의 ' * ' 아키텍처가 잘못 되었습니다. 유효한 아키텍처는 다음과 같습니다. *

잘못 된 아키텍처가 지정 되었습니다. 아키텍처가 올바른지 확인 하세요.

<a name="MT0076" />

### <a name="mt0076-no-architecture-specified-using-the---abi-argument-an-architecture-is-required-for--projects"></a>MT0076: --Abi 인수를 사용 하 여 아키텍처가 지정 되지 않았습니다. \* 프로젝트에는 아키텍처가 필요 합니다.

이는 일반적으로 Xamarin.ios의 버그를 나타냅니다. 테스트 사례를 사용 하 여 [github](https://github.com/xamarin/xamarin-macios/issues/new) 에서 새 문제를 해결 하세요.

<a name="MT0077" />

### <a name="mt0077-watchos-projects-must-be-extensions"></a>MT0077: WatchOS 프로젝트는 확장 이어야 합니다.

이는 일반적으로 Xamarin.ios의 버그를 나타냅니다. 테스트 사례를 사용 하 여 [github](https://github.com/xamarin/xamarin-macios/issues/new) 에서 새 문제를 해결 하세요.

<a name="MT0078" />

### <a name="mt0078-incremental-builds-are-enabled-with-a-deployment-target--80-currently--this-is-not-supported-the-resulting-application-will-not-launch-on-ios-9-so-the-deployment-target-will-be-set-to-80"></a>MT0078: 증분 빌드는 8.0 (현재 *) < 배포 대상으로 사용 하도록 설정 됩니다. 이는 지원 되지 않습니다 (결과 응용 프로그램은 iOS 9에서 시작 되지 않음). 따라서 배포 대상이 8.0로 설정 됩니다.

이 경고는 증분 빌드가 제대로 작동 하도록이 빌드에 대해 배포 대상이 8.0로 설정 되었음을 알리는 경고입니다.

증분 빌드는 배포 대상이 8.0 이상인 경우에만 지원 됩니다. 그 결과 응용 프로그램은 iOS 9에서 시작 되지 않기 때문입니다.

<a name="MT0079" />

### <a name="mt0079-the-recommended-xcode-version-for-xamarinios--is-xcode--or-later-the-current-xcode-version-found-in--is-"></a>MT0079: Xamarin.ios *에 권장 되는 Xcode 버전은 Xcode * 이상입니다. 현재 Xcode 버전 (*에 있음)은 *입니다.

이는 현재 버전의 Xcode가이 버전의 Xamarin.ios에 권장 되는 버전의 Xcode를 알리는 경고입니다.

최적의 동작을 위해 Xcode를 업그레이드 하세요.

<a name="MT0080" />

### <a name="mt0080-disabling-newrefcount---new-refcountfalse-is-deprecated"></a>MT0080: NewRefCount를 사용 하지 않도록 설정 하는 중입니다.--new-refcount: false는 사용 되지 않습니다.

이 경고는 새 refcount (--new-refcount: false)를 사용 하지 않도록 설정 하는 요청이 무시 되었음을 알리는 경고입니다.

이제 모든 프로젝트에 대해 새 refcount 기능이 필수 이며, 따라서 더 이상 사용 하지 않도록 설정할 수 없습니다.

<a name="MT0081" />

### <a name="mt0081-the-command-line-argument---download-crash-report-also-requires---download-crash-report-to"></a>MT0081: 명령줄 인수--다운로드-충돌 보고서에는--다운로드-충돌 보고도 필요 합니다.

<a name="MT0082" />

### <a name="mt0082-repl---enable-repl-is-only-supported-when-linking-is-not-used---nolink"></a>MT0082: REPL (--enable-repl)은 연결이 사용 되지 않는 경우에만 지원 됩니다 (--nolink).

<a name="MT0083" />

### <a name="mt0083-asm-only-bitcode-is-not-supported-on-watchos-use-either---bitcodemarker-or---bitcodefull"></a>MT0083: Asm 전용 bitcode는 watchOS에서 지원 되지 않습니다. --Bitcode: marker 또는--bitcode: full 중 하나를 사용 합니다.

<a name="MT0084" />

### <a name="mt0084-bitcode-is-not-supported-in-the-simulator-do-not-pass---bitcode-when-building-for-the-simulator"></a>MT0084: Bitcode는 시뮬레이터에서 지원 되지 않습니다. 시뮬레이터에 대해 빌드할 때--bitcode를 전달 하지 마십시오.

<a name="MT0085" />

### <a name="mt0085-no-reference-to--was-found-it-will-be-added-automatically"></a>MT0085: ' * '에 대 한 참조를 찾을 수 없습니다. 자동으로 추가 됩니다.

<a name="MT0086" />

### <a name="mt0086-a-target-framework---target-framework-must-be-specified-when-building-for-tvos-or-watchos"></a>MT0086: TVOS 또는 WatchOS에 대해 빌드할 때 대상 프레임 워크 (--target 프레임 워크)를 지정 해야 합니다.

이는 Xamarin.ios의 버그를 나타냅니다. [Github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요.

<a name="MT0087" />

### <a name="mt0087-incremental-builds---fastdev-is-not-supported-with-the-boehm-gc-incremental-builds-will-be-disabled"></a>MT0087: 증분 빌드 (--fastdev)는 Boehm GC에서 지원 되지 않습니다. 증분 빌드를 사용할 수 없습니다.

<a name="MT0088" />

### <a name="mt0088-the-gc-must-be-in-cooperative-mode-for-watchos-apps-please-remove-the---coopfalse-argument-to-mtouch"></a>MT0088: GC는 watchOS apps에 대 한 협조적 모드에 있어야 합니다. Mtouch에 대 한--coop: false 인수를 제거 하세요.

<a name="MT0089" />

### <a name="mt0089-the-option--cannot-take-the-value--when-cooperative-mode-is-enabled-for-the-gc"></a>MT0089: GC에 대해\*협조적 모드를 사용 하는\*경우 ' ' 옵션은 ' ' 값을 사용할 수 없습니다.

<a name="MT0091" />

### <a name="mt0091-this-version-of-xamarinios-requires-the--sdk-shipped-with-xcode--either-upgrade-xcode-to-get-the-required-header-files-or-set-the-managed-linker-behaviour-to-link-framework-sdks-only-to-try-to-avoid-the-new-apis"></a>MT0091: 이 버전의 Xamarin.ios에는 * SDK (Xcode *와 함께 제공 됨)가 필요 합니다. Xcode를 업그레이드 하 여 필요한 헤더 파일을 가져오거나, 관리 되는 링커 동작을 프레임 워크 Sdk만 연결 하도록 설정 합니다 (새 Api를 방지 하려는 경우).

Xamarin.ios에는 응용 프로그램을 빌드하기 위해 오류 메시지에 지정 된 SDK 버전의 헤더 파일이 필요 합니다. 이 오류를 해결 하는 권장 방법은 필요한 SDK를 얻기 위해 Xcode를 업그레이드 하는 것입니다. 이렇게 하면 필요한 모든 헤더 파일이 포함 됩니다. 여러 버전의 Xcode가 설치 되어 있거나 기본이 아닌 위치에서 Xcode를 사용 하려는 경우에는 IDE의 기본 설정에서 올바른 Xcode 위치를 설정 해야 합니다.

다른 방법으로는 관리 되는 링커를 사용할 수 있습니다. 그러면 대부분의 경우 헤더 파일이 누락 되거나 불완전 한 새 API를 포함 하 여 사용 되지 않는 API가 제거 됩니다. 그러나 프로젝트에서 Xcode 제공 하는 것 보다 최신 SDK에 도입 된 API를 사용 하는 경우에는이 작업이 수행 되지 않습니다.

마지막 straw 솔루션은 프로젝트에 필요한 SDK를 지 원하는 이전 버전의 Xamarin.ios를 사용 하는 것입니다.

<!-- MT0092 used by mlaunch -->

<a name="MT0093" />

### <a name="mt0093-could-not-find-mlaunch"></a>MT0093: ' Mlaunch '를 찾을 수 없습니다.

<!-- MT0094 is not reported anymore -->

<a name="MT0095" />

### <a name="mt0095-aot-files-could-not-be-copied-to-the-destination-directory-dest-error"></a>MT0095: Aot 파일을 대상 디렉터리 {dest} (으)로 복사할 수 없습니다. {error}

<a name="MT0096" />

### <a name="mt0096-no-reference-to-xamariniosdll-was-found"></a>MT0096: Xamarin.ios에 대 한 참조를 찾을 수 없습니다.

<!-- MT0097: used by mmp -->
<!-- MT0098: used by mmp -->

<a name="MT0099" />

### <a name="mt0099-internal-error--please-file-a-bug-report-with-a-test-case-httpbugzillaxamarincom"></a>MT0099: 내부 오류 *. 테스트 사례 ()http://bugzilla.xamarin.com) 를 사용 하 여 버그 보고서를 파일에 입력 하세요.

이 오류 메시지는 Xamarin.ios에서 내부 일관성 검사가 실패할 때 보고 됩니다.

이는 일반적으로 Xamarin.ios의 버그를 나타냅니다. 테스트 사례를 사용 하 여 [github](https://github.com/xamarin/xamarin-macios/issues/new) 에서 새 문제를 해결 하세요.

<a name="MT0100" />

### <a name="mt0100-invalid-assembly-build-target--please-file-a-bug-report-with-a-test-case-httpbugzillaxamarincom"></a>MT0100: 어셈블리 빌드 대상이 잘못 되었습니다. ' * '. 테스트 사례 ()http://bugzilla.xamarin.com) 를 사용 하 여 버그 보고서를 파일에 입력 하세요.

이 오류 메시지는 Xamarin.ios에서 내부 일관성 검사가 실패할 때 보고 됩니다.

이는 일반적으로 Xamarin.ios의 버그를 나타냅니다. 테스트 사례를 사용 하 여 [github](https://github.com/xamarin/xamarin-macios/issues/new) 에서 새 문제를 해결 하세요.

<a name="MT0101" />

### <a name="mt0101-the-assembly--is-specified-multiple-times-in---assembly-build-target-arguments"></a>MT0101: 어셈블리 ' * '가 어셈블리 빌드-대상 인수에서 여러 번 지정 되었습니다.

오류 메시지에 언급 된 어셈블리는 어셈블리 빌드-대상 인수에서 여러 번 지정 됩니다. 각 어셈블리를 한 번만 언급 했는지 확인 하세요.

<a name="MT0102" />

### <a name="mt0102-the-assemblies--and--have-the-same-target-name--but-different-targets--and-"></a>MT0102: '\*' 및 '\*' 어셈블리에 동일한 대상 이름 ('\*\*')이 있지만 대상 (' ' 및 ' * ')은 다릅니다.

오류 메시지에 언급 된 어셈블리에 충돌 하는 빌드 대상이 있습니다.

예를 들어:

```
  --assembly-build-target:Assembly1.dll=framework=MyBinary --assembly-build-target:Assembly2.dll=dynamiclibrary=MyBinary
```

이 예제에서는 동일한 설정 (`MyBinary`)을 사용 하 여 동적 라이브러리와 프레임 워크를 모두 만들려고 합니다.

<a name="MT0103" />

### <a name="mt0103-the-static-object--contains-more-than-one-assembly--but-each-static-object-must-correspond-with-exactly-one-assembly"></a>MT0103: 정적 개체 '\*'에 둘 이상의 어셈블리 ('\*')가 포함 되어 있지만 각 정적 개체는 정확히 하나의 어셈블리와 일치 해야 합니다.

오류 메시지에 언급 된 어셈블리는 모두 단일 정적 개체로 컴파일됩니다. 이는 허용 되지 않습니다. 모든 어셈블리는 다른 정적 개체로 컴파일해야 합니다.

예를 들어:

```
--assembly-build-target:Assembly1.dll=staticobject=MyBinary --assembly-build-target:Assembly2.dll=staticobject=MyBinary
```

이 예제에서는 허용 되지 않는 두 개의 어셈블리`MyBinary`(`Assembly1.dll` 및 `Assembly2.dll`)로 구성 된 정적 개체 ()를 빌드하려고 시도 합니다.

<a name="MT0105" />

### <a name="mt0105-no-assembly-build-target-was-specified-for-"></a>MT0105: ' * '에 대해 어셈블리 빌드 대상이 지정 되지 않았습니다.

를 사용 하 여 `--assembly-build-target`어셈블리 빌드 대상을 지정 하는 경우 앱의 모든 어셈블리에 빌드 대상이 할당 되어야 합니다.

이 오류는 오류 메시지에 언급 된 어셈블리에 어셈블리 빌드 대상이 할당 되지 않은 경우에 보고 됩니다.

자세한 내용은에 대 `--assembly-build-target` 한 설명서를 참조 하십시오.

<a name="MT0106" />

### <a name="mt0106-the-assembly-build-target-name--is-invalid-the-character--is-not-allowed"></a>MT0106: 어셈블리 빌드 대상 이름 '\*'이 (가) 잘못 되었습니다. 문자 ' '은 (는)\*허용 되지 않습니다.

어셈블리 빌드 대상 이름은 올바른 파일 이름 이어야 합니다.

예를 들어 다음 값은이 오류를 트리거합니다.

```
--assembly-build-target:Assembly1.dll=staticobject=my/path.o
```

`my/path.o` 는 디렉터리 구분 문자 때문에 올바른 파일 이름이 아닙니다.

<a name="MT0107" />

### <a name="mt0107-the-assemblies--have-different-custom-llvm-optimizations--which-is-not-allowed-when-they-are-all-compiled-to-a-single-binary"></a>MT0107: '\*' 어셈블리의 사용자 지정 LLVM 최적화 (\*)가 서로 다릅니다 .이는 모두 단일 이진으로 컴파일되는 경우 허용 되지 않습니다.

<a name="MT0108" />

### <a name="mt0108-the-assembly-build-target--did-not-match-any-assemblies"></a>MT0108: 어셈블리 빌드 대상 ' * '이 (가) 어셈블리와 일치 하지 않습니다.

<a name="MT0109" />

### <a name="mt0109-the-assembly-0-was-loaded-from-a-different-path-than-the-provided-path-provided-path-1-actual-path-2"></a>MT0109: 어셈블리 '{0}'이 (가) 제공 된 경로와 다른 경로에서 로드 되었습니다 (제공 {1}된 경로:, {2}실제 경로:).

응용 프로그램에서 참조 하는 어셈블리가 요청 된 것과 다른 위치에서 로드 되었음을 나타내는 경고입니다.

이는 앱에서 이름이 같지만 위치가 다른 여러 어셈블리를 참조 하는 것을 의미할 수 있습니다 .이로 인해 예기치 않은 결과가 발생할 수 있습니다 (첫 번째 어셈블리만 사용 됨).

<a name="MT0110" />

### <a name="mt0110-incremental-builds-have-been-disabled-because-this-version-of-xamarinios-does-not-support-incremental-builds-in-projects-that-include-third-party-binding-libraries-and-that-compiles-to-bitcode"></a>MT0110: 이 버전의 Xamarin.ios는 타사 바인딩 라이브러리를 포함 하 고 bitcode로 컴파일되는 프로젝트의 증분 빌드를 지원 하지 않으므로 증분 빌드를 사용할 수 없습니다.

이 버전의 Xamarin.ios는 타사 바인딩 라이브러리를 포함 하 고 bitcode (tvOS 및 watchOS projects)로 컴파일되는 프로젝트의 증분 빌드를 지원 하지 않으므로 증분 빌드를 사용할 수 없습니다.

사용자의 작업은 필요 하지 않습니다 .이 메시지는 전적으로 정보를 제공 합니다.

자세한 내용은 버그 #[51710](https://bugzilla.xamarin.com/show_bug.cgi?id=51710)을 참조 하세요.

이 경고는 더 이상 보고 되지 않습니다.

<a name="MT0111" />

### <a name="mt0111-bitcode-has-been-enabled-because-this-version-of-xamarinios-does-not-support-building-watchos-projects-using-llvm-without-enabling-bitcode"></a>MT0111: 이 버전의 Xamarin.ios는 bitcode를 사용 하지 않고 LLVM를 사용 하 여 watchOS 프로젝트 빌드를 지원 하지 않으므로 bitcode를 사용할 수 있습니다.

이 버전의 Xamarin.ios는 bitcode를 사용 하지 않고 LLVM를 사용 하 여 watchOS 프로젝트 빌드를 지원 하지 않으므로 bitcode가 자동으로 사용 하도록 설정 되었습니다.

사용자의 작업은 필요 하지 않습니다 .이 메시지는 전적으로 정보를 제공 합니다.

자세한 내용은 버그 #[51634](https://bugzilla.xamarin.com/show_bug.cgi?id=51634)을 참조 하세요.

<a name="MT0112" />

### <a name="mt0112-native-code-sharing-has-been-disabled-because-"></a>MT0112: 기본 코드 공유가 사용 하지 않도록 설정 되었습니다.

코드 공유를 사용 하지 않도록 설정 하는 방법에는 여러 가지가 있습니다.

* 컨테이너 응용 프로그램의 배포 대상은 iOS 8.0 (*) 보다 이전 버전입니다.

네이티브 코드 공유는 iOS 8.0에서 도입 된 사용자 프레임 워크를 사용 하 여 구현 되므로 네이티브 코드 공유에는 iOS 8.0이 필요 합니다.

* 컨테이너 앱에는 I18N assemblies (*)가 포함 되어 있기 때문입니다.

컨테이너 앱에 I18N 어셈블리가 포함 된 경우에는 네이티브 코드 공유가 현재 지원 되지 않습니다.

* 컨테이너 앱은 관리 되는 링커 (*)에 대 한 사용자 지정 xml 정의를 포함 하기 때문입니다.

네이티브 코드 공유는 관리 되는 링커에 대 한 사용자 지정 xml 정의를 사용 하는 프로젝트에서는 지원 되지 않습니다.

<a name="MT0113" />

### <a name="mt0113-native-code-sharing-has-been-disabled-for-the-extension--because-"></a>MT0113: * 이므로 ' * ' 확장에 대 한 네이티브 코드 공유를 사용할 수 없습니다.

* bitcode 옵션은 컨테이너 앱 (\*)과 확장 (\*) 간에 서로 다릅니다.

  네이티브 코드를 공유 하려면 코드를 공유 하는 프로젝트 간에 bitcode 옵션이 일치 해야 합니다.

* --어셈블리 빌드 대상 옵션은 컨테이너 앱 (\*)과 확장 (\*) 간에 서로 다릅니다.

  네이티브 코드를 공유 하려면 코드를 공유 하는 프로젝트 간에--어셈블리 빌드-대상 옵션이 동일 해야 합니다.

  이 문제는 증분 빌드가 모든 프로젝트에서 활성화 또는 비활성화 되지 않은 경우에 발생할 수 있습니다.

* I18N 어셈블리는 컨테이너 앱 (\*)과 확장 (\*) 간에 서로 다릅니다.

  네이티브 코드 공유는 현재 I18N 어셈블리가 포함 된 확장에 대해 지원 되지 않습니다.

* AOT 컴파일러에 대 한 인수는 컨테이너 앱 (\*)과 확장 (\*) 간에 서로 다릅니다.

  네이티브 코드를 공유 하려면 AOT 컴파일러에 대 한 인수가 코드를 공유 하는 프로젝트 간에 달라 야 합니다.

* AOT 컴파일러에 대 한 다른 인수는 컨테이너 앱 (\*)과 확장 (\*) 간에 서로 다릅니다.

  네이티브 코드를 공유 하려면 AOT 컴파일러에 대 한 인수가 코드를 공유 하는 프로젝트 간에 달라 야 합니다.

  이 상태는 프로젝트 간에 ' 모든 32 비트 부동 소수점 작업을 64 비트 float로 수행 합니다. '가 다른 경우에 발생 합니다.

* LLVM는 컨테이너 앱 (\*)과 확장 (\*)에서 모두 사용 하거나 사용 하지 않도록 설정 됩니다.

  네이티브 코드를 공유 하려면 코드를 공유 하는 모든 프로젝트에 대해 LLVM를 사용 하거나 사용 하지 않도록 설정 해야 합니다.

* 관리 되는 링커 설정은 컨테이너 앱 (\*)과 확장 (\*) 간에 서로 다릅니다.

  네이티브 코드를 공유 하려면 코드를 공유 하는 모든 프로젝트에 대해 관리 되는 링커 설정이 동일 해야 합니다.

* 관리 되는 링커에 대 한 건너뛴 어셈블리는 컨테이너 앱 (\*)과 확장 (\*) 간에 서로 다릅니다.

  네이티브 코드를 공유 하려면 코드를 공유 하는 모든 프로젝트에 대해 관리 되는 링커 설정이 동일 해야 합니다.

* 확장에는 관리 되는 링커 (*)에 대 한 사용자 지정 xml 정의가 포함 되어 있기 때문입니다.

  네이티브 코드 공유는 관리 되는 링커에 대 한 사용자 지정 xml 정의를 사용 하는 프로젝트에서는 지원 되지 않습니다.

* 컨테이너 앱은 ABI *에 대해 빌드되지 않으므로 확장에서이 ABI를 위해 빌드하는 중입니다.

  네이티브 코드를 공유 하려면 컨테이너 앱이에 대 한 모든 앱 확장이 빌드되는 모든 아키텍처를 기반으로 해야 합니다.

  예:이 상태는 ARM64 + ARMv7에 대해 확장이 빌드될 때 발생 하지만 컨테이너 앱은 ARM64에 대해서만 빌드됩니다.

* 컨테이너 앱은 확장의 abi ( \*\*)와 호환 되지 않는 abi에 대해 빌드 중입니다.

  네이티브 코드를 공유 하려면 모든 프로젝트가 정확히 동일한 API를 위해 빌드해야 합니다.

  예:이 상태는 ARMv7 + llvm + thumb2에 대해 확장이 빌드될 때 발생 하지만 컨테이너 앱은 ARMv7 + llvm에 대해서만 빌드됩니다.

* 컨테이너 앱은 ' '\*\*에서 ' ' 어셈블리를 참조 하기 때문에 확장은 ' * '와 다른 버전을 참조 합니다.

  네이티브 코드를 공유 하려면 코드를 공유 하는 모든 프로젝트가 모든 어셈블리에 대해 동일한 버전을 사용 해야 합니다.

<!-- MT0114: used by mmp -->

<a name="MT0115" />

### <a name="mt0115-it-is-recommended-to-reference-dynamic-symbols-using-code---dynamic-symbol-modecode-when-bitcode-is-enabled"></a>MT0115: Bitcode를 사용 하는 경우 코드 (--동적 기호 모드 = 코드)를 사용 하 여 동적 기호를 참조 하는 것이 좋습니다.

Xamarin.ios 프로젝트는 네이티브 기호를 동적으로 참조 하는 경우가 많습니다. 즉, 네이티브 링커가 이러한 기호를 사용 하는 것을 볼 수 없으므로 네이티브 링커에서 네이티브 연결 프로세스 중에 이러한 네이티브 기호를 제거할 수 있습니다.

일반적으로 xamarin.ios는 `-u symbol` 링커 플래그를 사용 하 여 이러한 기호를 유지 하도록 네이티브 링커에 요청 하지만 bitcode를 위해 컴파일하는 경우 네이티브 링커에서 플래그를 `-u` 허용 하지 않습니다.

Xamarin.ios는 대체 솔루션을 구현 했습니다. 이러한 기호를 참조 하는 추가 네이티브 코드를 생성 하므로 네이티브 링커에서는 이러한 기호를 사용 하는 것을 볼 수 있습니다. Bitcode로 컴파일하는 경우 자동으로 수행 됩니다.

이 `--dynamic-symbol-mode=linker` mtouch에 전달 되 면이 대체 솔루션을 사용할 수 없으며 xamarin.ios가 네이티브 링커에 전달 `-u` 하려고 시도 합니다. 이로 인해 네이티브 링커 오류가 발생할 수 있습니다.

이 솔루션은 프로젝트의 빌드 `--dynamic-symbol-mode=linker` 옵션에서 추가 mtouch 인수에서 인수를 제거 하는 것입니다.

<!-- 0116 - 0124: free to use -->

<a name="MT0116" />

### <a name="mt0116-invalid-architecture-arch-32-bit-architectures-are-not-supported-when-deployment-target-is-11-or-later-make-sure-the-project-does-not-build-for-a-32-bit-architecture"></a>MT0116: 잘못 된 아키텍처: {아치}. 32 비트 아키텍처는 배포 대상이 11 이상이 면 지원 되지 않습니다. 프로젝트에서 32 비트 아키텍처에 대 한 빌드를 수행 하지 않는지 확인 합니다.

iOS 11에는 32 비트 응용 프로그램에 대 한 지원이 포함 되어 있지 않으므로 배포 대상이 iOS 11 이상인 경우 32 비트 응용 프로그램에 대 한 빌드를 지원 하지 않습니다.

프로젝트의 iOS 빌드 옵션에서 대상 아키텍처를 arm64로 변경 하거나 프로젝트의 info.plist에서 배포 대상을 이전 iOS 버전으로 변경 합니다.

<a name="MT0117" />

### <a name="mt0117-cant-launch-a-32-bit-app-on-a-simulator-that-only-supports-64-bit"></a>MT0117: 64 비트만 지 원하는 시뮬레이터에서 32 비트 앱을 시작할 수 없습니다.

<a name="MT0118" />

### <a name="mt0118-aot-files-could-not-be-found-at-the-expected-directory-msymdir"></a>MT0118: 필요한 디렉터리 ' {msymdir} '에서 Aot 파일을 찾을 수 없습니다.

<!-- 0119 - 0123: free to use -->

<a name="MT0123" />

### <a name="mt0123-the-executable-assembly--does-not-reference-"></a>MT0123: 실행 가능 어셈블리 *는 *을 (를) 참조 하지 않습니다.

실행 어셈블리의 플랫폼 어셈블리 (Xamarin.ios/TVOS/WatchOS)에 대 한 참조를 찾을 수 없습니다.

이는 일반적으로 실행 프로젝트에 플랫폼 어셈블리의 모든 항목을 사용 하는 코드가 없는 경우에 발생 합니다. 예를 들어, 빈 Main 메서드 (및 다른 코드 없음)는 다음 오류를 표시 합니다.

```csharp
class Program {
    void Main (string[] args)
    {
    }
}
```

플랫폼 어셈블리의 API를 사용 하면 오류가 해결 됩니다.

```csharp
class Program {
    void Main (string[] args)
    {
        System.Console.WriteLine (typeof (UIKit.UIWindow));
    }
}
```

<a name="MT0124" />

### <a name="mt0124-could-not-set-the-current-language-to-lang-according-to-langlang-exception"></a>MT0124: 현재 언어를 ' {lang} ' (LANG = {LANG}에 따라)로 설정할 수 없음: {exception}

이 경고는 현재 언어를 오류 메시지의 언어로 설정할 수 없음을 나타내는 경고입니다.

현재 언어는 시스템 언어로 기본 됩니다.

<a name="MT0125" />

### <a name="mt0125-the---assembly-build-target-command-line-argument-is-ignored-in-the-simulator"></a>MT0125: --어셈블리 빌드-대상 명령줄 인수는 시뮬레이터에서 무시 됩니다.

아무런 조치도 필요 하지 않습니다 .이 메시지는 전적으로 정보를 제공 합니다.

<a name="MT0126" />

### <a name="mt0126-incremental-builds-have-been-disabled-because-incremental-builds-are-not-supported-in-the-simulator"></a>MT0126: 증분 빌드가 시뮬레이터에서 지원 되지 않으므로 증분 빌드를 사용할 수 없습니다.

아무런 조치도 필요 하지 않습니다 .이 메시지는 전적으로 정보를 제공 합니다.

<a name="MT0127" />

### <a name="mt0127-incremental-builds-have-been-disabled-because-this-version-of-xamarinios-does-not-support-incremental-builds-in-projects-that-include-more-than-one-third-party-binding-libraries"></a>MT0127: 이 버전의 Xamarin.ios는 둘 이상의 타사 바인딩 라이브러리를 포함 하는 프로젝트의 증분 빌드를 지원 하지 않으므로 증분 빌드가 사용 하지 않도록 설정 되었습니다.

이 버전의 Xamarin.ios는 항상 여러 타사 바인딩 라이브러리를 사용 하 여 프로젝트를 올바르게 빌드하지 않으므로 증분 빌드가 자동으로 사용 하지 않도록 설정 되었습니다.

아무런 조치도 필요 하지 않습니다 .이 메시지는 전적으로 정보를 제공 합니다.

자세한 내용은 버그 #[52727](https://bugzilla.xamarin.com/show_bug.cgi?id=52727)을 참조 하세요.

<a name="MT0128" />

### <a name="mt0128-could-not-touch-the-file--"></a>MT0128: ' * ' 파일을 건드리지 못했습니다. *

파일을 터치 하는 동안 오류가 발생 했습니다 (부분 빌드가 제대로 수행 되었는지 확인 하기 위해 수행 됨).

이 경고는 무시할 수 있습니다. 문제가 발생 하는 경우 [GitHub](https://github.com/xamarin/xamarin-macios/issues/new) 에서 새로운 문제가 발생 하 고 조사 됩니다.

<a name="MT0135" />

### <a name="mt0135-did-not-link-system-framework-0-referenced-by-assembly-1-because-it-was-introduced-in-2-3-and-were-using-the-2-4-sdk"></a>MT0135: '{0}' {2} {2} {3}어셈블리에서 참조 하는 ' ' 어셈블리에서 참조 했으며 SDK를 사용 중 이므로이 어셈블리 {4} 에서참조하는시스템프레임워크''을(를)연결하지못했습니다.{1}

응용 프로그램을 빌드하기 위해 Xamarin.ios는 시스템 라이브러리에 연결 해야 하며, 일부는 오류 메시지에 지정 된 SDK 버전에 따라 달라 집니다. 이전 버전의 SDK를 사용 하 고 있기 때문에 해당 Api에 대 한 호출이 런타임에 실패할 수 있습니다.

이 오류를 해결 하는 권장 방법은 Xcode를 업그레이드 하 여 필요한 SDK를 가져오는 것입니다. 여러 버전의 Xcode가 설치 되어 있거나 기본이 아닌 위치에서 Xcode를 사용 하려는 경우에는 IDE의 기본 설정에서 올바른 Xcode 위치를 설정 해야 합니다.

또는 관리 되는 [링커가](https://docs.microsoft.com/en-us/xamarin/ios/deploy-test/linker) 지정 된 라이브러리를 필요로 하는 새 api (대부분의 경우)를 포함 하 여 사용 하지 않는 api를 제거할 수 있도록 합니다. 그러나 프로젝트에 Xcode에서 제공 하는 것 보다 최신 SDK에 도입 된 Api가 필요한 경우에는이 작업이 수행 되지 않습니다.

Straw 솔루션으로, 빌드 프로세스 중에 이러한 새 Sdk를 제공 하지 않아도 되는 이전 버전의 Xamarin.ios를 사용 합니다.

## <a name="mt1xxx-project-related-error-messages"></a>MT1xxx: 프로젝트 관련 오류 메시지

### <a name="mt10xx-installer--mtouch"></a>MT10xx: 설치 관리자/mtouch

<!--
 MT1xxx file copy / symlinks (project related)
  MT10xx installer.cs / mtouch.cs
  -->

<a name="MT1001" />

### <a name="mt1001-could-not-find-an-application-at-the-specified-directory"></a>MT1001: 지정 된 디렉터리에서 응용 프로그램을 찾을 수 없습니다.

<a name="MT1002" />

### <a name="mt1002-could-not-create-symlinks-files-were-copied"></a>MT1002: Symlink를 만들 수 없습니다. 파일이 복사 되었습니다.

<a name="MT1003" />

### <a name="mt1003-could-not-kill-the-application--you-may-have-to-kill-the-application-manually"></a>MT1003: ' * ' 응용 프로그램을 중지할 수 없습니다. 응용 프로그램을 수동으로 중지 해야 할 수도 있습니다.

<a name="MT1004" />

### <a name="mt1004-could-not-get-the-list-of-installed-applications"></a>MT1004: 설치 된 응용 프로그램 목록을 가져올 수 없습니다.

## <a name="mt1xxx-project-related-error-messages"></a>MT1xxx: 프로젝트 관련 오류 메시지

<a name="MT1005" />

### <a name="mt1005-could-not-kill-the-application--on-the-device----you-may-have-to-kill-the-application-manually"></a>MT1005: ' '\*\*장치에서 ' ' 응용 프로그램을 중지할 수 없습니다. *-응용 프로그램을 수동으로 중지 해야 할 수 있습니다.

<a name="MT1006" />

### <a name="mt1006-could-not-install-the-application--on-the-device--"></a>MT1006: ' '\*\*장치에 ' ' 응용 프로그램을 설치할 수 없습니다. *.

<a name="MT1007" />

### <a name="mt1007-failed-to-launch-the-application--on-the-device---you-can-still-launch-the-application-manually-by-tapping-on-it"></a>MT1007: ' '\*\*장치에서 ' ' 응용 프로그램을 시작 하지 못했습니다. *. 응용 프로그램을 탭 하 여 수동으로 시작할 수 있습니다.

<a name="MT1008" />

### <a name="mt1008-failed-to-launch-the-simulator"></a>MT1008: 시뮬레이터를 시작 하지 못했습니다.

Mtouch가 시뮬레이터를 시작 하지 못한 경우이 오류가 보고 됩니다.   이는 오래 된 시뮬레이터 프로세스 또는 데드 프로세스가 이미 실행 중이기 때문에 발생할 수 있습니다.

Unix 명령줄에서 실행 되는 다음 명령을 사용 하 여 중단 된 시뮬레이터 프로세스를 종료할 수 있습니다.

```bash
$ launchctl list|grep UIKitApplication|awk '{print $3}'|xargs launchctl remove
```

<a name="MT1009" />

### <a name="mt1009-could-not-copy-the-assembly--to--"></a>MT1009: ' ' 어셈블리\*를 '\*'에 복사할 수 없습니다. *

이는 특정 버전의 Xamarin.ios에서 알려진 문제입니다.

이 문제가 발생 하면 다음 해결 방법을 시도해 보세요.

```bash
sudo chmod 0644 /Library/Frameworks/Xamarin.iOS.framework/Versions/Current/lib/mono/*/*.mdb
```

그러나 최신 버전의 Xamarin.ios에서이 문제가 해결 되었으므로 [github](https://github.com/xamarin/xamarin-macios/issues/new) 에서 전체 버전 정보 및 빌드 로그 출력과 함께 새로운 문제를 해결 하세요.

<a name="MT1010" />

### <a name="mt1010-could-not-load-the-assembly--"></a>MT1010: ' * ' 어셈블리를 로드할 수 없습니다. *

<a name="MT1011" />

### <a name="mt1011-could-not-add-missing-resource-file-"></a>MT1011: 누락 된 리소스 파일을 추가할 수 없습니다. ' * '

<a name="MT1012" />

### <a name="mt1012-failed-to-list-the-apps-on-the-device--"></a>MT1012: ' * ' 장치에서 앱을 나열 하지 못했습니다. *

<a name="MT1013" />

### <a name="mt1013-dependency-tracking-error-no-files-to-compare-please-file-a-bug-report-at-httpbugzillaxamarincom-with-a-test-case"></a>MT1013: 종속성 추적 오류: 비교할 파일이 없습니다. 테스트 사례를 사용 하 여 http://bugzilla.xamarin.com 에서 버그 보고서를 파일에 입력 하세요.

이는 Xamarin.ios의 버그를 나타냅니다. 테스트 사례를 사용 하 여 [github](https://github.com/xamarin/xamarin-macios/issues/new) 에서 새 문제를 해결 하세요.

<a name="MT1014" />

### <a name="mt1014-failed-to-re-use-cached-version-of--"></a>MT1014: ' * '의 캐시 된 버전을 다시 사용 하지 못했습니다. *.

<a name="MT1015" />

### <a name="mt1015-failed-to-create-the-executable--"></a>MT1015: ' * ' 실행 파일을 만들지 못했습니다. *

<a name="MT1015" />

### <a name="mt1015-failed-to-create-the-executable--"></a>MT1015: ' * ' 실행 파일을 만들지 못했습니다. *

<a name="MT1016" />

### <a name="mt1016-failed-to-create-the-notice-file-because-a-directory-already-exists-with-the-same-name"></a>MT1016: 이름이 같은 디렉터리가 이미 존재 하기 때문에 알림 파일을 만들지 못했습니다.

프로젝트에서 디렉터리 `NOTICE` 를 제거 합니다.

<a name="MT1017" />

### <a name="mt1017-failed-to-create-the-notice-file-"></a>MT1017: 알림 파일을 만들지 못했습니다. *.

<a name="MT1018" />

### <a name="mt1018-your-application-failed-code-signing-checks-and-could-not-be-installed-on-the-device--check-your-certificates-provisioning-profiles-and-bundle-ids-probably-your-device-is-not-part-of-the-selected-provisioning-profile-error-0xe8008015"></a>MT1018: 응용 프로그램에서 코드 서명 검사에 실패 했으며 장치 ' * '에 설치할 수 없습니다. 인증서, 프로 비전 프로필 및 번들 id를 확인 합니다. 장치가 선택한 프로 비전 프로필에 포함 되지 않을 수 있습니다 (오류: 0xe8008015).

<a name="MT1019" />

### <a name="mt1019-your-application-has-entitlements-not-supported-by-your-current-provisioning-profile-and-could-not-be-installed-on-the-device--please-check-the-ios-device-log-for-more-detailed-information-error-0xe8008016"></a>MT1019: 응용 프로그램에 현재 프로 비전 프로필에서 지원 하지 않는 자격이 있으므로 ' * ' 장치에 설치할 수 없습니다. 자세한 내용은 iOS 장치 로그를 확인 하세요 (오류: 0xe8008016).

이 문제는 다음과 같은 경우에 발생할 수 있습니다.

* 현재 프로 비전 프로필이 지원 하지 않는 자격이 응용 프로그램에 있습니다.
  가능한 해결 방법:
  - 응용 프로그램에 필요한 자격을 지 원하는 다른 프로 비전 프로필을 지정 합니다.
  - 현재 프로 비전 프로필에서 지원 되지 않는 자격을 제거 합니다.
* 배포 하려는 장치가 사용 중인 프로 비전 프로필에 포함 되어 있지 않습니다.
  가능한 해결 방법:
  - Xcode의 템플릿에서 새 앱을 만들고, 동일한 프로 비전 프로필을 선택 하 고, 동일한 장치에 배포 합니다. 경우에 따라 Xcode에서 새 장치를 사용 하 여 프로 비전 프로필을 자동으로 새로 고칠 수 있습니다. 다른 경우에는 Xcode에서 수행할 작업을 요청 합니다.
  -IOS 개발자 센터로 이동 하 여 새 장치를 사용 하 여 프로 비전 프로필을 업데이트 한 다음 업데이트 된 프로 비전 프로필을 컴퓨터에 다운로드 합니다.

대부분의 경우 오류에 대 한 자세한 정보는 iOS 장치 로그에 출력 되므로 문제를 진단 하는 데 도움이 될 수 있습니다.

<a name="MT1020" />

### <a name="mt1020-failed-to-launch-the-application--on-the-device--"></a>MT1020: ' '\*\*장치에서 ' ' 응용 프로그램을 시작 하지 못했습니다. *

<a name="MT1021" />

### <a name="mt1021-could-not-copy-the-file--to--2"></a>MT1021: ' ' 파일\*을 '\*'에 복사할 수 없습니다.{2}

파일을 복사할 수 없습니다. 복사 작업의 오류 메시지에 오류에 대 한 자세한 정보가 있습니다.

<a name="MT1022" />

### <a name="mt1022-could-not-copy-the-directory--to--2"></a>MT1022: ' ' 디렉터리\*를 '\*'에 복사할 수 없습니다.{2}

디렉터리를 복사할 수 없습니다. 복사 작업의 오류 메시지에 오류에 대 한 자세한 정보가 있습니다.

<a name="MT1023" />

### <a name="mt1023-could-not-communicate-with-the-device-to-find-the-application---"></a>MT1023: ' * ' 응용 프로그램을 찾기 위해 장치와 통신할 수 없습니다. *

장치에서 응용 프로그램을 조회 하는 동안 오류가 발생 했습니다.

이 문제를 해결 하기 위해 수행 해야 하는 작업:

* 장치에서 응용 프로그램을 삭제 하 고 다시 시도 하세요.
* 장치를 분리 하 고 다시 연결 합니다.
* 장치를 다시 부팅 합니다.
* Mac을 다시 부팅 합니다.

<a name="MT1024" />

### <a name="mt1024-the-application-signature-could-not-be-verified-on-device--please-make-sure-that-the-provisioning-profile-is-installed-and-not-expired-error-0xe8008017"></a>MT1024: ' * ' 장치에서 응용 프로그램 시그니처를 확인할 수 없습니다. 프로 비전 프로필이 설치 되어 있고 만료 되지 않았는지 확인 하세요 (오류: 0xe8008017).

서명을 확인할 수 없어 장치에서 응용 프로그램 설치를 거부 했습니다.

프로 비전 프로필이 설치 되어 있고 만료 되지 않았는지 확인 하세요.

<a name="MT1025" />

### <a name="mt1025-could-not-list-the-crash-reports-on-the-device-"></a>MT1025: 장치 *에서 충돌 보고서를 나열할 수 없습니다.

장치에서 충돌 보고서를 나열 하는 동안 오류가 발생 했습니다.

이 문제를 해결 하기 위해 수행 해야 하는 작업:

* 장치에서 응용 프로그램을 삭제 하 고 다시 시도 하세요.
* 장치를 분리 하 고 다시 연결 합니다.
* 장치를 다시 부팅 합니다.
* Mac을 다시 부팅 합니다.
* 장치를 iTunes와 동기화 합니다. 그러면 장치에서 충돌 보고서가 제거 됩니다.

<a name="MT1026" />

### <a name="mt1026-could-not-download-the-crash-report--from-the-device-"></a>MT1026: 장치에서 충돌 보고서 *를 다운로드할 수 없습니다 *.

장치에서 충돌 보고서를 다운로드 하는 동안 오류가 발생 했습니다.

이 문제를 해결 하기 위해 수행 해야 하는 작업:

* 장치에서 응용 프로그램을 삭제 하 고 다시 시도 하세요.
* 장치를 분리 하 고 다시 연결 합니다.
* 장치를 다시 부팅 합니다.
* Mac을 다시 부팅 합니다.
* 장치를 iTunes와 동기화 합니다. 그러면 장치에서 충돌 보고서가 제거 됩니다.

<a name="MT1027" />

### <a name="mt1027-cant-use-xcode-7-to-launch-applications-on-devices-with-ios--xcode-7-only-supports-ios-6"></a>MT1027: IOS *를 사용 하 여 장치에서 응용 프로그램을 시작 하려면 Xcode 7 +를 사용할 수 없습니다. (Xcode 7은 iOS 6 이상만 지원 합니다.)

6\.0 미만의 iOS 버전을 사용 하는 장치에서 응용 프로그램을 시작 하려면 Xcode 7 +를 사용할 수 없습니다.

이전 버전의 Xcode를 사용 하거나 앱을 수동으로 탭 하 여 시작 하세요.

<a name="MT1028" />

### <a name="mt1028-invalid-device-specification--expected-ios-watchos-or-all"></a>MT1028: 장치 사양이 잘못 되었습니다. ' * '. ' Ios ', ' watchos ' 또는 ' a l l '이 필요 합니다.

--Device를 사용 하 여 전달 된 장치 사양이 잘못 되었습니다. 유효한 값은 ' ios ', ' watchos ' 또는 ' a l l '입니다.

<a name="MT1029" />

### <a name="mt1029-could-not-find-an-application-at-the-specified-directory-"></a>MT1029: 지정 된 디렉터리에서 응용 프로그램을 찾을 수 없습니다. *

--Launchdev에 전달 된 응용 프로그램 경로가 없습니다. 올바른 앱 번들을 지정 하세요.

<a name="MT1030" />

### <a name="mt1030-launching-applications-on-device-using-a-bundle-identifier-is-deprecated-please-pass-the-full-path-to-the-bundle-to-launch"></a>MT1030: 번들 식별자를 사용 하 여 장치에서 응용 프로그램을 시작 하는 것은 사용 되지 않습니다. 시작 하려면 번들에 대 한 전체 경로를 전달 하세요.

번들 id 대신 장치에서 시작 하려면 앱에 대 한 경로를 전달 하는 것이 좋습니다.

<a name="MT1031" />

### <a name="mt1031-could-not-launch-the-app--on-the-device--because-the-device-is-locked-please-unlock-the-device-and-try-again"></a>MT1031: 장치가 잠겨 있어서 ' '\*\*장치에서 ' ' 앱을 시작할 수 없습니다. 장치 잠금을 해제 하 고 다시 시도 하세요.

장치 잠금을 해제 하 고 다시 시도 하세요.

<a name="MT1032" />

### <a name="mt1032-this-application-executable-might-be-too-large--mb-to-execute-on-device-if-bitcode-was-enabled-you-might-want-to-disable-it-for-development-it-is-only-required-to-submit-applications-to-apple"></a>MT1032: 이 응용 프로그램 실행 파일은 장치에서 실행 하기에 너무 클 수 있습니다 (* MB). Bitcode를 사용 하는 경우 개발에 사용 하지 않도록 설정 하는 것이 좋습니다. Apple에 응용 프로그램을 제출 하는 데만 필요 합니다.

<a name="MT1033" />

### <a name="mt1033-could-not-uninstall-the-application--from-the-device--"></a>MT1033: ' '\*\*장치에서 ' ' 응용 프로그램을 제거할 수 없습니다. *

<!-- 1034 used by mmp -->

<a name="MT1035" />

### <a name="mt1035-cannot-include-different-versions-of-the-framework-name"></a>MT1035: ' {Name} ' 프레임 워크의 다른 버전을 포함할 수 없습니다.

동일한 프레임 워크의 서로 다른 버전에 연결할 수 없습니다.

이는 일반적으로 확장이 컨테이너 앱과 다른 버전의 프레임 워크를 참조 하는 경우에 발생 합니다 (타사 바인딩 어셈블리를 통해).

이 오류 다음에는 각 프레임 워크에 대 한 경로를 나열 하는 여러 [MT1036](#MT1036) 오류가 발생 합니다.

<a name="MT1036" />

### <a name="mt1036-framework-name-included-from-path-related-to-previous-error"></a>MT1036: ' {Name} ' 프레임 워크가 {path} (이전 오류와 관련 됨)에 포함 되어 있습니다.

이 오류는 [MT1036](#MT1036)와 함께 보고 됩니다. 자세한 내용은 [MT1036](#MT1036) 를 참조 하세요.

### <a name="mt11xx-debug-service"></a>MT11xx: 디버그 서비스

<!--
  MT11xx DebugService.cs
  -->

<a name="MT1101" />

### <a name="mt1101-could-not-start-app"></a>MT1101: 앱을 시작할 수 없음

<a name="MT1102" />

### <a name="mt1102-could-not-attach-to-the-app-to-kill-it-"></a>MT1102: 앱에 연결할 수 없습니다 (중지). *

<a name="MT1103" />

### <a name="mt1103-could-not-detach"></a>MT1103: 분리할 수 없음

<a name="MT1104" />

### <a name="mt1104-failed-to-send-packet-"></a>MT1104: 패킷을 보내지 못함: *

<a name="MT1105" />

### <a name="mt1105-unexpected-response-type"></a>MT1105: 예기치 않은 응답 형식

<a name="MT1106" />

### <a name="mt1106-could-not-get-list-of-applications-on-the-device-request-timed-out"></a>MT1106: 장치에서 응용 프로그램 목록을 가져올 수 없습니다. 요청 시간이 초과 되었습니다.

<a name="MT1107" />

### <a name="mt1107-application-failed-to-launch-"></a>MT1107: 응용 프로그램을 시작 하지 못함: *

장치가 잠겨 있는지 확인 하세요.

엔터프라이즈 앱을 배포 하거나 무료 프로 비전 프로필을 사용 하는 경우 개발자를 신뢰 하는 것일 수 있습니다 ( <a href="https://stackoverflow.com/a/30726375/183422">여기</a>에 설명 되어 있음).

<a name="MT1108" />

### <a name="mt1108-could-not-find-developer-tools-for-this-xx-yy-device"></a>MT1108: 이 XX (YY) 장치용 개발자 도구를 찾을 수 없습니다.

Mtouch에서 몇 가지 작업을 수행 `DeveloperDiskImage.dmg` 하려면 파일이 있어야 합니다.   이 파일은 Xcode의 일부 이며 일반적으로에 대해 `Xcode.app/Contents/Developer/iPhoneOS.platform/DeviceSupport/VERSION/DeveloperDiskImage.dmg`빌드하는 데 사용 하는 SDK를 기준으로 합니다.

이 오류는 연결 된 장치와 일치 하는 dmg DeveloperDiskImage 없기 때문에 발생할 수 있습니다.

<a name="MT1109" />

### <a name="mt1109-application-failed-to-launch-because-the-device-is-locked-please-unlock-the-device-and-try-again"></a>MT1109: 장치가 잠겨 있어 응용 프로그램을 시작 하지 못했습니다. 장치 잠금을 해제 하 고 다시 시도 하세요.

장치가 잠겨 있는지 확인 하세요.

<a name="MT1110" />

### <a name="mt1110-application-failed-to-launch-because-of-ios-security-restrictions-please-ensure-the-developer-is-trusted"></a>MT1110: IOS 보안 제한으로 인해 응용 프로그램을 시작 하지 못했습니다. 개발자를 신뢰할 수 있는지 확인 하세요.

엔터프라이즈 앱을 배포 하거나 무료 프로 비전 프로필을 사용 하는 경우 개발자를 신뢰 하는 것일 수 있습니다 ( <a href="https://stackoverflow.com/a/30726375/183422">여기</a>에 설명 되어 있음).

<a name="MT1111" />

### <a name="mt1111-application-launched-successfully-but-its-not-possible-to-wait-for-the-app-to-exit-as-requested-because-its-not-possible-to-detect-app-termination-when-launching-using-gdbserver"></a>MT1111: 응용 프로그램이 시작 되었지만 앱이 요청에 따라 종료 될 때까지 기다릴 수 없습니다. gdbserver를 사용 하 여 시작할 때 앱 종료를 검색할 수 없기 때문입니다.

### <a name="mt12xx-simulator"></a>MT12xx: 시뮬레이터

<!--
  MT12xx simcontroller.cs
  -->

<a name="MT1201" />

### <a name="mt1201-could-not-load-the-simulator-"></a>MT1201: 시뮬레이터를 로드할 수 없습니다. *

<a name="MT1202" />

### <a name="mt1202-invalid-simulator-configuration-"></a>MT1202: 잘못 된 시뮬레이터 구성: *

<a name="MT1203" />

### <a name="mt1203-invalid-simulator-specification-"></a>MT1203: 잘못 된 시뮬레이터 사양: *

<a name="MT1204" />

### <a name="mt1204-invalid-simulator-specification--runtime-not-specified"></a>MT1204: 잘못 된 시뮬레이터 사양 ' * ': 런타임이 지정 되지 않았습니다.

<a name="MT1205" />

### <a name="mt1205-invalid-simulator-specification--device-type-not-specified"></a>MT1205: 잘못 된 시뮬레이터 사양 ' * ': 장치 유형을 지정 하지 않았습니다.

<a name="MT1206" />

### <a name="mt1206-could-not-find-the-simulator-runtime-"></a>MT1206: 시뮬레이터 런타임 ' * '를 찾을 수 없습니다.

<a name="MT1207" />

### <a name="mt1207-could-not-find-the-simulator-device-type-"></a>MT1207: 시뮬레이터 장치 유형 ' * '을 찾을 수 없습니다.

<a name="MT1208" />

### <a name="mt1208-could-not-find-the-simulator-runtime-"></a>MT1208: 시뮬레이터 런타임 ' * '를 찾을 수 없습니다.

<a name="MT1209" />

### <a name="mt1209-could-not-find-the-simulator-device-type-"></a>MT1209: 시뮬레이터 장치 유형 ' * '을 찾을 수 없습니다.

<a name="MT1210" />

### <a name="mt1210-invalid-simulator-specification--unknown-key-"></a>MT1210: 잘못 된 시뮬레이터 사양 \*:, 알 수\*없는 키 ' '

<a name="MT1211" />

### <a name="mt1211-the-simulator-version--does-not-support-the-simulator-type-"></a>MT1211: 시뮬레이터 버전 '\*'은 (는) 시뮬레이터 형식 ' '\*을 (를) 지원 하지 않습니다.

<a name="MT1212" />

### <a name="mt1212-failed-to-create-a-simulator-version-where-type---and-runtime--"></a>MT1212: 형식 = * 및 런타임 = * 인 시뮬레이터 버전을 만들지 못했습니다.

<a name="MT1213" />

### <a name="mt1213-invalid-simulator-specification-for-xcode-4-"></a>MT1213: Xcode 4: *에 대 한 시뮬레이터 사양이 잘못 되었습니다.

<a name="MT1214" />

### <a name="mt1214-invalid-simulator-specification-for-xcode-5-"></a>MT1214: Xcode 5: *에 대 한 시뮬레이터 사양이 잘못 되었습니다.

<a name="MT1215" />

### <a name="mt1215-invalid-sdk-specified-"></a>MT1215: 잘못 된 SDK를 지정 했습니다. *

<a name="MT1216" />

### <a name="mt1216-could-not-find-the-simulator-udid-"></a>MT1216: 시뮬레이터 UDID ' * '를 찾을 수 없습니다.

<a name="MT1217" />

### <a name="mt1217-could-not-load-the-app-bundle-at-"></a>MT1217: ' * '에서 앱 번들을 로드할 수 없습니다.

<a name="MT1218" />

### <a name="mt1218-no-bundle-identifier-found-in-the-app-at-"></a>MT1218: ' * '의 앱에서 번들 식별자를 찾을 수 없습니다.

<a name="MT1219" />

### <a name="mt1219-could-not-find-the-simulator-for-"></a>MT1219: ' * '에 대 한 시뮬레이터를 찾을 수 없습니다.

<a name="MT1220" />

### <a name="mt1220-could-not-find-the-latest-simulator-runtime-for-device-"></a>MT1220: ' * ' 장치에 대 한 최신 시뮬레이터 런타임을 찾을 수 없습니다.

일반적으로 Xcode에 문제가 있음을 나타냅니다.

이 문제를 해결 하기 위해 수행 해야 하는 작업:

* Xcode에서 시뮬레이터를 한 번 사용 합니다.
* --Sdk \<버전 >를 사용 하 여 명시적 sdk 버전을 전달 합니다.
* Xcode를 다시 설치 합니다.

<a name="MT1221" />

### <a name="mt1221-could-not-find-the-paired-iphone-simulator-for-the-watchos-simulator-"></a>MT1221: WatchOS 시뮬레이터 ' * '에 대 한 쌍을 이루는 iPhone 시뮬레이터를 찾을 수 없습니다.

WatchOS 시뮬레이터에서 WatchOS 앱을 시작 하는 경우 쌍을 이루는 iOS 시뮬레이터도 있어야 합니다.

Watch 시뮬레이터는 Xcode의 장치 UI (메뉴 창 > 장치)를 사용 하 여 iOS 시뮬레이터와 쌍을 이룰 수 있습니다.

### <a name="mt13xx-linkwith"></a>MT13xx: [LinkWith]

<!--
  MT13xx [LinkWith]
  -->

<a name="MT1301" />

### <a name="mt1301-native-library---was-ignored-since-it-does-not-match-the-current-build-architectures-"></a>MT1301: 네이티브 라이브러리 `*` (\*)가 현재 빌드\*아키텍처 ()와 일치 하지 않으므로 무시 되었습니다.

<a name="MT1302" />

### <a name="mt1302-could-not-extract-the-native-library--from--please-ensure-the-native-library-was-properly-embedded-in-the-managed-assembly-if-the-assembly-was-built-using-a-binding-project-the-native-library-must-be-included-in-the-project-and-its-build-action-must-be-objcbindingnativelibrary"></a>MT1302: ' + '에서 네이티브 라이브러리 ' * '를 추출할 수 없습니다. 네이티브 라이브러리가 관리 되는 어셈블리에 제대로 포함 되어 있는지 확인 하세요. 어셈블리가 바인딩 프로젝트를 사용 하 여 빌드된 경우 네이티브 라이브러리가 프로젝트에 포함 되어야 하 고 해당 빌드 작업은 ' ObjcBindingNativeLibrary ' 여야 합니다.

<a name="MT1303" />

### <a name="mt1303-could-not-decompress-the-native-framework--from--please-review-the-build-log-for-more-information-from-the-native-zip-command"></a>MT1303: \*'\*'에서 네이티브 프레임 워크 ' '의 압축을 해제할 수 없습니다. 기본 ' zip ' 명령에서 자세한 정보를 보려면 빌드 로그를 검토 하세요.

바인딩 라이브러리에서 지정 된 네이티브 프레임 워크의 압축을 풀지 못했습니다.

기본 ' zip ' 명령에서이 오류에 대 한 자세한 내용을 확인 하려면이 오류 로그를 검토 하세요.

<a name="MT1304" />

### <a name="mt1304-the-embedded-framework--in--is-invalid-it-does-not-contain-an-infoplist"></a>MT1304: *의 포함 프레임 워크 ' * '가 잘못 되었습니다. info.plist이 포함 되어 있지 않습니다.

지정 된 포함 프레임 워크에 info.plist가 포함 되어 있지 않으므로 올바른 프레임 워크가 아닙니다.

프레임 워크가 올바른지 확인 하세요.

<a name="MT1305" />

### <a name="mt1305-the-binding-library--contains-a-user-framework--but-embedded-user-frameworks-require-ios-80-the-current-deployment-target-is--please-set-the-deployment-target-in-the-infoplist-file-to-at-least-80"></a>MT1305: 바인딩 라이브러리 '\*'에 사용자 프레임 워크 (\*)가 포함 되어 있지만 포함 된 사용자 프레임 워크에는 iOS 8.0 (현재 배포 대상이 *)가 필요 합니다. Info.plist 파일의 배포 대상을 8.0 이상으로 설정 하십시오.

지정 된 바인딩 라이브러리에 포함 된 프레임 워크가 포함 되어 있지만 Xamarin.ios는 iOS 8.0 이상에 포함 된 프레임 워크만 지원 합니다.

Info.plist 파일의 배포 대상을 8.0 이상으로 설정 하 여이 오류를 해결 하세요 (또는 포함 된 프레임 워크를 사용 하지 않음).

### <a name="mt14xx-crash-reports"></a>MT14xx: 충돌 보고서

<!--
  MT14xx    CrashReports.cs
  -->

<a name="MT1400" />

### <a name="mt1400-could-not-open-crash-report-service-afcconnectionopen-returned-"></a>MT1400: 충돌 보고서 서비스를 열 수 없습니다. AFCConnectionOpen 반환 *

장치에서 충돌 보고서에 액세스 하는 동안 오류가 발생 했습니다.

이 문제를 해결 하기 위해 수행 해야 하는 작업:

* 장치에서 응용 프로그램을 삭제 하 고 다시 시도 하세요.
* 장치를 분리 하 고 다시 연결 합니다.
* 장치를 다시 부팅 합니다.
* Mac을 다시 부팅 합니다.
* 장치를 iTunes와 동기화 합니다. 그러면 장치에서 충돌 보고서가 제거 됩니다.

<a name="MT1401" />

### <a name="mt1401-could-not-close-crash-report-service-afcconnectionclose-returned-"></a>MT1401: 충돌 보고서 서비스를 닫을 수 없습니다. AFCConnectionClose 반환 *

장치에서 충돌 보고서에 액세스 하는 동안 오류가 발생 했습니다.

이 문제를 해결 하기 위해 수행 해야 하는 작업:

* 장치에서 응용 프로그램을 삭제 하 고 다시 시도 하세요.
* 장치를 분리 하 고 다시 연결 합니다.
* 장치를 다시 부팅 합니다.
* Mac을 다시 부팅 합니다.
* 장치를 iTunes와 동기화 합니다. 그러면 장치에서 충돌 보고서가 제거 됩니다.

<a name="MT1402" />

### <a name="mt1402-could-not-read-file-info-for--afcfileinfoopen-returned-"></a>MT1402: *에 대 한 파일 정보를 읽을 수 없습니다. AFCFileInfoOpen에서 반환 된 *

장치에서 충돌 보고서에 액세스 하는 동안 오류가 발생 했습니다.

이 문제를 해결 하기 위해 수행 해야 하는 작업:

* 장치에서 응용 프로그램을 삭제 하 고 다시 시도 하세요.
* 장치를 분리 하 고 다시 연결 합니다.
* 장치를 다시 부팅 합니다.
* Mac을 다시 부팅 합니다.
* 장치를 iTunes와 동기화 합니다. 그러면 장치에서 충돌 보고서가 제거 됩니다.

<a name="MT1403" />

### <a name="mt1403-could-not-read-crash-report-afcdirectoryopen--returned-"></a>MT1403: 충돌 보고서를 읽을 수 없습니다. AFCDirectoryOpen (*) 반환 됨: *

장치에서 충돌 보고서에 액세스 하는 동안 오류가 발생 했습니다.

이 문제를 해결 하기 위해 수행 해야 하는 작업:

* 장치에서 응용 프로그램을 삭제 하 고 다시 시도 하세요.
* 장치를 분리 하 고 다시 연결 합니다.
* 장치를 다시 부팅 합니다.
* Mac을 다시 부팅 합니다.
* 장치를 iTunes와 동기화 합니다. 그러면 장치에서 충돌 보고서가 제거 됩니다.

<a name="MT1404" />

### <a name="mt1404-could-not-read-crash-report-afcfilerefopen--returned-"></a>MT1404: 충돌 보고서를 읽을 수 없습니다. 반환 된 AFCFileRefOpen (*): *

장치에서 충돌 보고서에 액세스 하는 동안 오류가 발생 했습니다.

이 문제를 해결 하기 위해 수행 해야 하는 작업:

* 장치에서 응용 프로그램을 삭제 하 고 다시 시도 하세요.
* 장치를 분리 하 고 다시 연결 합니다.
* 장치를 다시 부팅 합니다.
* Mac을 다시 부팅 합니다.
* 장치를 iTunes와 동기화 합니다. 그러면 장치에서 충돌 보고서가 제거 됩니다.

<a name="MT1405" />

### <a name="mt1405-could-not-read-crash-report-afcfilerefread--returned-"></a>MT1405: 충돌 보고서를 읽을 수 없습니다. 반환 된 AFCFileRefRead (*): *

장치에서 충돌 보고서에 액세스 하는 동안 오류가 발생 했습니다.

이 문제를 해결 하기 위해 수행 해야 하는 작업:

* 장치에서 응용 프로그램을 삭제 하 고 다시 시도 하세요.
* 장치를 분리 하 고 다시 연결 합니다.
* 장치를 다시 부팅 합니다.
* Mac을 다시 부팅 합니다.
* 장치를 iTunes와 동기화 합니다. 그러면 장치에서 충돌 보고서가 제거 됩니다.

<a name="MT1406" />

### <a name="mt1406-could-not-list-crash-reports-afcdirectoryopen--returned-"></a>MT1406: 충돌 보고서를 나열할 수 없습니다. AFCDirectoryOpen (*) 반환 됨: *

장치에서 충돌 보고서에 액세스 하는 동안 오류가 발생 했습니다.

이 문제를 해결 하기 위해 수행 해야 하는 작업:

* 장치에서 응용 프로그램을 삭제 하 고 다시 시도 하세요.
* 장치를 분리 하 고 다시 연결 합니다.
* 장치를 다시 부팅 합니다.
* Mac을 다시 부팅 합니다.
* 장치를 iTunes와 동기화 합니다. 그러면 장치에서 충돌 보고서가 제거 됩니다.

<!--- 1407 used by mmp -->

### <a name="mt16xx-macho"></a>MT16xx: MachO

<!--
  MT16xx    MachO.cs
  -->

<a name="MT1600" />

### <a name="mt1600-not-a-mach-o-dynamic-library-unknown-header-0x-"></a>MT1600: March-o 바이너리도 동적 라이브러리가 아닙니다. (알 수 없는 헤더 ' 0x * '): *.

문제의 동적 라이브러리를 처리 하는 동안 오류가 발생 했습니다.

동적 라이브러리가 올바른 March-o 바이너리도 동적 라이브러리 인지 확인 하십시오.

터미널의 `file` 명령을 사용 하 여 라이브러리의 형식을 확인할 수 있습니다.

```
file -arch all -l /path/to/library.dylib
```

<a name="MT1601" />

### <a name="mt1601-not-a-static-library-unknown-header--"></a>MT1601: 정적 라이브러리가 아닙니다 (알 수 없는 헤더 ' * '). *.

문제의 정적 라이브러리를 처리 하는 동안 오류가 발생 했습니다.

정적 라이브러리가 올바른 March-o 바이너리도 정적 라이브러리 인지 확인 하세요.

터미널의 `file` 명령을 사용 하 여 라이브러리의 형식을 확인할 수 있습니다.

```
file -arch all -l /path/to/library.a
```

<a name="MT1602" />

### <a name="mt1602-not-a-mach-o-dynamic-library-unknown-header-0x-"></a>MT1602: March-o 바이너리도 동적 라이브러리가 아닙니다. (알 수 없는 헤더 ' 0x * '): *.

문제의 동적 라이브러리를 처리 하는 동안 오류가 발생 했습니다.

동적 라이브러리가 올바른 March-o 바이너리도 동적 라이브러리 인지 확인 하십시오.

터미널의 `file` 명령을 사용 하 여 라이브러리의 형식을 확인할 수 있습니다.

```
file -arch all -l /path/to/library.dylib
```

<a name="MT1603" />

### <a name="mt1603-unknown-format-for-fat-entry-at-position--in-"></a>MT1603: *의 위치 *에서 fat 항목의 형식을 알 수 없습니다.

의심 되는 fat 아카이브를 처리 하는 동안 오류가 발생 했습니다.

Fat 보관 파일이 유효한 지 확인 하세요.

터미널의 `file` 명령을 사용 하 여 fat 보관 파일의 형식을 확인할 수 있습니다.

```
file -arch all -l /path/to/file
```

<a name="MT1604" />

### <a name="mt1604-file-of-type--is-not-a-macho-file-"></a>MT1604: * 형식의 파일은 MachO 파일 (*)이 아닙니다.

문제의 MachO 파일을 처리 하는 동안 오류가 발생 했습니다.

파일이 올바른 March-o 바이너리도 동적 라이브러리 인지 확인 하십시오.

터미널의 `file` 명령을 사용 하 여 파일 형식을 확인할 수 있습니다.

```
file -arch all -l /path/to/file
```

## <a name="mt2xxx-linker-error-messages"></a>MT2xxx: 링커 오류 메시지

<!--
 MT2xxx Linker
  MT20xx Linker (general) errors
  -->

<a name="MT2001" />

### <a name="mt2001-could-not-link-assemblies"></a>MT2001: 어셈블리를 연결할 수 없습니다.

이 오류는 관리 되는 링커에서 예외와 같이 오류가 발생 하 여 처리 중인 어셈블리를 완료 하거나 저장할 수 없음을 의미 합니다. 정확한 오류에 대 한 자세한 내용은 빌드 로그에 포함 됩니다 (예:).

```
error MT2001: Could not link assemblies.
    Method: `System.Void Todo.TodoListPageCS/<<-ctor>b__1_0>d::SetStateMachine(System.Runtime.CompilerServices.IAsyncStateMachine)`
    Assembly: `QuickTodo, Version=1.0.6297.28241, Culture=neutral, PublicKeyToken=null`
Reason: Value cannot be null.
Parameter name: instruction
```

이러한 문제에 대 한 버그 보고서를 파일에 두어야 합니다. 대부분의 경우 적절 한 수정이 게시 될 때까지 해결 방법을 제공할 수 있습니다. 위의 정보는 문제를 해결 하기 위해 테스트 사례 및/또는 어셈블리 범주화 y와 함께 중요 합니다.

<a name="MT2002" />

### <a name="mt2002-can-not-resolve-reference-"></a>MT2002: 참조를 확인할 수 없습니다. *

<a name="MT2003" />

### <a name="mt2003-option--will-be-ignored-since-linking-is-disabled"></a>MT2003: 연결이 사용 하지 않도록 설정 되어 있으므로 ' * ' 옵션이 무시 됩니다.

<a name="MT2004" />

### <a name="mt2004-extra-linker-definitions-file--could-not-be-located"></a>MT2004: ' * ' 추가 링커 정의 파일을 찾을 수 없습니다.

<a name="MT2005" />

### <a name="mt2005-definitions-from--could-not-be-parsed"></a>MT2005: ' * '의 정의를 구문 분석할 수 없습니다.

<a name="MT2006" />

### <a name="mt2006-can-not-load-mscorlibdll-from--please-reinstall-xamarinios"></a>MT2006: : *에서 mscorlib.dll을 로드할 수 없습니다. Xamarin.ios를 다시 설치 하세요.

이는 일반적으로 Xamarin.ios 설치에 문제가 있음을 나타냅니다. Xamarin.ios를 다시 설치 해 보세요.

<!--- 2007 used by mmp -->
<!--- 2009 used by mmp -->

<a name="MT2010" />

### <a name="mt2010-unknown-httpmessagehandler--valid-values-are-httpclienthandler-default-cfnetworkhandler-or-nsurlsessionhandler"></a>MT2010: 알 수 없는 HttpMessageHandler `*`입니다. 유효한 값은 HttpClientHandler (기본값), CFNetworkHandler 또는 NSUrlSessionHandler입니다.

<a name="MT2011" />

### <a name="mt2011-unknown-tlsprovider---valid-values-are-default-or-appletls"></a>MT2011: 알 수 없는 TlsProvider `*`입니다.  유효한 값은 default 또는 사과 etls입니다.

에 `tls-provider=` 지정 된 값이 유효한 TLS (전송 계층 보안) 공급자가 아닌 경우

`default` 및`appletls` 는 유일 하 게 유효한 값 이며, 둘 다 동일한 옵션을 나타냅니다 .이 옵션은 기본 Apple tls API를 사용 하 여 SSL/TLS 지원을 제공 하기 위한 것입니다.

<!--- 2012 used by mmp -->
<!--- 2013 used by mmp -->
<!--- 2014 used by mmp -->

<a name="MT2015" />

### <a name="mt2015-invalid-httpmessagehandler--for-watchos-the-only-valid-value-is-nsurlsessionhandler"></a>MT2015: WatchOS에 대 한 `*` httpmessagehandler가 잘못 되었습니다. 유일 하 게 유효한 값은 NSUrlSessionHandler입니다.

이 경고는 프로젝트 파일이 잘못 된 HttpMessageHandler를 지정 하기 때문에 발생 하는 경고입니다.

이전 버전의 미리 보기 도구는 기본적으로 프로젝트 파일에 잘못 된 값을 생성 했습니다.

이 경고를 해결 하려면 텍스트 편집기에서 프로젝트 파일을 열고 XML에서 모든 HttpMessageHandler 노드를 제거 합니다.

<a name="MT2016" />

### <a name="mt2016-invalid-tlsprovider-legacy-option-the-only-valid-value-appletls-will-be-used"></a>MT2016: Tlsprovider `legacy` 옵션이 잘못 되었습니다. 올바른 값 `appletls` 만 사용 됩니다.

완전히 관리 되는 SSLv3/TLSv1 only 공급자 인 공급자는더이상xamarin.ios와함께제공되지않습니다.`legacy` 이 이전 공급자를 사용 하 고 있으며 이제 최신 `appletls` 공급자를 사용 하 여 빌드하는 프로젝트입니다.

이 경고를 해결 하려면 텍스트 편집기에서 프로젝트 파일을 열고 XML에서 ' MtouchTlsProvider ' ' 노드를 모두 제거 합니다.

<a name="MT2017" />

### <a name="mt2017-could-not-process-xml-description"></a>MT2017: XML 설명을 처리할 수 없습니다.

이는 사용자가 제공한 [사용자 지정 XML 링커 구성 파일](~/cross-platform/deploy-test/linker.md) 에 오류가 있음을 의미 합니다. 파일을 검토 하십시오.

<a name="MT2018" />

### <a name="mt2018-the-assembly--is-referenced-from-two-different-locations--and-"></a>MT2018: ' ' 어셈블리\*는 서로 다른 두 위치 '\*' 및 ' * '에서 참조 됩니다.

오류 메시지에 언급 된 어셈블리는 여러 위치에서 로드 됩니다. 항상 동일한 버전의 어셈블리를 사용 해야 합니다.

<a name="MT2019" />

### <a name="mt2019-can-not-load-the-root-assembly-"></a>MT2019: ' * ' 루트 어셈블리를 로드할 수 없습니다.

루트 어셈블리를 로드할 수 없습니다. 오류 메시지의 경로가 기존 파일을 참조 하 고 올바른 .NET 어셈블리 인지 확인 하십시오.

<a name="MT202x" />

### <a name="mt202x-binding-optimizer-failed-processing-"></a>MT202x: 바인딩 최적화 프로그램 처리 `...`에 실패 했습니다.

생성 된 바인딩 코드를 최적화 하는 동안 예기치 않은 오류가 발생 했습니다. 문제의 원인이 되는 요소는 오류 메시지에 이름이 지정 됩니다. 이 문제를 해결 하려면 자세한 정보 표시를 사용 하는 전체 빌드 로그 (즉 `-v -v -v -v` , **추가 mtouch 인수**)와 함께 [github](https://github.com/xamarin/xamarin-macios/issues/new) 에서 새 문제를 제공 해야 합니다.

마지막 숫자 `x` 는 다음과 같습니다.
* `0`어셈블리 이름
* `1`형식 이름
* `3`메서드 이름

<a name="MT2030" />

### <a name="mt2030-remove-user-resources-failed-processing-"></a>MT2030: 사용자 리소스 제거를 처리 `...`하지 못했습니다.

사용자 리소스를 제거 하는 동안 예기치 않은 오류가 발생 했습니다. 오류 메시지에서 문제의 원인이 되는 어셈블리의 이름을 지정 합니다. 이 문제를 해결 하려면 자세한 정보 표시를 사용 하는 전체 빌드 로그 ( 즉, `-v -v -v -v`에서 **추가 mtouch 인수**)와 함께 [github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제에 어셈블리를 제공 해야 합니다.

사용자 리소스는 빌드 시 응용 프로그램 번들을 만들기 위해 추출 해야 하는 어셈블리 (리소스) 내에 포함 된 파일입니다. 다음을 포함합니다.

* `__monotouch_content_*`및 `__monotouch_pages_*` 리소스
* 바인딩 어셈블리 내에 포함 된 네이티브 라이브러리

<a name="MT2040" />

### <a name="mt2040-default-httpmessagehandler-setter-failed-processing-"></a>MT2040: 기본 HttpMessageHandler setter를 처리 `...`하지 못했습니다.

응용 프로그램에 대 한 기본값 `HttpMessageHandler` 을 설정 하는 동안 예기치 않은 오류가 발생 했습니다. 자세한 정보 표시를 사용하 는 전체 빌드 로그 (즉, `-v -v -v -v`에서 **추가 mtouch 인수**)와 함께 [github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요.

<a name="MT2050" />

### <a name="mt2050-code-remover-failed-processing-"></a>MT2050: 코드 Remover 처리 `...`에 실패 했습니다.

응용 프로그램을 사용 하 여 BCL 전달에서 코드를 제거 하려고 할 때 예기치 않은 오류가 발생 했습니다. 자세한 정보 표시를 사용하 는 전체 빌드 로그 (즉, `-v -v -v -v`에서 **추가 mtouch 인수**)와 함께 [github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요.

<a name="MT2060" />

### <a name="mt2060-sealer-failed-processing-"></a>MT2060: Sealer 처리 `...`에 실패 했습니다.

형식이 나 메서드를 봉인 하거나 (최종) 일부 메서드를 가상화 할 때 예기치 않은 오류가 발생 했습니다. 오류 메시지에서 문제의 원인이 되는 어셈블리의 이름을 지정 합니다. 이 문제를 해결 하려면 자세한 정보 표시를 사용 하는 전체 빌드 로그 ( 즉, `-v -v -v -v`에서 **추가 mtouch 인수**)와 함께 [github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제에 어셈블리를 제공 해야 합니다.

<a name="MT2070" />

### <a name="mt2070-metadata-reducer-failed-processing-"></a>MT2070: 메타 데이터 리 듀 서 처리 `...`에 실패 했습니다.

응용 프로그램에서 메타 데이터를 줄이려고 할 때 예기치 않은 오류가 발생 했습니다. 오류 메시지에서 문제의 원인이 되는 어셈블리의 이름을 지정 합니다. 이 문제를 해결 하려면 자세한 정보 표시를 사용 하는 전체 빌드 로그 ( 즉, `-v -v -v -v`에서 **추가 mtouch 인수**)와 함께 [github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제에 어셈블리를 제공 해야 합니다.

<a name="MT2080" />

### <a name="mt2080-marknsobjects-failed-processing-"></a>MT2080: MarkNSObjects를 처리 `...`하지 못했습니다.

응용 프로그램에서 서브 클래스를 표시 `NSObject` 하는 동안 예기치 않은 오류가 발생 했습니다. 오류 메시지에서 문제의 원인이 되는 어셈블리의 이름을 지정 합니다. 이 문제를 해결 하려면 자세한 정보 표시를 사용 하는 전체 빌드 로그 ( 즉, `-v -v -v -v`에서 **추가 mtouch 인수**)와 함께 [github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제에 어셈블리를 제공 해야 합니다.

<a name="MT2090" />

### <a name="mt2090-inliner-failed-processing-"></a>MT2090: 인라인 처리자 처리 `...`에 실패 했습니다.

응용 프로그램에서 코드를 인라인 하는 동안 예기치 않은 오류가 발생 했습니다. 오류 메시지에서 문제의 원인이 되는 어셈블리의 이름을 지정 합니다. 이 문제를 해결 하려면 자세한 정보 표시를 사용 하는 전체 빌드 로그 (즉, `-v -v -v -v`에서 **추가 mtouch 인수**)와 함께 [github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제에 어셈블리를 제공 해야 합니다.

<!-- MT21xx: more linker errors -->

<!--- 2100 used by mmp -->

<a name="MT2100" />

### <a name="mt2100-smart-enum-conversion-preserver-failed-processing-"></a>MT2100: 스마트 열거형 변환 Preserver 처리 `...`에 실패 했습니다.

응용 프로그램에서 스마트 열거형의 변환 메서드를 표시 하는 동안 예기치 않은 오류가 발생 했습니다. 오류 메시지에서 문제의 원인이 되는 어셈블리의 이름을 지정 합니다. 이 문제를 해결 하려면 자세한 정보 표시를 사용 하는 전체 빌드 로그 (즉, `-v -v -v -v`에서 **추가 mtouch 인수**)와 함께 [github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제에 어셈블리를 제공 해야 합니다.

<a name="MT2101" />

### <a name="mt2101-cant-resolve-the-reference--referenced-from-the-method--in-"></a>MT2101: ' * '의 '\*\*' 메서드에서 참조 되는 ' ' 참조를 확인할 수 없습니다.

오류 메시지에 언급 된 메서드를 처리 하는 동안 잘못 된 어셈블리 참조가 발생 했습니다.

오류 메시지에서 문제의 원인이 되는 어셈블리의 이름을 지정 합니다. 이 문제를 해결 하려면 자세한 정보 표시를 사용 하는 전체 빌드 로그 ( 즉, `-v -v -v -v`에서 **추가 mtouch 인수**)와 함께 [github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제에 어셈블리를 제공 해야 합니다.

<a name="MT2102" />

### <a name="mt2102-error-processing-the-method--in-the-assembly--"></a>MT2102: ' '\*\*어셈블리에서 ' ' 메서드를 처리 하는 동안 오류가 발생 했습니다. *

오류 메시지에 언급 된 메서드를 표시 하는 동안 예기치 않은 오류가 발생 했습니다.

오류 메시지에서 문제의 원인이 되는 어셈블리의 이름을 지정 합니다. 이 문제를 해결 하려면 자세한 정보 표시를 사용 하는 전체 빌드 로그 ( 즉, `-v -v -v -v`에서 **추가 mtouch 인수**)와 함께 [github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제에 어셈블리를 제공 해야 합니다.

<a name="MT2103" />

### <a name="mt2103-error-processing-assembly--"></a>MT2103: '\*' 어셈블리를 처리 하는 동안 오류가 발생 했습니다. *

어셈블리를 처리 하는 동안 예기치 않은 오류가 발생 했습니다.

오류 메시지에서 문제의 원인이 되는 어셈블리의 이름을 지정 합니다. 이 문제를 해결 하려면 자세한 정보 표시를 사용 하는 전체 빌드 로그 ( 즉, `-v -v -v -v`에서 **추가 mtouch 인수**)와 함께 [github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제에 어셈블리를 제공 해야 합니다.

<a name="MT2104" />

### <a name="mm2104-unable-to-link-assembly-0-as-it-is-mixed-mode"></a>MM2104: '{0}' 어셈블리는 혼합 모드 이므로 연결할 수 없습니다.

혼합 모드 어셈블리는 링커에서 처리할 수 없습니다.

혼합 https://docs.microsoft.com/cpp/dotnet/mixed-native-and-managed-assemblies 모드 어셈블리에 대 한 자세한 내용은을 참조 하세요.

## <a name="mt3xxx-aot-error-messages"></a>MT3xxx: AOT 오류 메시지

<!--
 MT3xxx AOT
  MT30xx AOT (general) errors
  -->

<a name="MT3001" />

### <a name="mt3001-could-not-aot-the-assembly-"></a>MT3001: ' * ' 어셈블리를 AOT 수 없습니다.

이는 일반적으로 AOT 컴파일러의 버그를 나타냅니다. 오류를 재현 하는 데 사용할 수 있는 프로젝트를 사용 하 여 [github](https://github.com/xamarin/xamarin-macios/issues/new) 에서 새 문제를 해결 하세요.

프로젝트의 iOS 빌드 옵션에서 증분 빌드를 사용 하지 않도록 설정 하 여이 문제를 해결할 수 있는 경우도 있습니다 (버그 이기 때문에 계속 해 서 보고 하세요).

<a name="MT3002" />

### <a name="mt3002-aot-restriction-method--must-be-static-since-it-is-decorated-with-monopinvokecallback-see-developerxamarincomguidesiosadvanced_topicslimitationsreverse_callbacksiosinternalslimitationsmdreverse-callbacks"></a>MT3002: AOT 제한: ' * ' 메서드는 [MonoPInvokeCallback]로 데코 레이트 되므로 정적 이어야 합니다. [Developer.xamarin.com/guides/ios/advanced_topics/limitations/#Reverse_Callbacks](~/ios/internals/limitations.md#reverse-callbacks) 를 참조 하세요.

이 오류 메시지는 AOT 컴파일러에서 제공 됩니다.

<a name="MT3003" />

### <a name="mt3003-conflicting---debug-and---llvm-options-soft-debugging-is-disabled"></a>MT3003: --Debug 및--llvm 옵션이 충돌 합니다. 소프트 디버깅이 사용 되지 않습니다.

LLVM를 사용 하는 경우 디버깅이 지원 되지 않습니다. 앱을 디버깅 해야 하는 경우 먼저 LLVM를 사용 하지 않도록 설정 합니다.

<a name="MT3004" />

### <a name="mt3004-could-not-aot-the-assembly--because-it-doesnt-exist"></a>MT3004: ' * ' 어셈블리는 존재 하지 않으므로 AOT 수 없습니다.

<a name="MT3005" />

### <a name="mt3005-the-dependency--of-the-assembly--was-not-found-please-review-the-projects-references"></a>MT3005: ' '\*\*어셈블리의 ' ' 종속성을 찾을 수 없습니다. 프로젝트의 참조를 검토 하십시오.

이는 일반적으로 어셈블리가 다른 버전의 플랫폼 어셈블리 (일반적으로 .NET 4 버전의 mscorlib.dll)를 참조 하는 경우에 발생 합니다.

이는 지원 되지 않으며 제대로 빌드 또는 실행 되지 않을 수 있습니다. 어셈블리는 Xamarin.ios 버전에 없는 mscorlib.dll의 .NET 4 버전의 API를 사용할 수 있습니다.

<a name="MT3006" />

### <a name="mt3006-could-not-compute-a-complete-dependency-map-for-the-project-this-will-result-in-slower-build-times-because-xamarinios-cant-properly-detect-what-needs-to-be-rebuilt-and-what-does-not-need-to-be-rebuilt-please-review-previous-warnings-for-more-details"></a>MT3006: 프로젝트에 대 한 완전 한 종속성 맵을 계산할 수 없습니다. 이렇게 하면 Xamarin.ios가 다시 작성 해야 하는 항목 (및 다시 빌드할 필요가 없는 항목)을 제대로 검색할 수 없기 때문에 빌드 시간이 느려집니다. 자세한 내용은 이전 경고를 검토 하세요.

 제대로 빌드 또는 실행 (어셈블리는 Xamarin.ios 버전이 없는 mscorlib.dll의 .NET 4 버전의 API를 사용할 수 있음)

<a name="MT3007" />

### <a name="mt3007-debug-info-files-mdb-will-not-be-loaded-when-llvm-is-enabled"></a>MT3007: Llvm를 사용 하는 경우 디버그 정보 파일 (* .mdb)이 로드 되지 않습니다.

<a name="MT3008" />

### <a name="mt3008-bitcode-support-requires-the-use-of-the-llvm-aot-backend---llvm"></a>MT3008: Bitcode를 지원 하려면 LLVM AOT 백엔드를 사용 해야 합니다 (--LLVM).

Bitcode를 지원 하려면 LLVM AOT 백엔드 (--LLVM)를 사용 해야 합니다.

Bitcode 지원을 사용 하지 않도록 설정 하거나 LLVM를 사용 하도록 설정 하세요.

<!--- 3009 used by mmp -->
<!--- 3010 used by mmp -->

## <a name="mt4xxx-code-generation-error-messages"></a>MT4xxx: 코드 생성 오류 메시지

### <a name="mt40xx-main"></a>MT40xx: 주

<!--
 MT4xxx code generation
  MT40xx main.m
  -->

<a name="MT4001" />

### <a name="mt4001-the-main-template-could-not-be-expanded-to-"></a>MT4001: 주 템플릿을으로 `*`확장할 수 없습니다.

을 생성 하 `main.m`는 동안 오류가 발생 했습니다. [Github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요.

<a name="MT4002" />

### <a name="mt4002-failed-to-compile-the-generated-code-for-pinvoke-methods-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4002: P/Invoke 메서드에 대해 생성 된 코드를 컴파일하지 못했습니다. 버그 보고서를 파일에 넣으십시오. http://bugzilla.xamarin.com

P/Invoke 메서드에 대해 생성 된 코드를 컴파일하지 못했습니다. [Github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요.

### <a name="mt41xx-registrar"></a>MT41xx: 등록자

<!--
  MT41xx registrar.m
  -->

<a name="MT4101" />

### <a name="mt4101-the-registrar-cannot-build-a-signature-for-type-"></a>MT4101: 등록자는 형식 `*`에 대 한 서명을 작성할 수 없습니다.

런타임에서는 목표에서/C로 마샬링하는 방법을 인식 하지 못하는 내보낸 API에서 형식을 찾았습니다.

Xamarin.ios가 문제의 유형을 지원 해야 하는 경우 [github](https://github.com/xamarin/xamarin-macios/issues/new)에서 향상 된 기능을 제공 하세요.

<a name="MT4102" />

### <a name="mt4102-the-registrar-found-an-invalid-type--in-signature-for-method--use--instead"></a>MT4102: 등록 기관에서 메서드의 `*` `*`시그니처에 잘못 된 형식을 찾았습니다. 대신 `*`를 사용하세요.

이는 현재 다음 한 가지 유형 으로만 발생 합니다. System.DateTime. 대신 목표-C와 동등한 (NSDate)를 사용 합니다.

<a name="MT4103" />

### <a name="mt4103-the-registrar-found-an-invalid-type--in-signature-for-method--the-type-implements-inativeobject-but-does-not-have-a-constructor-that-takes-two-intptr-bool-arguments"></a>MT4103: 등록 자가 메서드의 `*`시그니처에 잘못 된 `*` 형식을 찾았습니다. 형식은 INativeObject를 구현 하지만 두 개의 (IntPtr, bool) 인수를 사용 하는 생성자가 없습니다.

이는 등록 자가 언급 된 특성과 함께 서명의 형식을 발견할 때 발생 합니다. 등록자는 형식의 새 인스턴스를 만들어야 할 수 있습니다 .이 경우에는 (intptr, bool) 시그니처가 포함 된 생성자가 필요 합니다. 첫 번째 인수 (IntPtr)는 관리 되는 핸들을 지정 하 고, 두 번째 인수는 호출자가 네이티브의 소유권을 획득 하는 경우 두 번째 인수를 지정 합니다. 핸들 (이 값이 false 인 경우 ' 유지 '가 개체에서 호출 됨)

<a name="MT4104" />

### <a name="mt4104-the-registrar-cannot-marshal-the-return-value-for-type--in-signature-for-method-"></a>MT4104: 등록자는 메서드에 `*` `*`대해 시그니처의 형식에 대 한 반환 값을 마샬링할 수 없습니다.

런타임에서는 목표에서/C로 마샬링하는 방법을 인식 하지 못하는 내보낸 API에서 형식을 찾았습니다.

Xamarin.ios가 문제의 유형을 지원 해야 하는 경우 [github](https://github.com/xamarin/xamarin-macios/issues/new)에서 향상 된 기능을 제공 하세요.

<a name="MT4105" />

### <a name="mt4105-the-registrar-cannot-marshal-the-parameter-of-type--in-signature-for-method-"></a>MT4105: 등록자는 메서드에 `*` `*`대해 시그니처의 형식의 매개 변수를 마샬링할 수 없습니다.

Xamarin.ios가 문제의 유형을 지원 해야 하는 경우 [github](https://github.com/xamarin/xamarin-macios/issues/new)에서 향상 된 기능을 제공 하세요.

<a name="MT4106" />

### <a name="mt4106-the-registrar-cannot-marshal-the-return-value-for-structure--in-signature-for-method-"></a>MT4106: 등록자는 메서드에 `*` `*`대해 시그니처의 구조체에 대 한 반환 값을 마샬링할 수 없습니다.

런타임에서는 목표에서/C로 마샬링하는 방법을 인식 하지 못하는 내보낸 API에서 형식을 찾았습니다.

Xamarin.ios가 문제의 유형을 지원 해야 하는 경우 [github](https://github.com/xamarin/xamarin-macios/issues/new)에서 향상 된 기능을 제공 하세요.

<a name="MT4107" />

### <a name="mt4107-the-registrar-cannot-marshal-the-parameter-of-type--in-signature-for-method-"></a>MT4107: 등록자는 메서드에 `*` `+`대해 시그니처의 형식의 매개 변수를 마샬링할 수 없습니다.

런타임에서는 목표에서/C로 마샬링하는 방법을 인식 하지 못하는 내보낸 API에서 형식을 찾았습니다.

Xamarin.ios가 문제의 유형을 지원 해야 하는 경우 [github](https://github.com/xamarin/xamarin-macios/issues/new)에서 향상 된 기능을 제공 하세요.

<a name="MT4108" />

### <a name="mt4108-the-registrar-cannot-get-the-objectivec-type-for-managed-type-"></a>MT4108: 등록 기관에서 관리 되는 형식 `*`에 대 한 ObjectiveC 형식을 가져올 수 없습니다.

런타임에서는 목표에서/C로 마샬링하는 방법을 인식 하지 못하는 내보낸 API에서 형식을 찾았습니다.

Xamarin.ios가 문제의 유형을 지원 해야 하는 경우 [github](https://github.com/xamarin/xamarin-macios/issues/new)에서 향상 된 기능을 제공 하세요.

<a name="MT4109" />

### <a name="mt4109-failed-to-compile-the-generated-registrar-code-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4109: 생성 된 등록자 코드를 컴파일하지 못했습니다. 버그 보고서를 파일에 넣으십시오. http://bugzilla.xamarin.com

등록자에 대해 생성 된 코드를 컴파일하지 못했습니다. 빌드 로그에는 코드가 컴파일되지 않는 이유를 설명 하는 네이티브 컴파일러의 출력이 포함 됩니다.

이는 항상 Xamarin.ios의 버그입니다. 프로젝트 또는 테스트 사례와 함께 [github](https://github.com/xamarin/xamarin-macios/issues/new) 에서 새 문제를 해결 하세요.

<a name="MT4110" />

### <a name="mt4110-the-registrar-cannot-marshal-the-out-parameter-of-type--in-signature-for-method-"></a>MT4110: 등록자는 메서드에 `*` `*`대해 시그니처의 형식의 out 매개 변수를 마샬링할 수 없습니다.

<a name="MT4111" />

### <a name="mt4111-the-registrar-cannot-build-a-signature-for-type--in-method-"></a>MT4111: 등록자는 메서드에서 `*` `*`형식의 시그니처를 작성할 수 없습니다.

<a name="MT4112" />

### <a name="mt4112-the-registrar-found-an-invalid-type--registering-generic-types-with-objective-c-is-not-supported-and-may-lead-to-random-behavior-andor-crashes-for-backwards-compatibility-with-older-versions-of-xamarinios-it-is-possible-to-ignore-this-error-by-passing---unsupported--enable-generics-in-registrar-as-an-additional-mtouch-argument-in-the-projects-ios-build-options-page-see-developerxamarincomguidesiosadvanced_topicsregistrariosinternalsregistrarmd-for-more-information"></a>MT4112: 등록 기관에서 잘못 된 형식을 `*`찾았습니다. 목적-C를 사용 하 여 제네릭 형식을 등록 하는 것은 지원 되지 않으며, 임의 동작 및/또는 충돌이 발생할 수 있습니다 (이전 버전의 xamarin.ios와 이전 버전과의 호환성을 위해 추가 mtouch `--unsupported--enable-generics-in-registrar` 를 전달 하 여이 오류를 무시할 수 있음). 프로젝트의 iOS 빌드 옵션 페이지에 있는 인수입니다. 자세한 내용은 [developer.xamarin.com/guides/ios/advanced_topics/registrar](~/ios/internals/registrar.md) 를 참조 하세요.

<a name="MT4113" />

### <a name="mt4113-the-registrar-found-a-generic-method--exporting-generic-methods-is-not-supported-and-will-lead-to-random-behavior-andor-crashes"></a>MT4113: 등록자에서 제네릭 메서드 '\*.\*'을 (를) 찾았습니다. 제네릭 메서드를 내보내는 것은 지원 되지 않으며 임의 동작 및/또는 작동이 중단 됩니다.

<a name="MT4114" />

### <a name="mt4114-unexpected-error-in-the-registrar-for-the-method----please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4114: '\*. ' 메서드의 등록자에서 예기치 않은 오류가 발생\*했습니다. 버그 보고서를 파일에 넣으십시오. http://bugzilla.xamarin.com

<a name="MT4116" />

### <a name="mt4116-could-not-register-the-assembly--"></a>MT4116: ' * ' 어셈블리를 등록할 수 없습니다. *

<a name="MT4117" />

### <a name="mt4117-the-registrar-found-a-signature-mismatch-in-the-method----the-selector-indicates-the-method-takes--parameters-while-the-managed-method-has--parameters"></a>MT4117: 등록자의 ' *.* ' 메서드에서 시그니처 불일치를 발견 했습니다. 선택기는 메서드가 * 매개 변수를 사용 하는 반면, 관리 되는 메서드에는 * 매개 변수가 있습니다.

<a name="MT4118" />

### <a name="mt4118-cannot-register-two-managed-types--and--with-the-same-native-name-"></a>MT4118: 동일한 네이티브 이름 (' * '\*)을 사용\*하 여 두 개의 관리 되는 형식 (' ' 및 ' ')을 등록할 수 없습니다.

<a name="MT4119" />

### <a name="mt4119-could-not-register-the-selector--of-the-member--because-the-selector-is-already-registered-on-a-different-member"></a>MT4119: 선택기 ' '이 (가\*) 다른 멤버에 이미\*등록 되어 있으므로 '. * ' 멤버의 선택기 ' '을 (를) 등록할 수 없습니다.

<a name="MT4120" />

### <a name="mt4120-the-registrar-found-an-unknown-field-type--in-field--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4120: 등록자의 '\*. * ' 필드에\*알 수 없는 필드 형식 ' '이 (가) 있습니다. 버그 보고서를 파일에 넣으십시오. http://bugzilla.xamarin.com

이 오류는 Xamarin.ios의 버그를 나타냅니다. [Github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요.

<a name="MT4121" />

### <a name="mt4121-cannot-use-gccg-to-compile-the-generated-code-from-the-static-registrar-when-using-the-accounts-framework-the-header-files-provided-by-apple-used-during-the-compilation-require-clang-either-use-clang---compilerclang-or-the-dynamic-registrar---registrardynamic"></a>MT4121: 계정 프레임 워크를 사용 하는 경우 (Apple에서 제공 하는 헤더 파일에는 Clang 필요), GCC/G + +를 사용 하 여 정적 등록 기관에서 생성 된 코드를 컴파일할 수 없습니다. Clang (--컴파일러: Clang) 또는 동적 등록자 (--등록자: 동적)를 사용 합니다.

<a name="MT4122" />

### <a name="mt4122-cannot-use-the-clang-compiler-provided-in-the--sdk-to-compile-the-generated-code-from-the-static-registrar-when-non-ascii-type-names--are-present-in-the-application-either-use-gccg---compilergccg-the-dynamic-registrar---registrardynamic-or-a-newer-sdk"></a>MT4122: 에서 제공 하는 Clang 컴파일러를 사용할 수 없습니다 *.* 응용 프로그램에 ASCII가 아닌 형식 이름 (' * ')이 있으면 SDK를 통해 정적 등록자에서 생성 된 코드를 컴파일합니다. GCC/G + + (--컴파일러: GCC | G + +), 동적 등록자 (--등록자: 동적) 또는 최신 SDK를 사용 합니다.

<a name="MT4123" />

### <a name="mt4123-the-type-of-the-variadic-parameter-in-the-variadic-function--must-be-systemintptr"></a>MT4123: Variadic 함수 ' * '의 variadic 매개 변수 형식은 System.object 여야 합니다.

<a name="MT4124" />

### <a name="mt4124-invalid--found-on--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4124: ' * '에 잘못 된 *가 있습니다. 버그 보고서를 파일에 넣으십시오. http://bugzilla.xamarin.com

이 오류는 Xamarin.ios의 버그를 나타냅니다. [Github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요.

<a name="MT4125" />

### <a name="mt4125-the-registrar-found-an-invalid-type--in-signature-for-method--the-interface-must-have-a-protocol-attribute-specifying-its-wrapper-type"></a>MT4125: 등록 자가 '\*' 메서드의 시그니처에 잘못\*된 형식 ' '이 (가) 있습니다. 인터페이스에는 래퍼 형식을 지정 하는 프로토콜 특성이 있어야 합니다.

<a name="MT4126" />

### <a name="mt4126-cannot-register-two-managed-protocols--and--with-the-same-native-name-"></a>MT4126: 동일한 네이티브 이름 (' * '\*)을 사용\*하 여 두 개의 관리 되는 프로토콜 (' ' 및 ' ')을 등록할 수 없습니다.

<a name="MT4127" />

### <a name="mt4127-cannot-register-more-than-one-interface-method-for-the-method--which-is-implementing-"></a>MT4127: ' '을 (를)\*\*구현 하는 메서드 ' '에 대 한 인터페이스 메서드를 두 개 이상 등록할 수 없습니다.

<a name="MT4128" />

### <a name="mt4128-the-registrar-found-an-invalid-generic-parameter-type--in-the-method--the-generic-parameter-must-have-an-nsobject-constraint"></a>MT4128: 등록자의 '\*' 메서드에서 잘못 된 제네릭 매개\*변수 형식 ' '이 (가) 발견 되었습니다. 제네릭 매개 변수에는 ' NSObject ' 제약 조건이 있어야 합니다.

<a name="MT4129" />

### <a name="mt4129-the-registrar-found-an-invalid-generic-return-type--in-the-method--the-generic-return-type-must-have-an-nsobject-constraint"></a>MT4129: 등록자의 '\*' 메서드에서 잘못 된 제네릭 반환\*형식 ' '이 (가) 발견 되었습니다. 제네릭 반환 형식에는 ' NSObject ' 제약 조건이 있어야 합니다.

<a name="MT4130" />

### <a name="mt4130-the-registrar-cannot-export-static-methods-in-generic-classes-"></a>MT4130: 등록자는 제네릭 클래스 (' * ')에서 정적 메서드를 내보낼 수 없습니다.

<a name="MT4131" />

### <a name="mt4131-the-registrar-cannot-export-static-properties-in-generic-classes-"></a>MT4131: 등록자는 제네릭 클래스 ('\*.\*')에서 정적 속성을 내보낼 수 없습니다.

<a name="MT4132" />

### <a name="mt4132-the-registrar-found-an-invalid-generic-return-type--in-the-property--the-return-type-must-have-an-nsobject-constraint"></a>MT4132: 등록자의 '\*' 속성에 잘못 된 제네릭\*반환 형식 ' '이 (가) 있습니다. 반환 형식에는 ' NSObject ' 제약 조건이 있어야 합니다.

<a name="MT4133" />

### <a name="mt4133-cannot-construct-an-instance-of-the-type--from-objective-c-because-the-type-is-generic-runtime-exception"></a>MT4133: 제네릭 형식 이므로 ' * ' 형식의 인스턴스를 생성할 수 없습니다. [런타임 예외]

<a name="MT4134" />

### <a name="mt4134-your-application-is-using-the--framework-which-isnt-included-in-the-ios-sdk-youre-using-to-build-your-app-this-framework-was-introduced-in-ios--while-youre-building-with-the-ios--sdk-please-select-a-newer-sdk-in-your-apps-ios-build-options"></a>MT4134: 응용 프로그램에서 앱을 빌드하는 데 사용 하는 iOS SDK에 포함 되지 않은 ' * ' 프레임 워크를 사용 하 고 있습니다 .이 프레임 워크는 ios * SDK를 사용 하 여 빌드하는 동안 ios *에서 도입 되었습니다. 앱의 iOS 빌드 옵션에서 최신 SDK를 선택 하세요.

<a name="MT4135" />

### <a name="mt4135-the-member--has-an-export-attribute-that-doesnt-specify-a-selector-a-selector-is-required"></a>MT4135: '\*.\*' 멤버에 선택기를 지정 하지 않는 내보내기 특성이 있습니다. 선택 기가 필요 합니다.

<a name="MT4136" />

### <a name="mt4136-the-registrar-cannot-marshal-the-parameter-type--of-the-parameter--in-the-method-"></a>MT4136: 등록자는 '\*\*. * ' 메서드에서 '\*' 매개 변수의 매개 변수 형식 ' '을 (를) 마샬링할 수 없습니다.

<!-- MT4137 is unused -->

<a name="MT4138" />

### <a name="mt4138-the-registrar-cannot-marshal-the-property-type--of-the-property-"></a>MT4138: 등록자는 '\*. * ' 속성의\*' ' 속성 형식을 마샬링할 수 없습니다.

<a name="MT4139" />

### <a name="mt4139-the-registrar-cannot-marshal-the-property-type--of-the-property--properties-with-the-connect-attribute-must-have-a-property-type-of-nsobject-or-a-subclass-of-nsobject"></a>MT4139: 등록자는 '\*. * ' 속성의\*' ' 속성 형식을 마샬링할 수 없습니다. [Connect] 특성이 있는 속성에는 NSObject (또는 NSObject의 서브 클래스)의 속성 형식이 있어야 합니다.

<a name="MT4140" />

### <a name="mt4140-the-registrar-found-a-signature-mismatch-in-the-method----the-selector-indicates-the-variadic-method-takes--parameters-while-the-managed-method-has--parameters"></a>MT4140: 등록자의 ' *.* ' 메서드에서 시그니처 불일치를 발견 했습니다. 선택기는 variadic 메서드가 * 매개 변수를 사용 하는 반면, 관리 되는 메서드에는 * 매개 변수가 있습니다.

<a name="MT4141" />

### <a name="mt4141-cannot-register-the-selector--on-the-member--because-xamarinios-implicitly-registers-this-selector"></a>MT4141: Xamarin.ios는이 선택기를\*암시적으로 등록 하므로 '\*' 멤버에 선택기 ' '를 등록할 수 없습니다.

이는 프레임 워크 형식을 서브클래싱 하 고 ' retain ', ' release ' 또는 ' dealloc ' 메서드를 구현 하려고 할 때 발생 합니다.

```csharp
class MyNSObject : NSObject
{
  [Export ("retain")]
  new void Retain () {}

  [Export ("release")]
  new void Release () {}

  [Export ("dealloc")]
  new void Dealloc () {}
}
```

그러나 클래스가 프레임 워크 형식의 첫 번째 하위 클래스가 아닌 경우에는 이러한 메서드를 재정의할 수 있습니다.

```csharp
class MyNSObject : NSObject
{
}

class MyCustomNSObject : MyNSObject
{
  [Export ("retain")]
  new void Retain () {}

  [Export ("release")]
  new void Release () {}

  [Export ("dealloc")]
  new void Dealloc () {}
}
```

`retain`이 경우 xamarin.ios는 `release` 클래스`MyNSObject` 에서 및 `dealloc` 를 재정의 하 고 충돌은 발생 하지 않습니다.

<a name="MT4142" />

### <a name="mt4142-failed-to-register-the-type-"></a>MT4142: ' * ' 형식을 등록 하지 못했습니다.

<a name="MT4143" />

### <a name="mt4143-the-objectivec-class--could-not-be-registered-it-does-not-seem-to-derive-from-any-known-objectivec-class-including-nsobject"></a>MT4143: ObjectiveC 클래스 ' * '를 등록할 수 없습니다. 알려진 ObjectiveC 클래스 (NSObject 포함)에서 파생 되지 않은 것 같습니다.

<a name="MT4144" />

### <a name="mt4144-cannot-register-the-method--since-it-does-not-have-an-associated-trampoline-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4144: ' * ' 메서드는 연결 된 trampoline을 포함 하지 않으므로 등록할 수 없습니다. 버그 보고서를 제출 하세요 http://bugzilla.xamarin.com 합니다.

이는 Xamarin.ios의 버그를 나타냅니다. [Github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요.

<a name="MT4145" />

### <a name="mt4145-invalid-enum--enums-with-the-native-attribute-must-have-a-underlying-enum-type-of-either-long-or-ulong"></a>MT4145: 열거형 ' * '이 잘못 되었습니다. [Native] 특성이 있는 열거형에는 ' long ' 또는 ' ulong '의 기본 열거형 형식이 있어야 합니다.

<a name="MT4146" />

### <a name="mt4146-the-name-parameter-of-the-registrar-attribute-on-the-class---contains-an-invalid-character--"></a>MT4146: \*' ' ('\*') 클래스에서 등록자 특성의 Name 매개 변수에 잘못 된 문자 '\*' ()이 (\*가) 있습니다.

Objectice-C 클래스의 이름에는 공백을 포함할 수 없습니다. 즉 `Register` , 해당 관리 되는 클래스 `Name` 의 특성에는 공백이 포함 될 수 없습니다.

오류 메시지에 언급 `Register` 된 관리 되는 클래스의 특성에 공백이 포함 되어 있지 않은지 확인 하십시오.

<a name="MT4147" />

### <a name="mt4147-detected-a-protocol-inheriting-from-the-jsexport-protocol-while-using-the-dynamic-registrar-it-is-not-possible-to-export-protocols-to-javascriptcore-dynamically-the-static-registrar-must-be-used-add---registrarstatic-to-the-additional-mtouch-arguments-in-the-projects-ios-build-options-to-select-the-static-registrar"></a>MT4147: 는 동적 등록자를 사용 하는 동안 JSExport 프로토콜에서 상속 되는 프로토콜을 검색 했습니다. JavaScriptCore에 대 한 프로토콜을 동적으로 내보낼 수는 없습니다. 정적 등록자를 사용 해야 합니다 (정적 등록자를 선택 하려면 프로젝트의 iOS 빌드 옵션에서 추가 mtouch 인수에 '--등록자: static 추가).

<a name="MT4148" />

### <a name="mt4148-the-registrar-found-a-generic-protocol--exporting-generic-protocols-is-not-supported"></a>MT4148: 등록자에서 제네릭 프로토콜 ' * '를 찾았습니다. 제네릭 프로토콜 내보내기는 지원 되지 않습니다.

<a name="MT4149" />

### <a name="mt4149-cannot-register-the-method--because-the-type-of-the-first-parameter--does-not-match-the-category-type-"></a>MT4149: 첫 번째 매개 변수 형식\*(\*\*' ')이 범주 유형 ('\*')과 일치 하지 않으므로 '. ' 메서드를 등록할 수 없습니다.

<a name="MT4150" />

### <a name="mt4150-cannot-register-the-type--because-the-type-property--in-its-category-attribute-does-not-inherit-from-nsobject"></a>MT4150: Category 특성의 type 속성\*('\*')이 nsobject에서 상속 되지 않으므로 ' ' 형식을 등록할 수 없습니다.

<a name="MT4151" />

### <a name="mt4151-cannot-register-the-type--because-the-type-property-in-its-category-attribute-isnt-set"></a>MT4151: 범주 특성의 Type 속성이 설정 되지 않았으므로 ' * ' 형식을 등록할 수 없습니다.

<a name="MT4152" />

### <a name="mt4152-cannot-register-the-type--as-a-category-because-it-implements-inativeobject-or-subclasses-nsobject"></a>MT4152: ' * ' 형식은 INativeObject 또는 서브 클래스 NSObject를 구현 하므로 범주로 등록할 수 없습니다.

<a name="MT4153" />

### <a name="mt4153-cannot-register-the-type--as-a-category-because-its-generic"></a>MT4153: '\*' 형식은 제네릭 이므로 범주로 등록할 수 없습니다.

<a name="MT4154" />

### <a name="mt4154-cannot-register-the-method--as-a-category-method-because-its-generic"></a>MT4154: 제네릭 이므로 '\*' 메서드를 범주 메서드로 등록할 수 없습니다.

<a name="MT4155" />

### <a name="mt4155-cannot-register-the-method--with-the-selector--as-a-category-method-on--because-the-objective-c-already-has-an-implementation-for-this-selector"></a>MT4155: 목표 C에 이미이\*선택기에 대 한 구현이\*있으므로 선택기 ' '를 사용 하 여 ' ' 메서드를 ' * '의 범주 메서드로 등록할 수 없습니다.

<a name="MT4156" />

### <a name="mt4156-cannot-register-two-categories--and--with-the-same-native-name-"></a>MT4156: 동일한 네이티브 이름 (' *\*')을\*사용 하 여 두 개의 범주 (' ' 및 ' ')를 등록할 수 없습니다.

<a name="MT4157" />

### <a name="mt4157-cannot-register-the-category-method--because-at-least-one-parameter-is-required-and-its-type-must-match-the-category-type-"></a>MT4157: 하나 이상의 매개 변수가 필요한 데\*' ' 범주 메서드를 등록할 수 없습니다. 해당 형식이 범주 유형 '\*'과 (와) 일치 해야 합니다.

<a name="MT4158" />

### <a name="mt4158-cannot-register-the-constructor--in-the-category--because-constructors-in-categories-are-not-supported"></a>MT4158: 범주에서 생성자가 지원 되지 않으므로 범주 *에서 생성자 *를 등록할 수 없습니다.

<a name="MT4159" />

### <a name="mt4159-cannot-register-the-method--as-a-category-method-because-category-methods-must-be-static"></a>MT4159: 범주 메서드는 정적 이어야 하므로 ' * ' 메서드를 범주 메서드로 등록할 수 없습니다.

<a name="MT4160" />

### <a name="mt4160-invalid-character---found-in-selector--for-"></a>MT4160: '\*\*\*'의 선택기 ' '에 잘못 된 문자 ' ' ()이 (가) 있습니다.\*

<a name="MT4161" />

### <a name="mt4161-the-registrar-found-an-unsupported-structure--all-fields-in-a-structure-must-also-be-structures-field--with-type-2-is-not-a-structure"></a>MT4161: 등록자에서 지원 되지 않는 구조 '\*'이 (가) 있습니다. 구조체의 모든 필드도 구조체 여야 합니다. '\*{2}' 형식의 ' ' 필드는 구조체가 아닙니다.

등록 자가 지원 되지 않는 필드가 있는 구조를 발견 했습니다.

또한 목표 C에 노출 되는 구조체의 모든 필드는 구조체 (클래스가 아님) 여야 합니다.

<a name="MT4162" />

### <a name="mt4162-the-type--used-as--2-is-not-available-in---it-was-introduced-in---please-build-with-a-newer--sdk-usually-done-by-using-the-most-recent-version-of-xcode"></a>MT4162: * *에서\*' ' (으 {2})로 사용 되는 ' ' 유형은 사용할 수 없습니다 (* *에 도입\* 됨). 최신 * SDK를 사용 하 여 빌드 하세요 (일반적으로 최신 버전의 Xcode를 사용 하 여 수행).

등록 기관에서 현재 SDK에 포함 되지 않은 형식을 찾았습니다.

Xcode를 업그레이드 하세요.

<a name="MT4163" />

### <a name="mt4163-internal-error-in-the-registrar--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4163: 등록자 (*)에서 내부 오류가 발생 했습니다. 버그 보고서를 파일에 넣으십시오. http://bugzilla.xamarin.com

이 오류는 Xamarin.ios의 버그를 나타냅니다. [Github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요.

<a name="MT4164" />

### <a name="mt4164-cannot-export-the-property--because-its-selector--is-an-objective-c-keyword-please-use-a-different-name"></a>MT4164: ' ' 속성의 선택기\*'\*'이 (가) 목표-C 키워드 이므로 ' ' 속성을 내보낼 수 없습니다. 다른 이름을 사용 하세요.

해당 속성에 대 한 선택기는 유효한 목표-C 식별자가 아닙니다.

올바른 목표-C 식별자를 선택기로 사용 하세요.

<a name="MT4165" />

### <a name="mt4165-the-registrar-couldnt-find-the-type-systemvoid-in-any-of-the-referenced-assemblies"></a>MT4165: 등록 기관에서 참조 된 어셈블리 중 하나에서 ' System.object ' 형식을 찾을 수 없습니다.

이 오류는 주로 Xamarin.ios의 버그를 나타냅니다. [Github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요.

<a name="MT4166" />

### <a name="mt4166-cannot-register-the-method--because-the-signature-contains-a-type--that-isnt-a-reference-type"></a>MT4166: 시그니처에 참조 형식이 아닌 형식\*(\*)이 포함 되어 있으므로 ' ' 메서드를 등록할 수 없습니다.

이는 일반적으로 Xamarin.ios의 버그를 나타냅니다. [Github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요.

<a name="MT4167" />

### <a name="mt4167-cannot-register-the-method--because-the-signature-contains-a-generic-type--with-a-generic-argument-type-that-isnt-an-nsobject-subclass-"></a>MT4167: 시그니처에 nsobject 하위 클래스\*(*)가 아닌 제네릭 인수 형식을 사용\*하는 제네릭 형식 ()이 포함 되어 있으므로 ' ' 메서드를 등록할 수 없습니다.

이는 일반적으로 Xamarin.ios의 버그를 나타냅니다. [Github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요.

<a name="MT4168" />

### <a name="mt4168-cannot-register-the-type-managed_name-because-its-objective-c-name-exported_name-is-an-objective-c-keyword-please-use-a-different-name"></a>MT4168: ' {Managed name} '은\_(는) 객관적인 이름 ' {내보내진\_name} '이 (가) 목표-c 키워드 이므로 해당 유형을 등록할 수 없습니다. 다른 이름을 사용 하세요.

해당 형식에 대 한 목표-C 이름이 올바른 목표-C 식별자가 아닙니다.

올바른 목표-C 식별자를 사용 하십시오.

<a name="MT4169" />

### <a name="mt4169-failed-to-generate-a-pinvoke-wrapper-for-method-message"></a>MT4169: {Method}에 대 한 P/Invoke 래퍼를 생성 하지 못했습니다: {message}

Xamarin.ios가 설명 된에 대 한 P/Invoke 래퍼 함수를 생성 하지 못했습니다.
근본적인 원인에 대 한 보고 된 오류 메시지를 확인 하십시오.

<a name="MT4170" />

### <a name="mt4170-the-registrar-cant-convert-from-managed-type-to-native-type-for-the-return-value-in-the-method-method"></a>MT4170: 등록자는 {method} 메서드의 반환 값에 대해 ' {managed type} '에서 ' {native type} ' (으)로 변환할 수 없습니다.

오류 <a href="#MT4172">MT4172</a>에 대 한 설명을 참조 하세요.

<a name="MT4171" />

### <a name="mt4171-the-bindas-attribute-on-the-member-member-is-invalid-the-bindas-type-type-is-different-from-the-property-type-type"></a>MT4171: {Member} 멤버에 대 한 BindAs 특성이 잘못 되었습니다. BindAs 유형 {type}은 (는) 속성 유형 {type}과 (와) 다릅니다.

BindAs 특성의 형식이 연결 된 멤버의 형식과 일치 하는지 확인 하세요.

<a name="MT4172" />

### <a name="mt4172-the-registrar-cant-convert-from-native-type-to-managed-type-for-the-parameter-parameter-name-in-the-method-method"></a>MT4172: 등록자는 {method} 메서드의 ' {parameter name} ' 매개 변수에 대해 ' {native type} '에서 ' {managed type} ' (으)로 변환할 수 없습니다.

등록 기관은 언급 된 형식 간의 변환을 지원 하지 않습니다.

이는 Xamarin.ios에서 해당 API를 제공 하는 경우 Xamarin.ios의 버그입니다. [Github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요.

네이티브 라이브러리에 대 한 바인딩 프로젝트를 개발 하는 동안이를 실행 하면 새 형식의 조합에 대 한 지원을 추가할 수 있습니다. 이 경우 테스트 사례를 사용 하 여 [github](https://github.com/xamarin/xamarin-macios/issues/new) 에 향상 된 기능을 제공 하 고 평가 합니다.

## <a name="mt5xxx-gcc-and-toolchain-error-messages"></a>MT5xxx: GCC 및 도구 체인 오류 메시지

### <a name="mt51xx-compilation"></a>MT51xx: 컴파일

<!--
 MT5xxx GCC and toolchain
  MT51xx compilation
  -->

<a name="MT5101" />

### <a name="mt5101-missing--compiler-please-install-xcode-command-line-tools-component"></a>MT5101: ' * ' 컴파일러가 없습니다. Xcode ' 명령줄 도구 ' 구성 요소를 설치 하세요.

<a name="MT5102" />

### <a name="mt5102-failed-to-assemble-the-file--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5102: ' * ' 파일을 어셈블할 수 없습니다. 버그 보고서를 파일에 넣으십시오. http://bugzilla.xamarin.com

<a name="MT5103" />

### <a name="mt5103-failed-to-compile-the-file--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5103: ' * ' 파일을 컴파일하지 못했습니다. 버그 보고서를 파일에 넣으십시오. http://bugzilla.xamarin.com

<a name="MT5104" />

### <a name="mt5104-could-not-find-neither-the--nor-the--compiler-please-install-xcode-command-line-tools-component"></a>MT5104: '\*' 및 '\*' 컴파일러를 찾을 수 없습니다. Xcode ' 명령줄 도구 ' 구성 요소를 설치 하세요.

<!-- 5105 is used by mmp -->

<a name="MT5106" />

### <a name="mt5106-could-not-compile-the-files--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5106: ' * ' 파일을 컴파일할 수 없습니다. 버그 보고서를 파일에 넣으십시오. http://bugzilla.xamarin.com

이는 일반적으로 Xamarin.ios의 버그를 나타냅니다. [github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요.

### <a name="mt52xx-linking"></a>MT52xx: 연결

<!--
  MT52xx linking
  -->

<a name="MT5201" />

### <a name="mt5201-native-linking-failed-please-review-the-build-log-and-the-user-flags-provided-to-gcc-"></a>MT5201: 네이티브 연결에 실패 했습니다. Gcc: *에 제공 된 빌드 로그 및 사용자 플래그를 검토 하세요.

<a name="MT5202" />

### <a name="mt5202-native-linking-failed-please-review-the-build-log"></a>MT5202: 네이티브 연결에 실패 했습니다. 빌드 로그를 검토 하세요.

<a name="MT5203" />

### <a name="mt5203-native-linking-warning-"></a>MT5203: 네이티브 링크 경고: *

<!--- 5204-5208 are not used -->

<a name="MT5209" />

### <a name="mt5209-native-linking-error-"></a>MT5209: 네이티브 연결 오류: *

<a name="MT5210" />

### <a name="mt5210-native-linking-failed-undefined-symbol--please-verify-that-all-the-necessary-frameworks-have-been-referenced-and-native-libraries-are-properly-linked-in"></a>MT5210: 네이티브 링크 실패, 정의 되지 않은 기호: *. 필요한 모든 프레임 워크가 참조 되었고 네이티브 라이브러리가 제대로 연결 되어 있는지 확인 하세요.

이는 네이티브 링커가 어딘가에 참조 되는 기호를 찾을 수 없는 경우에 발생 합니다. 이는 다음과 같은 몇 가지 이유로 발생할 수 있습니다.

* 타사 바인딩에는 프레임 워크가 필요 하지만 바인딩에는 해당 `[LinkWith]` 특성이 지정 되지 않습니다. 해결책이
  - 타사 바인딩의 작성자 이거나 소스에 대 한 액세스 권한이 있는 경우 필요한 프레임 워크를 포함 하도록 바인딩의 `[LinkWith]` 특성을 수정 합니다.

    ```csharp
    [LinkWith ("mylib.a", Frameworks = "SystemConfiguration")]
    ```

  - 타사 바인딩을 수정할 수 없는 경우에 `-gcc_flags '-framework SystemFramework'` `mtouch` 전달 하 여 필요한 프레임 워크를 수동으로 연결할 수 있습니다 .이 작업은 프로젝트의 iOS 빌드 옵션 페이지에서 추가 mtouch 인수를 수정 하 여 수행 합니다. 모든 프로젝트 구성에 대해이 작업을 수행 해야 합니다.
* 경우에 따라 관리 되는 바인딩은 여러 네이티브 라이브러리로 구성 되며 모두 바인딩에 포함 되어야 합니다. 각 바인딩 프로젝트에 네이티브 라이브러리가 두 개 이상 있을 수 있으므로 솔루션은 필요한 네이티브 라이브러리를 모두 바인딩 프로젝트에 추가 하는 것입니다.</li>
* 관리 되는 바인딩은 네이티브 라이브러리에 없는 네이티브 기호를 참조 합니다.
    이는 일반적으로 바인딩이 일정 시간 동안 존재 하 고 해당 시간 동안 네이티브 코드가 수정 되어 특정 네이티브 클래스가 제거 되거나 이름이 변경 된 경우에 발생 합니다.
* P/Invoke가 존재 하지 않는 네이티브 기호를 참조 합니다. Xamarin.ios 7.4부터이 경우 <a href="#MT5214">MT5214</a> 오류가 보고 됩니다. 자세한 내용은 MT5214를 참조 하세요.
* 타사 바인딩/라이브러리가를 사용 하 여 C++빌드 되었지만 바인딩이 `[LinkWith]` 특성에이를 지정 하지 않습니다. 이는 기호에 올바른 C++ 기호가 있기 때문에 일반적으로 쉽게 인식할 수 있습니다 (한 가지 일반적인 예 `__ZNKSt9exception4whatEv`는).
  - 타사 바인딩의 작성자 이거나 소스에 대 한 액세스 권한이 있는 경우, 바인딩의 `[LinkWith]` 특성을 수정 하 여 `IsCxx` 플래그를 설정 합니다.

    ```csharp
    [LinkWith ("mylib.a", IsCxx = true)]
    ```

  - 타사 바인딩을 수정할 수 없거나 타사 라이브러리를 사용 하 여 수동으로 연결 하는 경우 mtouch에 전달 `-cxx` 하 여 해당 플래그를 설정할 수 있습니다 .이 작업은 프로젝트의 iOS 빌드 옵션 페이지에서 추가 mtouch 인수를 수정 하 여 수행 됩니다. . 모든 프로젝트 구성에 대해이 작업을 수행 해야 합니다.

<a name="MT5211" />

### <a name="mt5211-native-linking-failed-undefined-objective-c-class--the-symbol--could-not-be-found-in-any-of-the-libraries-or-frameworks-linked-with-your-application"></a>MT5211: 네이티브 링크 실패, 정의 되지 않은 목표-C \*클래스:. 응용 프로그램과 연결\*된 라이브러리나 프레임 워크에서 ' ' 기호를 찾을 수 없습니다.

이는 네이티브 링커가 어딘가에 참조 된 목표 C 클래스를 찾을 수 없는 경우에 발생 합니다. 이는 다음과 같은 몇 가지 이유로 발생할 수 있습니다. [MT5210](#MT5210) 와 동일 합니다.

* 타사 바인딩에는 목표 C 프로토콜이 바인딩되어 있지만 api 정의의 특성에는 `[Protocol]` 주석이 지정 되지 않았습니다. 해결책이
  - 누락 `[Protocol]` 된 특성을 추가 합니다.

    ```csharp
    [BaseType (typeof (NSObject))]
    [Protocol] // Add this
    public interface MyProtocol
    {
    }
    ```

<a name="MT5212" />

### <a name="mt5212-native-linking-failed-duplicate-symbol-"></a>MT5212: 네이티브 연결에 실패 했습니다. 중복 된 기호: *.

이는 네이티브 링커가 모든 네이티브 라이브러리 간에 중복 된 기호를 발견할 때 발생 합니다. 이 오류 다음에는 각 기호 발생에 대 한 위치에 하나 이상의 [MT5213](#MT5213) 오류가 있을 수 있습니다. 이 오류의 가능한 원인은 다음과 같습니다.

* 동일한 네이티브 라이브러리가 두 번 포함 됩니다.
* 동일한 기호를 정의 하는 두 개의 고유 네이티브 라이브러리가 발생 합니다.
* 네이티브 라이브러리가 제대로 빌드되지 않고 동일한 기호를 두 번 이상 포함 합니다.
  터미널에서 다음 명령 집합을 사용 하 여 확인할 수 있습니다 (x86_64/armv7/armv7s 용 thumb-2/arm64를 빌드하는 아키텍처에 따라 바꾸기).

  ```
  # Native libraries are usually fat libraries, containing binary code for
  # several architectures in the same file. First we extract the binary
  # code for the architecture we're interested in.
  lipo libNative.a -thin i386 -output libNative.i386.a

  # Now query the native library for the duplicated symbol.
  nm libNative.i386.a | fgrep 'SYMBOL'

  # You can also list the object files inside the native library.
  # In most cases this will reveal duplicated object files.
  ar -t libNative.i386.a
  ```

  다음과 같은 몇 가지 방법으로이 문제를 해결할 수 있습니다.

  - 네이티브 라이브러리의 공급자가이를 수정 하 고 업데이트 된 버전을 제공 하도록 요청 합니다.
  - 추가 개체 파일을 제거 하 여 직접 수정 합니다 .이는 문제가 실제로 중복 된 개체 파일에 있는 경우에만 작동 합니다.

  ```
  # Find out if the library is a fat library, and which
  # architectures it contains.
  lipo -info libNative.a

  # Extract each architecture (i386/x86_64/armv7/armv7s/arm64) to a separate file
  lipo libNative.a -thin ARCH -output libNative.ARCH.a

  # Extract the object files for the offending architecture
  # This will remove the duplicates by overwriting them
  # (since they have the same filename)
  mkdir -p ARCH
  cd ARCH
  ar -x ../libNative.ARCH.a

  # Reassemble the object files in an .a
  ar -r ../libNative.ARCH.a *.o
  cd ..

  # Reassemble the fat library
  lipo *.a -create -output libNative.a
  ```

  - 링커에 사용 하지 않는 코드를 제거 하도록 요청 합니다. 다음 조건이 모두 충족 되 면 xamarin.ios가 자동으로이 작업을 수행 합니다.
    - 모든 타사 바인딩의 `[LinkWith]` 특성에서 smartlink를 사용 하도록 설정 했습니다.

      ```csharp
      [assembly: LinkWith ("libNative.a", SmartLink = true)]
      ```

    - Mtouch (프로젝트의 iOS 빌드 옵션의 추가 mtouch 인수 필드)에 가전달되지않습니다.`-gcc_flags`
    - 프로젝트의 iOS 빌드 옵션에서 추가 mtouch 인수를 추가 `-gcc_flags -dead_strip` 하 여 링커에서 사용 하지 않는 코드를 직접 제거 하도록 요청할 수도 있습니다.

<a name="MT5213" />

### <a name="mt5213-duplicate-symbol-in--location-related-to-previous-error"></a>MT5213: 중복 기호: * (이전 오류와 관련 된 위치)

이 오류는 [MT5212](#MT5212)와 함께 보고 됩니다. 자세한 내용은 [MT5212](#MT5212) 를 참조 하세요.

<a name="MT5214" />

### <a name="mt5214-native-linking-failed-undefined-symbol--this-symbol-was-referenced-the-managed-member--please-verify-that-all-the-necessary-frameworks-have-been-referenced-and-native-libraries-linked"></a>MT5214: 네이티브 링크 실패, 정의 되지 않은 기호: *. 이 기호는 관리 되는 멤버 *를 참조 했습니다. 필요한 모든 프레임 워크가 참조 되었으며 네이티브 라이브러리가 연결 되어 있는지 확인 하세요.

이 오류는 관리 코드에 존재 하지 않는 네이티브 메서드에 대 한 P/Invoke가 포함 되어 있을 때 보고 됩니다. 예:

```csharp
using System.Runtime.InteropServices;
class MyImports {
   [DllImport ("__Internal")]
   public static extern void MyInexistentFunc ();
}
```

몇 가지 가능한 해결 방법이 있습니다.

- 소스 코드에서 질문의 P/Invoke를 제거 합니다.
- 모든 어셈블리에 대해 관리 되는 링커를 사용 하도록 설정 합니다 .이 작업은 프로젝트의 iOS 빌드 옵션에서 "링커 동작"을 "모든 어셈블리"로 설정 하 여 수행 합니다. 이렇게 하면 앱에서 사용 하지 않는 P/Invoke를 모두 효과적으로 제거 합니다 (이전 지점과는 달리 자동으로). 단점은이로 인해 시뮬레이터 빌드 속도가 다소 느려지고 앱이 중단 될 수 있다는 것입니다. [여기](~/ios/deploy-test/linker.md) 에는 링커에 대 한 자세한 정보를 찾을 수 있습니다.
- 누락 된 네이티브 기호가 포함 된 두 번째 네이티브 라이브러리를 만듭니다. 이것은 단지 해결 방법입니다. 해당 함수를 호출 하려고 하면 앱이 충돌 합니다.

<a name="MT5215" />

### <a name="mt5215-references-to--might-require-additional--frameworkxxx-or--lxxx-instructions-to-the-native-linker"></a>MT5215: ' * '에 대 한 참조에는 네이티브 링커에 대 한 추가 프레임 워크 = XXX 또는-lXXX 명령이 필요할 수 있습니다.

이 경고는 해당 라이브러리를 참조 하는 P/Invoke가 검색 되었지만 앱에 연결 되어 있지 않음을 나타내는 경고입니다.

<a name="MT5216" />

### <a name="mt5216-native-linking-failed-for--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5216: *에 대 한 네이티브 연결에 실패 했습니다. 버그 보고서를 파일에 넣으십시오. http://bugzilla.xamarin.com

이 오류는 AOT 컴파일러의 출력을 연결할 때 보고 됩니다.

이 오류는 주로 Xamarin.ios의 버그를 나타냅니다. [Github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요.

<a name="MT5217" />

### <a name="mt5217-native-linking-possibly-failed-because-the-linker-command-line-was-too-long--characters"></a>MT5217: 링커 명령줄이 너무 길어서 (* 자) 네이티브 링크에 오류가 발생 했을 수 있습니다.

네이티브 연결에 실패 했습니다. 링커 명령이 너무 길어서이 오류가 발생할 수 있습니다.

Xamarin.ios 프로젝트는 네이티브 기호를 동적으로 참조 하는 경우가 많습니다. 즉, 네이티브 링커가 이러한 기호를 사용 하는 것을 볼 수 없으므로 네이티브 링커에서 네이티브 연결 프로세스 중에 이러한 네이티브 기호를 제거할 수 있습니다.

일반적으로 xamarin.ios는 `-u symbol` 링커 플래그를 사용 하 여 이러한 기호를 유지 하도록 네이티브 링커에 요청 하지만 이러한 기호가 많은 경우 전체 명령줄이 운영 체제에서 지정한 최대 명령줄 길이를 초과할 수 있습니다.

이러한 동적 기호에 대 한 몇 가지 가능한 소스는 다음과 같습니다.

* 정적으로 연결 된 라이브러리의 메서드에 P/invoke를 호출 합니다 (dll `__Internal` 이름이 DllImport 특성 `[DllImport ("__Internal")]`에 있는 경우).
* 바인딩 프로젝트에서 정적으로 연결 된 라이브러리의 메모리 위치에 대`[Field]` 한 필드 참조 (특성)
* 목표-C 클래스는 바인딩 프로젝트에서 정적으로 연결 된 라이브러리 (증분 빌드를 사용 하는 경우 또는 정적 등록자를 사용 하지 않는 경우)에서 참조 됩니다.

가능한 해결 방법:

* 관리 되는 링커를 사용 하도록 설정 합니다 (SDK 어셈블리만이 아닌 모든 어셈블리의 경우 가능 하면). 이 경우 링커의 명령줄이 최대값을 초과 하지 않도록 동적 기호의 소스를 충분히 제거할 수 있습니다.
* P/Invoke, 필드 참조 및/또는 목표-C 클래스의 수를 줄입니다.
* 더 짧은 이름을 갖도록 동적 기호를 다시 작성 합니다.
* 프로젝트 `-dlsym:false` 의 iOS 빌드 옵션에서 추가 mtouch 인수로 전달 합니다. 이 옵션을 사용 하는 경우 Xamarin.ios는 AOT 컴파일된 코드에서 네이티브 참조를 생성 하며이 기호를 유지 하도록 링커에 요청할 필요가 없습니다. 그러나이는 장치 빌드에만 적용 되며 정적 라이브러리에 존재 하지 않는 함수에 P/Invoke가 있는 경우 링커 오류가 발생 합니다.
* 프로젝트 `--dynamic-symbol-mode=code` 의 iOS 빌드 옵션에서 추가 mtouch 인수로 전달 합니다. 이 옵션을 사용 하는 경우 Xamarin.ios는 명령줄 인수를 사용 하 여 이러한 기호를 유지 하도록 네이티브 링커에 요청 하는 대신 이러한 기호를 참조 하는 추가 네이티브 코드를 생성 합니다. 이 방법의 단점은 실행 파일의 크기가 약간 증가 한다는 것입니다.
* 정적 등록 자가 이미 장치 빌드 `--registrar:static` 에 대 한 기본 이기 때문에 시뮬레이터 빌드에 대 한 프로젝트의 iOS 빌드 옵션에서 추가 mtouch 인수로 전달 하 여 정적 등록자를 사용 하도록 설정 합니다. 정적 등록자는 목표 C 클래스를 정적으로 참조 하는 코드를 생성 하므로 네이티브 링커에서 이러한 클래스를 유지 하도록 요청할 필요가 없습니다.
* 장치 빌드의 경우 증분 빌드를 사용 하지 않도록 설정 합니다. 증분 빌드를 사용 하는 경우 정적 등록자에 의해 생성 된 코드는 네이티브 링커가 고려 하지 않습니다. 즉, Xamarin.ios에서 계속 해 서 참조 된 목표 C 클래스를 유지 하도록 링커에 요청 해야 합니다. 따라서 증분 빌드를 사용 하지 않도록 설정 하면 이러한 필요성을 방지할 수 있습니다.

<a name="MT5218" />

### <a name="mt5218-cant-ignore-the-dynamic-symbol-symbol---ignore-dynamic-symbolsymbol-because-it-was-not-detected-as-a-dynamic-symbol"></a>MT5218: 동적 기호가 검색 되지 않았으므로 동적 기호 {symbol} (--ignore-symbol = {symbol})을 (를) 무시할 수 없습니다.

명령줄 인수가 `--ignore-dynamic-symbol=symbol` 전달 되었지만이 기호가 수동으로 유지 되어야 하는 동적 기호로 인식 된 기호가 아닙니다.

이에 대 한 두 가지 주요 이유는 다음과 같습니다.

* 기호 이름이 잘못 되었습니다.
  * 기호 이름 앞에 밑줄을 추가 하지 않습니다.
  * 객관적인 C 클래스 `OBJC_CLASS_$_<classname>`에 대 한 기호는입니다.
* 기호가 올바르지만 일반적인 방법으로 이미 유지 되 고 있는 기호입니다. 일부 빌드 옵션을 사용 하면 정확한 동적 기호 목록이 달라질 수 있습니다.

### <a name="mt53xx-other-tools"></a>MT53xx: 기타 도구

<!--
  MT53xx other tools
  -->

<a name="MT5301" />

### <a name="mt5301-missing-strip-tool-please-install-xcode-command-line-tools-component"></a>MT5301: ' 스트립 ' 도구가 없습니다. Xcode ' 명령줄 도구 ' 구성 요소를 설치 하세요.

<a name="MT5302" />

### <a name="mt5302-missing-dsymutil-tool-please-install-xcode-command-line-tools-component"></a>MT5302: ' Dsymutil ' 도구가 없습니다. Xcode ' 명령줄 도구 ' 구성 요소를 설치 하세요.

<a name="MT5303" />

### <a name="mt5303-failed-to-generate-the-debug-symbols-dsym-directory-please-review-the-build-log"></a>MT5303: 디버그 기호 (dSYM directory)를 생성 하지 못했습니다. 빌드 로그를 검토 하세요.

디버그 기호를 만들기 위해 final. app 디렉터리에서 dsymutil을 실행 하는 동안 오류가 발생 했습니다. 빌드 로그를 검토 하 여 항목을 수정 하는 방법에 대 한 자세한 내용은 빌드 로그를 검토 하세요.

<a name="MT5304" />

### <a name="mt5304-failed-to-strip-the-final-binary-please-review-the-build-log"></a>MT5304: 최종 이진 파일을 제거 하지 못했습니다. 빌드 로그를 검토 하세요.

응용 프로그램에서 디버깅 정보를 제거 하기 위해 ' 스트립 ' 도구를 실행 하는 동안 오류가 발생 했습니다.

<a name="MT5305" />

### <a name="mt5305-missing-lipo-tool-please-install-xcode-command-line-tools-component"></a>MT5305: ' Lipo ' 도구가 없습니다. Xcode ' 명령줄 도구 ' 구성 요소를 설치 하세요.

<a name="MT5306" />

### <a name="mt5306-failed-to-create-the-a-fat-library-please-review-the-build-log"></a>MT5306: Fat 라이브러리를 만들지 못했습니다. 빌드 로그를 검토 하세요.

' Lipo ' 도구를 실행 하는 동안 오류가 발생 했습니다. 빌드 로그를 검토 하 여 ' lipo '에서 보고 한 오류를 확인 하세요.

<a name="MT5307" />

### <a name="mt5307-failed-to-sign-the-executable-please-review-the-build-log"></a>MT5307: 실행 파일에 서명 하지 못했습니다. 빌드 로그를 검토 하세요.

응용 프로그램에 서명 하는 동안 오류가 발생 했습니다. 빌드 로그를 검토 하 여 ' codesign '에서 보고 된 오류를 확인 하세요.

<!-- 5308 is used by mmp -->
<!-- 5309 is used by mmp -->
<!-- 5310 is used by mmp -->

## <a name="mt6xxx-mtouch-internal-tools-error-messages"></a>MT6xxx: mtouch internal tools 오류 메시지

### <a name="mt600x-stripper"></a>MT600x: Stripper

<!--
 MT6xxx mtouch internal tools
  MT600x Stripper
  -->

<a name="MT6001" />

### <a name="mt6001-running-version-of-cecil-doesnt-support-assembly-stripping"></a>MT6001: 실행 중인 Cecil 버전은 어셈블리 제거를 지원 하지 않습니다.

<a name="MT6002" />

### <a name="mt6002-could-not-strip-assembly-"></a>MT6002: 어셈블리 `*`를 제거할 수 없습니다.

응용 프로그램의 어셈블리에서 관리 코드 (IL 코드 제거)를 제거 하는 동안 오류가 발생 했습니다.

<a name="MT6003" />

### <a name="mt6003-unauthorizedaccessexception-message"></a>MT6003: [System.unauthorizedaccessexception message]

응용 프로그램에서 디버깅 기호를 제거 하는 동안 보안 오류가 발생 했습니다.

## <a name="mt7xxx-msbuild-error-messages"></a>MT7xxx: MSBuild 오류 메시지

<!--
 MT7xxx msbuild errors
  -->

<a name="MT7001" />

### <a name="mt7001-could-not-resolve-host-ips-for-wifi-debugger-settings"></a>MT7001: WiFi 디버거 설정의 호스트 Ip를 확인할 수 없습니다.

*MSBuild 작업: DetectDebugNetworkConfigurationTaskBase*

문제 해결 단계:

- 실행 `csharp -e 'System.Net.Dns.GetHostEntry (System.Net.Dns.GetHostName ()).AddressList'` 을 시도 합니다 (IP 주소를 제공 하 고 오류는 표시 하지 않음).
- 다음과 같은 추가 정보를 \`제공할\`수 있는 "ping 호스트 이름"을 실행 해 봅니다.`cannot resolve MyHost.local: Unknown host`

일부 경우에는 "로컬 네트워크" 문제 이며에서 `127.0.0.1   MyHost.local` `/etc/hosts`알 수 없는 호스트를 추가 하 여 해결할 수 있습니다.

<a name="MT7002" />

### <a name="mt7002-this-machine-does-not-have-any-network-adapters-this-is-required-when-debugging-or-profiling-on-device-over-wifi"></a>MT7002: 이 컴퓨터에 네트워크 어댑터가 없습니다. WiFi를 통해 장치에서 디버깅 또는 프로 파일링 하는 경우에 필요 합니다.

*MSBuild 작업: DetectDebugNetworkConfigurationTaskBase*

<a name="MT7003" />

### <a name="mt7003-the-app-extension--does-not-contain-an-infoplist"></a>MT7003: 앱 확장 ' * '에 info.plist가 포함 되어 있지 않습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7004" />

### <a name="mt7004-the-app-extension--does-not-specify-a-cfbundleidentifier"></a>MT7004: ' * ' 앱 확장은 CFBundleIdentifier를 지정 하지 않습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7005" />

### <a name="mt7005-the-app-extension--does-not-specify-a-cfbundleexecutable"></a>MT7005: ' * ' 앱 확장은 CFBundleExecutable를 지정 하지 않습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7006" />

### <a name="mt7006-the-app-extension--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a>MT7006: 앱 확장 '\*'에 잘못 된 CFBundleIdentifier (\*)가 있습니다. 주 앱 번들의 CFBundleIdentifier (*)로 시작 하지 않습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7007" />

### <a name="mt7007-the-app-extension--has-a-cfbundleidentifier--that-ends-with-the-illegal-suffix-key"></a>MT7007: 앱 확장 '\*'에 잘못 된 접미사 "\*. key"로 끝나는 CFBundleIdentifier ()가 있습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7008" />

### <a name="mt7008-the-app-extension--does-not-specify-a-cfbundleshortversionstring"></a>MT7008: ' * ' 앱 확장은 CFBundleShortVersionString를 지정 하지 않습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7009" />

### <a name="mt7009-the-app-extension--has-an-invalid-infoplist-it-does-not-contain-an-nsextension-dictionary"></a>MT7009: 앱 확장 ' * '의 info.plist에 잘못 된 정보가 있습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7010" />

### <a name="mt7010-the-app-extension--has-an-invalid-infoplist-the-nsextension-dictionary-does-not-contain-an-nsextensionpointidentifier-value"></a>MT7010: 앱 확장 ' * '에 잘못 된 정보가 있습니다. info.plist: NNSExtensionPointIdentifier 장력 사전에는 값이 포함 되어 있지 않습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7011" />

### <a name="mt7011-the-watchkit-extension--has-an-invalid-infoplist-the-nsextension-dictionary-does-not-contain-an-nsextensionattributes-dictionary"></a>MT7011: WatchKit 확장 ' * '에 잘못 된 정보가 있습니다. info.plist: n 장력 사전에 NSExtensionAttributes 사전이 없습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7012" />

### <a name="mt7012-the-watchkit-extension--does-not-have-exactly-one-watch-app"></a>MT7012: WatchKit 확장 ' * '에는 정확히 하나의 watch 앱이 없습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7013" />

### <a name="mt7013-the-watchkit-extension--has-an-invalid-infoplist-uirequireddevicecapabilities-must-contain-the-watch-companion-capability"></a>MT7013: WatchKit 확장 ' * '에 잘못 된 정보가 있습니다. info.plist: UIRequiredDeviceCapabilities에는 ' 조사식 도우미 ' 기능이 포함 되어 있어야 합니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7014" />

### <a name="mt7014-the-watch-app--does-not-contain-an-infoplist"></a>MT7014: Watch 앱 ' * '에 info.plist가 포함 되어 있지 않습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7015" />

### <a name="mt7015-the-watch-app--does-not-specify-a-cfbundleidentifier"></a>MT7015: ' * ' 시청 앱은 CFBundleIdentifier를 지정 하지 않습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7016" />

### <a name="mt7016-the-watch-app--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a>MT7016: Watch 앱 '\*'에 잘못 된 CFBundleIdentifier (\*)가 있습니다. 주 앱 번들의 CFBundleIdentifier (*)로 시작 하지 않습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7017" />

### <a name="mt7017-the-watch-app--does-not-have-a-valid-uidevicefamily-value-expected-watch-4-but-found--"></a>MT7017: Watch 앱 '\*'에 유효한 uidevicefamily 값이 없습니다. ' Watch (4) '가 필요한 데 '\* (*) '이 (가) 있습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7018" />

### <a name="mt7018-the-watch-app--does-not-specify-a-cfbundleexecutable"></a>MT7018: ' * ' 조사식 앱은 CFBundleExecutable를 지정 하지 않습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7019" />

### <a name="mt7019-the-watch-app--has-an-invalid-wkcompanionappbundleidentifier-value--it-does-not-match-the-main-app-bundles-cfbundleidentifier-"></a>MT7019: Watch 앱 '\*'에 잘못 된 WKCompanionAppBundleIdentifier 값 ('\*')이 있습니다. 주 앱 번들의 CFBundleIdentifier (' * ')와 일치 하지 않습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7020" />

### <a name="mt7020-the-watch-app--has-an-invalid-infoplist-the-wkwatchkitapp-key-must-be-present-and-have-a-value-of-true"></a>MT7020: Watch 앱 ' * '에 잘못 된 정보가 있습니다. info.plist: WKWatchKitApp 키가 있어야 하 고 값이 ' t r u e ' 여야 합니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7021" />

### <a name="mt7021-the-watch-app--has-an-invalid-infoplist-the-lsrequiresiphoneos-key-must-not-be-present"></a>MT7021: Watch 앱 ' * '에 잘못 된 정보가 있습니다. info.plist: LSRequiresIPhoneOS 키가 없어야 합니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7022" />

### <a name="mt7022-the-watch-app--does-not-contain-a-watch-extension"></a>MT7022: Watch 앱 ' * '에는 감시 확장이 포함 되어 있지 않습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7023" />

### <a name="mt7023-the-watch-extension--does-not-contain-an-infoplist"></a>MT7023: ' * ' 조사식 확장에 info.plist가 포함 되어 있지 않습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7024" />

### <a name="mt7024-the-watch-extension--does-not-specify-a-cfbundleidentifier"></a>MT7024: ' * ' 조사식 확장은 CFBundleIdentifier를 지정 하지 않습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7025" />

### <a name="mt7025-the-watch-extension--does-not-specify-a-cfbundleexecutable"></a>MT7025: ' * ' 조사식 확장은 CFBundleExecutable를 지정 하지 않습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7026" />

### <a name="mt7026-the-watch-extension--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a>MT7026: '\*' 조사식 확장에 잘못 된 CFBundleIdentifier (\*)가 있습니다. 주 앱 번들의 CFBundleIdentifier (*)로 시작 하지 않습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7027" />

### <a name="mt7027-the-watch-extension--has-a-cfbundleidentifier--that-ends-with-the-illegal-suffix-key"></a>MT7027: 조사식 확장 '\*'에 잘못 된 접미사 "\*. key"로 끝나는 CFBundleIdentifier ()가 있습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7028" />

### <a name="mt7028-the-watch-extension--has-an-invalid-infoplist-it-does-not-contain-an-nsextension-dictionary"></a>MT7028: ' * ' 조사식 확장에 잘못 된 정보가 있습니다. info.plist: NSExtension 사전이 포함 되어 있지 않습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7029" />

### <a name="mt7029-the-watch-extension--has-an-invalid-infoplist-the-nsextensionpointidentifier-must-be-comapplewatchkit"></a>MT7029: ' * ' 조사식 확장에 잘못 된 정보가 있습니다. info.plist: NSExtensionPointIdentifier는 "watchkit" 여야 합니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7030" />

### <a name="mt7030-the-watch-extension--has-an-invalid-infoplist-the-nsextension-dictionary-must-contain-nsextensionattributes"></a>MT7030: ' * ' 조사식 확장에 잘못 된 정보가 있습니다. info.plist: NNSExtensionAttributes 장력 사전은을 포함 해야 합니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7031" />

### <a name="mt7031-the-watch-extension--has-an-invalid-infoplist-the-nsextensionattributes-dictionary-must-contain-a-wkappbundleidentifier"></a>MT7031: ' * ' 조사식 확장에 잘못 된 정보가 있습니다. info.plist: NSExtensionAttributes dictionary에 WKAppBundleIdentifier를 포함 해야 합니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7032" />

### <a name="mt7032-the-watchkit-extension--has-an-invalid-infoplist-uirequireddevicecapabilities-should-not-contain-the-watch-companion-capability"></a>MT7032: WatchKit 확장 ' * '에 잘못 된 정보가 있습니다. info.plist: UIRequiredDeviceCapabilities에는 ' 조사식 도우미 ' 기능이 포함 되어서는 안 됩니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7033" />

### <a name="mt7033-the-watch-app--does-not-contain-an-infoplist"></a>MT7033: Watch 앱 ' * '에 info.plist가 포함 되어 있지 않습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7034" />

### <a name="mt7034-the-watch-app--does-not-specify-a-cfbundleidentifier"></a>MT7034: ' * ' 시청 앱은 CFBundleIdentifier를 지정 하지 않습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7035" />

### <a name="mt7035-the-watch-app--does-not-have-a-valid-uidevicefamily-value-expected--but-found--"></a>MT7035: Watch 앱 '\*'에 유효한 uidevicefamily 값이 없습니다. ' '이 (가)\* 필요한\*데 ' () '가 있습니다.\*

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7036" />

### <a name="mt7036-the-watch-app--does-not-specify-a-cfbundleexecutable"></a>MT7036: ' * ' 시청 앱은 CFBundleExecutable를 지정 하지 않습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7037" />

### <a name="mt7037-the-watchkit-extension-extensionname-has-an-invalid-wkappbundleidentifier-value--it-does-not-match-the-watch-apps-cfbundleidentifier-"></a>MT7037: WatchKit 확장 ' {extensionName} '에 잘못 된 WKAppBundleIdentifier 값 ('\*')이 있습니다. Watch 앱의 CFBundleIdentifier ('\*')와 일치 하지 않습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7038" />

### <a name="mt7038-the-watch-app--has-an-invalid-infoplist-the-wkcompanionappbundleidentifier-must-exist-and-must-match-the-main-app-bundles-cfbundleidentifier"></a>MT7038: Watch 앱 ' * '에 잘못 된 정보가 있습니다. info.plist: WKCompanionAppBundleIdentifier가 있어야 하며 주 앱 번들의 CFBundleIdentifier와 일치 해야 합니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7039" />

### <a name="mt7039-the-watch-app--has-an-invalid-infoplist-the-lsrequiresiphoneos-key-must-not-be-present"></a>MT7039: Watch 앱 ' * '에 잘못 된 정보가 있습니다. info.plist: LSRequiresIPhoneOS 키가 없어야 합니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7040" />

### <a name="mt7040-the-app-bundle-appbundlepath-does-not-contain-an-infoplist"></a>MT7040: 앱 번들 {AppBundlePath}에 info.plist이 포함 되어 있지 않습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7041" />

### <a name="mt7041-main-infoplist-path-does-not-specify-a-cfbundleidentifier"></a>MT7041: Info.plist path는 CFBundleIdentifier를 지정 하지 않습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7042" />

### <a name="mt7042-main-infoplist-path-does-not-specify-a-cfbundleexecutable"></a>MT7042: Info.plist path는 CFBundleExecutable를 지정 하지 않습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7043" />

### <a name="mt7043-main-infoplist-path-does-not-specify-a-cfbundlesupportedplatforms"></a>MT7043: Info.plist path는 CFBundleSupportedPlatforms를 지정 하지 않습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7044" />

### <a name="mt7044-main-infoplist-path-does-not-specify-a-uidevicefamily"></a>MT7044: Info.plist 경로에서 UIDeviceFamily를 지정 하지 않습니다.

*MSBuild 작업: ValidateAppBundleTaskBase*

<a name="MT7045" />

### <a name="mt7045-unrecognized-format-"></a>MT7045: 인식할 수 없는 형식: *.

*MSBuild 작업: PropertyListEditorTaskBase*

여기서 *는 다음과 같습니다.

- string
- 배열
- dict
- bool
- REAL
- integer
- 날짜
- 데이터

<a name="MT7046" />

### <a name="mt7046-add-entry--incorrectly-specified"></a>MT7046: 추가: 항목이 잘못 지정 되었습니다.

*MSBuild 작업: PropertyListEditorTaskBase*

<a name="MT7047" />

### <a name="mt7047-add-entry--contains-invalid-array-index"></a>MT7047: 추가: 항목 *에 잘못 된 배열 인덱스가 있습니다.

*MSBuild 작업: PropertyListEditorTaskBase*

<a name="MT7048" />

### <a name="mt7048-add--entry-already-exists"></a>MT7048: 추가: * 항목이 이미 있습니다.

*MSBuild 작업: PropertyListEditorTaskBase*

<a name="MT7049" />

### <a name="mt7049-add-cant-add-entry--to-parent"></a>MT7049: 추가: 항목을 부모에 추가할 수 없습니다.

*MSBuild 작업: PropertyListEditorTaskBase*

<a name="MT7050" />

### <a name="mt7050-delete-cant-delete-entry--from-parent"></a>MT7050: 삭제: 부모 항목에서 * 항목을 삭제할 수 없습니다.

*MSBuild 작업: PropertyListEditorTaskBase*

<a name="MT7051" />

### <a name="mt7051-delete-entry--contains-invalid-array-index"></a>MT7051: 삭제: 항목 *에 잘못 된 배열 인덱스가 있습니다.

*MSBuild 작업: PropertyListEditorTaskBase*

<a name="MT7052" />

### <a name="mt7052-delete-entry--does-not-exist"></a>MT7052: 삭제: 항목 (*)이 없습니다.

*MSBuild 작업: PropertyListEditorTaskBase*

<a name="MT7053" />

### <a name="mt7053-import-entry--incorrectly-specified"></a>MT7053: 마법사 항목이 잘못 지정 되었습니다.

*MSBuild 작업: PropertyListEditorTaskBase*

<a name="MT7054" />

### <a name="mt7054-import-entry--contains-invalid-array-index"></a>MT7054: 마법사 항목 *에 잘못 된 배열 인덱스가 있습니다.

*MSBuild 작업: PropertyListEditorTaskBase*

<a name="MT7055" />

### <a name="mt7055-import-error-reading-file-"></a>MT7055: 마법사 파일을 읽는 동안 오류가 발생 했습니다. *.

*MSBuild 작업: PropertyListEditorTaskBase*

<a name="MT7056" />

### <a name="mt7056-import-cant-add-entry--to-parent"></a>MT7056: 마법사 항목을 부모에 추가할 수 없습니다.

*MSBuild 작업: PropertyListEditorTaskBase*

<a name="MT7057" />

### <a name="mt7057-merge-cant-add-array-entries-to-dict"></a>MT7057: 결합 Dict에 배열 항목을 추가할 수 없습니다.

*MSBuild 작업: PropertyListEditorTaskBase*

<a name="MT7058" />

### <a name="mt7058-merge-specified-entry-must-be-a-container"></a>MT7058: 결합 지정 된 항목은 컨테이너 여야 합니다.

*MSBuild 작업: PropertyListEditorTaskBase*

<a name="MT7059" />

### <a name="mt7059-merge-entry--contains-invalid-array-index"></a>MT7059: 결합 항목 *에 잘못 된 배열 인덱스가 있습니다.

*MSBuild 작업: PropertyListEditorTaskBase*

<a name="MT7060" />

### <a name="mt7060-merge-entry--does-not-exist"></a>MT7060: 결합 항목 (*)이 없습니다.

*MSBuild 작업: PropertyListEditorTaskBase*

<a name="MT7061" />

### <a name="mt7061-merge-error-reading-file-"></a>MT7061: 결합 파일을 읽는 동안 오류가 발생 했습니다. *.

*MSBuild 작업: PropertyListEditorTaskBase*

<a name="MT7062" />

### <a name="mt7062-set-entry--incorrectly-specified"></a>MT7062: 다음과 같이 설정합니다. 항목이 잘못 지정 되었습니다.

*MSBuild 작업: PropertyListEditorTaskBase*

<a name="MT7063" />

### <a name="mt7063-set-entry--contains-invalid-array-index"></a>MT7063: 다음과 같이 설정합니다. 항목 *에 잘못 된 배열 인덱스가 있습니다.

*MSBuild 작업: PropertyListEditorTaskBase*

<a name="MT7064" />

### <a name="mt7064-set-entry--does-not-exist"></a>MT7064: 다음과 같이 설정합니다. 항목 (*)이 없습니다.

*MSBuild 작업: PropertyListEditorTaskBase*

<a name="MT7065" />

### <a name="mt7065-unknown-propertylist-editor-action-"></a>MT7065: 알 수 없는 PropertyList 편집기 동작: *.

*MSBuild 작업: PropertyListEditorTaskBase*

<a name="MT7066" />

### <a name="mt7066-error-loading--"></a>MT7066: ' * ' 로드 오류: *.

*MSBuild 작업: PropertyListEditorTaskBase*

<a name="MT7067" />

### <a name="mt7067-error-saving--"></a>MT7067: ' * ' 저장 중 오류 발생: *.

*MSBuild 작업: PropertyListEditorTaskBase*

## <a name="mt8xxx-runtime-error-messages"></a>MT8xxx: 런타임 오류 메시지

<!--
 MT8xxx runtime
  MT800x misc
  -->

<a name="MT8001" />

### <a name="mt8001-version-mismatch-between-the-native-xamarinios-runtime-and-monotouchdll-please-reinstall-xamarinios"></a>MT8001: 네이티브 Xamarin.ios 런타임과 monotouch.dialog의 버전이 일치 하지 않습니다. Xamarin.ios를 다시 설치 하세요.

<a name="MT8002" />

### <a name="mt8002-could-not-find-the-method--in-the-type-"></a>MT8002: ' '\*\*형식에서 ' ' 메서드를 찾을 수 없습니다.

<a name="MT8003" />

### <a name="mt8003-failed-to-find-the-closed-generic-method--on-the-type-"></a>MT8003: \*'\*' 형식에서 폐쇄형 제네릭 메서드 ' '을 (를) 찾지 못했습니다.

<a name="MT8004" />

### <a name="mt8004-cannot-create-an-instance-of--for-the-native-object-0x-of-type--because-another-instance-already-exists-for-this-native-object-of-type-"></a>MT8004: 이 네이티브 개체 (* 유형)에 대해 다른 인스턴스가 이미 존재 하기 때문에 네이티브 개체 0x * (' * ' 유형)에 대해 *의 인스턴스를 만들 수 없습니다.

<a name="MT8005" />

### <a name="mt8005-wrapper-type--is-missing-its-native-objectivec-class-"></a>MT8005: 래퍼 형식 '\*'에 네이티브 ObjectiveC 클래스 '\*'이 (가) 없습니다.

<a name="MT8006" />

### <a name="mt8006-failed-to-find-the-selector--on-the-type-"></a>MT8006: \*'\*' 형식에서 선택기 ' '을 (를) 찾지 못했습니다.

<a name="MT8007" />

### <a name="mt8007-cannot-get-the-method-descriptor-for-the-selector--on-the-type--because-the-selector-does-not-correspond-to-a-method"></a>MT8007: 선택기 '\*'이 (가) 메서드에 해당 하지 않으므로 ' '\*형식에 대 한 메서드 설명자를 가져올 수 없습니다.

<a name="MT8008" />

### <a name="mt8008-the-loaded-version-of-xamariniosdll-was-compiled-for--bits-while-the-process-is--bits-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8008: 로드 된 버전의 Xamarin.ios는 * bits에 대해 컴파일 되었지만 프로세스는 * 비트입니다. 버그를 제출 하세요 http://bugzilla.xamarin.com 합니다.

이는 빌드 프로세스에서 문제가 발생 했음을 나타냅니다. [Github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요.

<a name="MT8009" />

### <a name="mt8009-unable-to-locate-the-block-to-delegate-conversion-method-for-the-method-s-parameter--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8009: 메서드에 대 한 변환 메서드를 위임할 블록을 찾을 수 없습니다 *.* ' s 매개 변수 # *. 버그를 제출 하세요 http://bugzilla.xamarin.com 합니다.

이는 API가 올바르게 바인딩되지 않았음을 나타냅니다. Xamarin에서 노출 하는 API 인 경우 [github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요. 타사 바인딩 인 경우 공급 업체에 문의 하세요.

<a name="MT8010" />

### <a name="mt8010-native-type-size-mismatch-between-xamariniosmacdll-and-the-executing-architecture-xamariniosmacdll-was-built-for--bit-while-the-current-process-is--bit"></a>MT8010: Xamarin 간에 네이티브 형식 크기가 일치 하지 않습니다. [iOS | Mac] .dll 및 실행 중인 아키텍처입니다. Xamarin. [iOS | Mac] .dll이 * 비트에 대해 빌드 되었지만 현재 프로세스는 * 비트입니다.

이는 빌드 프로세스에서 문제가 발생 했음을 나타냅니다. [Github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요.

<a name="MT8011" />

### <a name="mt8011-unable-to-locate-the-delegate-to-block-conversion-attribute-delegateproxy-for-the-return-value-for-the-method--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8011: 메서드의 반환 값에 대 한 변환 특성 ([DelegateProxy])을 차단 하는 대리자를 찾을 수 없습니다 *.* 버그를 제출 하세요 http://bugzilla.xamarin.com 합니다.

Xamarin.ios에서 런타임에 필요한 메서드를 찾을 수 없습니다 (대리자를 블록으로 변환).

이는 일반적으로 Xamarin.ios의 버그를 나타냅니다. [Github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요.

<a name="MT8012" />

### <a name="mt8012-invalid-delegateproxyattribute-for-the-return-value-for-the-method--delegatetype-is-null-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8012: 메서드의 반환 값에 대 한 DelegateProxyAttribute이 잘못 되었습니다 *.* DelegateType가 null입니다. 버그를 제출 하세요 http://bugzilla.xamarin.com 합니다.

해당 메서드에 대 한 DelegateProxy 특성이 잘못 되었습니다.

이는 일반적으로 Xamarin.ios의 버그를 나타냅니다. [Github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요.

<a name="MT8013" />

### <a name="mt8013-invalid-delegateproxyattribute-for-the-return-value-for-the-method--delegatetype-2-specifies-a-type-without-a-handler-field-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8013: 메서드의 반환 값에 대 한 DelegateProxyAttribute이 잘못 되었습니다 *.* DelegateType ({2})은 ' Handler ' 필드가 없는 형식을 지정 합니다. 버그를 제출 하세요 http://bugzilla.xamarin.com 합니다.

해당 메서드에 대 한 특성이잘못되었습니다.`[DelegateProxy]`

이는 일반적으로 Xamarin.ios의 버그를 나타냅니다. [Github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요.

<a name="MT8014" />

### <a name="mt8014-invalid-delegateproxyattribute-for-the-return-value-for-the-method--the-delegatetypes-2-handler-field-is-null-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8014: 메서드의 반환 값에 대 한 DelegateProxyAttribute이 잘못 되었습니다 *.* DelegateType의 ({2}) ' Handler ' 필드가 null입니다. 버그를 제출 하세요 http://bugzilla.xamarin.com 합니다.

해당 메서드에 대 한 특성이잘못되었습니다.`[DelegateProxy]`

이는 일반적으로 Xamarin.ios의 버그를 나타냅니다. [Github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요.

<a name="MT8015" />

### <a name="mt8015-invalid-delegateproxyattribute-for-the-return-value-for-the-method--the-delegatetypes-2-handler-field-is-not-a-delegate-its-a--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8015: 메서드의 반환 값에 대 한 DelegateProxyAttribute이 잘못 되었습니다 *.* DelegateType의 ({2}) ' Handler ' 필드는 대리자가 아닙니다. *입니다. 버그를 제출 하세요 http://bugzilla.xamarin.com 합니다.

해당 메서드에 대 한 DelegateProxy 특성이 잘못 되었습니다.

이는 일반적으로 Xamarin.ios의 버그를 나타냅니다. [Github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요.

<a name="MT8016" />

### <a name="mt8016-unable-to-convert-delegate-to-block-for-the-return-value-for-the-method--because-the-input-isnt-a-delegate-its-a--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8016: 대리자를 메서드의 반환 값에 대 한 블록으로 변환할 수 없습니다 *.* 입력이 대리자가 아니기 때문에 *입니다. 버그를 제출 하세요 http://bugzilla.xamarin.com 합니다.

해당 메서드에 대 한 특성이잘못되었습니다.`[DelegateProxy]`

이는 일반적으로 Xamarin.ios의 버그를 나타냅니다. [Github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요.

<!-- 8017 is used by mmp -->

<a name="MT8018" />

### <a name="mt8018-internal-consistency-error-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT8018: 내부 일관성 오류입니다. 버그 보고서를 제출 하세요 http://bugzilla.xamarin.com 합니다.

이는 Xamarin.ios의 버그를 나타냅니다. [Github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요.

<a name="MT8019" />

### <a name="mt8019-could-not-find-the-assembly--in-the-loaded-assemblies"></a>MT8019: 로드 된 어셈블리에서 어셈블리 *를 찾을 수 없습니다.

이는 Xamarin.ios의 버그를 나타냅니다. [Github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요.

<a name="MT8020" />

### <a name="mt8020-could-not-find-the-module-with-metadatatoken--in-the-assembly-"></a>MT8020: 어셈블리 *에서 MetadataToken *를 사용 하 여 모듈을 찾을 수 없습니다.

이는 Xamarin.ios의 버그를 나타냅니다. [Github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요.

<a name="MT8021" />

### <a name="mt8021-unknown-implicit-token-type-"></a>MT8021: 알 수 없는 암시적 토큰 유형: *.

이는 Xamarin.ios의 버그를 나타냅니다. [Github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요.

<a name="MT8022" />

### <a name="mt8022-expected-the-token-reference--to-be-a--but-its-a--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT8022: 토큰 참조 *는 *로 예상 되지만 *입니다. 버그 보고서를 제출 하세요 http://bugzilla.xamarin.com 합니다.

이는 Xamarin.ios의 버그를 나타냅니다. [Github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요.

<a name="MT8023" />

### <a name="mt8023-an-instance-object-is-required-to-construct-a-closed-generic-method-for-the-open-generic-method--token-reference--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT8023: 인스턴스 개체는 개방형 제네릭 메서드: * (토큰 참조: *)에 대해 폐쇄형 제네릭 메서드를 생성 하는 데 필요 합니다. 버그 보고서를 제출 하세요 http://bugzilla.xamarin.com 합니다.

이는 Xamarin.ios의 버그를 나타냅니다. [Github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요.

<a name="MT8024" />

### <a name="mt8024-could-not-find-a-valid-extension-type-for-the-smart-enum-smart_type-please-file-a-bug-at-httpsbugzillaxamarincom"></a>MT8024: 스마트 열거형 ' {smart_type} '에 대 한 올바른 확장 유형을 찾을 수 없습니다. 버그를 제출 하세요 https://bugzilla.xamarin.com 합니다.

이는 Xamarin.ios의 버그를 나타냅니다. [Github](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 해결 하세요.
