---
title: Xamarin.ios 오류 메시지 (mmp)
description: 이 문서에는 컴파일된 어셈블리를 실행 가능 Mac 응용 프로그램으로 패키징하는 데 사용 되는 도구인 mmp에서 생성 된 오류가 나열 되어 있습니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5B26339F-A202-4E41-9229-D0BC9E77868E
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/27/2018
ms.openlocfilehash: 48399d35d27a700fa0b24583cce9cd0335f0e354
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84572080"
---
# <a name="xamarinmac-error-messages-mmp"></a>Xamarin.ios 오류 메시지 (mmp)

## <a name="mm0xxx-mmp-error-messages"></a>MM0xxx: mmp 오류 메시지

예를 들어 매개 변수, 환경, 누락 된 도구입니다.

<a name="MM0000"></a>

#### <a name="mm0000-unexpected-error---please-file-a-bug-report-at-httpsgithubcomxamarinxamarin-maciosissuesnew"></a>MM0000: 예기치 않은 오류입니다 .에서 버그 보고서를 파일에 넣으십시오.https://github.com/xamarin/xamarin-macios/issues/new

예기치 않은 오류 조건이 발생 했습니다. 다음과 같이 가능한 한 많은 정보를 사용 하 여 [버그 보고서를 파일](https://github.com/xamarin/xamarin-macios/issues/new) 에 입력 하세요.

* 최대 세부 정보 표시 (예: `-v -v -v -v` **추가 mmp 인수**)를 사용 하는 전체 빌드 로그
* 오류를 재현 하는 최소 테스트 사례입니다. 하거나
* 모든 버전 정보 롤업

정확한 버전 정보를 얻는 가장 쉬운 방법은 **Xamarin Studio** 메뉴, Xamarin Studio 항목에 **대 한** 자세한 **정보 표시** 단추를 사용 하 고 버전 정보 롤업를 복사/붙여 넣는 것입니다 ( **정보 복사** 단추를 사용할 수 있음).

<a name="MM0001"></a>

#### <a name="mm0001-this-version-of-xamarinmac-requires-mono-0-the-current-mono-version-is-1-please-update-the-monoframework-from-httpmono-projectcomdownloads"></a>MM0001:이 버전의 Xamarin.ios에는 Mono가 필요 {0} 합니다 (현재 mono 버전은 {1} ). Mono 프레임 워크를 업데이트 하십시오.http://mono-project.com/Downloads

<a name="MM0003"></a>

#### <a name="mm0003-application-name-0exe-conflicts-with-an-sdk-or-product-assembly-dll-name"></a>MM0003: 응용 프로그램 이름 ' {0} .exe '는 SDK 또는 제품 어셈블리 (.dll) 이름과 충돌 합니다.

<a name="MM0007"></a>

#### <a name="mm0007-the-root-assembly-0-does-not-exist"></a>MM0007: 루트 어셈블리 ' '이 (가) 없습니다. {0}

<a name="MM0008"></a>

#### <a name="mm0008-you-should-provide-one-root-assembly-only-found-0-assemblies-1"></a>MM0008: 하나의 루트 어셈블리만 제공 해야 합니다 {0} . 어셈블리를 찾았습니다. ' {1} '

<a name="MM0009"></a>

#### <a name="mm0009-error-while-loading-assemblies-"></a>MM0009: 어셈블리를 로드 하는 동안 오류가 발생 했습니다. *.

루트 어셈블리 참조에서 어셈블리를 로드 하는 동안 오류가 발생 했습니다. 빌드 출력에 추가 정보가 제공 될 수 있습니다.

<a name="MM0010"></a>

#### <a name="mm0010-could-not-parse-the-command-line-arguments-0"></a>MM0010: 명령줄 인수를 구문 분석할 수 없습니다.{0}

<!-- 0013 is unused -->

<a name="MM0016"></a>

#### <a name="mm0016-the-option-0-has-been-deprecated"></a>MM0016: ' ' 옵션은 {0} 사용 되지 않습니다.

<a name="MM0017"></a>

#### <a name="mm0017-you-should-provide-a-root-assembly"></a>MM0017: 루트 어셈블리를 제공 해야 합니다.

<a name="MM0018"></a>

#### <a name="mm0018-unknown-command-line-argument-0"></a>MM0018: 알 수 없는 명령줄 인수입니다. ' {0} '

<a name="MM0020"></a>

#### <a name="mm0020-the-valid-options-for-0-are-1"></a>MM0020: ' '에 대 한 올바른 옵션은 {0} ' {1} '입니다.

<a name="MM0023"></a>

#### <a name="mm0023-application-name-0exe-conflicts-with-another-user-assembly"></a>MM0023: 응용 프로그램 이름 ' {0} .exe '이 (가) 다른 사용자 어셈블리와 충돌 합니다.

<a name="MM0026"></a>

#### <a name="mm0026-could-not-parse-the-command-line-argument-0-1"></a>MM0026: 명령줄 인수 ' '을 (를) 구문 분석할 수 없습니다 {0} .{1}

<a name="MM0043"></a>

#### <a name="mm0043-the-boehm-garbage-collector-is-not-supported-the-sgen-garbage-collector-has-been-selected-instead"></a>MM0043: Boehm 가비지 수집기는 지원 되지 않습니다. 대신 SGen 가비지 수집기를 선택 했습니다.

<a name="MM0050"></a>

#### <a name="mm0050-you-cannot-provide-a-root-assembly-if---no-root-assembly-is-passed"></a>MM0050:--root 어셈블리가 전달 되지 않는 경우 루트 어셈블리를 제공할 수 없습니다.

<a name="MM0051"></a>

#### <a name="mm0051-an-output-directory---output-is-required-if---no-root-assembly-is-passed"></a>MM0051:--root 어셈블리가 전달 되지 않는 경우 출력 디렉터리 (--output)가 필요 합니다.

<a name="MM0053"></a>

#### <a name="mm0053-cannot-disable-new-refcount-with-the-unified-api"></a>MM0053: Unified API에서 새 refcount를 비활성화할 수 없습니다.

<a name="MM0056"></a>

#### <a name="mm0056-cannot-find-xcode-in-any-of-our-default-locations-please-install-xcode-or-pass-a-custom-path-using---sdkrootpath"></a>MM0056: 기본 위치에서 Xcode를 찾을 수 없습니다. Xcode를 설치 하거나--sdkroot =를 사용 하 여 사용자 지정 경로를 전달 하세요.\<path>

<a name="MM0059"></a>

#### <a name="mm0059-could-not-find-the-currently-selected-xcode-on-the-system-0"></a>MM0059: 시스템에서 현재 선택 된 Xcode를 찾을 수 {0} 없습니다.

<a name="MM0060"></a>

#### <a name="mm0060-could-not-find-the-currently-selected-xcode-on-the-system-xcode-select---print-path-returned-0-but-that-directory-does-not-exist"></a>MM0060: 시스템에서 현재 선택 된 Xcode를 찾을 수 없습니다. ' xcode '이 (가) ' {0} '을 (를) 반환 했지만 해당 디렉터리가 없습니다.

<a name="MM0068"></a>

#### <a name="mm0068-invalid-value-for-target-framework-0"></a>MM0068: 대상 프레임 워크에 대 한 값이 잘못 {0} 되었습니다.

<a name="MM0071"></a>

#### <a name="mm0071-unknown-platform--this-usually-indicates-a-bug-in-xamarinmac-please-file-a-bug-report-at-httpsbugzillaxamarincom-with-a-test-case"></a>MM0071: 알 수 없는 플랫폼: *. 이는 일반적으로 Xamarin.ios의 버그를 나타냅니다. https://bugzilla.xamarin.com테스트 사례를 사용 하 여에서 버그 보고서를 파일에 입력 하세요.

이는 일반적으로 Xamarin.ios의 버그를 나타냅니다. [https://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=Xamarin.Mac)테스트 사례를 사용 하 여에서 버그 보고서를 파일에 입력 하세요.

<a name="MM0073"></a>

#### <a name="mm0073-xamarinmac--does-not-support-a-deployment-target-of--the-minimum-is--please-select-a-newer-deployment-target-in-your-projects-infoplist"></a>MM0073: Xamarin.ios는 \* 의 배포 대상을 지원 하지 않습니다 \* (최소 \* ). 프로젝트의 info.plist에서 최신 배포 대상을 선택 하세요.

최소 배포 대상은 오류 메시지에 지정 된 대상입니다. 프로젝트의 info.plist에서 최신 배포 대상을 선택 하세요.

배포 대상 업데이트를 수행할 수 없는 경우 이전 버전의 Xamarin.ios를 사용 하세요.

<a name="MM0074"></a>

#### <a name="mm0074-xamarinmac--does-not-support-a-deployment-target-of--the-maximum-is--please-select-an-older-deployment-target-in-your-projects-infoplist-or-upgrade-to-a-newer-version-of-xamarinmac"></a>MM0074: Xamarin.ios는 \* 의 배포 대상을 지원 하지 않습니다 \* (최대값은 \* ). 프로젝트의 info.plist에서 이전 배포 대상을 선택 하거나 새 버전의 Xamarin.ios로 업그레이드 하세요.

Xamarin.ios는이 특정 버전의 Xamarin.ios가 빌드된 버전 보다 높은 버전으로 최소 배포 대상을 설정 하는 것을 지원 하지 않습니다.

프로젝트의 info.plist에서 이전 최소 배포 대상을 선택 하거나 새 버전의 Xamarin.ios로 업그레이드 하세요.

<a name="MM0079"></a>

#### <a name="mm0079-internal-error---no-executable-was-copied-into-the-app-bundle-please-contact-supportxamarincom"></a>MM0079: 내부 오류-앱 번들로 복사 된 실행 파일이 없습니다. ' '에 문의 하세요. support@xamarin.com

<a name="MM0080"></a>

#### <a name="mm0080-disabling-newrefcount---new-refcountfalse-is-deprecated"></a>MM0080: NewRefCount를 비활성화 합니다.--new-refcount: false는 사용 되지 않습니다.

<!-- 0088 used by mtouch -->
<!-- 0089 used by mtouch -->

<a name="MM0091"></a>

#### <a name="mm0091-this-version-of-xamarinmac-requires-the--sdk-shipped-with-xcode--either-upgrade-xcode-to-get-the-required-header-files-or-use-the-dynamic-registrar-or-set-the-managed-linker-behaviour-to-link-platform-or-link-framework-sdks-only-to-try-to-avoid-the-new-apis"></a>MM0091:이 버전의 Xamarin.ios에는 \* SDK (Xcode와 함께 제공 됨 \* )가 필요 합니다. Xcode를 업그레이드 하 여 필요한 헤더 파일을 가져오거나, 동적 등록 기관을 사용 하거나, 관리 되는 링커 동작을 플랫폼에 연결 하거나 프레임 워크 Sdk만 연결 하도록 설정 합니다 (새 Api를 방지 하려는 경우).

Xamarin.ios를 사용 하려면 오류 메시지에 지정 된 SDK 버전의 헤더 파일이 필요 하며, 정적 등록자를 사용 하 여 응용 프로그램을 빌드해야 합니다. 이 오류를 해결 하는 권장 방법은 필요한 SDK를 얻기 위해 Xcode를 업그레이드 하는 것입니다. 이렇게 하면 필요한 모든 헤더 파일이 포함 됩니다. 여러 버전의 Xcode가 설치 되어 있거나 기본이 아닌 위치에서 Xcode를 사용 하려는 경우에는 IDE의 기본 설정에서 올바른 Xcode 위치를 설정 해야 합니다.

한 가지 잠재적인 대안 솔루션은 관리 되는 링커를 사용 하도록 설정 하는 것입니다. 그러면 대부분의 경우 헤더 파일이 누락 되거나 불완전 한 새 API를 포함 하 여 사용 되지 않는 API가 제거 됩니다. 그러나 프로젝트에서 Xcode 제공 하는 것 보다 최신 SDK에 도입 된 API를 사용 하는 경우에는이 작업이 수행 되지 않습니다.

또 다른 잠재적인 대체 솔루션은 대신 동적 등록자를 사용 하는 것입니다. 이렇게 하면 형식을 동적으로 등록 하 고 헤더 파일 요구 사항을 제거 하 여 시작 비용을 부과 합니다.

마지막 straw 솔루션은 프로젝트에 필요한 SDK를 지 원하는 이전 버전의 Xamarin.ios를 사용 하는 것입니다.

<a name="MM0097"></a>

#### <a name="mm0097-machineconfig-file-0-can-not-be-found"></a>MM0097: machine.config 파일 ' {0} '을 (를) 찾을 수 없습니다.

<a name="MM0098"></a>

#### <a name="mm0098-aot-compilation-is-only-available-on-unified"></a>MM0098: AOT 컴파일은 통합 에서만 사용할 수 있습니다.

<a name="MM0099"></a>

#### <a name="mm0099-internal-error-0-please-file-a-bug-report-with-a-test-case-httpsbugzillaxamarincom"></a>MM0099: 내부 오류 {0} 입니다. 테스트 사례 ()를 사용 하 여 버그 보고서를 파일에 입력 하세요 https://bugzilla.xamarin.com) .

<a name="MM0114"></a>

#### <a name="mm0114-hybrid-aot-compilation-requires-all-assemblies-to-be-aot-compiled"></a>MM0114: 하이브리드 AOT 컴파일을 사용 하려면 모든 어셈블리를 AOT 컴파일해야 합니다.

<a name="MM0129"></a>

#### <a name="mm0129-debugging-symbol-file-for--does-not-match-the-assembly-and-is-ignored"></a>MM0129: ' * '에 대 한 디버깅 기호 파일이 어셈블리와 일치 하지 않으므로 무시 됩니다.

지정 된 어셈블리에 대 한 디버깅 기호 (이식 가능한 pdb만 해당) 또는 .mdb 파일을 로드할 수 없습니다.

이는 일반적으로 어셈블리가 기호 보다 최신 이거나 이전 버전 임을 의미 합니다. 일치 하지 않으므로 사용할 수 없으며 기호가 무시 됩니다.

이 경고는 빌드하는 응용 프로그램에 영향을 주지 않지만 완전히 디버깅 하지 못할 수 있습니다 (특히 지정 된 어셈블리의 코드). 또한 예외, 스택 추적 및 충돌 보고서에 일부 정보가 없을 수 있습니다.

이후 릴리스에서이 문제를 해결할 수 있도록 어셈블리 패키지 (예: NuGet 작성자)의 게시자에이 문제를 보고 하세요.

<a name="MM0130"></a>

#### <a name="mm0130-no-root-assemblies-found-you-should-provide-at-least-one-root-assembly"></a>MM0130: 루트 어셈블리를 찾을 수 없습니다. 루트 어셈블리를 하나 이상 제공 해야 합니다.

를 실행 `--runregistrar` 하는 경우 루트 어셈블리를 하나 이상 제공 해야 합니다.

<a name="MM0131"></a>

#### <a name="mm0131-product-assembly-0-not-found-in-assembly-list-1"></a>MM0131: 제품 어셈블리 ' '이 (가) {0} 어셈블리 목록에 없습니다. ' {1} '

`--runregistrar`어셈블리 목록에는 실행할 때 제품 어셈블리 XamMac이 포함 되어야 합니다.

<a name="MM0132"></a>

#### <a name="mm0132-unknown-optimization--valid-values-are-"></a>MM0132: 알 수 없는 최적화: \* . 유효한 값은 다음과 같습니다.\*

지정 된 최적화를 인식할 수 없습니다.

허용 되는 형식은입니다 `[+|-]optimization-name` `optimization-name` . 여기서은 오류 메시지에 나열 된 값 중 하나입니다.

각 최적화에 대 한 전체 설명은 [빌드 최적화](~/cross-platform/macios/optimizations.md) 를 참조 하세요.

<a name="MM0133"></a>

#### <a name="mm0133-found-more-than-1-assembly-matching-0-choosing-first-1"></a>MM0133: ' '을 (를) 선택 하는 두 개 이상의 어셈블리가 있습니다 {0} . {1} ' '

<a name="MM0134"></a>

#### <a name="mm0134-32-bit-applications-should-be-migrated-to-64-bit"></a>MM0134:32 비트 응용 프로그램을 64 비트로 마이그레이션해야 합니다.

Apple은 macOS 앱 스토어에서 32 비트 앱 (2018 년 1 월부터)을 제출할 수 없다는 것을 발표 했습니다.

또한 32 비트 응용 프로그램은 높은 시에라리온 (손상 없음) 후에 macOS 버전에서 실행 되지 않습니다.

자세한 내용은 다음을 참조 하세요.https://developer.apple.com/news/?id=06282017a

응용 프로그램 및 모든 종속성을 64 비트로 업데이트 하는 것이 좋습니다.

<a name="MM0135"></a>

#### <a name="mm0135-did-not-link-system-framework-0-referenced-by-assembly-1-because-it-was-introduced-in-2-3-and-were-using-the-2-4-sdk"></a>MM0135: ' ' {0} 어셈블리에서 참조 {1} 되었으며 {2} {3} SDK를 사용 하 고 있으므로 {2} {4} ' ' 어셈블리에서 참조 하는 ' ' 시스템 프레임 워크를 연결 하지 못했습니다.

응용 프로그램을 빌드하려면 Xamarin.ios는 시스템 라이브러리에 연결 해야 하며, 그 중 일부는 오류 메시지에 지정 된 SDK 버전에 따라 달라 집니다. 이전 버전의 SDK를 사용 하 고 있기 때문에 해당 Api에 대 한 호출이 런타임에 실패할 수 있습니다.

이 오류를 해결 하는 권장 방법은 Xcode를 업그레이드 하 여 필요한 SDK를 가져오는 것입니다. 여러 버전의 Xcode가 설치 되어 있거나 기본이 아닌 위치에서 Xcode를 사용 하려는 경우에는 IDE의 기본 설정에서 올바른 Xcode 위치를 설정 해야 합니다.

또는 관리 되는 [링커가](https://docs.microsoft.com/xamarin/mac/deploy-test/linker) 지정 된 라이브러리를 필요로 하는 새 api (대부분의 경우)를 포함 하 여 사용 하지 않는 api를 제거할 수 있도록 합니다. 그러나 프로젝트에 Xcode에서 제공 하는 것 보다 최신 SDK에 도입 된 Api가 필요한 경우에는이 작업이 수행 되지 않습니다.

마지막으로 straw 솔루션으로, 빌드 프로세스 중에 이러한 새 Sdk를 제공 하지 않아도 되는 이전 버전의 Xamarin.ios를 사용 합니다.

## <a name="mm1xxx-file-copy--symlinks-project-related"></a>MM1xxx: 파일 복사/symlink (프로젝트 관련)

<a name="MM1034"></a>

#### <a name="mm1034-could-not-create-symlink-file---target-error-number"></a>MM1034: symlink ' {file} ' > ' {target} '을 (를) 만들 수 없습니다. 오류 {number}

### <a name="mm14xx-product-assemblies"></a>MM14xx: 제품 어셈블리

<a name="MM1401"></a>

#### <a name="mm1401-the-required-0-assembly-is-missing-from-the-references"></a>MM1401: 필요한 ' {0} ' 어셈블리가 참조에 없습니다.

<a name="MM1402"></a>

#### <a name="mm1402-the-assembly-0-is-not-compatible-with-this-tool"></a>MM1402: ' ' 어셈블리는 {0} 이 도구와 호환 되지 않습니다.

<a name="MM1403"></a>

#### <a name="mm1403-0-1-could-not-be-found-target-framework-0-is-unusable-to-package-the-application"></a>MM1403: {0} ' {1} '을 (를) 찾을 수 없습니다. 대상 프레임 워크 ' '은 (는) {0} 응용 프로그램을 패키징하는 데 사용할 수 없습니다.

<a name="MM1404"></a>

#### <a name="mm1404-target-framework-0-is-invalid"></a>MM1404: 대상 프레임 워크 ' {0} '이 (가) 잘못 되었습니다.

<a name="MM1405"></a>

#### <a name="mm1405-usefullxammacframework-must-always-target-framework-net-45-not-0-which-is-invalid"></a>MM1405: useFullXamMacFramework는 유효 하지 않은 ' '가 아닌 프레임 워크 .NET 4.5을 항상 대상으로 해야 합니다. {0}

<a name="MM1406"></a>

#### <a name="mm1406-target-framework-0-is-invalid-when-targeting-xamarinmac-45-net-framwork"></a>MM1406: {0} xamarin.ios 4.5 .net framwork를 대상으로 지정 하는 경우 대상 프레임 워크 ' '이 (가) 유효 하지 않습니다.

<a name="MM1407"></a>

#### <a name="mm1407-mismatch-between-xamarinmac-reference-0-and-target-framework-selected-1"></a>MM1407: Xamarin.ios 참조 ' {0} '와 대상 프레임 워크가 일치 하지 않습니다. ' '이 (가) 선택 {1} 되었습니다.

### <a name="mm15xx-assembly-gathering-not-requiring-linker-errors"></a>MM15xx: 어셈블리 수집 (링커를 요구 하지 않음) 오류

<a name="MM1501"></a>

#### <a name="mm1501-can-not-resolve-reference-0"></a>MM1501: 참조를 확인할 수 없습니다.{0}

### <a name="machocs"></a>MachO.cs

<a name="MM1600"></a>

#### <a name="mm1600-not-a-mach-o-dynamic-library-unknown-header-0x0-1"></a>MM1600: March-o 바이너리도 동적 라이브러리가 아닙니다. (알 수 없는 헤더 ' 0x {0} '): {1} .

<a name="MM1601"></a>

#### <a name="mm1601-not-a-static-library-unknown-header-0-1"></a>MM1601: 정적 라이브러리가 아닙니다. (알 수 없는 헤더 ' {0} '): {1} .

<a name="MM1602"></a>

#### <a name="mm1602-not-a-mach-o-dynamic-library-unknown-header-0x0-1"></a>MM1602: March-o 바이너리도 동적 라이브러리가 아닙니다. (알 수 없는 헤더 ' 0x {0} '): {1} .

<a name="MM1603"></a>

#### <a name="mm1603-unknown-format-for-fat-entry-at-position-0-in-1"></a>MM1603:의 위치에 있는 fat 항목의 형식을 알 수 없습니다 {0} {1} .

<a name="MM1604"></a>

#### <a name="mm1604-file-of-type-0-is-not-a-macho-file-1"></a>MM1604: 형식의 파일이 {0} MachO 파일이 아닙니다 ( {1} ).

## <a name="mm2xxx-linker"></a>MM2xxx: 링커

### <a name="mm20xx-linker-general-errors"></a>MM20xx: 링커 (일반) 오류

<a name="MM2001"></a>

#### <a name="mm2001-could-not-link-assemblies"></a>MM2001: 어셈블리를 연결할 수 없습니다.

<a name="MM2002"></a>

#### <a name="mm2002-can-not-resolve-reference-0"></a>MM2002: 참조를 확인할 수 없습니다.{0}

<a name="MM2003"></a>

#### <a name="mm2003-option-0-will-be-ignored-since-linking-is-disabled"></a>MM2003: {0} 연결이 사용 하지 않도록 설정 되어 있으므로 ' ' 옵션이 무시 됩니다.

<a name="MM2004"></a>

#### <a name="mm2004-extra-linker-definitions-file-0-could-not-be-located"></a>MM2004: ' ' 추가 링커 정의 파일 {0} 을 찾을 수 없습니다.

<a name="MM2005"></a>

#### <a name="mm2005-definitions-from-0-could-not-be-parsed"></a>MM2005: ' '의 정의를 {0} 구문 분석할 수 없습니다.

<a name="MM2006"></a>

#### <a name="mm2006-native-library-0-was-referenced-but-could-not-be-found"></a>MM2006: 네이티브 라이브러리 ' {0} '이 (가) 참조 되었지만 찾을 수 없습니다.

<a name="MM2007"></a>

#### <a name="mm2007-xamarinmac-unified-api-against-a-full-net-profile-does-not-support-linking-pass-the--nolink-flag"></a>MM2007: 전체 .NET 프로필에 대 한 Xamarin.ios Unified API는 연결을 지원 하지 않습니다. -Nolink 플래그를 전달 합니다.

<a name="MM2009"></a>

#### <a name="mm2009-referenced-by-01------this-message-is-related-to-mm2006-"></a>MM2009:에서 참조 {0} 합니다. {1} \* \* 이 메시지는 MM2006와 관련 되어 있습니다.     \*\*

<a name="MM2010"></a>

#### <a name="mm2010-unknown-httpmessagehandler-0-valid-values-are-httpclienthandler-default-cfnetworkhandler-or-nsurlsessionhandler"></a>MM2010: 알 수 없는 HttpMessageHandler `{0}` 입니다. 유효한 값은 HttpClientHandler (기본값), CFNetworkHandler 또는 NSUrlSessionHandler입니다.

<a name="MM2011"></a>

#### <a name="mm2011-unknown-tlsprovider-0--valid-values-are-default-or-appletls"></a>MM2011: 알 수 없는 TLSProvider ' {0} .  유효한 값은 default 또는 사과 etls입니다.

<a name="MM2012"></a>

#### <a name="mm2012-only-first-0-of-1-referenced-by-warnings-shown--this-message-related-to-2009-"></a>MM2012: {0} {1} "참조 된 사람" 경고만 표시 됩니다. \*\*이 메시지는 2009와 관련 되어 있습니다.\*\*

<a name="MM2013"></a>

#### <a name="mm2013-failed-to-resolve-the-reference-to-0-referenced-in-1-the-app-will-not-include-the-referenced-assembly-and-may-fail-at-runtime"></a>MM2013: ""에서 참조 되는 ""에 대 한 참조를 확인 하지 못했습니다 {0} {1} . 앱은 참조 된 어셈블리를 포함 하지 않으며 런타임에 실패할 수 있습니다.

<a name="MM2014"></a>

#### <a name="mm2014-xamarinmac-extensions-do-not-support-linking-request-for-linking-will-be-ignored--this-message-is-obsolete-in-xm-36-"></a>MM2014: Xamarin.ios 확장은 연결을 지원 하지 않습니다. 연결에 대 한 요청은 무시 됩니다. \*\*이 메시지는 XM 3.6 이상에서 사용 되지 않습니다.\*\*

<!-- 2015 used by mtouch -->

<a name="MM2016"></a>

#### <a name="mm2016-invalid-tlsprovider-0-option-the-only-valid-value-1-will-be-used"></a>MM2016: 잘못 된 TlsProvider `{0}` 옵션입니다. 올바른 값만 `{1}` 사용 됩니다.

<a name="MM2017"></a>

#### <a name="mm2017-could-not-process-xml-description-0"></a>MM2017: XML 설명을 처리할 수 없습니다.{0}

<a name="MM202x"></a>

#### <a name="mm202x-binding-optimizer-failed-processing-"></a>MM202x: 바인딩 최적화 프로그램이 처리 `...` 에 실패 했습니다.

<a name="MM2100"></a>

#### <a name="mm2100-xamarinmac-classic-api-does-not-support-platform-linking"></a>MM2100: Xamarin.ios Classic API는 플랫폼 링크를 지원 하지 않습니다.

<a name="MM2103"></a>

#### <a name="mm2103-error-processing-assembly--"></a>MM2103: 어셈블리 ' '을 (를) 처리 하는 동안 오류 발생 \* : *

어셈블리를 처리 하는 동안 예기치 않은 오류가 발생 했습니다.

오류 메시지에서 문제의 원인이 되는 어셈블리의 이름을 지정 합니다. 이 문제를 해결 하려면 자세한 정보 표시를 사용 하는 전체 빌드 로그 [bug report](https://bugzilla.xamarin.com) (즉, `-v -v -v -v` **추가 mtouch 인수**)와 함께 버그 보고서에 어셈블리를 제공 해야 합니다.

<a name="MM2104"></a>

#### <a name="mm2104-unable-to-link-assembly-0-as-it-is-mixed-mode"></a>MM2104: ' {0} ' 어셈블리는 혼합 모드 이므로 연결할 수 없습니다.

혼합 모드 어셈블리는 링커에서 처리할 수 없습니다.

https://docs.microsoft.com/cpp/dotnet/mixed-native-and-managed-assemblies혼합 모드 어셈블리에 대 한 자세한 내용은을 참조 하세요.

<a name="MM2106"></a>

#### <a name="mm2106-could-not-optimize-the-call-to-blockliteralsetupblockunsafe-in--at-offset--because-"></a>MM2106:의 오프셋에서 BlockLiteral. SetupBlock [Unsafe] 호출을 최적화 하지 못했습니다 \* \* \* .

링커는 또는에 대 한 호출을 최적화할 수 없을 때이 경고를 보고 `BlockLiteral.SetupBlock` `Block.SetupBlockUnsafe` 합니다.

메시지는를 호출 하는 메서드를 가리키고 `BlockLiteral.SetupBlock[Unsafe]` 호출을 최적화할 수 없는 이유에 대 한 단서를 제공할 수도 있습니다.

오류가 발생 한 상황을 조사 하 고 향후 더 많은 시나리오를 가능 하 게 할 수 있도록 전체 빌드 로그와 함께 [문제](https://github.com/xamarin/xamarin-macios/issues/new) 를 파일 하세요.

<a name="MM2107"></a>

#### <a name="mm2107-its-not-safe-to-remove-the-dynamic-registrar-because-reasons"></a>MM2107: {이유} 때문에 동적 등록자를 제거 하는 것은 안전 하지 않습니다.

개발자가 mmp에 전달 하 여 동적 등록자의 제거를 요청 하는 경우 링커에서이 경고를 보고 `--optimize:remove-dynamic-registrar` 하지만 링커가 안전 하지 않은 것으로 확인 합니다.

이 경고를 제거 하려면 mmp에 최적화 인수를 제거 하거나를 전달 `--nowarn:2107` 하 여 무시 합니다.

기본적으로이 옵션은 가능 하 고 안전 하 게 사용할 수 있도록 자동으로 설정 됩니다.

<a name="MM2108"></a>

#### <a name="mm2108-0-was-stripped-of-architectures-except-1-to-comply-with-app-store-restrictions-this-could-break-exisiting-codesigning-signatures-consider-stripping-the-library-with-lipo-or-disabling-with---optimize-trim-architectures"></a>MM2108: ' ' {0} 은 ( {1} 는) 앱 스토어 제한을 준수 하기 위해 ' '를 제외한 아키텍처를 제거 했습니다. 이렇게 하면 기존 코드 서명 서명이 손상 될 수 있습니다. Lipo로 라이브러리를 제거 하거나--optimize =-trim 아키텍처를 사용 하 여 비활성화 하는 것이 좋습니다.

이제 앱 스토어는 32 비트 변형이 포함 된 라이브러리 및 프레임 워크를 포함 하는 응용 프로그램을 거부 합니다. 라이브러리는 최종 응용 프로그램 번들로 복사 될 때 사용 되지 않는 아키텍처를 제거 했습니다.

이는 일반적으로 안전 하며 응용 프로그램 번들 크기를 추가 혜택으로 줄입니다. 그러나 코드 서명 된 모든 번들 프레임 워크의 시그니처는 무효화 됩니다 (응용 프로그램이 서명 된 경우 나중에 다시 서명).

를 사용 하 여 `lipo` 필요 없는 아키텍처를 원본 라이브러리에서 영구적으로 제거 하는 것이 좋습니다. 응용 프로그램을 앱 스토어에 게시 하지 않는 경우 추가 MMP 인수로 전달 하 여이 제거를 비활성화할 수 있습니다 `--optimize=-trim-architectures` .

<a name="MM2109"></a>

#### <a name="mm2109-xamarinmac-classic-api-does-not-support-platform-linking"></a>MM2109: Xamarin.ios Classic API는 플랫폼 링크를 지원 하지 않습니다.

## <a name="mm3xxx-aot"></a>MM3xxx: AOT

### <a name="mm30xx-aot-general-errors"></a>MM30xx: AOT (일반) 오류

<a name="MM3001"></a>

#### <a name="mm3001-could-not-aot-the-assembly-0"></a>MM3001: ' ' 어셈블리를 AOT 수 없습니다. {0}

<!-- 3002 used by mtouch -->
<!-- 3003 used by mtouch -->
<!-- 3004 used by mtouch -->
<!-- 3005 used by mtouch -->
<!-- 3006 used by mtouch -->
<!-- 3007 used by mtouch -->
<!-- 3008 used by mtouch -->

<a name="MM3009"></a>

#### <a name="mm3009-aot-of-0-was-requested-but-was-not-found"></a>MM3009: AOT ' '이 (가) {0} 요청 되었지만 찾을 수 없습니다.

<a name="MM3010"></a>

#### <a name="mm3010-exclusion-of-aot-of-0-was-requested-but-was-not-found"></a>MM3010: AOT ' '의 제외를 {0} 요청 했지만 찾을 수 없습니다.

## <a name="mm4xxx-code-generation"></a>MM4xxx: 코드 생성

### <a name="mm40xx-driverm"></a>MM40xx: driver. m

<a name="MM4001"></a>

#### <a name="mm4001-the-main-template-could-not-be-expanded-to-0"></a>MM4001: 주 템플릿을으로 확장할 수 없습니다 `{0}` .

### <a name="mm41xx-registrar"></a>MM41xx: 등록자

<a name="MM4134"></a>

#### <a name="mm4134-your-application-is-using-the-0-framework-which-isnt-included-in-the-macos-sdk-youre-using-to-build-your-app-this-framework-was-introduced-in-osx-2-while-youre-building-with-the-macos-1-sdk-this-configuration-is-not-supported-with-the-static-registrar-pass---registrardynamic-as-an-additional-mmp-argument-in-your-projects-mac-build-option-to-select-alternatively-select-a-newer-sdk-in-your-apps-mac-build-options"></a>MM4134: 응용 프로그램에서 응용 프로그램 {0} 을 빌드하는 데 사용 하는 MacOS sdk에 포함 되지 않은 ' ' 프레임 워크를 사용 하 고 있습니다 .이 프레임 워크는 {2} macos sdk를 사용 하 여 빌드하는 동안 OSX에 도입 되었습니다 {1} . 이 구성은 정적 등록자 (프로젝트의 Mac 빌드 옵션에서 선택할 수 있는 추가 mmp 인수로 사용 됨)에서 지원 되지 않습니다. 또는 앱의 Mac 빌드 옵션에서 최신 SDK를 선택 합니다.

<a name="MM4173"></a>

#### <a name="mm4173-the-registrar-cant-compute-the-block-signature-for-the-delegate-of-type-delegate-type-in-the-method-method-because-"></a>MM4173: 등록자는 * 때문에 {method} 메서드에서 {delegate type} 형식의 대리자에 대 한 블록 서명을 계산할 수 없습니다.

이 경고는 등록 자가 계산을 계산할 수 없기 때문에 등록 자가 지정 된 메서드의 블록 서명을 생성 된 등록자 코드에 삽입할 수 없음을 나타내는 경고입니다.

즉, 블록 시그니처는 런타임에 계산 되어야 하며,이는 다소 느립니다.

현재이 경고에는 두 가지 가능한 이유가 있습니다.

1. 관리 되는 대리자의 형식은 `System.Delegate` 또는 `System.MulticastDelegate` 입니다. 이러한 형식은 특정 서명을 표시 하지 않습니다. 즉, 등록 자가 해당 네이티브 서명을 계산할 수 없습니다. 이 경우 해결 방법은 블록에 특정 대리자 형식을 사용 하는 것입니다 `--nowarn:4173` . 또는 프로젝트의 Mac 빌드 옵션에 추가 mmp 인수를 추가 하 여 경고를 무시할 수 있습니다.
2. 등록 기관에서 대리자의 메서드를 찾을 수 없습니다 `Invoke` . 이는 발생 하지 않으므로 문제를 해결할 수 있도록 테스트 프로젝트와 관련 된 [문제](https://github.com/xamarin/xamarin-macios/issues/new) 를 해결 하세요.

<a name="MT4174"></a>

#### <a name="mt4174-unable-to-locate-the-block-to-delegate-conversion-method-for-the-method-methods-parameter-parameter"></a>MT4174: 메서드 {method}의 매개 변수 # {parameter}에 대 한 변환 메서드를 위임 하는 블록을 찾을 수 없습니다.

이 경고는 정적 등록 기관에서 목표-C 블록에 대 한 대리자를 만드는 메서드를 찾을 수 없음을 나타내는 경고입니다. 런타임에 메서드를 찾으려고 시도 하지만 MT8009 예외를 사용 하 여 실패할 수도 있습니다.

이 경고가 발생 하는 한 가지 가능한 원인은 블록을 사용 하는 API에 대 한 바인딩을 수동으로 작성 하는 것입니다. 바인딩 프로젝트를 사용 하 여 목표-C 코드를 바인딩하는 것이 좋습니다. 특히 블록을 사용 하는 경우에는 해당 코드를 수동으로 수행 하는 것이 매우 복잡 하기 때문입니다.

그렇지 않은 경우 테스트 사례를 사용 하 여 [문제를 해결](https://github.com/xamarin/xamarin-macios/issues/new) 하십시오.

## <a name="mm5xxx-gcc-and-toolchain"></a>MM5xxx: GCC 및 도구 체인

### <a name="mm51xx-compilation"></a>MM51xx: 컴파일

<a name="MM5101"></a>

#### <a name="mm5101-missing-0-compiler-please-install-xcode-command-line-tools-component"></a>MM5101: ' {0} ' 컴파일러가 없습니다. Xcode ' 명령줄 도구 ' 구성 요소를 설치 하세요.

<!-- 5102 used by mtouch -->

<a name="MM5103"></a>

#### <a name="mm5103-failed-to-compile-error-code---0-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MM5103: 컴파일하지 못했습니다. 오류 코드- {0} . 버그 보고서를 파일에 넣으십시오.http://bugzilla.xamarin.com

<!-- 5104 used by mtouch -->

### <a name="mm52xx-linking"></a>MM52xx: 연결

<a name="MM5202"></a>

#### <a name="mm5202-monoframework-mdk-is-missing-please-install-the-mdk-for-your-monoframework-version-from-httpmono-projectcomdownloads"></a>MM5202: Mono. 프레임 워크 MDK가 없습니다. 다음에서 Mono 프레임 워크 버전에 대 한 MDK을 설치 하세요.http://mono-project.com/Downloads

<a name="MM5203"></a>

#### <a name="mm5203-cant-find-libxammaca-likely-because-of-a-corrupted-xamarinmac-installation-please-reinstall-xamarinmac"></a>MM5203: libxammac를 찾을 수 없습니다. Xamarin.ios가 손상 되었기 때문일 수 있습니다. Xamarin.ios를 다시 설치 하세요.

<a name="MM5204"></a>

#### <a name="mm5204-invalid-architecture-x86_64-is-only-supported-on-non-classic-profiles"></a>MM5204: 아키텍처가 잘못 되었습니다. x86_64은 클래식이 아닌 프로필 에서만 지원 됩니다.

<a name="MM5205"></a>

#### <a name="mm5205-invalid-architecture-0-valid-architectures-are-i386-and-x86_64-when---profilemobile"></a>MM5205: ' ' 아키텍처가 잘못 되었습니다 {0} . 유효한 아키텍처는 i386 및 x86_64 (--profile = mobile 인 경우)입니다.

<a name="MM5218"></a>

#### <a name="mm5218-cant-ignore-the-dynamic-symbol-symbol---ignore-dynamic-symbolsymbol-because-it-was-not-detected-as-a-dynamic-symbol"></a>MM5218: 동적 기호로 검색 되지 않았으므로 동적 기호 {symbol} (--ignore-symbol = {symbol})을 (를) 무시할 수 없습니다.

[해당 mtouch 경고](~/ios/troubleshooting/mtouch-errors.md#MT5218)를 참조 하세요.

<!-- 5206 used by mtouch -->
<!-- 5207 used by mtouch -->
<!-- 5208 used by mtouch -->
<!-- 5209 used by mtouch -->
<!-- 5210 used by mtouch -->
<!-- 5211 used by mtouch -->
<!-- 5212 used by mtouch -->
<!-- 5213 used by mtouch -->
<!-- 5214 used by mtouch -->
<!-- 5215 used by mtouch -->
<!-- 5216 used by mtouch -->
<!-- 5217 used by mtouch -->

### <a name="mm53xx-other-tools"></a>MM53xx: 기타 도구

<a name="MM5301"></a>

#### <a name="mm5301-pkg-config-could-not-be-found-please-install-the-monoframework-from-httpmono-projectcomdownloads"></a>MM5301: pkg를 찾을 수 없습니다. 다음에서 Mono 프레임 워크를 설치 하세요.http://mono-project.com/Downloads

<!-- 5302 used by mtouch -->
<!-- 5303 used by mtouch -->
<!-- 5304 used by mtouch -->

<a name="MM5305"></a>

#### <a name="mm5305-missing-otool-tool-please-install-xcode-command-line-tools-component"></a>MM5305: ' otool ' 도구가 없습니다. Xcode ' 명령줄 도구 ' 구성 요소를 설치 하세요.

<a name="MM5306"></a>

#### <a name="mm5306-missing-dependencies-please-install-xcode-command-line-tools-component"></a>MM5306: 종속성이 누락 되었습니다. Xcode ' 명령줄 도구 ' 구성 요소를 설치 하세요.

<a name="MM5308"></a>

#### <a name="mm5308-xcode-license-agreement-may-not-have-been-accepted--please-launch-xcode"></a>MM5308: Xcode 사용권 계약이 수락 되지 않았을 수 있습니다.  Xcode를 시작 하십시오.

<a name="MM5309"></a>

#### <a name="mm5309-native-linking-failed-with-error-code-1-check-build-log-for-details"></a>MM5309: 네이티브를 연결 하지 못했습니다 (오류 코드 1). 자세한 내용은 빌드 로그를 확인 하세요.

<a name="MM5310"></a>

#### <a name="mm5310-install_name_tool-failed-with-an-error-code-0-check-build-log-for-details"></a>MM5310: install_name_tool 실패 했습니다. 오류 코드는 ' {0} '입니다. 자세한 내용은 빌드 로그를 확인 하세요.

<a name="MM5311"></a>

#### <a name="mm5311-lipo-failed-with-an-error-code-0-check-build-log-for-details"></a>MM5311: lipo가 오류 코드 ' ' (으)로 인해 실패 했습니다 {0} . 자세한 내용은 빌드 로그를 확인 하세요.

<!-- MM6xxx: mmp internal tools -->
<!-- MM7xxx: reserved -->

## <a name="mm8xxx-runtime"></a>MM8xxx: runtime

### <a name="mm800x-misc"></a>MM800x: 기타

<!-- 8000 used by mtouch -->
<!-- 8001 used by mtouch -->
<!-- 8002 used by mtouch -->
<!-- 8003 used by mtouch -->
<!-- 8004 used by mtouch -->
<!-- 8005 used by mtouch -->
<!-- 8006 used by mtouch -->
<!-- 8007 used by mtouch -->
<!-- 8008 used by mtouch -->
<!-- 8009 used by mtouch -->
<!-- 8010 used by mtouch -->
<!-- 8011 used by mtouch -->
<!-- 8012 used by mtouch -->
<!-- 8013 used by mtouch -->
<!-- 8014 used by mtouch -->
<!-- 8015 used by mtouch -->
<!-- 8016 used by mtouch -->

<a name="MM8017"></a>

#### <a name="mm8017-the-boehm-garbage-collector-is-not-supported-please-use-sgen-instead"></a>MM8017: Boehm 가비지 수집기는 지원 되지 않습니다. 대신 SGen를 사용 하세요.

<a name="MM8025"></a>

#### <a name="mm8025-failed-to-compute-the-token-reference-for-the-type-typeassemblyqualifiedname-because-reasons"></a>MM8025: ' {type 형식에 대 한 토큰 참조를 계산 하지 못했습니다. {AssemblyQualifiedName} ' 이유}

이는 Xamarin.ios의 버그를 나타냅니다. 에서 버그를 파일로 신고 하세요 [https://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=Xamarin.Mac) .

잠재적 해결 방법은 `register-protocols` `--optimize:-register-protocols` 프로젝트의 Mac 빌드 옵션에서 추가 mmp 인수로 전달 하 여 최적화를 사용 하지 않도록 설정 하는 것입니다.

<a name="MM8026"></a>

#### <a name="mm8026--is-not-supported-when-the-dynamic-registrar-has-been-linked-away"></a>MM8026: *는 동적 등록 기관이 연결 된 경우에는 지원 되지 않습니다.

이는 일반적으로 Xamarin.ios의 버그를 나타냅니다. 필요한 경우 동적 등록자를 연결 해서는 안 됩니다. 에서 버그를 파일로 신고 하세요 [https://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) .

`--optimize=-remove-dynamic-registrar`프로젝트의 Mac 빌드 옵션에서 추가 mmp 인수를 추가 하 여 링커에서 동적 등록자를 유지 하도록 강제할 수 있습니다.
