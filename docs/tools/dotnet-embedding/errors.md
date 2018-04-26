---
title: .NET 오류를 포함 합니다.
ms.prod: xamarin
ms.assetid: 932C3F0C-D968-42D1-BB14-D97C73361983
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 04/11/2018
ms.openlocfilehash: 0bc4451d8eb93b826fc673bc4e163c9b7b68c36e
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/26/2018
---
# <a name="net-embedding-errors"></a>.NET 오류를 포함 합니다.

## <a name="em0xxx-binding-error-messages"></a>EM0xxx: 바인딩 오류 메시지

예: 매개 변수, 환경

<!-- 0xxx: the generator itself, e.g. parameters, environment -->

<a name="EM0000" />

### <a name="em0000-unexpected-error---please-fill-a-bug-report-at-httpsgithubcommonoembeddinator-4000issues"></a>EM0000: 예기치 않은 오류-에 버그 보고서를 채우십시오. https://github.com/mono/Embeddinator-4000/issues

예기치 않은 오류가 발생 했습니다. 하십시오 [문제를 제출](https://github.com/mono/Embeddinator-4000/issues) 최대한 많은 정보를 포함 합니다.

* 최대 자세한 정도를 빌드 로그, 전체
* 오류를 재현 하는 최소한의 테스트 사례
* 모든 버전 정보

사용 하는 정확한 버전 정보를 가져올 수는 가장 쉬운 방법은 **Xamarin Studio** 메뉴 **Xamarin Studio에 대 한** 항목 **자세한 정보 표시** 단추를 선택한 버전 복사/붙여넣기 정보 (사용할 수 있습니다는 **복사본 정보** 단추).

<a name="EM0001" />

### <a name="em0001-could-not-create-output-directory-x"></a>EM0001: 출력 디렉터리를 만들 수 없습니다. `X`

지정 하는 디렉터리 이름 `-o=DIR` 존재 하지 않습니다 및 만들 수 없습니다. 파일 시스템에 대 한 잘못 된 이름이 수도 있습니다.

<a name="EM0002" />

### <a name="em0002-option-x-is-not-supported"></a>EM0002: 옵션 `X` 지원 되지 않습니다

도구 옵션을 지원 하지 않는 `X`합니다. 다른 버전의 도구 지원 하는지 또는이 환경에 적용 되지 않는 것 같습니다.

<a name="EM0003" />

### <a name="em0003-the-platform-x-is-not-valid"></a>EM0003: 플랫폼 `X` 올바르지 않습니다.

이 도구는 플랫폼을 지원 하지 않습니다 `X`합니다. 다른 버전의 도구 지원 하는지 또는이 환경에 적용 되지 않는 것 같습니다.

<a name="EM0004" />

### <a name="em0004-the-target-x-is-not-valid"></a>EM0004: 대상 `X` 올바르지 않습니다.

이 도구는 대상을 지원 하지 `X`합니다. 다른 버전의 도구 지원 하는지 또는이 환경에 적용 되지 않는 것 같습니다.

<a name="EM0005" />

### <a name="em0005-the-compilation-target-x-is-not-valid"></a>EM0005: 컴파일 대상 `X` 올바르지 않습니다.

이 도구는 컴파일 대상을 지원 하지 `X`합니다. 다른 버전의 도구 지원 하는지 또는이 환경에 적용 되지 않는 것 같습니다.

<a name="EM0006" />

### <a name="em0006-could-not-find-the-xcode-location"></a>EM0006: Xcode 위치를 찾을 수 없습니다.

이 도구를 사용 하 여 현재 선택 된 Xcode 위치 찾지 못했습니다는 `xcode-select -p` 명령입니다. 이 명령은 성공 하면 반환 올바른 Xcode 위치를 확인 하십시오.

<a name="EM0007" />

### <a name="em0007-could-not-get-the-sdk-version-for-sdk"></a>EM0007: '{sdk}'에 대 한 sdk 버전을 가져올 수 없습니다.

이 도구를 사용 하 여 SDK 버전 가져올 수 없습니다는 `xcrun --show-sdk-version --sdk {sdk}` 명령입니다. 이 명령은 성공 하 고 SDK 버전을 반환을 확인 하십시오.

<a name="EM0008" />

### <a name="em0008-the-architecture-arch-is-not-valid-for-platform-valid-architectures-for-platform-are-architectures"></a>EM0008: 아키텍처 '{arch을 (를)' {플랫폼}에 대해 올바르지 않습니다. {플랫폼}에 대 한 올바른 아키텍처는: '{아키텍처}'.

오류 메시지에 아키텍처 대상된 플랫폼에 대해 올바르지 않습니다. -Abi 옵션 올바른 아키텍처가 전달 되 고 있는지 확인 하십시오.

<a name="EM0009" />

### <a name="em0009-the-feature-x-is-not-currently-implemented-by-the-generator"></a>EM0009: 기능 `X` 생성기에서 현재 구현 되지 않습니다

생성기의 이후 릴리스에서 수정 하 여 사용 하는 알려진된 문제입니다. 기여를 기다리겠습니다.

<a name="EM0010" />

### <a name="em0010-cant-merge-the-frameworks-simulatorframework-and-deviceframework-because-the-file-file-exists-in-both"></a>파일 '{파일을 (를)' 모두에 존재 하기 때문에 프레임 워크 '{simulatorFramework}' 및 '{deviceFramework을 (를)' EM0010: 병합할 수 없습니다.

도구는 일반 파일을 서로 있기 때문에 오류 메시지에 언급 된 프레임 워크를 병합할 수 없습니다.

이 Embeddinator 4000;의 버그를 나타낼 수 있습니다. 에 버그 보고서를 제출 하세요 [ https://github.com/mono/Embeddinator-4000/issues ](https://github.com/mono/Embeddinator-4000/issues) 테스트 사례와 합니다.

<a name="EM0011" />

### <a name="em0011-the-assembly-x-does-not-exist"></a>: EM0011 어셈블리 `X` 존재 하지 않습니다.

이 도구는 어셈블리를 찾을 수 없습니다 `X` 인수에 지정 합니다.

<a name="EM0012" />

### <a name="em0012-the-assembly-name-x-is-not-unique"></a>EM0012: 어셈블리 이름 `X` 고유 하지 않습니다

제공 된 하나 이상의 어셈블리 동일 하 고 내부 이름이 있으며이를 런타임에 구분할 수 없습니다.

가장 일반적인 원인은 명령줄 인수에 어셈블리를 두 번 이상 지정 했는지입니다. 그러나가 함께 원래 이름과 복사본을 여러 개 이름이 바뀐된 어셈블리 여전히 유지 수 없습니다.

<a name="EM0013" />

### <a name="em0013-cant-find-the-assembly-x-referenced-by-y"></a>EM0013: 'X', 'Y'에서 참조 어셈블리를 찾을 수 없습니다.

이 도구에는 'X', 'Y' 어셈블리에서 참조 어셈블리를 찾을 수 없습니다. 참조 된 모든 어셈블리를 바인딩해야 어셈블리와 동일한 디렉터리에 있는지 확인 하십시오.

<a name="EM0014" />

### <a name="em0014-could-not-find-product-product-minversion-is-required"></a>EM0014: {제품을 (를) 찾을 수 없습니다 ({제품} {min_version}가 필요 함).

시스템에서 오류 메시지에 종속성을 찾을 수 없습니다.

<a name="EM0015" />

### <a name="em0015-could-not-find-a-valid-version-of-product-found-version-but-at-least-minversion-is-required"></a>EM0015: {제품}의 올바른 버전을 찾을 수 없습니다 ({버전}를 찾을 수 있지만 적어도 {min_version}가 필요).

메시지는 시스템에 파일이 너무 오래 되어 있지만 종속성 오류에서 언급 합니다. 최신 버전으로 업데이트 하십시오.

<a name="EM0016" />

### <a name="em0016-could-not-create-symlink-file---target-error-number"></a>EM0016: 기호화 된 링크를 만들 수 없습니다 '{target}'-> '{파일 (를)': 오류 {number}

오류 메시지에는 기호화 된 링크를 만들 수 없습니다.

<a name="EM0026" />

### <a name="em0026-could-not-parse-the-command-line-argument-a-"></a>명령줄 인수 'A' 구문 분석할 EM0026 수 없습니다: *

명령줄 옵션에 대 한 구문 `A` 도구에서 분석할 수 없습니다. 가능성이, 잘못 된 문서 또는 올바른 구문에 대 한 도움말을 확인 하십시오.

<a name="EM0099" />

### <a name="em0099-internal-error--please-file-a-bug-report-with-a-test-case-httpsgithubcommonoembeddinator-4000issues"></a>: EM0099 내부 오류 * 합니다. 테스트 사례와 버그 보고서를 제출 하세요 (https://github.com/mono/Embeddinator-4000/issues)합니다.

Embeddinator 4000에서 내부 일관성 확인 작업을 실패 한 경우이 오류 메시지가 보고 됩니다.

이 Embeddinator 4000;의 버그를 나타냅니다. 에 버그 보고서를 제출 하세요 [ https://github.com/mono/Embeddinator-4000/issues ](https://github.com/mono/Embeddinator-4000/issues) 테스트 사례와 합니다.

<!-- 1xxx: code processing -->

## <a name="em1xxx-code-processing"></a>EM1xxx: 코드 처리

<a name="EM1010" />

### <a name="em1010-type-t-is-not-generated-because-x-are-not-supported"></a>EM1010: 입력 `T` 생성 되지 않습니다 `X` 지원 되지 않습니다.

이는 **경고** 하는 형식을 `T` 무시 됩니다 (즉, 아무것도 생성 됨) 사용 하기 때문에 `X`, 지원 되지 않는 기능입니다.

참고: 지원 되는 기능 도구의 새 버전으로 발전 합니다.

<a name="EM1011" />

### <a name="em1011-type-t-is-not-generated-because-it-lacks-marshaling-code-with-a-native-counterpart"></a>EM1011: 입력 `T` 해당 하는 네이티브 도구를 사용 하 여 마샬링 코드 없기 때문에 생성 되지 않습니다.

이것이 **경고** 하는 형식을 `T` 무시 됩니다 (즉, 아무것도 생성 됨)에 추가 마샬링 필요한.NET framework에서 검색을 표시 하기 때문에 합니다.

참고: 이후 버전의 도구에서 몇 가지 제한 사항과 함께 지원 가져올 수 있습니다 하는 것입니다.

<a name="EM1020" />

### <a name="em1020-constructor-c-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1020: 생성자 `C` 매개 변수 형식으로 인해 생성 되지 않습니다 `T` 지원 되지 않습니다.

이는 **경고** 하는 생성자 `C` 무시 됩니다 (즉, 아무것도 생성 됨) 때문에 형식 매개 변수가 `T` 지원 되지 않습니다.

있어야 이유 자세한 정보를 제공 하는 이전 경고 입력 `T` 지원 되지 않습니다.

참고: 지원 되는 기능 도구의 새 버전으로 발전 합니다.

<a name="EM1021" />

### <a name="em1021-constructor-c-has-default-values-for-which-no-wrapper-is-generated"></a>EM1021: 생성자 `C` 래퍼가 되지 생성 되는 기본값을 포함 합니다.

이 한 **경고** 하는 생성자의 기본 매개 변수 `C` 인해 추가 코드가 생성 되지 않습니다. 가장 일반적인 원인은 기존 방법에 이미 동일한 서명을 가진입니다. 예: .net에서 할 수입니다.

```
public class MyType {
    public MyType () { ... }
    public MyType (int i = 0) { ... }
}
```

생성 된 이러한 경우 두 개의 `init` 선택기를 만들 모노을 모두 호출 되지만 나중에 대 한 없는 래퍼는 존재 합니다.

<a name="EM1030" />

### <a name="em1030-method-m-is-not-generated-because-return-type-t-is-not-supported"></a>EM1030: 메서드 `M` 생성 되지 않습니다 반환 형식 `T` 지원 되지 않습니다.

이것은 **경고** 하는 메서드가 `M` 무시 됩니다 (즉, 아무것도 생성 됨) 반환 형식 이므로 `T` 지원 되지 않습니다.

있어야 이유 자세한 정보를 제공 하는 이전 경고 입력 `T` 지원 되지 않습니다.

참고: 지원 되는 기능 도구의 새 버전으로 발전 합니다.

<a name="EM1031" />

### <a name="em1031-method-m-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1031: 메서드 `M` 매개 변수 형식으로 인해 생성 되지 않습니다 `T` 지원 되지 않습니다.

이는 **경고** 하는 메서드 `M` 무시 됩니다 (즉, 아무것도 생성 됨) 때문에 형식 매개 변수가 `T` 지원 되지 않습니다.

있어야 이유 자세한 정보를 제공 하는 이전 경고 입력 `T` 지원 되지 않습니다.

참고: 지원 되는 기능 도구의 새 버전으로 발전 합니다.

<a name="EM1032" />

### <a name="em1032-method-m-has-default-values-for-which-no-wrapper-is-generated"></a>EM1032: 메서드 `M` 래퍼가 되지 생성 되는 기본값을 포함 합니다.

이는 **경고** 하는 방법의 기본 매개 변수 `M` 인해 추가 코드가 생성 되지 않습니다. 가장 일반적인 원인은 기존 방법에 이미 동일한 서명을 가진입니다. 예: .net에서 할 수입니다.

```
public class MyType {
    public int Increment () { ... }
    public int Increment (int i = 0) { ... }
}
```

생성 된 이러한 경우 두 개의 `increment` 선택기를 만들 모노을 모두 호출 되지만 나중에 대 한 없는 래퍼는 존재 합니다.

<a name="EM1033" />

### <a name="em1033-method-m-is-not-generated-because-another-method-exposes-the-operator-with-a-friendly-name"></a>EM1033: 메서드 `M` 이름 사용 하 여 연산자를 표시 하는 또 다른 방법은 때문에 생성 되지 않습니다.

이는 **경고** 하는 메서드가 `M` 이름 사용 하 여 연산자를 표시 하는 또 다른 방법은 때문에 생성 되지 않습니다. (https://msdn.microsoft.com/library/ms229032(v=vs.110).aspx)

<a name="EM1034" />

### <a name="em1034-extension-method-m-is-not-generated-inside-a-category-because-they-cannot-be-created-on-primitive-type-t-a-normal-static-method-was-generated"></a>EM1034: 확장 메서드 `M` 기본 형식에서 만들 수 없기 때문에 범주 내 생성 되지 않습니다 `T`합니다. 일반, 정적 메서드 생성 되었습니다.

이는 **경고** 하는 확장 메서드는 primivite에 형식 (예: `System.Int32`)를 찾을 수 있습니다. ObjC에는 기본 형식에 범주를 만들 수 없습니다. 대신 일반, 정적 메서드는 생성기 생성 됩니다.

<a name="EM1040" />

### <a name="em1040-property-p-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1040: 속성 `P` 매개 변수 형식으로 인해 생성 되지 않습니다 `T` 지원 되지 않습니다.

이 한 **경고** 하는 속성 `P` 무시 됩니다 (즉, 아무것도 생성 됨) 때문에 노출 된 형식 `T` 지원 되지 않습니다.

있어야 이유 자세한 정보를 제공 하는 이전 경고 입력 `T` 지원 되지 않습니다.

참고: 지원 되는 기능 도구의 새 버전으로 발전 합니다.

<a name="EM1041" />

### <a name="em1041-indexed-properties-on-t-is-not-generated-because-multiple-indexed-properties-are-not-supported"></a>EM1041:에서 인덱싱된 속성 `T` 여러 인덱싱된 속성은 지원 되지 않으므로 생성 되지 않습니다.

이 한 **경고** 하는 인덱싱된 속성에 `T` 무시 됩니다 (즉, 아무것도 생성 됨) 여러 인덱싱된 속성은 지원 되지 않으므로 합니다.

<a name="EM1050" />

### <a name="em1050-field-f-is-not-generated-because-of-field-type-t-is-not-supported"></a>EM1050: 필드 `F` 필드 형식으로 인해 생성 되지 않습니다 `T` 지원 되지 않습니다.

이 한 **경고** 하는 필드 `F` 무시 됩니다 (즉, 아무것도 생성 됨) 때문에 노출 된 형식 `T` 지원 되지 않습니다.

있어야 이유 자세한 정보를 제공 하는 이전 경고 입력 `T` 지원 되지 않습니다.

참고: 지원 되는 기능 도구의 새 버전으로 발전 합니다.

<a name="EM1051" />

### <a name="em1051-element-e-is-generated-instead-as-f-because-its-name-conflicts-with-an-important-objective-c-selector"></a>EM1051: 요소 `E` 대신 생성 됩니다으로 `F` 는 중요 한 목표 c 선택기와 해당 이름 충돌 하기 때문입니다.

이는 **경고** 하는 요소 `E` 대신 생성 됩니다으로 `F` 는 중요 한 목표 c 선택기와 해당 이름 충돌 하기 때문입니다.

선택기에는 [NSObjectProtocol](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc) objective c의는 중요 한 의미가 없으며 신중 하 게 재정의 해야 합니다.

참고: 예약 된 선택기 목록 도구의 새 버전으로 발전 합니다.

<a name="EM1052" />

### <a name="em1052-element-e-is-not-generated-its-name-conflicts-with-other-elements-on-the-same-class"></a>EM1052: 요소 `E` 생성 되지 않습니다는 동일한 클래스의 다른 요소와 해당 이름이 충돌 합니다.

이 한 **경고** 해당 요소 `E` 동일한 클래스의 다른 요소와 해당 이름이 충돌 하는 대로 생성 되지 않습니다.

<a name="EM1053" />

### <a name="em1053-target-e-is-not-supported-for-xamarinios-and-xamarinmac-only-the-framework-option-is-considered-supported-and-should-be-used"></a>EM1053: 대상 `E` Xamarin.iOS 및 Xamarin.Mac에 지원 되지 않습니다. 만 `framework` 옵션은 환경이 선택적 지원 및 사용 해야 합니다.

이 한 **경고** 대상으로 하는 `E` Xamarin.iOS 및 Xamarin.Mac 사용 사례에 대 한 지원 되지 않는 것으로 간주 됩니다. 

정적 또는 동적 Embeddinator 라이브러리의 소비 추가 작업 단계 또는 조정 해야 할 수 있습니다 및 대부분의 경우 사용 하지 않아야 합니다.

제거 프로그램 `--target` 매개 변수 또는 pass `--target=framework` 대신 합니다.

<!-- 2xxx: code generation -->

## <a name="em2xxx-code-generation"></a>EM2xxx: 코드 생성

<!-- 3xxx: reserved -->
<!-- 4xxx: reserved -->
<!-- 5xxx: reserved -->
<!-- 6xxx: reserved -->
<!-- 7xxx: reserved -->
<!-- 8xxx: reserved -->
<!-- 9xxx: reserved -->
