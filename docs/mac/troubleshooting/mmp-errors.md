---
title: Xamarin.Mac 오류 메시지 (mmp)
description: 이 문서에서는 mmp, 실행 가능한 Mac 응용 프로그램에 컴파일된 어셈블리를 패키지 하는 데 사용 하는 도구에서 생성 된 오류를 나열 합니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5B26339F-A202-4E41-9229-D0BC9E77868E
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/27/2018
ms.openlocfilehash: 640d1adc048bec167508d8c288b62d498f061b0d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61233204"
---
# <a name="xamarinmac-error-messages-mmp"></a>Xamarin.Mac 오류 메시지 (mmp)

## <a name="mm0xxx-mmp-error-messages"></a>MM0xxx: mmp 오류 메시지

예: 매개 변수, 환경, 도구 누락입니다.

<a name="MM0000" />

#### <a name="mm0000-unexpected-error---please-file-a-bug-report-at-httpsgithubcomxamarinxamarin-maciosissuesnew"></a>MM0000: 예기치 않은 오류-파일 버그를 보고 하십시오에서 https://github.com/xamarin/xamarin-macios/issues/new

예기치 않은 오류가 발생 했습니다. 하세요 [버그 보고서](https://github.com/xamarin/xamarin-macios/issues/new) 최대한 많은 정보를 포함 합니다.

* 전체 최대의 자세한 정도 사용 하 여 로그를 빌드합니다 (예: `-v -v -v -v` 에 **추가 mmp 인수**);
* 오류를 재현 하는 최소한의 테스트 사례 및
* 모든 버전 정보

정확한 버전 정보를 얻을 수는 가장 쉬운 방법은 사용 하는 것을 **Xamarin Studio** 메뉴에서 **Xamarin Studio에 대 한** 항목인 **자세한 정보 표시** 단추 및 버전에 대 한 복사/붙여넣기 정보 (사용할 수는 **복사 정보** 단추).

<a name="MM0001" />

#### <a name="mm0001-this-version-of-xamarinmac-requires-mono-0-the-current-mono-version-is-1-please-update-the-monoframework-from-httpmono-projectcomdownloads"></a>MM0001: Xamarin.mac이 버전에 Mono {0} (현재 Mono 버전은 {1}). Mono.framework를 업데이트 하세요. http://mono-project.com/Downloads

<a name="MM0003" />

#### <a name="mm0003-application-name-0exe-conflicts-with-an-sdk-or-product-assembly-dll-name"></a>MM0003: 응용 프로그램 이름 '{0}.exe' SDK 또는 제품 어셈블리 (.dll) 이름과 충돌 합니다.

<a name="MM0007" />

#### <a name="mm0007-the-root-assembly-0-does-not-exist"></a>MM0007: 루트 어셈블리 '{0}' 존재 하지 않는

<a name="MM0008" />

#### <a name="mm0008-you-should-provide-one-root-assembly-only-found-0-assemblies-1"></a>MM0008: 에 있는 하나의 루트 어셈블리를 제공 해야 {0} 어셈블리: '{1}'

<a name="MM0010" />

#### <a name="mm0010-could-not-parse-the-command-line-arguments-0"></a>MM0010: 명령줄 인수를 구문 분석 하지 못했습니다. {0}

<!-- 0013 is unused -->

<a name="MM0016" />

#### <a name="mm0016-the-option-0-has-been-deprecated"></a>MM0016: 옵션 '{0}'가 사용 되지 않습니다.

<a name="MM0017" />

#### <a name="mm0017-you-should-provide-a-root-assembly"></a>MM0017: 루트 어셈블리를 제공 해야

<a name="MM0018" />

#### <a name="mm0018-unknown-command-line-argument-0"></a>MM0018: 알 수 없는 명령줄 인수: '{0}'

<a name="MM0020" />

#### <a name="mm0020-the-valid-options-for-0-are-1"></a>MM0020: 유효한 옵션에 대 한 '{0}'는'{1}'.

<a name="MM0023" />

#### <a name="mm0023-application-name-0exe-conflicts-with-another-user-assembly"></a>MM0023: 응용 프로그램 이름 '{0}.exe' 다른 사용자 어셈블리와 충돌 합니다.

<a name="MM0026" />

#### <a name="mm0026-could-not-parse-the-command-line-argument-0-1"></a>MM0026: 명령줄 인수 구문 분석 하지 못했습니다 '{0}'. {1}

<a name="MM0043" />

#### <a name="mm0043-the-boehm-garbage-collector-is-not-supported-the-sgen-garbage-collector-has-been-selected-instead"></a>MM0043: Boehm 가비지 수집기가 지원 되지 않습니다. SGen 가비지 수집기가 대신 선정 되었습니다.

<a name="MM0050" />

#### <a name="mm0050-you-cannot-provide-a-root-assembly-if---no-root-assembly-is-passed"></a>MM0050: 아니요-루트-어셈블리에 전달 되는 경우-루트 어셈블리를 제공할 수 없습니다.

<a name="MM0051" />

#### <a name="mm0051-an-output-directory---output-is-required-if---no-root-assembly-is-passed"></a>MM0051: 출력 디렉터리 (-출력) 필요한 경우-없음-루트-어셈블리에서 전달 됩니다.

<a name="MM0053" />

#### <a name="mm0053-cannot-disable-new-refcount-with-the-unified-api"></a>MM0053: 새 refcount 통합 API를 사용 하 여 비활성화할 수 없습니다.

<a name="MM0056" />

#### <a name="mm0056-cannot-find-xcode-in-any-of-our-default-locations-please-install-xcode-or-pass-a-custom-path-using---sdkrootpath"></a>MM0056: 이 기본 위치에서 Xcode를 찾을 수 없습니다. --Sdkroot를 사용 하 여 사용자 지정 경로 전달 하거나 Xcode를 설치 하십시오 =<path>

<a name="MM0059" />

#### <a name="mm0059-could-not-find-the-currently-selected-xcode-on-the-system-0"></a>MM0059: 시스템에서 현재 선택한 Xcode를 찾을 수 없습니다: {0};

<a name="MM0060" />

#### <a name="mm0060-could-not-find-the-currently-selected-xcode-on-the-system-xcode-select---print-path-returned-0-but-that-directory-does-not-exist"></a>MM0060: 시스템에서 현재 선택한 Xcode를 찾을 수 없습니다. ' xcode-select--인쇄-path' 반환 '{0}' 있지만, 해당 디렉터리가 존재 하지 않습니다.

<a name="MM0068" />

#### <a name="mm0068-invalid-value-for-target-framework-0"></a>MM0068: 대상 프레임 워크에 대 한 값이 잘못 되었습니다: {0}합니다.

<a name="MM0071" />

#### <a name="mm0071-unknown-platform--this-usually-indicates-a-bug-in-xamarinmac-please-file-a-bug-report-at-httpsbugzillaxamarincom-with-a-test-case"></a>MM0071: 알 수 없는 플랫폼: *. 이것은 보통 Xamarin.Mac;에 버그를 나타냅니다. 버그 보고서를 제출 하세요 https://bugzilla.xamarin.com 테스트 사례를 사용 하 여 합니다.

이것은 보통 Xamarin.Mac;에 버그를 나타냅니다. 버그 보고서를 제출 하세요 [ https://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=Xamarin.Mac) 테스트 사례를 사용 하 여 합니다.

<a name="MM0079" />

#### <a name="mm0079-internal-error---no-executable-was-copied-into-the-app-bundle-please-contact-supportxamarincom"></a>MM0079: 내부 오류-실행 파일이 없는 앱 번들에 복사 되었습니다. 문의 'support@xamarin.com'

<a name="MM0080" />

#### <a name="mm0080-disabling-newrefcount---new-refcountfalse-is-deprecated"></a>MM0080: NewRefCount를 사용 하지 않도록 설정,--새로 만들기-refcount:false, 사용 되지 않습니다.

<!-- 0088 used by mtouch -->
<!-- 0089 used by mtouch -->

<a name="MM0091" />

#### <a name="mm0091-this-version-of-xamarinmac-requires-the--sdk-shipped-with-xcode--either-upgrade-xcode-to-get-the-required-header-files-or-use-the-dynamic-registrar-or-set-the-managed-linker-behaviour-to-link-platform-or-link-framework-sdks-only-to-try-to-avoid-the-new-apis"></a>MM0091: Xamarin.mac이 버전에는 * SDK (Xcode와 함께 제공 *). 하거나 필요한 헤더 파일을 가져오고, 동적 등록 기관을 사용 하거나 플랫폼 연결 또는 링크 프레임 워크 Sdk만 (하려고 새 api)로 관리 되는 링커 동작을 설정 하는 Xcode를 업그레이드 합니다.

Xamarin.Mac 정적 등록자를 사용 하 여 응용 프로그램을 빌드하는 오류 메시지에서 지정 된 SDK 버전에서 헤더 파일에 필요 합니다. 이 오류를 수정 하는 권장된 방법은 필요한 SDK 받기 위해 Xcode를 업그레이드 하는 것, 여기에 모든 필수 헤더 파일이 포함 됩니다. 여러 버전의 Xcode 설치 했거나 기본이 아닌 위치에는 Xcode를 사용 하려는 경우에 IDE의 기본 설정에서 올바른 Xcode 위치를 설정 해야 합니다.

하나의 잠재적인, 대체 솔루션을 관리 되는 링커를 사용 하도록 설정 하는 것입니다. 사용 되지 않는 API 등 대부분의 경우, 누락 된 (또는 불완전) 헤더 파일이 있는 새 API 제거 합니다. 그러나가 제공이 프로젝트에 Xcode 1 보다 최신 SDK에 도입 된 API를 사용 하는 경우를 작동 하지 않습니다.

둘째 잠재적인, 대체 솔루션을 동적 등록 기관을 대신 사용 됩니다. 이 동적으로 형식을 등록 하 여 시작 비용을 부과 하지만 헤더 파일 요구 사항을 제거. 

몰 랐 하 어 요 솔루션 Xamarin.Mac의 이전 버전을 사용 하는 것을 프로젝트에 SDK를 지 원하는 하나 필요 합니다.

<a name="MM0097" />

#### <a name="mm0097-machineconfig-file-0-can-not-be-found"></a>MM0097: machine.config 파일 '{0}' 찾을 수 없습니다.

<a name="MM0098" />

#### <a name="mm0098-aot-compilation-is-only-available-on-unified"></a>MM0098: AOT 컴파일만 통합에서 제공 됩니다.

<a name="MM0099" />

#### <a name="mm0099-internal-error-0-please-file-a-bug-report-with-a-test-case-httpbugzillaxamarincom"></a>MM0099: 내부 오류 {0}합니다. 테스트 사례를 사용 하 여 버그 보고서를 제출 하세요 (http://bugzilla.xamarin.com)합니다.

<a name="MM0114" />

#### <a name="mm0114-hybrid-aot-compilation-requires-all-assemblies-to-be-aot-compiled"></a>MM0114: 하이브리드 AOT 컴파일 aot 컴파일되어야 하는 모든 어셈블리에 필요 합니다.

## <a name="mm1xxx-file-copy--symlinks-project-related"></a>MM1xxx: 파일을 복사 / symlink (관련 프로젝트)

<a name="MM1034" />

#### <a name="mm1034-could-not-create-symlink-file---target-error-number"></a>MM1034: Symlink를 만들 수 없습니다 '{대상}'-> '{file}': 오류 {number 개}

### <a name="mm14xx-product-assemblies"></a>MM14xx: 어셈블리 제품

<a name="MM1401" />

#### <a name="mm1401-the-required-0-assembly-is-missing-from-the-references"></a>MM1401: 필수 '{0}' 어셈블리 참조에서 누락 되었습니다.

<a name="MM1402" />

#### <a name="mm1402-the-assembly-0-is-not-compatible-with-this-tool"></a>MM1402: 어셈블리 '{0}'이 도구와 호환 되지 않습니다

<a name="MM1403" />

#### <a name="mm1403-0-1-could-not-be-found-target-framework-0-is-unusable-to-package-the-application"></a>: MM1403 {0} '{1}' 찾을 수 없습니다. 대상 프레임 워크 '{0}' 응용 프로그램 패키지를 사용할 수 없습니다.

<a name="MM1404" />

#### <a name="mm1404-target-framework-0-is-invalid"></a>MM1404: 대상 프레임 워크 '{0}' 올바르지 않습니다.

<a name="MM1405" />

#### <a name="mm1405-usefullxammacframework-must-always-target-framework-net-45-not-0-which-is-invalid"></a>MM1405: useFullXamMacFramework 해야 항상 대상.NET 4.5 framework '{0}'는 유효 하지 않습니다

<a name="MM1406" />

#### <a name="mm1406-target-framework-0-is-invalid-when-targetting-xamarinmac-45-net-framwork"></a>MM1406: 대상 프레임 워크 '{0}' 올바르지 때 대상으로 Xamarin.Mac 4.5.NET 프레임 워크입니다.

<a name="MM1407" />

#### <a name="mm1407-mismatch-between-xamarinmac-reference-0-and-target-framework-selected-1"></a>MM1407: Xamarin.Mac 참조 간에 불일치가 있습니다. '{0}'및 대상 프레임 워크 선택'{1}'.

### <a name="mm15xx-assembly-gathering-not-requiring-linker-errors"></a>MM15xx: 어셈블리 (링커 요구 하지 않는) 수집 하는 중 오류

<a name="MM1501" />

#### <a name="mm1501-can-not-resolve-reference-0"></a>MM1501: 참조를 확인할 수 없습니다. {0}

### <a name="machocs"></a>MachO.cs

<a name="MM1600" />

#### <a name="mm1600-not-a-mach-o-dynamic-library-unknown-header-0x0-1"></a>MM1600: 일치-O 동적 라이브러리가 아닌 (알 수 없는 헤더 ' 0 x{0}'): {1}합니다.

<a name="MM1601" />

#### <a name="mm1601-not-a-static-library-unknown-header-0-1"></a>MM1601: 정적 라이브러리가 아닌 (알 수 없는 헤더 '{0}'): {1}합니다.

<a name="MM1602" />

#### <a name="mm1602-not-a-mach-o-dynamic-library-unknown-header-0x0-1"></a>MM1602: 일치-O 동적 라이브러리가 아닌 (알 수 없는 헤더 ' 0 x{0}'): {1}합니다.

<a name="MM1603" />

#### <a name="mm1603-unknown-format-for-fat-entry-at-position-0-in-1"></a>MM1603: 위치의 fat 항목에 대 한 알 수 없는 서식 {0} 에서 {1}합니다.

<a name="MM1604" />

#### <a name="mm1604-file-of-type-0-is-not-a-macho-file-1"></a>MM1604: 파일 형식 {0} 멋진 파일이 아닙니다 ({1}).

## <a name="mm2xxx-linker"></a>MM2xxx: 링커

### <a name="mm20xx-linker-general-errors"></a>MM20xx: (일반) 링커 오류

<a name="MM2001" />

#### <a name="mm2001-could-not-link-assemblies"></a>MM2001: 어셈블리를 연결할 수 없습니다.

<a name="MM2002" />

#### <a name="mm2002-can-not-resolve-reference-0"></a>MM2002: 참조를 확인할 수 없습니다. {0}

<a name="MM2003" />

#### <a name="mm2003-option-0-will-be-ignored-since-linking-is-disabled"></a>MM2003: 옵션 '{0}' 연결 되어 있으므로 무시 됩니다.

<a name="MM2004" />

#### <a name="mm2004-extra-linker-definitions-file-0-could-not-be-located"></a>MM2004: 추가 링커 정의 파일 '{0}' 찾을 수 없습니다.

<a name="MM2005" />

#### <a name="mm2005-definitions-from-0-could-not-be-parsed"></a>MM2005: 정의에서 '{0}' 구문 분석할 수 없습니다.

<a name="MM2006" />

#### <a name="mm2006-native-library-0-was-referenced-but-could-not-be-found"></a>MM2006: 네이티브 라이브러리 '{0}' 참조 되었지만 찾을 수 없습니다.

<a name="MM2007" />

#### <a name="mm2007-xamarinmac-unified-api-against-a-full-net-profile-does-not-support-linking-pass-the--nolink-flag"></a>MM2007: Xamarin.Mac 통합 API는 전체.NET 프로필에 대 한 연결을 지원 하지 않습니다. -Nolink 플래그를 전달 합니다.

<a name="MM2009" />

#### <a name="mm2009-referenced-by-01------this-message-is-related-to-mm2006-"></a>MM2009: 참조 {0}.{1}     * *이 메시지는 MM2006 관련이 * *

<a name="MM2010" />

#### <a name="mm2010-unknown-httpmessagehandler-0-valid-values-are-httpclienthandler-default-cfnetworkhandler-or-nsurlsessionhandler"></a>MM2010: 알 수 없는 HttpMessageHandler `{0}`합니다. 유효한 값은 HttpClientHandler (기본값), CFNetworkHandler 또는 NSUrlSessionHandler

<a name="MM2011" />

#### <a name="mm2011-unknown-tlsprovider-0--valid-values-are-default-or-appletls"></a>MM2011: 알 수 없는 TLSProvider '{0}합니다.  유효한 값은 기본 또는 appletls

<a name="MM2012" />

#### <a name="mm2012-only-first-0-of-1-referenced-by-warnings-shown--this-message-related-to-2009-"></a>MM2012: 첫 번째 {0} 의 {1} "참조" 경고를 표시 합니다. * * 2009이이 메시지와 관련 된 \*\*

<a name="MM2013" />

#### <a name="mm2013-failed-to-resolve-the-reference-to-0-referenced-in-1-the-app-will-not-include-the-referenced-assembly-and-may-fail-at-runtime"></a>MM2013: 에 대 한 참조를 확인 하지 못했습니다 "{0}"에 언급 된"{1}"입니다. 앱 참조 된 어셈블리에 포함 되지 않습니다 하 고 런타임에 실패할 수 있습니다.

<a name="MM2014" />

#### <a name="mm2014-xamarinmac-extensions-do-not-support-linking-request-for-linking-will-be-ignored--this-message-is-obsolete-in-xm-36-"></a>MM2014: Xamarin.Mac 확장 연결 하는 것을 지원 하지 않습니다. 요청 연결 무시 됩니다. * *이 메시지는 XM 3.6 이상에서 사용 되지 않음 \*\*

<!-- 2015 used by mtouch -->

<a name="MM2016" />

#### <a name="mm2016-invalid-tlsprovider-0-option-the-only-valid-value-1-will-be-used"></a>MM2016: 잘못 된 TlsProvider `{0}` 옵션입니다. 유효한 값 `{1}` 사용 됩니다.

<a name="MM2017" />

#### <a name="mm2017-could-not-process-xml-description-0"></a>MM2017: XML 설명을 처리할 수 없습니다. {0}

<a name="MM202x" />

#### <a name="mm202x-binding-optimizer-failed-processing-"></a>MM202x: 최적화 프로그램은 바인딩 처리 하지 못했습니다. `...`합니다.

<a name="MM2100" />

#### <a name="mm2100-xamarinmac-classic-api-does-not-support-platform-linking"></a>MM2100: Xamarin.Mac 클래식 API 플랫폼에 연결 하는 것을 지원 하지 않습니다.

<a name="MM2103" />

#### <a name="mm2103-error-processing-assembly--"></a>MM2103: 어셈블리 처리 오류 '\*': *

어셈블리 처리는 예기치 않은 오류가 발생 했습니다.

오류 메시지에 문제의 원인이 되는 어셈블리 이름은입니다. 어셈블리가이 문제를 해결 하기 위해에 제공 해야 합니다는 [버그 보고서](https://bugzilla.xamarin.com) 사용 하도록 설정 하는 세부 정보 표시를 사용 하 여 전체 빌드 로그와 함께 (즉 `-v -v -v -v` 에 **추가 mtouch 인수**).

<a name="MM2104" />

#### <a name="mm2104-unable-to-link-assembly-0-as-it-is-mixed-mode"></a>MM2104: 어셈블리에 연결할 수 없습니다. '{0}' 혼합 모드 이므로 합니다.

링커에 의해 혼합 모드 어셈블리를 처리할 수 있습니다.

참조 https://msdn.microsoft.com/library/x0w2664k.aspx 혼합 모드 어셈블리에 대 한 자세한 내용은 합니다.

## <a name="mm3xxx-aot"></a>MM3xxx: AOT

### <a name="mm30xx-aot-general-errors"></a>MM30xx: AOT (일반) 오류

<a name="MM3001" />

#### <a name="mm3001-could-not-aot-the-assembly-0"></a>MM3001: AOT 어셈블리 하지 못했습니다 '{0}'

<!-- 3002 used by mtouch -->
<!-- 3003 used by mtouch -->
<!-- 3004 used by mtouch -->
<!-- 3005 used by mtouch -->
<!-- 3006 used by mtouch -->
<!-- 3007 used by mtouch -->
<!-- 3008 used by mtouch -->

<a name="MM3009" />

#### <a name="mm3009-aot-of-0-was-requested-but-was-not-found"></a>MM3009: AOT의 '{0}' 요청 되었지만 찾을 수 없습니다

<a name="MM3010" />

#### <a name="mm3010-exclusion-of-aot-of-0-was-requested-but-was-not-found"></a>MM3010: AOT의 제외 '{0}' 요청 되었지만 찾을 수 없습니다

## <a name="mm4xxx-code-generation"></a>MM4xxx: 코드 생성

### <a name="mm40xx-driverm"></a>MM40xx: driver.m

<a name="MM4001" />

#### <a name="mm4001-the-main-template-could-not-be-expanded-to-0"></a>MM4001: 기본 템플릿은를 확장할 수 없는 `{0}`합니다.

### <a name="mm41xx-registrar"></a>MM41xx: registrar

<a name="MM4134" />

#### <a name="mm4134-your-application-is-using-the-0-framework-which-isnt-included-in-the-macos-sdk-youre-using-to-build-your-app-this-framework-was-introduced-in-osx-2-while-youre-building-with-the-macos-1-sdk-this-configuration-is-not-supported-with-the-static-registrar-pass---registrardynamic-as-an-additional-mmp-argument-in-your-projects-mac-build-option-to-select-alternatively-select-a-newer-sdk-in-your-apps-mac-build-options"></a>MM4134: 응용 프로그램에서 사용 된 '{0}' framework 응용 프로그램을 사용 중인 MacOS SDK에 포함 되지 않습니다 (이 프레임 워크는 OSX에서 도입 되었습니다 {2}MacOS를 사용 하 여 빌드하는 동안, {1} SDK.) 이 구성은 정적 등록 기관 (pass-등록자: 동적으로 선택 하려면 프로젝트의 Mac 빌드 옵션에서 추가 mmp 인수) 지원 되지 않습니다. 또는 앱의 Mac 빌드 옵션에서 최신 SDK를 선택 합니다.

## <a name="mm5xxx-gcc-and-toolchain"></a>MM5xxx: GCC 및 도구 체인

### <a name="mm51xx-compilation"></a>MM51xx: 컴파일

<a name="MM5101" />

#### <a name="mm5101-missing-0-compiler-please-install-xcode-command-line-tools-component"></a>MM5101: 누락 된 '{0}' 컴파일러. Xcode '명령줄 도구' 구성 요소를 설치 하세요.

<!-- 5102 used by mtouch -->

<a name="MM5103" />

#### <a name="mm5103-failed-to-compile-error-code---0-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MM5103: 컴파일하지 못했습니다. 오류 코드- {0}합니다. 버그 보고서를 제출 하세요 http://bugzilla.xamarin.com

<!-- 5104 used by mtouch -->

### <a name="mm52xx-linking"></a>MM52xx: linking

<a name="MM5202" />

#### <a name="mm5202-monoframework-mdk-is-missing-please-install-the-mdk-for-your-monoframework-version-from-httpmono-projectcomdownloads"></a>MM5202: Mono.framework MDK 누락 되었습니다. 사용 중인 Mono.framework 버전에 대 한 MDK를 설치 하세요 http://mono-project.com/Downloads

<a name="MM5203" />

#### <a name="mm5203-cant-find-libxammaca-likely-because-of-a-corrupted-xamarinmac-installation-please-reinstall-xamarinmac"></a>MM5203: Xamarin.Mac 설치 손상된으로 인해 가능성이 libxammac.a를 찾을 수 없습니다. Xamarin.Mac을 다시 설치 하십시오.

<a name="MM5204" />

#### <a name="mm5204-invalid-architecture-x8664-is-only-supported-on-non-classic-profiles"></a>MM5204: 잘못 된 아키텍처입니다. x86_64는 클래식이 아닌 프로필에만 지원 됩니다.

<a name="MM5205" />

#### <a name="mm5205-invalid-architecture-0-valid-architectures-are-i386-and-x8664-when---profilemobile"></a>MM5205: 잘못 된 아키텍처 '{0}'. 올바른 아키텍처 i386 되며 x86_64 (프로 파일링 경우-= 모바일).

<a name="MM5218" />

#### <a name="mm5218-cant-ignore-the-dynamic-symbol-symbol---ignore-dynamic-symbolsymbol-because-it-was-not-detected-as-a-dynamic-symbol"></a>MM5218: 동적 기호 {기호}를 무시할 수 없습니다 (-무시-동적-기호 = {symbol}) 하므로 동적 기호로 시 감지 되지 않았습니다.

참조 된 [해당 mtouch 경고](~/ios/troubleshooting/mtouch-errors.md#MT5218)합니다.

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

### <a name="mm53xx-other-tools"></a>다른 도구에 MM53xx:

<a name="MM5301" />

#### <a name="mm5301-pkg-config-could-not-be-found-please-install-the-monoframework-from-httpmono-projectcomdownloads"></a>MM5301: config 패키지를 찾을 수 없습니다. Mono.framework를 설치 하세요. http://mono-project.com/Downloads

<!-- 5302 used by mtouch -->
<!-- 5303 used by mtouch -->
<!-- 5304 used by mtouch -->

<a name="MM5305" />

#### <a name="mm5305-missing-otool-tool-please-install-xcode-command-line-tools-component"></a>MM5305: 누락 된 'otool' 도구입니다. Xcode '명령줄 도구' 구성 요소를 설치 하세요.

<a name="MM5306" />

#### <a name="mm5306-missing-dependencies-please-install-xcode-command-line-tools-component"></a>MM5306: 누락 된 종속성입니다. Xcode '명령줄 도구' 구성 요소를 설치 하세요.

<a name="MM5308" />

#### <a name="mm5308-xcode-license-agreement-may-not-have-been-accepted--please-launch-xcode"></a>MM5308: Xcode 라이선스 계약 허용 되지 않을 수 있습니다.  Xcode를 시작 하세요.

<a name="MM5309" />

#### <a name="mm5309-native-linking-failed-with-error-code-1--check-build-log-for-details"></a>MM5309: 네이티브 연결 오류 코드 1로 실패 했습니다.  자세한 내용은 빌드 로그를 확인 합니다.

<a name="MM5310" />

#### <a name="mm5310-installnametool-failed-with-an-error-code-0-check-build-log-for-details"></a>MM5310: install_name_tool 하지 못했으며 오류 코드 '{0}'. 자세한 내용은 빌드 로그를 확인 합니다.

<!-- MM6xxx: mmp internal tools -->
<!-- MM7xxx: reserved -->

## <a name="mm8xxx-runtime"></a>MM8xxx: 런타임

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

<a name="MM8017" />

#### <a name="mm8017-the-boehm-garbage-collector-is-not-supported-please-use-sgen-instead"></a>MM8017: Boehm 가비지 수집기가 지원 되지 않습니다. SGen를 대신 사용 하십시오.

