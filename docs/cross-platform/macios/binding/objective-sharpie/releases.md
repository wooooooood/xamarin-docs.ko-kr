---
title: 목표 Sharpie 릴리스 내역
description: 이 문서에서는 목표-C 코드에 대 한 C# 바인딩 만들기를 자동화 하는 데 사용 되는 도구인 Sharpie의 릴리스 기록을 설명 합니다.
ms.prod: xamarin
ms.assetid: 1F4A1BE1-7205-43F4-89D0-6C8672F52598
author: conceptdev
ms.author: crdun
ms.date: 10/11/2017
ms.openlocfilehash: b5362c0a809423e2782ee60faa96658cf132d752
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290867"
---
# <a name="objective-sharpie-release-history"></a>목표 Sharpie 릴리스 내역

## <a name="34-october-11-2017"></a>3.4 (2017 년 10 월 11 일)

[다운로드 v 3.4.0](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie-3.4.0.pkg)

* Xcode 9에 대 한 지원: iOS 11, macOS 10.13, tvOS 11 및 watchOS 4
* 이제 SIMD 및 tgmath 문제를 해결 해야 합니다.
* 원격 분석이 완전히 제거 되었습니다.

## <a name="33-august-3-2016"></a>3.3 (2016 년 8 월 3 일)

[다운로드 v 3.3.0](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.3.0.pkg)

