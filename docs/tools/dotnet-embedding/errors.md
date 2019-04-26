---
title: .NET 포함 오류
description: 이 문서에서는.NET을 포함 하 여 생성 된 오류를 설명 합니다. 오류 코드에 의해 나열 되 고 문제 해결에 도움이 되는 설명을 지정 합니다.
ms.prod: xamarin
ms.assetid: 932C3F0C-D968-42D1-BB14-D97C73361983
author: lobrien
ms.author: laobri
ms.date: 04/11/2018
ms.openlocfilehash: 5c3dd406f1132f51a86ddf574ab7ad0b279bc9ec
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61215343"
---
# <a name="net-embedding-errors"></a>.NET 포함 오류

## <a name="em0xxx-binding-error-messages"></a>EM0xxx: 바인딩 오류 메시지

예: 매개 변수, 환경

<!-- 0xxx: the generator itself, e.g. parameters, environment -->

<a name="EM0000" />

### <a name="em0000-unexpected-error---please-fill-a-bug-report-at-httpsgithubcommonoembeddinator-4000issues"></a>EM0000: 예기치 않은 오류-에서 버그 보고서를 작성 하세요 https://github.com/mono/Embeddinator-4000/issues

예기치 않은 오류가 발생 했습니다. 하세요 [문제를 제출](https://github.com/mono/Embeddinator-4000/issues) 최대한 많은 정보를 포함 합니다.

* 전체 로그를 최대한 자세하게 구축
* 오류를 재현 하는 최소한의 테스트 사례
* 모든 버전 정보

정확한 버전 정보를 얻을 수는 가장 쉬운 방법은 사용 하는 것을 **Xamarin Studio** 메뉴에서 **Xamarin Studio에 대 한** 항목인 **자세한 정보 표시** 단추 및 버전에 대 한 복사/붙여넣기 정보 (사용할 수는 **복사 정보** 단추).

<a name="EM0001" />

### <a name="em0001-could-not-create-output-directory-x"></a>EM0001: 출력 디렉터리를 만들지 못했습니다. `X`

지정 된 디렉터리 이름을 `-o=DIR` 없습니다 및 만들 수 없습니다. 파일 시스템에 대 한 잘못 된 이름을 수 있습니다.

<a name="EM0002" />

### <a name="em0002-option-x-is-not-supported"></a>EM0002: 옵션 `X` 지원 되지 않습니다

도구 옵션을 지원 하지 않습니다 `X`합니다. 이 환경에서는 적용 되지 않습니다 또는 다른 버전의 도구에서 지 원하는 것 같습니다.

<a name="EM0003" />

### <a name="em0003-the-platform-x-is-not-valid"></a>EM0003: 플랫폼 `X` 올바르지 않습니다.

도구는 플랫폼을 지원 하지 않습니다 `X`합니다. 이 환경에서는 적용 되지 않습니다 또는 다른 버전의 도구에서 지 원하는 것 같습니다.

<a name="EM0004" />

### <a name="em0004-the-target-x-is-not-valid"></a>EM0004: 대상 `X` 올바르지 않습니다.

이 도구는 대상을 지원 하지 않습니다 `X`합니다. 이 환경에서는 적용 되지 않습니다 또는 다른 버전의 도구에서 지 원하는 것 같습니다.

<a name="EM0005" />

### <a name="em0005-the-compilation-target-x-is-not-valid"></a>EM0005: 컴파일 대상 `X` 올바르지 않습니다.

도구에서는 컴파일 대상 없습니다 `X`합니다. 이 환경에서는 적용 되지 않습니다 또는 다른 버전의 도구에서 지 원하는 것 같습니다.

<a name="EM0006" />

### <a name="em0006-could-not-find-the-xcode-location"></a>EM0006: Xcode 위치를 찾을 수 없습니다.

도구를 사용 하 여 현재 선택 된 Xcode 위치를 찾을 수 없습니다는 `xcode-select -p` 명령입니다. 이 명령은 성공 하 고 올바른 Xcode 위치를 반환 합니다. 확인 하세요.

<a name="EM0007" />

### <a name="em0007-could-not-get-the-sdk-version-for-sdk"></a>EM0007: '{Sdk}'에 대 한 sdk 버전을 가져올 수 없습니다.

도구를 사용 하 여 SDK 버전을 가져올 수 없습니다는 `xcrun --show-sdk-version --sdk {sdk}` 명령입니다. 이 명령은 성공 하 고 SDK 버전을 반환 합니다. 확인 하세요.

<a name="EM0008" />

### <a name="em0008-the-architecture-arch-is-not-valid-for-platform-valid-architectures-for-platform-are-architectures"></a>EM0008: 아키텍처 '{arch}' {플랫폼}에 대해 올바르지 않습니다. {0} 플랫폼}에 대 한 유효한 아키텍처는: '{아키텍처}'.

오류 메시지에는 아키텍처 대상된 플랫폼에 대해 올바르지 않습니다. -Abi 옵션을 올바른 아키텍처가 전달 되도록 확인 하십시오.

<a name="EM0009" />

### <a name="em0009-the-feature-x-is-not-currently-implemented-by-the-generator"></a>EM0009: 기능 `X` 생성기에서 현재 구현 되지 않습니다

이 생성기의 향후 릴리스에서 해결 하려고 하는 알려진된 문제입니다. 작성 글을 기다리겠습니다.

<a name="EM0010" />

### <a name="em0010-cant-merge-the-frameworks-simulatorframework-and-deviceframework-because-the-file-file-exists-in-both"></a>EM0010: '{File}' 파일 모두에 존재 하기 때문에 프레임 워크 '{0} simulatorFramework}' 및 '{deviceFramework}'를 병합할 수 없습니다.

도구는 서로 공통 된 파일이 있기 때문에 오류 메시지에 언급 된 프레임 워크를 병합 하지 못했습니다.

이.NET 포함;에 버그를 나타낼 수 있습니다. 버그 보고서를 제출 하세요 [ https://github.com/mono/Embeddinator-4000/issues ](https://github.com/mono/Embeddinator-4000/issues) 테스트 사례를 사용 하 여 합니다.

<a name="EM0011" />

### <a name="em0011-the-assembly-x-does-not-exist"></a>EM0011: 어셈블리 `X` 존재 하지 않습니다.

이 도구는 어셈블리를 찾지 못했습니다 `X` 인수에 지정 합니다.

<a name="EM0012" />

### <a name="em0012-the-assembly-name-x-is-not-unique"></a>EM0012: 어셈블리 이름을 `X` 고유 하지 않습니다.

제공 된 하나 이상의 어셈블리 이름이 동일 하 고 내부 및 런타임 시 구별할 수 없습니다.

가장 가능성이 높은 원인은 어셈블리에서 명령줄 인수를 두 번 이상 지정 되어 있는지 보여 줍니다. 그러나가 함께 원래 이름이 며 여러 복사본을 이름이 변경 된 어셈블리 여전히 유지 수 없습니다.

<a name="EM0013" />

### <a name="em0013-cant-find-the-assembly-x-referenced-by-y"></a>EM0013: 'X', 'Y'에서 참조 된 어셈블리를 찾을 수 없습니다.

도구에는 'X', 'Y' 어셈블리가 참조 어셈블리를 찾을 수 없습니다. 모든 참조 된 어셈블리는 바인딩할 어셈블리와 동일한 디렉터리에 있는지 확인 하십시오.

<a name="EM0014" />

### <a name="em0014-could-not-find-product-product-minversion-is-required"></a>EM0014: {Product}를 찾을 수 없습니다 ({product} {min_version} 필수임).

시스템에서 오류 메시지에 대 한 종속성을 찾을 수 없습니다.

<a name="EM0015" />

### <a name="em0015-could-not-find-a-valid-version-of-product-found-version-but-at-least-minversion-is-required"></a>EM0015: {Product}의 올바른 버전을 찾을 수 없습니다 ({버전}을 찾을 수 있지만 적어도 {min_version} 필요).

종속성 오류에서 언급 되는 메시지 시스템에서 발견 되었지만 너무 오래 되어 있습니다. 최신 버전으로 업데이트 하세요.

<a name="EM0016" />

### <a name="em0016-could-not-create-symlink-file---target-error-number"></a>EM0016: Symlink를 만들 수 없습니다 '{대상}'-> '{file}': 오류 {number 개}

오류 메시지에 언급 한 symlink를 만들 수 없습니다.

<a name="EM0026" />

### <a name="em0026-could-not-parse-the-command-line-argument-a-"></a>명령줄 인수 'A'를 구문 분석할 EM0026 수 없습니다. *

명령줄 옵션에 대해 지정 된 구문을 `A` 도구로 분석할 수 없습니다. 가능성이 잘못 설명서 또는 올바른 구문에 대 한 도움말을 사용 하 여 확인 하세요.

<a name="EM0099" />

### <a name="em0099-internal-error--please-file-a-bug-report-with-a-test-case-httpsgithubcommonoembeddinator-4000issues"></a>EM0099: 내부 오류 * 합니다. 테스트 사례를 사용 하 여 버그 보고서를 제출 하세요 (https://github.com/mono/Embeddinator-4000/issues)합니다.

.NET 포함 된 내부 일관성 검사가 실패 하면이 오류 메시지가 보고 됩니다.

.NET 포함;에 버그가이 버그 보고서를 제출 하세요 [ https://github.com/mono/Embeddinator-4000/issues ](https://github.com/mono/Embeddinator-4000/issues) 테스트 사례를 사용 하 여 합니다.

<!-- 1xxx: code processing -->

## <a name="em1xxx-code-processing"></a>EM1xxx: 코드 처리

<a name="EM1010" />

### <a name="em1010-type-t-is-not-generated-because-x-are-not-supported"></a>EM1010: 형식 `T` 때문에 생성 되지 않습니다 `X` 지원 되지 않습니다.

이 **경고** 는 형식 `T` 무시 됩니다 (즉, 아무 것도 생성 됨) 사용 하므로 `X`, 지원 되지 않는 기능입니다.

참고: 지원 되는 기능 새 버전의 도구를 사용 하 여 진화 됩니다.

<a name="EM1011" />

### <a name="em1011-type-t-is-not-generated-because-it-lacks-marshaling-code-with-a-native-counterpart"></a>EM1011: 형식 `T` 네이티브는 대응 사용자를 사용 하 여 마샬링 코드를 포함 하지 않으므로 생성 되지 않습니다.

이 **경고** 하는 형식 `T` 무시 됩니다 (즉, 아무 것도 생성 됨) 마샬링 추가 해야 하는.NET framework에서 무언가 표시 하기 때문에 합니다.

참고: 이 이후 버전의 도구에서 몇 가지 제한 사항이 지원 가져올 수 있습니다.

<a name="EM1020" />

### <a name="em1020-constructor-c-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1020: 생성자 `C` 매개 변수 형식으로 인해 생성 되지 않습니다 `T` 지원 되지 않습니다.

이 **경고** 는 생성자 `C` 무시 됩니다 (즉, 아무 것도 생성 됨) 때문에 형식 매개 변수가 `T` 지원 되지 않습니다.

이유는 자세한 정보를 제공 하는 이전 경고 형식 이어야 합니다 `T` 지원 되지 않습니다.

참고: 지원 되는 기능 새 버전의 도구를 사용 하 여 진화 됩니다.

<a name="EM1021" />

### <a name="em1021-constructor-c-has-default-values-for-which-no-wrapper-is-generated"></a>EM1021: 생성자 `C` 없습니다 래퍼 생성 되는 기본값을 포함 합니다.

이 **경고** 하는 생성자의 기본 매개 변수 `C` 코드를 추가로 생성 되지 않습니다. 가장 일반적인 원인은 기존 메서드에 이미 동일한 서명을 가진 것입니다. 예: .net에서 가질 수입니다.

```
public class MyType {
    public MyType () { ... }
    public MyType (int i = 0) { ... }
}
```

생성 된 이러한 경우 두 개의 `init` 선택기를 만들 Mono 모두 호출 되지만 이후 없습니다 래퍼는 존재 합니다.

<a name="EM1030" />

### <a name="em1030-method-m-is-not-generated-because-return-type-t-is-not-supported"></a>EM1030: 메서드 `M` 때문에 생성 되지 않습니다 반환 형식 `T` 지원 되지 않습니다.

이 **경고** 하는 메서드 `M` 무시 됩니다 (즉, 아무 것도 생성 됨) 반환 형식 이므로 `T` 지원 되지 않습니다.

이유는 자세한 정보를 제공 하는 이전 경고 형식 이어야 합니다 `T` 지원 되지 않습니다.

참고: 지원 되는 기능 새 버전의 도구를 사용 하 여 진화 됩니다.

<a name="EM1031" />

### <a name="em1031-method-m-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1031: 메서드 `M` 매개 변수 형식으로 인해 생성 되지 않습니다 `T` 지원 되지 않습니다.

이 **경고** 는 메서드 `M` 무시 됩니다 (즉, 아무 것도 생성 됨) 때문에 형식 매개 변수가 `T` 지원 되지 않습니다.

이유는 자세한 정보를 제공 하는 이전 경고 형식 이어야 합니다 `T` 지원 되지 않습니다.

참고: 지원 되는 기능 새 버전의 도구를 사용 하 여 진화 됩니다.

<a name="EM1032" />

### <a name="em1032-method-m-has-default-values-for-which-no-wrapper-is-generated"></a>EM1032: 메서드 `M` 없습니다 래퍼 생성 되는 기본값을 포함 합니다.

이 **경고** 는 메서드의 기본 매개 변수 `M` 코드를 추가로 생성 되지 않습니다. 가장 일반적인 원인은 기존 메서드에 이미 동일한 서명을 가진 것입니다. 예: .net에서 가질 수입니다.

```
public class MyType {
    public int Increment () { ... }
    public int Increment (int i = 0) { ... }
}
```

생성 된 이러한 경우 두 개의 `increment` 선택기를 만들 Mono 모두 호출 되지만 이후 없습니다 래퍼는 존재 합니다.

<a name="EM1033" />

### <a name="em1033-method-m-is-not-generated-because-another-method-exposes-the-operator-with-a-friendly-name"></a>EM1033: 메서드 `M` 다른 메서드 이름 사용 하 여 연산자를 노출 하기 때문에 생성 되지 않습니다.

이 **경고** 하는 메서드 `M` 다른 메서드 이름 사용 하 여 연산자를 노출 하기 때문에 생성 되지 않습니다. (https://msdn.microsoft.com/library/ms229032(v=vs.110).aspx)

<a name="EM1034" />

### <a name="em1034-extension-method-m-is-not-generated-inside-a-category-because-they-cannot-be-created-on-primitive-type-t-a-normal-static-method-was-generated"></a>EM1034: 확장 메서드 `M` 형식에서 만들 수 없기 때문에 범주 내에서 생성 되지 않습니다 `T`합니다. 일반적으로 정적 메서드가 생성 되었습니다.

이것이 **경고** 는 primivite에 확장 메서드는 형식 (예: `System.Int32`) 찾을 수 있습니다. Objective C에서는 기본 형식에 범주를 만들 수 없습니다. 대신 일반적으로 정적 메서드 생성기를 생성 합니다.

<a name="EM1040" />

### <a name="em1040-property-p-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1040: 속성 `P` 매개 변수 형식으로 인해 생성 되지 않습니다 `T` 지원 되지 않습니다.

이 **경고** 는 속성 `P` 무시 됩니다 (즉, 아무 것도 생성 됨) 때문에 노출 된 형식 `T` 지원 되지 않습니다.

이유는 자세한 정보를 제공 하는 이전 경고 형식 이어야 합니다 `T` 지원 되지 않습니다.

참고: 지원 되는 기능 새 버전의 도구를 사용 하 여 진화 됩니다.

<a name="EM1041" />

### <a name="em1041-indexed-properties-on-t-is-not-generated-because-multiple-indexed-properties-are-not-supported"></a>EM1041: 인덱싱된 속성 `T` 여러 인덱싱된 속성은 지원 되지 않으므로 생성 되지 않습니다.

이 **경고** 는 인덱싱된 속성 `T` 무시 됩니다 (즉, 아무 것도 생성 됨) 여러 인덱싱된 속성은 지원 되지 않으므로 합니다.

<a name="EM1050" />

### <a name="em1050-field-f-is-not-generated-because-of-field-type-t-is-not-supported"></a>EM1050: 필드 `F` 은 필드 형식으로 인해 생성 되지 않습니다 `T` 지원 되지 않습니다.

이 **경고** 하는 필드 `F` 무시 됩니다 (즉, 아무 것도 생성 됨) 때문에 노출 된 형식 `T` 지원 되지 않습니다.

이유는 자세한 정보를 제공 하는 이전 경고 형식 이어야 합니다 `T` 지원 되지 않습니다.

참고: 지원 되는 기능 새 버전의 도구를 사용 하 여 진화 됩니다.

<a name="EM1051" />

### <a name="em1051-element-e-is-generated-instead-as-f-because-its-name-conflicts-with-an-important-objective-c-selector"></a>EM1051: 요소 `E` 대신 생성 됩니다으로 `F` 중요-objective-c 선택기를 사용 하 여 해당 이름이 충돌 하기 때문입니다.

이 **경고** 하는 요소 `E` 대신 생성 됩니다으로 `F` 중요-objective-c 선택기를 사용 하 여 해당 이름이 충돌 하기 때문입니다.

선택기를 [NSObjectProtocol](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc) objective c에서 중요 한 의미가 및 신중 하 게 재정의 해야 합니다.

참고: 새 버전의 도구를 사용 하 여 예약 된 선택기 목록을 발전할 합니다.

<a name="EM1052" />

### <a name="em1052-element-e-is-not-generated-its-name-conflicts-with-other-elements-on-the-same-class"></a>EM1052: 요소 `E` 생성 되지 않습니다 동일한 클래스의 다른 요소를 사용 하 여 해당 이름이 충돌 합니다.

이 **경고** 요소 `E` 이름과 동일한 클래스의 다른 요소와 충돌 하는 대로 생성 되지 않습니다.

<a name="EM1053" />

### <a name="em1053-target-e-is-not-supported-for-xamarinios-and-xamarinmac-only-the-framework-option-is-considered-supported-and-should-be-used"></a>EM1053: 대상 `E` Xamarin.iOS 및 Xamarin.Mac 지원 되지 않습니다. 만 `framework` 옵션이 지원 되는 것으로 간주 되어 사용 해야 합니다.

이 **경고** 대상으로 하는 `E` Xamarin.iOS 및 Xamarin.Mac 사용 사례에 대해 지원 되지 않는 것으로 간주 됩니다. 

정적 또는 동적 포함 하는.NET 라이브러리의 소비 추가 작업 단계 또는 조정 해야 할 수 있습니다 및 대부분의 사용 하지 않아야 합니다.

제거해 하 `--target` 매개 변수 또는 pass `--target=framework` 대신 합니다.

<!-- 2xxx: code generation -->

## <a name="em2xxx-code-generation"></a>EM2xxx: 코드 생성

<!-- 3xxx: reserved -->
<!-- 4xxx: reserved -->
<!-- 5xxx: reserved -->
<!-- 6xxx: reserved -->
<!-- 7xxx: reserved -->
<!-- 8xxx: reserved -->
<!-- 9xxx: reserved -->