* Xcode 8 베타 4, iOS 10, macOS 10.12, tvOS 10 및 watchOS 3에 대 한 지원.
* 최신 Clang 마스터 빌드로 업데이트 됨 (2016-08-02)
* `sharpie bind`에서 `sharpie pod bind` 로 [원격 분석 전송 옵션을 유지](https://twitter.com/Symbiatch/status/760373403878559744) 합니다.

## <a name="32-june-14-2016"></a>3.2 (2016 년 6 월 14 일)

[다운로드 v 3.2.3](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.2.3.pkg)

* Xcode 8 베타 1, iOS 10, macOS 10.12, tvOS 10 및 watchOS 3에 대 한 지원.

## <a name="31-may-31-2016"></a>3.1 (2016 년 5 월 31 일)

[다운로드 v 3.1.1](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.1.1.pkg)

* CocoaPods 1.0에 대 한 지원
* 네이티브 `.framework` 를 먼저 빌드한 다음이를 바인딩하여 CocoaPods 바인딩 안정성이 향상 되었습니다.
* CocoaPods `.framework` 및 바인딩 정의를 `Binding` 디렉터리에 복사 하 여 xamarin.ios 및 xamarin.ios 바인딩 프로젝트와 쉽게 통합할 수 있습니다.

## <a name="30-october-5-2015"></a>3.0 (2015 5 월 5 일)

[다운로드 v 3.0.8](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.0.8.pkg)

* Xcode 7에 도입 된 간단한 제네릭 및 null 허용 여부를 비롯 한 새로운 목적-C 언어 기능 지원
* 최신 iOS 및 Mac Sdk를 지원 합니다.
* Xcode 프로젝트 빌드 및 구문 분석을 지원 합니다. 이제 전체 Xcode 프로젝트를 목표 Sharpie에 전달할 수 있으며,이를 통해 적절 한 작업 (예: `sharpie bind Project.xcodeproj -sdk ios`)을 파악할 수 있습니다.
* [CocoaPods](https://cocoapods.org) 지원 이제 목표 Sharpie (예: `sharpie pod init ios AFNetworking && sharpie pod bind`)에서 직접 CocoaPods를 구성, 빌드 및 바인딩할 수 있습니다.
* 프레임 워크를 바인딩할 때 프레임 워크는 프레임 워크를 지 원하는 경우 Clang 모듈을 사용 하도록 설정 하므로 프레임 워크의 구조가에서 정의 `module.modulemap`되기 때문에 프레임 워크를 가장 정확 하 게 구문 분석 합니다.
* 프레임 워크 제품을 빌드하는 Xcode 프로젝트의 경우 Xcode 프로젝트의 비 프레임 워크 대상으로 중간 제품 대상 대신 해당 제품을 구문 분석 하면 자동으로 해결 될 수 없는 모호성이 있을 수 있습니다.

## <a name="216-march-17-2015"></a>2.1.6 (3 월 17 일, 2015)

[다운로드 v 2.1.6](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.1.6.pkg)

* 고정 이항 연산자 식 바인딩: 식의 왼쪽이 오른쪽으로 잘못 바꿨습니다 (예: `1 << 0` 가 잘못 `0 << 1`바인딩 되었음). 이를 위해 Adam Kemp에 감사 드립니다.
* 및 `NSInteger` 를 i386 `int` `NSUInteger` 에서`nuint` 및 `uint` 대신 및`nint` 로 바인딩하는 문제를 수정 했습니다. 는 이제 Clang에 전달 되어 i386에서 `objc/NSObjCRuntime.h` 구문 분석 작업이 예상 대로 수행 됩니다. `-DNS_BUILD_32_LIKE_64`
* Mac OS X sdk (예: `-sdk macosx10.10`)에 대 한 기본 아키텍처는 이제 i386 대신 x86_64 이므로 `-arch` 기본값 재정의를 원하는 경우가 아니면 생략할 수 있습니다.

## <a name="210-march-15-2015"></a>2.1.0 (3 월 15 일, 2015)

[다운로드 v 2.1.0](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.1.0.pkg)

* [bxC#27849](https://bugzilla.xamarin.com/show_bug.cgi?id=27849): 가 `using ObjCRuntime;` 사용 될 때 `ArgumentSemantic` 이 생성 되는지 확인 합니다.
* [bxC#27850](https://bugzilla.xamarin.com/show_bug.cgi?id=27850): 가 `using System.Runtime.InteropServices;` 사용 될 때 `DllImport` 이 생성 되는지 확인 합니다.
* [bxC#27852](https://bugzilla.xamarin.com/show_bug.cgi?id=27852): `DllImport` 에서`__Internal`기호를 로드 하는 것이 기본값입니다.
* [bxC#27848](https://bugzilla.xamarin.com/show_bug.cgi?id=27848): 앞으로 선언 된 목표-C 컨테이너 선언을 건너뜁니다.
* [bxC#27846](https://bugzilla.xamarin.com/show_bug.cgi?id=27846): 단일 한정자를 사용 하는 프로토콜 형식을 구체적 인터페이스로 바인딩합니다`id<Foo>` ( `Foo` 대신 `Foundation.NSObject<Foo>`).
* [bxC#28037](https://bugzilla.xamarin.com/show_bug.cgi?id=28037): `UInt64` `uL` `u` , 및 리터럴을로`Int32` 끌어 값이 안전 하 게 들어갈`Int32`수 있는 경우 및/또는 접미사를 삭제 합니다. `Int64` `UInt32`
* [bxC#28038](https://bugzilla.xamarin.com/show_bug.cgi?id=28038): 원래 네이티브 이름이 접두사로 시작 하는 `k` 경우 열거형 이름 매핑을 수정 합니다.
* `sizeof`인수 형식이 C# 기본 형식으로 매핑되지 않는 C 식은 Clang에서 계산 되 고 잘못 된 C#생성을 방지 하기 위해 정수 리터럴로 바인딩됩니다.
* 형식이 블록인 속성의 목적-C 구문 수정 (목표-C 코드는 바인딩된 선언 위의 주석에 표시 됨)
* Decayed 형식을 원래 형식`int[]` 으로 바인딩 합니다. decays는 Clang의 의미 체계 분석 중에 `int*` , 대신 작성 `int[]` 된 원본으로 바인딩합니다.

이 시점 릴리스에서 수정 된 많은 버그를 보고 하기 위해 Dave Dunkin에 매우 많은 감사를 드립니다!

## <a name="200-march-9-2015"></a>2.0.0: 2015 년 3 월 9 일

[다운로드 v 2.0.0](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.0.0.pkg)

목표 Sharpie 2.0은 향상 된 Clang 기반 드라이버 및 파서와 새로운 NRefactory 기반 바인딩 엔진을 사용 하는 주요 릴리스입니다. 이러한 향상 된 구성 요소는 특히 형식 바인딩과 관련 하 여 _훨씬_ 더 나은 바인딩을 제공 합니다. 다른 많은 기능이 향상 되었습니다 .이는 향후 릴리스에서 사용자에 게 표시 되는 다양 한 기능을 제공 하는 객관적인 Sharpie.

목표 Sharpie 2.0은 Clang 3.6.1을 기반으로 합니다.

### <a name="type-binding-improvements"></a>형식 바인딩 기능 향상

* 이제 객관적인 C 블록이 지원 됩니다. 여기에는를 통해 `typedef`명명 된 익명/인라인 블록 및 블록이 포함 됩니다. 익명 블록은 `System.Action` 또는 `System.Func` 대리자로 바인딩되고 명명 된 블록은 강력한 이름의 `delegate` 형식으로 바인딩됩니다.

* 또는 `typedef` `long` 와 같은builtin정수계열형식으로확인되는즉시익명열거형에대한명명추론기능이향상되었습니다.`int`

* C 포인터는 이제 대신 C# `unsafe` `System.IntPtr`포인터로 바인딩됩니다. 이로 인해 포인터 매개 변수 `out` 를 또는 `ref` 매개 변수로 설정 하려는 경우에 대 한 바인딩이 더 명확 하 게 됩니다. 매개 변수가 `out` 또는 `ref`인지 여부를 항상 유추할 수 있는 것은 아니므로 감사를 쉽게 처리할 수 있도록 포인터가 바인딩에 유지 됩니다.

* 위의 포인터 바인딩에 대 한 예외는 목표 C 개체에 대 한 2 차수 포인터가 매개 변수로 발견 된 경우입니다. 이러한 경우 규칙은 널리 사용할 수 있으며 매개 변수는 ( `out` `NSError **error` 예: → `out NSError error`)로 바인딩됩니다.

### <a name="verify-attribute"></a>Verify 특성

이제 목표 Sharpie에서 생성 된 바인딩에는 `[Verify]` 특성으로 주석이 추가 됩니다. 이러한 특성은 바인딩을 원래 C/목표값-C 선언과 비교 하 여 (바인딩된 선언 위의 설명에 제공 됨) 목표 Sharpie이 올바른 것을 _확인_ 해야 함을 의미 합니다.

모든 바인딩된 선언에는 확인을 사용 하는 _것이 좋지만_ `[Verify]` 특성으로 주석이 달린 선언에는이를 _반드시 사용 해야_ 합니다. 이는 대부분의 경우 원래 네이티브 소스 코드에 메타 데이터가 부족 하 여 바인딩을 가장 잘 만드는 방법을 유추할 수 없기 때문입니다. 최상의 바인딩 결정을 위해서는 헤더 파일 내에서 설명서 또는 코드 주석을 참조 해야 할 수 있습니다.

자세한 내용은 [특성 확인](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md) 설명서를 참조 하세요.

### <a name="other-notable-improvements"></a>기타 주목할 만한 기능

* `using`이제 바인딩 된 형식을 기반으로 문이 생성 됩니다. 예를 들어가 바인딩된 `NSURL` `using Foundation;` 경우에도 문이 생성 됩니다.

* `struct`및 `union` 선언이 이제 공용 구조체에 대 한 `[FieldOffset]` 트릭을 사용 하 여 바인딩됩니다.

* 상수 식 이니셜라이저가 있는 열거형 값은 이제 제대로 바인딩됩니다. 전체 식은로 C#변환 됩니다.

* 이제 Variadic 메서드와 블록이 바인딩되어 있습니다.

* 프레임 워크는 이제 옵션을 `-framework` 통해 지원 됩니다. 자세한 내용은 [네이티브 프레임 워크 바인딩에](~/cross-platform/macios/binding/objective-sharpie/index.md) 대 한 설명서를 참조 하세요.

* 목표-C 소스 코드는 지금 자동 검색 되므로 또는 `-ObjC` `-xobjective-c` 를 수동으로 Clang에 전달할 필요가 없습니다.

* 이제 Clang 모듈 사용법`@import`()이 자동으로 검색 되므로 Clang의 새 모듈 지원을 사용 하 `-fmodules` 는 라이브러리에 대해 Clang에 수동으로 전달할 필요가 없습니다.

* Xamarin Unified API은 이제 기본 바인딩 대상입니다. `-classic` 옵션을 사용 하 여 Classic API 32 비트만 대상으로 합니다.

### <a name="notable-bug-fixes"></a>주목할 만한 버그 수정

* 목표 `instancetype` -C 범주에서 사용 되는 경우 바인딩 수정
* 전체 이름 목표-C 범주
* 접두사 목표-C 프로토콜 `I` ( `INSCopying` 예: 대신 `NSCopying`)

## <a name="1135-december-21-2014"></a>1.1.35: 2014 년 12 월 21 일

[다운로드 v 1.1.35](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.35.pkg)

사소한 버그 수정

## <a name="111-december-15-2014"></a>1.1.1: 2014 년 12 월 15 일

[V 1.1.1 다운로드](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.1.pkg)

1.1.1은 1.5 년 4 2013 월의 초기 목표 미리 보기에 따라 Xamarin에서 년 내부 사용 및 개발의 첫 번째 주요 릴리스입니다. 이 릴리스는 일반적으로 안정적이 고 다양 한 네이티브 라이브러리에서 사용할 수 있는 첫 번째 이며 새로운 Clang 백엔드를 특징으로 합니다.

