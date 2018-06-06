---
title: 바인딩 Objective C 라이브러리
description: 이 문서를 만드는 C#에 대 한 바인딩을 Objective C 코드에서 이벤트, 메서드, 사용자 지정 컨트롤을 바인딩하는 방법을 설명 하는 방법에 대 한 고급 개요를 제공 합니다.
ms.prod: xamarin
ms.assetid: 8A832A76-A770-1A7C-24BA-B3E6F57617A0
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/06/2018
ms.openlocfilehash: f7c4be4254ce3e3301c0c1e98d37134f5524c23b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782322"
---
# <a name="binding-objective-c-libraries"></a>바인딩 Objective C 라이브러리

Xamarin.iOS 또는 Xamarin.Mac를 사용할 때에 제 3 자 Objective C 라이브러리를 사용 하려는 경우 발생할 수 있습니다. 이런 경우, 기본 Objective C 라이브러리에 C# 바인딩을 만드는 Xamarin 바인딩 프로젝트를 사용할 수 있습니다. 프로젝트는 C#으로 iOS 및 Mac Api를 사용 하는 동일한 도구를 사용 합니다.

이 문서에서는 Objective-c Api를 바인딩하는 방법을 설명,이 대 한 표준.NET 메커니즘을 사용 해야 C Api만을 바인딩하는 경우 [P/Invoke 프레임 워크](http://www.mono-project.com/docs/advanced/pinvoke/)합니다.
C 라이브러리에 정적으로 연결 하는 방법에 대 한 세부 정보는에서 사용할 수는 [네이티브 라이브러리 연결](~/ios/platform/native-interop.md) 페이지.

우리의 도우미 참조 [바인딩 형식 참조 가이드](~/cross-platform/macios/binding/binding-types-reference.md)합니다.
또한 내부에서 수행 되는 작업에 대 한 자세한 내용을 보려면 원하는 확인할 우리의 [바인딩 개요](~/cross-platform/macios/binding/overview.md) 페이지.

IOS 및 Mac 라이브러리에 대 한 바인딩은 만들 수도 있습니다.
하지만이 페이지에서는 Mac 바인딩을 매우 유사을 바인딩하는 iOS에서 작업 하는 방법을 설명 합니다.

**IOS에 대 한 샘플 코드**

사용할 수 있습니다는 [iOS Binding 샘플](https://github.com/xamarin/monotouch-samples/tree/master/BindingSample) 프로젝트 바인딩을으로 실험해 보세요.

<a name="Getting_Started" />

## <a name="getting-started"></a>시작

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

바인딩을 만들 수는 가장 쉬운 방법은 Xamarin.iOS 바인딩 프로젝트를 만드는 것입니다.
할 수 있는이 Visual Studio가 Mac에 대 한 프로젝트 형식을 선택 하 여 **iOS > 라이브러리 > 바인딩 라이브러리**:

[![](objective-c-libraries-images/00-sml.png "이 작업을 수행 Visual Studio가 Mac 용 iOS 라이브러리 바인딩 라이브러리 프로젝트 형식을 선택 하 여")](objective-c-libraries-images/00.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

바인딩을 만들 수는 가장 쉬운 방법은 Xamarin.iOS 바인딩 프로젝트를 만드는 것입니다.
프로젝트 형식을 선택 하 여 Windows에서 Visual Studio에서 이렇게 하려면 **Visual C# > iOS > 바인딩 라이브러리 (iOS)**:

[![](objective-c-libraries-images/00vs-sml.png "iOS 바인딩 라이브러리 iOS")](objective-c-libraries-images/00vs.png#lightbox)

> [!IMPORTANT]
> 참고: 바인딩 프로젝트 **Xamarin.Mac** Mac.에만 Visual Studio에서 지원 됩니다

-----

생성된 된 프로젝트 편집할 수 있는 작은 템플릿이 두 개의 파일 포함: `ApiDefinition.cs` 및 `StructsAndEnums.cs`합니다.

`ApiDefinition.cs` 를 API 계약 정의 C#으로 기본 Objective-c API는 프로젝션 하는 방법을 설명 하는 파일입니다. 구문 및이 파일의 내용은이 문서의 토론의 주제 되며 해당 내용을 C#의 인터페이스 및 대리자 선언을 C#으로 제한 됩니다. `StructsAndEnums.cs` 파일은 입력할 수 있는 모든 정의 데 필요한 인터페이스 및 대리자에 의해 파일입니다. 열거형 값 및 코드를 사용할 수 있는 구조가 포함 됩니다.

<a name="Binding_an_API" />

## <a name="binding-an-api"></a>API 바인딩

포괄적인 바인딩을 수행 하려면 하 Objective-c API 정의 이해 하 고.NET Framework 디자인 지침을 잘 이해 해야 합니다.

라이브러리에 바인딩할 일반적으로 API 정의 파일에는로 시작 됩니다. API 정의 파일은 단순히 C# 소스 파일 C#의 인터페이스는 소수의 드라이브는 데 도움이 되는 특성으로 주석이 지정 된 바인딩을 포함 하 합니다.  이 파일은 C# 및 Objective-c 간의 계약 란 정의 합니다.

예를 들어 라이브러리에 대 한 간단한 api 파일입니다.

```csharp
using Foundation;

namespace Cocos2D {
  [BaseType (typeof (NSObject))]
  interface Camera {
    [Static, Export ("getZEye")]
    nfloat ZEye { get; }

    [Export ("restore")]
    void Restore ();

    [Export ("locate")]
    void Locate ();

    [Export ("setEyeX:eyeY:eyeZ:")]
    void SetEyeXYZ (nfloat x, nfloat y, nfloat z);

    [Export ("setMode:")]
    void SetMode (CameraMode mode);
  }
}
```

클래스를 정의 하는 위의 샘플 `Cocos2D.Camera` 에서 파생 되는 `NSObject` 기본 형식 (이 형식에서 가져온 `Foundation.NSObject`)는 정적 속성을 정의 하 고 (`ZEye`), 인수 및 메서드를 사용 하는 두 메서드 3 개를 사용 합니다. 인수입니다.

자세한 내용은 API 파일 및 사용할 수 있는 특성의 형식에 대해서는 [API 정의 파일](~/cross-platform/macios/binding/objective-c-libraries.md#The_API_definition_file) 아래 섹션.

전체 바인딩을 생성 하려면 일반적으로 네 가지 구성 요소를 처리 합니다.

-  API 정의 파일 (`ApiDefinition.cs` 서식 파일에).
-  선택 사항: 모든 열거형 형식, API 정의 파일에 필요한 구조체 (`StructsAndEnums.cs` 서식 파일에).
-  생성 된 바인딩 확장은 API를 제공 더 많은 C# 친숙 한 (모든 C# 파일을 프로젝트에 추가)는 선택 사항: 추가 원본입니다.
-  바인딩할 네이티브 라이브러리

이 차트는 파일 간의 관계를 보여 줍니다.

 [![](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png "이 차트는 파일 간의 관계를 보여 줍니다.")](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png#lightbox)

API 정의 파일 네임 스페이스 및 인터페이스 정의 (멤버는 인터페이스를 포함할 수 있는)를 포함할 클래스, 열거형, 대리자 또는 구조체 포함 하지 않아야 하 고 있습니다. API 정의 파일에는 API를 생성 하는 데 사용 될 하는 계약을 단순히 되었습니다.

모든 추가 코드가 필요 하 시겠습니까? 열거형 또는 지원 되는 클래스를 "CameraMode" 위의 예에서는 별도 파일을 호스트 해야 CS 파일에 존재 하지 않는 하 고 예를 들어 별도 파일에서 호스트 되어야 해야 하는 열거형 값은 `StructsAndEnums.cs` :

```csharp
public enum CameraMode {
    FlyOver, Back, Follow
}
```

`APIDefinition.cs` 파일와 결합 하는 `StructsAndEnum` 클래스 및 라이브러리의 코어 바인딩을 생성 하는 데 사용 됩니다. 로 결과 라이브러리를 사용할 수 있습니다-는 하지만 일반적으로, 사용자를 위해 일부 C# 기능을 추가 하려면 결과 라이브러리 튜닝 하려는 됩니다. 몇 가지 예로 구현 된 `ToString()` 메서드 C# 인덱서를 제공 하 고 일부 네이티브 형식에서 암시적 변환을 추가 또는 일부 방법의 강력한 형식의 버전을 제공 합니다. 이러한 향상 된이 기능은 여분의 C# 파일에 저장 됩니다. 단순히 프로젝트에 C# 파일을 추가 하 고이 빌드 프로세스에 포함 됩니다.

코드를 구현 하는 방법을 보여 줍니다 프로그램 `Extra.cs` 파일입니다. 사용 하려는 partial 클래스의 조합에서 생성 되는 partial 클래스를 확장 하는 이러한으로 확인 된 `ApiDefinition.cs` 및 `StructsAndEnums.cs` 바인딩 핵심:

```csharp
public partial class Camera {
    // Provide a ToString method
    public override string ToString ()
    {
         return String.Format ("ZEye: {0}", ZEye);
    }
}
```

라이브러리를 빌드 네이티브 바인딩을 생성 합니다.

이 바인딩의 완료 하려면 네이티브 라이브러리 프로젝트에 추가 해야 합니다.  하거나 끌어서 Finder에서 솔루션 탐색기에서 프로젝트에 네이티브 라이브러리를 놓으면 프로젝트에 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 하 여 네이티브 라이브러리를 추가 하 여 이렇게 하려면 **추가**  >  **파일 추가** 네이티브 라이브러리를 선택 합니다.
일반적으로 네이티브 라이브러리 "lib" 라는 단어로 시작 하 고 "포함" 확장명으로 끝납니다. 이 작업을 수행 하는 경우 Mac 용 Visual Studio 파일 두 개를 추가 합니다:.a 파일 자동으로 채워지는 C# 파일 및 네이티브 라이브러리에 포함 하는 방법에 대 한 정보를 포함 합니다.

 [![](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png "일반적으로 네이티브 라이브러리 단어 라이브러리 글자와 확장.a 끝")](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png#lightbox)

콘텐츠는 `libMagicChord.linkwith.cs` 파일에는이 라이브러리를 어떻게 사용할 수 있는지에 대 한 정보가 있고 결과 DLL 파일에이 이진을 패키지 하려면 IDE에 지시 합니다.

```csharp
using System;
using ObjCRuntime;

[assembly: LinkWith ("libMagicChord.a", SmartLink = true, ForceLoad = true)]
```

세부 정보를 사용 하는 방법에 대 한 전체는 [ `[LinkWith]` ](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute) 특성에 문서화 된는 [바인딩 형식 참조 가이드](~/cross-platform/macios/binding/binding-types-reference.md)합니다.

된 남게 됩니다 프로젝트를 빌드할 때 이제는 `MagicChords.dll` 바인딩과 네이티브 라이브러리를 포함 하는 파일입니다. 이 프로젝트를 배포할 수 있습니다 또는 자체에 대 한 다른 개발자에 게 결과 DLL을 사용 합니다.

경우에 따라 몇 가지 열거형 값, 대리자 정의 또는 다른 형식 필요 여부를 찾을 수 있습니다. 이 계약 뿐 이므로 API 정의 파일에 배치 하지 마십시오

<a name="The_API_definition_file" />

## <a name="the-api-definition-file"></a>API 정의 파일

API 정의 파일 인터페이스의 수를 이루어져 있습니다. API 정의의 인터페이스를 클래스 선언에 사용할 수 및로 데코 레이트 되어야 합니다는 [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) 특성을 클래스에 대 한 기본 클래스를 지정 합니다.

왜에서는 사용 하지 않은 클래스 인터페이스 대신 계약 정의 대 한 궁금하실 수도 있습니다. 쓴 다음 API 정의 파일에서 메서드 본문을 제공 하지 또는 의미 있는 값을 반환 하거나 예외를 throw 하는 본문을 제공 하지 않은 메서드에 대 한 계약을 허용 하기 때문에 인터페이스를 선택 했습니다.

하지만 인터페이스 클래스를 생성 한 기본으로 사용 하 고 있으므로 데코레이팅하를 실행 하는 바인딩 특성을 사용 하 여 계약의 다양 한 부분에 의존 해야 했습니다.

<a name="Binding_Methods" />

### <a name="binding-methods"></a>바인딩 메서드

할 수 있는 가장 간단한 바인딩 메서드를 바인딩하는 합니다. 방금 C# 명명 규칙을 사용 하 여 인터페이스에서 메서드를 선언 하 고 사용 하 여 메서드를 데코레이팅하는 [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 특성입니다. [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 특성은 C# 이름을 Xamarin.iOS 런타임에서 Objective-c 이름의 링크 합니다. 매개 변수는 [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 특성은 Objective-c 선택기의 이름입니다. 몇 가지 예:

```csharp
// A method, that takes no arguments
[Export ("refresh")]
void Refresh ();

// A method that takes two arguments and return the result
[Export ("add:and:")]
nint Add (nint a, nint b);

// A method that takes a string
[Export ("draw:atColumn:andRow:")]
void Draw (string text, nint column, nint row);
```

위의 샘플에서 인스턴스 메서드를 바인딩할 수는 방법을 보여 줍니다. 사용 해야 정적 메서드에 바인딩하는 [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) 다음과 같은 특성:

```csharp
// A static method, that takes no arguments
[Static, Export ("refresh")]
void Beep ();
```

계약 인터페이스의 일부인 하며 인터페이스의 정적 포트 대 인스턴스 선언이 개념이 없습니다 이므로 특성을 사용 하 여 다시 한 번 필요 하기 때문에 이것이 필요 합니다. 바인딩을 통해 특정 메서드를 숨길 하려는 경우이 메서드를 데코레이팅 할 수 있습니다는 [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) 특성입니다.

`btouch-native` 명령을 확인 합니다. null이 아니어야 하는 참조 매개 변수를 소개 합니다. 사용 하 여 특정 매개 변수에 대해 null 값을 허용 하려는 경우는 [ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute) 특성에 다음과 같이 매개 변수:

```csharp
[Export ("setText:")]
string SetText ([NullAllowed] string text);
```

참조 형식으로 내보낼 때의 [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 키워드 할당 의미 체계를 지정할 수도 있습니다. 이것은 데이터가 없는 유출 확인 하는 데 필요 합니다.

<a name="Binding_Properties" />

### <a name="binding-properties"></a>바인딩 속성

메서드와 마찬가지로 방금 Objective-c 속성 사용 하 여 바인딩됩니다는 [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 특성 및 C# 속성에 직접 매핑됩니다. 메서드를와 마찬가지로 속성으로 데코레이팅 될 수는 [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) 및 [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) 특성입니다.

사용 하는 경우는 [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 표지 btouch 네이티브 속성에는 특성에는 실제로 두 가지 방법 바인딩합니다: getter와 setter입니다. 내보낼가 제공 하는 이름을 **basename** 및 setter의 첫 글자를 설정 합니다. "설정" 이라는 단어 앞에 추가 하 여 계산 됩니다는 **basename** 대문자로 변환 하 고 수행 하는 선택기에는 인수입니다. 즉 `[Export ("label")]` 에 적용 된 속성에는 실제로 "label" 바인딩합니다 및 "setLabel:" Objective-c 메서드.

경우에 따라 Objective-c 속성 위에서 설명한 패턴을 준수 하지 않는 고 이름이 수동으로 덮어씁니다. 이러한 경우에 바인딩을 사용 하 여 생성 되는 방식을 제어할 수 있습니다는 [ `[Bind]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAttribute) getter 또는 setter에 예를 들어 특성:

```csharp
[Export ("menuVisible")]
bool MenuVisible { [Bind ("isMenuVisible")] get; set; }
```

이 바인딩됩니다 "isMenuVisible" 및 "setMenuVisible:". 필요에 따라 다음 구문을 사용 하 여 속성에 바인딩할 수 있습니다.

```csharp
[Category, BaseType(typeof(UIView))]
interface UIView_MyIn
{
  [Export ("name")]
  string Name();

  [Export("setName:")]
  void SetName(string name);
}
```

여기서 getter 및 setter 명시적으로 정의에서 같이 `name` 및 `setName` 위의 바인딩.

사용 하 여 정적 속성에 대 한 지원 외에도 [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute)를 사용 하 여 thread 정적 속성 데코레이팅 할 수 있습니다 [ `[IsThreadStatic]` ](~/cross-platform/macios/binding/binding-types-reference.md#IsThreadStaticAttribute), 예:

```csharp
[Export ("currentRunLoop")][Static][IsThreadStatic]
NSRunLoop Current { get; }
```

메서드를 플래그가 지정 된 일부 매개 변수를 사용 하는 것 처럼 [ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute)를 적용할 수 있습니다 [ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute) 해당 null은 예를 들어 속성에 대 한 유효한 값을 나타내는 속성을에:

```csharp
[Export ("text"), NullAllowed]
string Text { get; set; }
```

[ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute) setter에서 직접 매개 변수를 지정할 수도 있습니다.

```csharp
[Export ("text")]
string Text { get; [NullAllowed] set; }
```

#### <a name="caveats-of-binding-custom-controls"></a>사용자 지정 컨트롤을 바인딩하는 주의 사항

사용자 지정 컨트롤에 대 한 바인딩을 설정 하는 경우 다음 절차를 고려해 야 합니다.

1. **바인딩 속성을 정적 이어야** -속성의 바인딩을 정의 하는 경우는 [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) 특성을 사용 해야 합니다.
 2. **속성 이름은 정확히 일치 해야** -이름 속성에 바인딩하는 데 사용 된 사용자 지정 컨트롤에서 속성의 이름에 정확히 일치 해야 합니다.
3. **속성 형식이 정확히 일치 해야** -속성에 바인딩하는 데 사용 된 변수 형식 사용자 지정 컨트롤에서 속성의 종류를 정확히 일치 해야 합니다.
4. **중단점 및 getter/setter** -getter에 중단점 배치 또는 속성의 setter 메서드를 적중 되지 것입니다.
5. **콜백을 관찰** -사용자 지정 컨트롤의 속성 값 변경에에서 대 한 알림을 관찰 콜백을 사용 해야 합니다.

위에 나열 된 제한 사항 중 하나를 관찰 하지 않으면 런타임에 자동으로 실패 한 바인딩 발생할 수 있습니다.

<a name="MutablePattern" />

#### <a name="objective-c-mutable-pattern-and-properties"></a>Objective C 변경할 수 있는 패턴 및 속성

Objective C 프레임 워크의 일부 클래스는 변경할 수 있는 하위 클래스를 변경할 수 없는 요소를 사용 합니다. 예를 들어 `NSString` 은 변경할 수 없는 버전 동안 `NSMutableString` 변환이 허용 하는 서브 클래스입니다.

이 클래스에서 일반적으로 getter, setter 속성을 포함 하는 변경할 수 없는 기본 클래스를 참조입니다. 및 setter를 소개 하기 위한 버전은 변경할 수 있습니다. 실제로 가능한 C#과 함께이 아니기 때문에 C#과 함께 작동 하는 요소에이 관용구를 매핑할 했습니다.

이 C#에 매핑되어 있는지 방법은 getter와 setter는 모두 기본 클래스에 추가 하지만와 setter에 플래그를 지정 하는 것을 [ `[NotImplemented]` ](~/cross-platform/macios/binding/binding-types-reference.md#NotImplementedAttribute) 특성입니다.

를 변경할 수 있는 하위 클래스에 사용 된 [ `[Override]` ](~/cross-platform/macios/binding/binding-types-reference.md#OverrideAttribute) 특성 속성을 속성의 부모 동작을 재정의 하 고 실제로에 합니다.

예제:

```csharp
[BaseType (typeof (NSObject))]
interface MyTree {
    string Name { get; [NotImplemented] set; }
}

[BaseType (typeof (MyTree))]
interface MyMutableTree {
    [Override]
    string Name { get; set; }
}
```

<a name="Binding_Constructors" />

### <a name="binding-constructors"></a>바인딩 생성자

`btouch-native` 도구 fours 생성자는 지정 된 클래스에 대 한 클래스에 자동으로 생성 됩니다 `Foo`를 생성 합니다.

-  `Foo ()`: 기본 생성자 (Objective-c의 "init" 생성자에는 maps)
-  `Foo (NSCoder)`: NIB 파일의 역직렬화 동안 사용 되는 생성자 (Objective-c의 매핑됩니다 "initWithCoder:" 생성자).
-  `Foo (IntPtr handle)`: 생성자는 핸들 기반 만들 때 호출 된다는 런타임에서 런타임은 관리 되지 않는 개체에서 관리 되는 개체를 노출 해야 합니다.
-  `Foo (NSEmptyFlag)`:이 이중 초기화 방지 하기 위해 파생된 클래스에서 사용 됩니다.

정의 하는 생성자에 대 한 인터페이스 정의 내부에 다음과 같은 서명을 사용 하 여 선언 하는 데 필요한: 반환 해야 합니다는 `IntPtr` 값과 방법의 이름에는 생성자 여야 합니다. 예를 들어 바인딩할는 `initWithFrame:` 생성자,이 사용 합니다:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);
```

<a name="Binding_Protocols" />

### <a name="binding-protocols"></a>바인딩 프로토콜

API 디자인 문서의 섹션에 설명 된 대로 [모델 및 프로토콜에 논의](~/ios/internals/api-design/index.md#Models), Xamarin.iOS로 플래그가 지정 된 클래스에 Objective-c 프로토콜을 매핑하는 [ `[Model]` ](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute) 특성입니다. Objective C 대리자 클래스를 구현할 때 일반적으로 사용 됩니다.

일반 바인딩된 클래스 및 대리자 클래스의 가장 큰 차이 대리자 클래스가 하나 이상의 선택적 메서드를 포함할 수입니다.

예를 들어는 `UIKit` 클래스 `UIAccelerometerDelegate`, Xamarin.iOS 인수를 바인딩하는 방법입니다.

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface UIAccelerometerDelegate {
        [Export ("accelerometer:didAccelerate:")]
        void DidAccelerate (UIAccelerometer accelerometer, UIAcceleration acceleration);
}
```

에 대 한 정의에 선택적 메서드가 이므로 `UIAccelerometerDelegate` 즐길 수 있는 일은 없습니다. 프로토콜에 필요한 메서드를 추가 해야 하지만 [ `[Abstract]` ](~/cross-platform/macios/binding/binding-types-reference.md#AbstractAttribute) 특성을 메서드에 합니다. 이렇게 하면 구현을 제공 하도록 사용자에 실제로 본문이 메서드에 대 한 시작 합니다.

일반적으로 프로토콜 메시지에 응답 하는 클래스에서 사용 됩니다. 일반적으로 이렇게 Objective C에는 프로토콜의 방법에 응답 하는 개체의 인스턴스를 "대리자" 속성에 할당 합니다.

지점 모두 Objective-c 느슨하게 결합 된 스타일을 지원 하기 위해 Xamarin.iOS의 규칙은의 인스턴스는 `NSObject` 와 할당할 수 있습니다는 대리자에도 노출 하 여 강력한 형식의 버전을 합니다. 이러한 이유로, 일반적으로 제공 둘 다는 `Delegate` 강력한 형식의 속성 및 `WeakDelegate` 느슨하게 입력 되는 합니다. 일반적으로 자유로운 형식의 버전을 바인딩한 [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute), 데 사용 되는 [ `[Wrap]` ](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) 특성을 강력한 형식의 버전을 제공 합니다.

바인딩하 방법을 표시는 `UIAccelerometer` 클래스:

```csharp
[BaseType (typeof (NSObject))]
interface UIAccelerometer {
        [Static] [Export ("sharedAccelerometer")]
        UIAccelerometer SharedAccelerometer { get; }

        [Export ("updateInterval")]
        double UpdateInterval { get; set; }

        [Wrap ("WeakDelegate")]
        UIAccelerometerDelegate Delegate { get; set; }

        [Export ("delegate", ArgumentSemantic.Assign)][NullAllowed]
        NSObject WeakDelegate { get; set; }
}
```

<a name="iOS7ProtocolSupport" />

**MonoTouch 7.0의에서 새로운 기능**

바인딩 기능 새롭고 향상 된 프로토콜 MonoTouch 7.0 부터는 통합 되었습니다.  이 새로운 지원 간편 하 게 Objective-c 관용구를 사용 하 여 지정된 된 클래스에서 하나 이상의 프로토콜을 채택 합니다.

모든 프로토콜 정의 `MyProtocol` Objective-c, 즉 이제는 `IMyProtocol` 는 프로토콜에서 필요한 모든 메서드가 나열 인터페이스 뿐만 아니라 모든 선택적 메서드를 제공 하는 확장 클래스입니다.  위의 편집기를 사용 하면 이전 추상 모델 클래스의 별도 하위 클래스를 사용 하지 않고도 프로토콜 메서드를 구현 하는 개발자 Xamarin Studio에서 새로운 지원 결합 합니다.

포함 된 모든 정의 [ `[Protocol]` ](~/cross-platform/macios/binding/binding-types-reference.md#ProtocolAttribute) 특성은 실제로 크게 프로토콜을 사용 하는 방식을 개선 하는 세 가지 지원 클래스를 생성 합니다.

```csharp
    // Full method implementation, contains all methods
    class MyProtocol : IMyProtocol {
        public void Say (string msg);
        public void Listen (string msg);
    }

    // Interface that contains only the required methods
    interface IMyProtocol: INativeObject, IDisposable {
        [Export ("say:")]
        void Say (string msg);
    }

    // Extension methods
    static class IMyProtocol_Extensions {
        public static void Optional (this IMyProtocol this, string msg);
        }
    }
```

**클래스 구현을** 의 개별 메서드를 재정의 하 고 전체 형식 안전성을 얻을 수 있는 완전 한 추상 클래스를 제공 합니다.  하지만 C# 다중 상속을 지원 하지 않으므로, 인해 있습니다 시나리오 수 있는 다른 기본 클래스를 사용 해야 하지만는 하는 인터페이스를 구현 하려면는

생성 된 **인터페이스 정의** 제공 합니다.  에 프로토콜에서 필요한 모든 메서드는 인터페이스입니다.  따라서 단순히 인터페이스를 구현 하 여 프로토콜을 구현 하려는 개발자가 있습니다.  런타임은 프로토콜 채택으로 형식을 자동으로 등록 됩니다.

고 해당 인터페이스만 필요한 메서드를 나열 합니다. 선택적 메서드를 노출 않는 됩니다.  즉, 프로토콜을 채택 하는 클래스 전체 형식 필요한 방법에 대 한 검사 받아볼 있지만 약한 형식 지정에 의존 해야 합니다 (사용 하 여 직접 [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 특성 및 서명 일치)는 선택 사항에 대 한 프로토콜 메서드입니다.

바인딩 도구는를 프로토콜을 사용 하는 API를 사용 하기 편리 하 게 하는 모든 선택적 메서드를 노출 하는 확장 메서드 클래스도 생성 합니다.  이 API를 사용 하는 상태로 됩니다 프로토콜 모든 메서드가 있는 것으로 취급할 수를 의미 합니다.

API에는 프로토콜 정의 사용 하려는 경우에 API 정의에서 기본 빈 인터페이스를 작성 해야 합니다.  MyProtocol API에서 사용 하려는 경우이 작업을 수행 해야 합니다.

```csharp
[BaseType (typeof (NSObject))]
[Model, Protocol]
interface MyProtocol {
    // Use [Abstract] when the method is defined in the @required section
    // of the protocol definition in Objective-C
    [Abstract]
    [Export ("say:")]
    void Say (string msg);

    [Export ("listen")]
    void Listen ();
}

interface IMyProtocol {}

[BaseType (typeof(NSObject))]
interface MyTool {
    [Export ("getProtocol")]
    IMyProtocol GetProtocol ();
}
```

위의 필요 하기 때문에 바인딩할 때는 `IMyProtocol` 존재 하지 않습니다, 즉 빈 인터페이스를 제공 해야 하는 이유입니다.

#### <a name="adopting-protocol-generated-interfaces"></a>프로토콜에서 생성 된 인터페이스를 채택합니다.

구현할 때마다 다음과 같이 프로토콜에 대해 생성 된 인터페이스 중 하나:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

인터페이스 메서드에 대 한 구현 자동으로 가져옵니다 내보내기 적절 한 이름으로이에 해당 하는 다음과 같습니다.

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    [Export ("getRowHeight:")]
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

암시적 또는 명시적으로 인터페이스를 구현 하는 경우에 중요 하지 않습니다.

<a name="Binding_Class_Extensions" />

### <a name="binding-class-extensions"></a>바인딩 클래스 확장

Objective C의 C#의 확장 메서드와 비슷합니다 개념상에서 새로운 메서드가 있는 클래스를 확장 하는 것이 같습니다. 사용할 수 있는 경우 다음이 방법 중 하나는 [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) 특성으로 Objective-c 메시지 받는 사람으로 메서드를 나타내는 플래그입니다.

예를 들어, Xamarin.iOS에서는 연결에 정의 된 확장 메서드 `NSString` 때 `UIKit` 메서드를 가져옵니다는 `NSStringDrawingExtensions`, 다음과 같이 합니다.

```csharp
[Category, BaseType (typeof (NSString))]
interface NSStringDrawingExtensions {
    [Export ("drawAtPoint:withFont:")]
    CGSize DrawString (CGPoint point, UIFont font);
}
```

<a name="Binding_Objective-C_Argument_Lists" />

### <a name="binding-objective-c-argument-lists"></a>바인딩 Objective-c 인수 목록

Objective C variadic 인수를 지원합니다. 예를 들어:

```objc
- (void) appendWorkers:(XWorker *) firstWorker, ...
  NS_REQUIRES_NIL_TERMINATION ;
```

C#에서이 메서드를 호출 하려면 다음과 같은 서명을 만드는 데 원하는 됩니다.

```csharp
[Export ("appendWorkers"), Internal]
void AppendWorkers (Worker firstWorker, IntPtr workersPtr)
```

라이브러리에 노출 하지만, 사용자에 게 위의 API 숨기기 internal로 메서드를 선언 합니다. 다음과 같은 메서드를 작성할 수 있습니다.

```csharp
public void AppendWorkers(params Worker[] workers)
{
    if (workers == null)
         throw new ArgumentNullException ("workers");

    var pNativeArr = Marshal.AllocHGlobal(workers.Length * IntPtr.Size);
    for (int i = 1; i < workers.Length; ++i)
        Marshal.WriteIntPtr (pNativeArr, (i - 1) * IntPtr.Size, workers[i].Handle);

    // Null termination
    Marshal.WriteIntPtr (pNativeArr, (workers.Length - 1) * IntPtr.Size, IntPtr.Zero);

    // the signature for this method has gone from (IntPtr, IntPtr) to (Worker, IntPtr)
    WorkerManager.AppendWorkers(workers[0], pNativeArr);
    Marshal.FreeHGlobal(pNativeArr);
}
```

<a name="Binding_Fields" />

### <a name="binding-fields"></a>바인딩 필드

경우에 따라 라이브러리에 선언 된 공용 필드에 액세스 합니다.

일반적으로 이러한 필드는 참조 해야 하는 문자열 또는 정수 값을 포함 합니다. 일반적으로 특정 알림 메시지를 나타내는 문자열로 및 사전에 키로 사용 됩니다.

필드를 바인딩하려면 프로그램 인터페이스 정의 파일에 속성을 추가 하 고 사용 하 여 속성 데코레이팅는 [ `[Field]` ](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute) 특성입니다. 이 특성에는 매개 변수 사용: C 이름을 조회 하는 기호입니다. 예를 들어:

```csharp
[Field ("NSSomeEventNotification")]
NSString NSSomeEventNotification { get; }
```

파생 되지 않은 정적 클래스의 다양 한 필드를 배치 하려는 경우 `NSObject`를 사용할 수 있습니다는 [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute_Class) 다음과 같이 클래스의 특성:

```csharp
[Static]
interface LonelyClass {
    [Field ("NSSomeEventNotification")]
    NSString NSSomeEventNotification { get; }
}
```

위의 생성 됩니다는 `LonelyClass` 에서 파생 되지 않은 `NSObject` 에 대 한 바인딩을 포함 됩니다는 `NSSomeEventNotification` 
 `NSString` 으로 노출는 `NSString`합니다.

[ `[Field]` ](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute) 특성 데이터 형식에 적용할 수 있습니다.

-  `NSString` 참조 (읽기 전용 속성만 해당)
-  `NSArray` 참조 (읽기 전용 속성만 해당)
-  32 비트 정수 (`System.Int32`)
-  64 비트 정수 (`System.Int64`)
-  32 비트 부동 소수점 (`System.Single`)
-  64 비트 부동 소수점 (`System.Double`)
-  `System.Drawing.SizeF`
-  `CGSize`

네이티브 필드 이름 외에 필드 위치, 라이브러리 이름을 전달 하 여 라이브러리 이름을 지정할 수 있습니다.

```csharp
[Static]
interface LonelyClass {
    [Field ("SomeSharedLibrarySymbol", "SomeSharedLibrary")]
    NSString SomeSharedLibrarySymbol { get; }
}
```

정적으로 연결 하는 경우 라이브러리가 없을 바인딩할를 사용 해야 하므로 `__Internal` 이름:

```csharp
[Static]
interface LonelyClass {
    [Field ("MyFieldFromALibrary", "__Internal")]
    NSString MyFieldFromALibrary { get; }
}
```

<a name="Binding_Enums" />

### <a name="binding-enums"></a>바인딩 열거형

추가할 수 있습니다 `enum` 바인딩을에서 직접 파일을 보다 쉽게 (하는 바인딩 및 최종 프로젝트 모두에 컴파일할 수)는 다른 소스 파일을 사용 하지 않고 API 정의-안에 사용 하도록 합니다.

예제:

```csharp
[Native] // needed for enums defined as NSInteger in ObjC
enum MyEnum {}

interface MyType {
    [Export ("initWithEnum:")]
    IntPtr Constructor (MyEnum value);
}
```

바꿀 직접 열거형을 만들 수 이기도 `NSString` 상수입니다. 이 경우 생성자를 **자동으로** 있습니다에 대 한 열거형 값과 NSString 상수를 변환 하는 메서드를 만듭니다.

예제:

```csharp
enum NSRunLoopMode {

    [DefaultEnumValue]
    [Field ("NSDefaultRunLoopMode")]
    Default,

    [Field ("NSRunLoopCommonModes")]
    Common,

    [Field (null)]
    Other = 1000
}

interface MyType {
    [Export ("performForMode:")]
    void Perform (NSString mode);

    [Wrap ("Perform (mode.GetConstant ())")]
    void Perform (NSRunLoopMode mode);
}
```

위의 예에서 데코레이팅 할 수 `void Perform (NSString mode);` 와 [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) 특성입니다. 이렇게 설정이 하면 **숨기기** 바인딩 소비자에서 상수 기반 API입니다.

그러나이 제한할는 더 좋은 API가 대체 사용으로 형식의 서브클래싱하는 [ `[Wrap]` ](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) 특성입니다. 이러한 생성 된 메서드는 아닙니다. `virtual`, 즉, 있습니다 won't 여부 될 수 있는 것을 무시할 수, 올바른 선택 합니다.

원래 표시 하는 대체 항목은 `NSString`-기반으로 정의 `[Protected]`합니다. 이렇게 하면 필요한 경우 사용할 서브클래싱을 wrap'ed 버전 계속 작동 되며 재정의 된 메서드를 호출 합니다.

### <a name="binding-nsvalue-nsnumber-and-nsstring-to-a-better-type"></a>바인딩 `NSValue`, `NSNumber`, 및 `NSString` 더 나은 형식

[ `[BindAs]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) 특성을 사용 하면 바인딩 `NSNumber`, `NSValue` 및 `NSString`(열거형)를 보다 정확 하 게 C# 형식으로. 특성 보다 효율적이 고, 더 정확 하 게 만드는 데 사용할 수는 네이티브 API 통해.NET API입니다.

(반환 값)에 대 한 메서드, 매개 변수 및 사용 하 여 속성 데코레이팅 할 수 있습니다 [ `[BindAs]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute)합니다. 유일한 제한 사항은 구성원은 **하지 않아야** 안에 [ `[Protocol]` ](~/cross-platform/macios/binding/binding-types-reference.md#ProtocolAttribute) 또는 [ `[Model]` ](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute) 인터페이스입니다.

예를 들어:

```csharp
[return: BindAs (typeof (bool?))]
[Export ("shouldDrawAt:")]
NSNumber ShouldDraw ([BindAs (typeof (CGRect))] NSValue rect);
```

이 출력 됩니다.

```csharp
[Export ("shouldDrawAt:")]
bool? ShouldDraw (CGRect rect) { ... }
```

작업이 수행 됩니다 내부적으로 `bool?`  <->  `NSNumber` 및 `CGRect`  <->  `NSValue` 변환 합니다.

[`[BindAs]`](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) 또한 배열을 지원 `NSNumber` `NSValue` 및 `NSString`(열거형)입니다.

예를 들어:

```csharp
[BindAs (typeof (CAScroll []))]
[Export ("supportedScrollModes")]
NSString [] SupportedScrollModes { get; set; }
```

이 출력 됩니다.

```csharp
[Export ("supportedScrollModes")]
CAScroll [] SupportedScrollModes { get; set; }
```

`CAScroll` 한 `NSString` 열거형, 백업에서는 오른쪽을 인출 합니다 `NSString` 값 및 형식 변환을 처리 합니다.

참조 하십시오는 [ `[BindAs]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) 설명서 지원 되는 변환 형식을 볼 수 있습니다.

<a name="Binding_Notifications" />

### <a name="binding-notifications"></a>바인딩 알림

알림은에 게시 된 메시지는 `NSNotificationCenter.DefaultCenter` 를 다른 응용 프로그램의 특정 부분에서 메시지를 브로드캐스팅하려면 메커니즘으로 사용 됩니다. 개발자가 일반적으로 사용 하 여 알림을 구독할는 [NSNotificationCenter](https://developer.xamarin.com/api/type/Foundation.NSNotificationCenter/)의 [AddObserver](https://developer.xamarin.com/api/type/Foundation.NSNotificationCenter/M/AddObserver/) 메서드. 에 저장 된 페이로드 메시지 알림 센터에 게시 하는 응용 프로그램, 때는 일반적으로 포함 된 [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/) 사전입니다. 이 사전은 약하게 형식화과 오류가 발생 하기 쉬우므로있지 않습니다 옵트아웃 정보를 가져오는 사용자는 일반적으로 키가 사전에 저장할 수 있는 값의 형식과 사전에 사용할 수 있는 설명서를 읽이 필요가 있습니다. 경우에 따라 키의 존재는 부울 값으로 사용 됩니다.

Xamarin.iOS 바인딩 생성기 알림 바인딩하는 개발자를 위한 지원을 제공 합니다. 설정 하면이 위해는 [ `[Notification]` ](~/cross-platform/macios/binding/binding-types-reference.md#NotificationAttribute) 되었습니다 되었습니다 속성에 대 한 특성으로 태그가 지정 된는 [ `[Field]` ](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute) 속성 (수 공개 또는 개인).

이 특성을 사용 하 여 알림을 하지 않는 페이로드를 전송 하는 인수 없이 하거나 지정할 수 있습니다는 `System.Type` "EventArgs"로 끝나는 이름의 일반적으로 API 정의에서 다른 인터페이스를 참조 하는 합니다. 생성기 바뀝니다 인터페이스 클래스 서브클래싱하는 `EventArgs` 여기에 나열 된 속성을 모두 포함 됩니다. [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 특성은 값을 인출 Objective-c 사전을 조회 하는 데 사용 되는 키의 이름을 나열 하는 EventArgs 클래스에 사용 해야 합니다.

예를 들어:

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassDidStartNotification")]
    NSString DidStartNotification { get; }
}
```

위의 코드에서는 중첩된 된 클래스를 발생 `MyClass.Notifications` 다음 방법을 사용 하 여:

```csharp
public class MyClass {
   [..]
   public Notifications {
      public static NSObject ObserveDidStart (EventHandler<NSNotificationEventArgs> handler)
   }
}
```

사용자를 다음 쉽게 알림을 구독 하려면에 게시는 [NSDefaultCenter](https://developer.xamarin.com/api/property/Foundation.NSNotificationCenter.DefaultCenter/) 다음과 같은 코드를 사용 하 여:

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
});
```

반환 된 값 `ObserveDidStart` 쉽게 다음과 같이 알림 수신을 중지할 데 사용할 수 있습니다.

```csharp
token.Dispose ();
```

호출할 수 있습니다 또는 [NSNotification.DefaultCenter.RemoveObserver](https://developer.xamarin.com/api/member/Foundation.NSNotificationCenter.RemoveObserver/p/Foundation.NSObject/) 토큰을 전달 합니다. 프로그램 알림 매개 변수를 포함 하는 경우에 도우미 지정 해야 `EventArgs` 다음과 같은 인터페이스:

```csharp
interface MyClass {
    [Notification (typeof (MyScreenChangedEventArgs)]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}

// The helper EventArgs declaration
interface MyScreenChangedEventArgs {
    [Export ("ScreenXKey")]
    nint ScreenX { get; set; }

    [Export ("ScreenYKey")]
    nint ScreenY { get; set; }

    [Export ("DidGoOffKey")]
    [ProbePresence]
    bool DidGoOff { get; }
}
```

위의 생성 한 `MyScreenChangedEventArgs` 클래스와 `ScreenX` 및 `ScreenY` 에서 데이터를 인출 하는 속성은 [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/) "ScreenXKey" 및 "ScreenYKey" 키를 사용 하 여 사전 각각 적절 한 변환을 적용 합니다. `[ProbePresence]` 특성은 키 설정 된 경우를 검색 하는 생성기에 사용 된 `UserInfo`, 값을 추출 하는 것이 아니라 합니다. 이 경우 여기서 키의 존재는 값 (일반적으로 값에 대 한 부울)에 사용 됩니다.

이 옵션을 사용 하면 다음과 같은 코드를 작성할 수 있습니다.

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

<a name="Binding_Categories" />

### <a name="binding-categories"></a>바인딩 범주

범주는 일련의 메서드와 클래스에서 사용할 수 있는 속성을 확장 하는 데는 Objective-c 메커니즘입니다.   실제로 기본 클래스의 기능을 확장 하거나 사용 됩니다 (예를 들어 `NSObject`) 특정 프레임 워크에 연결 된 경우 (예를 들어 `UIKit`)을 사용할 수는 있지만 새로운 프레임 워크에 연결 되어 있는 경우에 해당 메서드를 만드는 합니다.   경우에 따라 다른 기능으로는 클래스의 기능을 구성 하 사용 됩니다.   비슷하기 개념상에서 C# 확장 메서드입니다. 이 어떤 범주에에서 나타나는 모양을 Objective c:

```csharp
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

위의 예에서는 경우에의 인스턴스를 확장 하는 것을 라이브러리 `UIView` 메서드로 `makeBackgroundRed`합니다.

사용할 수 있는 바인딩하려면는 [ `[Category]` ](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute) 인터페이스 정의의 특성입니다.  사용 하는 경우는 [ `[Category]` ](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute) 특성의 의미는 [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) 특성이 기본 클래스를 확장 하, 확장 형식이 되도록 지정 하는 데 사용 되 고에서 변경 합니다.

에서는 다음 방법을 `UIView` 확장은 바인딩된 있으며 확장 메서드가 C#으로 변환 합니다.

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

위의 만듭니다는 `MyUIViewExtension` 포함 하는 클래스는 `MakeBackgroundRed` 확장 메서드.  즉, 이제 "MakeBackgroundRed" 호출할 수 있습니다에 `UIView` Objective c 얻을 동일한 기능을 제공 하는 하위 클래스 경우에 따라 다른 범주에는 시스템에서 클래스 확장 않고 장식 용도로 기능을 구성 하도록 사용 됩니다.  다음과 같습니다.

```csharp
@interface SocialNetworking (Twitter)
- (void) postToTwitter:(Message *) message;
@end

@interface SocialNetworking (Facebook)
- (void) postToFacebook:(Message *) message andPicture: (UIImage*)
picture;
@end
```

사용할 수 있지만 [ `[Category]` ](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute) 특성도 선언의이 장식 스타일에 대 한 수도 추가 하면 해당 클래스 정의에 모두 있습니다.  동일한 작업을 수행할 것 둘 다에 해당 합니다.

```csharp
[BaseType (typeof (NSObject))]
interface SocialNetworking {
}

[Category]
[BaseType (typeof (SocialNetworking))]
interface Twitter {
    [Export ("postToTwitter:")]
    void PostToTwitter (Message message);
}

[Category]
[BaseType (typeof (SocialNetworking))]
interface Facebook {
    [Export ("postToFacebook:andPicture:")]
    void PostToFacebook (Message message, UIImage picture);
}
```

범주를 병합 하려면 이러한 경우에만 짧습니다.

```csharp
[BaseType (typeof (NSObject))]
interface SocialNetworking {
    [Export ("postToTwitter:")]
    void PostToTwitter (Message message);

    [Export ("postToFacebook:andPicture:")]
    void PostToFacebook (Message message, UIImage picture);
}
```

<a name="Binding_Blocks" />

### <a name="binding-blocks"></a>바인딩 블록

블록은 없습니다. 목표를 C# 무명 메서드를 해당 하는 기능을 도입 하는 Apple에 의해 도입 된 새 구문을 예를 들어는 `NSSet` 클래스는 이제이 메서드를 노출 합니다.

```csharp
- (void) enumerateObjectsUsingBlock:(void (^)(id obj, BOOL *stop) block
```

라는 메서드를 선언 하는 위의 설명 `enumerateObjectsUsingBlock:` 라는 하나의 인수를 사용 하는 `block`합니다. 이 블록 현재 환경 ("this"이 포인터를 지역 변수 및 매개 변수에 대 한 액세스) 캡처에 대 한 지원을 있다는 점에서 C# 무명 메서드와 비슷합니다. 위의 메서드 `NSSet` 두 개의 매개 변수가 있는 블록을 호출는 `NSObject` (의 `id obj` 부분)와 부울에 대 한 (의 `BOOL *stop`) 부분.

이 종류 API의 btouch 바인딩할와 C# 위임 후 다음과 같은 API 진입점에서 참조 블록 형식 시그니처를 먼저 선언 해야:

```csharp
// This declares the callback signature for the block:
delegate void NSSetEnumerator (NSObject obj, ref bool stop)

// Later, inside your definition, do this:
[Export ("enumerateObjectUsingBlock:")]
void Enumerate (NSSetEnumerator enum)
```

및이 인해 사용자 코드에서 C# 함수를 호출할 수는 이제:

```csharp
var myset = new NSMutableSet ();
myset.Add (new NSString ("Foo"));

s.Enumerate (delegate (NSObject obj, ref bool stop){
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

원하는 경우에 람다를 사용할 수 있습니다.

```csharp
var myset = new NSMutableSet ();
mySet.Add (new NSString ("Foo"));

s.Enumerate ((obj, stop) => {
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

<a name="GeneratingAsync" />

### <a name="asynchronous-methods"></a>비동기 메서드

바인딩 생성기 비동기에 적합 한 방식으로 특정 클래스에 메서드를 바꿀 수 (작업 또는 작업을 반환 하는 메서드&lt;T&gt;).

사용할 수는 [ `[Async]` ](~/cross-platform/macios/binding/binding-types-reference.md#AsyncAttribute) void를 반환 하 고 마지막 인수가 콜백 메서드에 대 한 특성입니다.  이 메서드를 적용 하면 바인딩 생성기에서 접미사와 해당 메서드의 버전을 생성 `Async`합니다.  콜백 매개 변수를 사용 하는 경우 반환 값 됩니다는 `Task`, 콜백 매개 변수를 사용 하는 경우 결과 됩니다는 `Task<T>`합니다.  콜백이 여러 매개 변수를 사용 하는 경우 설정 해야는 `ResultType` 또는 `ResultTypeName` 생성 된 형식의 모든 속성을 포함 하는 원하는 이름을 지정할 수 있습니다.

예제:

```csharp
[Export ("loadfile:completed:")]
[Async]
void LoadFile (string file, Action<string> completed);
```

위의 코드는 모두는 LoadFile 메서드를 생성 및:

```csharp
[Export ("loadfile:completed:")]
Task<string> LoadFileAsync (string file);
```

<a name="Surfacing_Strong_Types" />

### <a name="surfacing-strong-types-for-weak-nsdictionary-parameters"></a>약한 NSDictionary 매개 변수를 강력한 형식에서의 표시가

Objective C API의 여러 위치에서 매개 변수가 전달 같이 약한 형식의 `NSDictionary` 특정 키와 값을 있지만 이러한 Api는 오류 (잘못 된 키를 전달 하 고 경고 없이 받을 수; 잘못 된 값을 전달 하 고 경고 없이 얻을 수 있습니다) 발생 하기 쉬운 하면 당황 스 러 및 사용 하려면 여러 ½ à ´ ï 가능한 키 이름 및 값을 조회 하는 설명서를 필요에 따라 합니다.

솔루션 강력한 형식의 버전의 API 및 원리를 자세히 파악할수록 매핑하는 다양 한 기본 키와 값을 제공 하는 강력한 형식의 버전을 제공 하는 것입니다.

따라서 예를 들어 Objective-c API 허용 하는 경우는 `NSDictionary` 키를 사용 하도록 문서화 되 고 `XyzVolumeKey` 이 `NSNumber` 볼륨 값 0.0에서 1.0 및 `XyzCaptionKey` 문자열을 사용 하는, 사용자가 멋진 API 원하는 다음과 같습니다.

```csharp
public class  XyzOptions {
    public nfloat? Volume { get; set; }
    public string Caption { get; set; }
}
```

`Volume` 속성은 nullable float으로 정의 처럼 Objective C의 규칙 이러한 사전이 되므로 여기서 값 설정 되어 있지 않습니다 시나리오는 값을 가질 필요가 없습니다.

이 작업을 수행 하려면 다음을 수행 하려면:

* 서브클래싱하는 강력한 형식의 클래스를 만든 [DictionaryContainer](https://developer.xamarin.com/api/type/Foundation.DictionaryContainer/) 하 고 각 속성에 대 한 다양 한 getter 및 setter를 제공 합니다.
* 수행 방법에 대 한 오버 로드 선언 `NSDictionary` 수행할 새 강력한 형식의 버전입니다.

수동으로 강력한 형식의 클래스를 하거나 만들 수도 있고 생성자를 사용 하 여 작업을 수행할 수 있습니다.  처음이 작업을 수동으로 진행 되는 상황을 이해 하는 방법을 차례로 자동을 탐색 합니다.

이 대 한 지원 파일을 만들어야 할 API 계약에는 설명 하지 않습니다.  이 XyzOptions 클래스를 만드는 작성 해야 합니다.

```csharp
public class XyzOptions : DictionaryContainer {
# if !COREBUILD
    public XyzOptions () : base (new NSMutableDictionary ()) {}
    public XyzOptions (NSDictionary dictionary) : base (dictionary){}

    public nfloat? Volume {
       get { return GetFloatValue (XyzOptionsKeys.VolumeKey); }
       set { SetNumberValue (XyzOptionsKeys.VolumeKey, value); }
    }
    public string Caption {
       get { return GetStringValue (XyzOptionsKeys.CaptionKey); }
       set { SetStringValue (XyzOptionsKeys.CaptionKey, value); }
    }
# endif
}
```

다음 하위 수준 API 기반으로 상위 수준 API를 표시 하는 래퍼 메서드를 제공 해야 합니다.

```csharp
[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

사용자가 API는 덮어쓸 수 필요가 없는 경우 숨길 수 있습니다 안전 하 게 NSDictionary 기반 API를 사용 하 여는 [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) 특성입니다.

사용 하 여 볼 수 있듯이 [ `[Wrap]` ](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) 특성을 새 API 진입점을 노출 하 고 우리의 강력한 형식의 사용 하 여 노출 했습니다 `XyzOptions` 클래스입니다.  래퍼 메서드가 null을 전달할 수도 있습니다.

언급 되지 않은 것 중 하나에 이제는 `XyzOptionsKeys` 값에서 제공 합니다.  일반적으로 그룹화 하는 API와 같은 정적 클래스에 표시 되는 키 `XyzOptionsKeys`, 다음과 같이 합니다.

```csharp
[Static]
class XyzOptionKeys {
    [Field ("kXyzVolumeKey")]
    NSString VolumeKey { get; }

    [Field ("kXyzCaptionKey")]
    NSString CaptionKey { get; }
}
```

에 대해 알아보겠습니다 이러한 강력한 형식의 사전 만들기에 대 한 자동 지원 합니다.  충분 한 상용구, 이렇게 및 사전 외부 파일을 사용 하는 대신 프로그램 API 계약에서 직접 정의할 수 있습니다.

강력한 형식의 사전을 만들려는 API의 인터페이스를 소개 하 고으로 데코레이팅는 [StrongDictionary](~/cross-platform/macios/binding/binding-types-reference.md#StrongDictionary) 특성입니다.  이 속성을 확인할 생성기를 만들도록 클래스에서 파생 되는 사용자 인터페이스와 동일한 이름을 가진 `DictionaryContainer` 것에 대 한 강력한 형식의 접근자를 제공 합니다.

[ `[StrongDictionary]` ](~/cross-platform/macios/binding/binding-types-reference.md#StrongDictionary) 특성은 하나의 매개 변수가 사전 키를 포함 하는 정적 클래스의 이름입니다.  다음 인터페이스의 각 속성에는 강력한 형식의 접근자 될 것입니다.  기본적으로 코드는 고 접근자를 만드는 정적 클래스에 "키" 접미사와 함께 속성의 이름을 사용 합니다.

외부 파일이 나 수동으로 getter 및 setter에 대 한 모든 속성을 생성 하지도 수동으로 키를 조회 하지는 더 이상 필요 하면 강력한 형식의 접근자를 만들 사용자가 직접 합니다.

다음은 전체 바인딩을 모양을입니다.

```csharp
[Static]
class XyzOptionKeys {
    [Field ("kXyzVolumeKey")]
    NSString VolumeKey { get; }

    [Field ("kXyzCaptionKey")]
    NSString CaptionKey { get; }
}
[StrongDictionary ("XyzOptionKeys")]
interface XyzOptions {
    nfloat Volume { get; set; }
    string Caption { get; set; }
}

[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

참조 하는 데 필요한 경우에 프로그램 `XyzOption` 멤버 다른 필드 (즉 이름이 아니라 접미사를 사용 하 여 속성의 `Key`)를 사용 하 여 속성 데코레이팅 할 수 있습니다는 [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 를 이름으로 특성 사용 하려고 합니다.

<a name="Type_mappings" />

## <a name="type-mappings"></a>형식 매핑

이 섹션에서는 Objective C 형식을 C#의 형식에 매핑되는 방법을 설명 합니다.

<a name="Simple_Types" />

### <a name="simple-types"></a>단순 형식

다음 표에서 Xamarin.iOS 세계 Objective C와 CocoaTouch 세계에서 형식을 매핑하 해야 하는 방법을 보여 줍니다.

|Objective C 형식 이름|Xamarin.iOS 통합 API 유형|
|---|---|
|`BOOL`, `GLboolean`|`bool`|
|`NSInteger`|`nint`|
|`NSUInteger`|`nuint`|
|`CFTimeInterval` / `NSTimeInterval`|`double`|
|`NSString` ([NSString 바인딩에 더 많은](~/ios/internals/api-design/nsstring.md))|`string`|
|`char *`|`string` (참고 항목: [ `[PlainString]` ](~/cross-platform/macios/binding/binding-types-reference.md#plainstring))|
|`CGRect`|`CGRect`|
|`CGPoint`|`CGPoint`|
|`CGSize`|`CGSize`|
|`CGFloat`, `GLfloat`|`nfloat`|
|CoreFoundation 형식 (`CF*`)|`CoreFoundation.CF*`|
|`GLint`|`nint`|
|`GLfloat`|`nfloat`|
|Foundation 형식 (`NS*`)|`Foundation.NS*`|
|`id`|`Foundation`.`NSObject`|
|`NSGlyph`|`nint`|
|`NSSize`|`CGSize`|
|`NSTextAlignment`|`UITextAlignment`|
|`SEL`|`ObjCRuntime.Selector`|
|`dispatch_queue_t`|`CoreFoundation.DispatchQueue`|
|`CFTimeInterval`|`double`|
|`CFIndex`|`nint`|
|`NSGlyph`|`nuint`|

<a name="Arrays" />

### <a name="arrays"></a>배열

Xamarin.iOS 런타임 C#에 배열 변환 자동으로 처리 `NSArrays` 반환 변환을 다시 허수 Objective-c 메서드 따라서 예를 들면을 수행 하는 `NSArray` 의 `UIViews`:

```csharp
// Get the peer views - untyped
- (NSArray *)getPeerViews ();

// Set the views for this container
- (void) setViews:(NSArray *) views
```

다음과 같은 바인딩된:

```csharp
[Export ("getPeerViews")]
UIView [] GetPeerViews ();

[Export ("setViews:")]
void SetViews (UIView [] views);
```

개념은 IDE에서이 사용자를 추측 또는 배열에 포함 된 개체의 실제 형식이 확인 하려면 설명서를 조회 하지 않고 실제 형식이 적절 한 코드 완성 기능을 제공 하면 강력한 형식의 C# 배열을 사용 하는 것입니다.

배열에 포함 된 실제 가장 많이 파생 된 형식은 아래로 하지 추적할 수 하는 경우에 사용할 수 있습니다 `NSObject []` 반환 값으로.

<a name="Selectors" />

### <a name="selectors"></a>선택기

특수 형식으로 서의 Objective-c API에 선택기 표시 `SEL`합니다. 선택기를 바인딩하는 경우에 형식을 매핑합니다 `ObjCRuntime.Selector`합니다.  일반적으로 선택기 개체, 대상 개체 및 선택 기가 포함 된 대상 개체에서를 호출 하는 API에서 노출 됩니다. 둘 다 기본적으로 제공 하는 C# 대리자에 해당:에서 메서드를 호출 하는 개체와 호출할 메서드를 모두 캡슐화 하는 것입니다.

이 바인딩이 같습니다.

```csharp
interface Button {
   [Export ("setTarget:selector:")]
   void SetTarget (NSObject target, Selector sel);
}
```

그리고이 응용 프로그램에 메서드는 일반적으로 사용 됩니다.

```csharp
class DialogPrint : UIViewController {
    void HookPrintButton (Button b)
    {
        b.SetTarget (this, new Selector ("print"));
    }

    [Export ("print")]
    void ThePrintMethod ()
    {
       // This does the printing
    }
}
```

바인딩이 더 좋은 C# 개발자를 하려면 일반적으로 입력할 것임 사용 하는 메서드는 `NSAction` C# 대리자와 람다 대신 사용할 수 있는 매개 변수는 `Target+Selector`합니다. 숨기기 일반적으로이 작업을 수행 하는 `SetTarget` 메서드를 사용 하 여 플래그를 지정는 [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) 특성 한 다음 다음과 같이 새로운 도우미 메서드를 노출 합니다.

```csharp
// API.cs
interface Button {
   [Export ("setTarget:selector:"), Internal]
   void SetTarget (NSObject target, Selector sel);
}

// Extensions.cs
public partial class Button {
     public void SetTarget (NSAction callback)
     {
         SetTarget (new NSActionDispatcher (callback), NSActionDispatcher.Selector);
     }
}
```

이제 다음과 같은 사용자 코드를 작성할 수 있습니다.

```csharp
class DialogPrint : UIViewController {
    void HookPrintButton (Button b)
    {
        // First Style
        b.SetTarget (ThePrintMethod);

        // Lambda style
        b.SetTarget (() => {  /* print here */ });
    }

    void ThePrintMethod ()
    {
       // This does the printing
    }
}
```

<a name="Strings" />

### <a name="strings"></a>문자열

사용 하는 메서드를 바인딩하는 경우는 `NSString`는 C# 문자열 형식의 둘 다에 반환 형식 및 매개 변수를 대체할 수 있습니다.

사용 하는 시기는 유일한 경우는 `NSString` 직접 있으면 문자열을 토큰으로 사용 됩니다. 문자열에 대 한 자세한 내용은 및 `NSString`, 읽기는 [NSString에서 API 디자인](~/ios/internals/api-design/nsstring.md) 문서.

일부 드문 경우에는 API는 C와 같은 문자열 노출 될 수 있습니다 (`char *`) Objective C 문자열 대신 (`NSString *`). 이 경우의 매개 변수를 주석을 달 수는 [ `[PlainString]` ](~/cross-platform/macios/binding/binding-types-reference.md#plainstring) 특성입니다.

<a name="outref_parameters" />

### <a name="outref-parameters"></a>out / ref 매개 변수

일부 Api는 매개 변수의 값을 반환 하거나 매개 변수 참조로 전달 합니다.

일반적으로 서명을 다음과 같이 보입니다.

```csharp
- (void) someting:(int) foo withError:(NSError **) retError
- (void) someString:(NSObject **)byref
```

첫 번째 예제에서는 오류 코드에 대 한 포인터를 반환 하는 일반적인 Objective-c 관용구는 `NSError` 포인터를 전달 하 고 값이 반환 될 때 설정 됩니다.   두 번째 방법은 Objective-c 메서드 수 개체 하 한 내용을 수정 하는 방법을 보여줍니다.   이 순수 출력 값이 아닌 참조로 전달 되는 것입니다.

이 바인딩에 다음과 같습니다.

```csharp
[Export ("something:withError:")]
void Something (nint foo, out NSError error);
[Export ("someString:")]
void SomeString (ref NSObject byref);
```

<a name="Memory_management_attributes" />

### <a name="memory-management-attributes"></a>메모리 관리 특성

사용 하는 경우는 [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 특성을 전달 하는 호출 된 메서드가 유지 되어야 하는 데이터, 예를 들어 두 번째 매개 변수로 전달 하 여 인수 의미 체계를 지정할 수 있습니다.

```csharp
[Export ("method", ArgumentSemantic.Retain)]
```

위의 값 "보관" 의미 체계가 있는 것으로 플래그 것입니다. 사용 가능한 의미 체계는.

-  할당
-  복사
-  보유

<a name="Style_Guidelines" />

### <a name="style-guidelines"></a>스타일 지정 지침

<a name="Using_[Internal]" />

#### <a name="using-internal"></a>[내부]를 사용 하 여

사용할 수는 [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) 공용 API에서 메서드를 숨기려면 특성입니다. 노출 된 API는 너무 낮은이 방법에 따라 별도 파일에서 높은 수준의 구현을 제공 하는 경우에서 이렇게 할 수 있습니다.

사용할 수도 있습니다이 바인딩 생성기에 대 한 제한 사항으로, 일부 고급 시나리오 바인딩되지 않은 형식을 노출할 수는 예를 들어 하 고 고유한 방식으로 바인딩할와 하려면 이러한 형식을 직접 사용자 고유의 방식에서입니다.

<a name="Event_Handlers_and_Callbacks" />

## <a name="event-handlers-and-callbacks"></a>이벤트 처리기와 콜백

Objective C 클래스는 일반적으로 알림에 브로드캐스트 하거나 대리자 클래스 (Objective-c 대리자)에서 메시지를 전송 하 여 정보를 요청 합니다.

이 모델을 완벽 하 게 지원 하 고 Xamarin.iOS에 표시 된 경우에 따라은 복잡할 수 있습니다. Xamarin.iOS C# 이벤트 패턴 및 이러한 상황에서 사용할 수 있는 클래스에 메서드 콜백 시스템을 노출 합니다. 이 실행 하려면 다음과 같은 코드 수 있습니다.

```csharp
button.Clicked += delegate {
    Console.WriteLine ("I was clicked");
};
```

바인딩 생성기는 C# 패턴으로 Objective-c 패턴 매핑할 하는 데 필요한 입력을 줄일 수 있습니다.

Xamarin.iOS 1.4를 시작 하는 것은 생성자가 특정 Objective-c 대리자에 대 한 바인딩을 만들고이 C# 이벤트와 호스트 유형에 대 한 속성으로 대리자를 노출 하도록 지정할 수도 수 있을 것입니다.

이 프로세스에 관련 된 두 클래스, 있으며 호스트 클래스는 현재 이벤트를 내보냅니다 있으며 이러한 보냅니다는 `Delegate` 또는 `WeakDelegate` 및 실제 대리자 클래스입니다.

다음 설치 프로그램을 고려 합니다.

```csharp
[BaseType (typeof (NSObject))]
interface MyClass {
    [Export ("delegate", ArgumentSemantic.Assign)][NullAllowed]
    NSObject WeakDelegate { get; set; }

    [Wrap ("WeakDelegate")][NullAllowed]
    MyClassDelegate Delegate { get; set; }
}

[BaseType (typeof (NSObject))]
interface MyClassDelegate {
    [Export ("loaded:bytes:")]
    void Loaded (MyClass sender, int bytes);
}
```

클래스를 래핑할 하려면 다음을 수행 해야 합니다.

-  호스트 클래스에서를 추가 하면 [`[BaseType]`](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute)  
   대리자 및 노출 하는 C# 이름을 역할을 하는 형식 선언 합니다. 위의 예에는 `typeof (MyClassDelegate)` 및 `WeakDelegate` 각각.
-  대리자 클래스 두 개 이상의 매개 변수가 있는 각 메서드에 자동으로 생성 된 EventArgs 클래스에 대 한 사용 하려는 하는 형식을 지정 해야 합니다.

바인딩 생성기를 단일 이벤트 대상 래핑에 국한 되지 않음, 이므로 일부 Objective-c 클래스에 둘 이상의 메시지를 내보내는 데 대리자 가능한 배열을이 설치 프로그램을 지원 하기 위해 제공 해야 합니다. 대부분의 설치 프로그램은 불필요, 하지만 생성자는 이러한 경우를 지원 하도록 준비 합니다.

결과 코드가 됩니다.

```csharp
[BaseType (typeof (NSObject),
    Delegates=new string [] {"WeakDelegate"},
    Events=new Type [] { typeof (MyClassDelegate) })]
interface MyClass {
        [Export ("delegate", ArgumentSemantic.Assign)][NullAllowed]
        NSObject WeakDelegate { get; set; }

        [Wrap ("WeakDelegate")][NullAllowed]
        MyClassDelegate Delegate { get; set; }
}

[BaseType (typeof (NSObject))]
interface MyClassDelegate {
        [Export ("loaded:bytes:"), EventArgs ("MyClassLoaded")]
        void Loaded (MyClass sender, int bytes);
}
```

`EventArgs` 의 이름을 지정 하는 데 사용 되는 `EventArgs` 클래스를 생성 합니다. 서명 당 하나를 사용 해야 (이 예제는 `EventArgs` 포함 됩니다는 `With` 형식 nint 속성).

위에서 설명한 정의 생성기는 생성 된 MyClass에 다음 이벤트가 생성 합니다.

```csharp
public MyClassLoadedEventArgs : EventArgs {
        public MyClassLoadedEventArgs (nint bytes);
        public nint Bytes { get; set; }
}

public event EventHandler<MyClassLoadedEventArgs> Loaded {
        add; remove;
}
```

따라서 코드를 다음과 같이 사용할 수 있습니다.

```csharp
MyClass c = new MyClass ();
c.Loaded += delegate (sender, args){
        Console.WriteLine ("Loaded event with {0} bytes", args.Bytes);
};
```

콜백 이벤트 호출에서와 마찬가지로, 차이점은 여러 잠재적인 구독자 대신 (에 메서드가 여러 개 후크 할 수는 예를 들어는 `Clicked` 이벤트 또는 `DownloadFinished` 이벤트) 콜백을 한 구독자를 하나만 사용할 수 있습니다.

프로세스는 동일, 유일한 차이점은 이름을 노출 하는 대신는 `EventArgs` 실제로 EventArgs 생성 되는 클래스 결과 C# 대리자 이름이 이름을 지정 하는 데 사용 됩니다.

대리자 클래스의 메서드는 값을 반환 하는 경우 바인딩 생성기는 부모 클래스 대신 이벤트의에서 대리자 메서드에이 매핑됩니다. 이러한 경우에 반환 되어야 하는 메서드에서 사용자를 연결 하지 않는 경우 대리자에 기본값을 제공 해야 합니다. 이렇게 하면 사용 하 여 [ `[DefaultValue]` ](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueAttribute) 또는 [ `[DefaultValueFromArgument]` ](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueFromArgumentAttribute) 특성입니다.

[`[DefaultValue]`](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueAttribute) 반환 값을 하드 코드 됩니다 동안 [ `[DefaultValueFromArgument]` ](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueFromArgumentAttribute) 입력된 인수를 반환할 지정 하는 데 사용 됩니다.

<a name="Enumerations_and_Base_Types" />

## <a name="enumerations-and-base-types"></a>열거형 및 기본 형식

열거형 또는 기본 형식 btouch 인터페이스 정의 시스템에서 직접 지원 되지 않는 참조할 수 있습니다. 이 수행 하려면 열거형 및 핵심 형식이 별도 파일에 놓고 btouch를 제공 하는 추가 파일 중 하나의 일부로이 포함 합니다.

<a name="Linking_the_Dependencies" />

## <a name="linking-the-dependencies"></a>종속성 링크

응용 프로그램의 일부분이 아닌 Api에 바인딩하는 경우에 이러한 라이브러리에 대 한 실행 파일은 연결 되었는지 확인 해야 합니다.

이렇게 호출 하 여 빌드 구성을 변경 하거나, Xamarin.iOS 라이브러리를 연결 하는 방법을 알려 주어 야는 `mtouch` 일부 추가 사용 하 여 명령을 사용 하 여 새 라이브러리와 연결 하는 방법을 지정 하는 인수를 빌드는 "-gcc_flags" 옵션을 다음과 같이 프로그램에 필요한 모든 추가 라이브러리를 포함 하는 따옴표 붙은 문자열 옵니다.

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load -lSystemLibrary -framework CFNetwork -ObjC"
```

위의 예제에서는 연결 `libMyLibrary.a`, `libSystemLibrary.dylib` 및 `CFNetwork` framework 라이브러리를 최종 실행 파일에 있습니다.

어셈블리 수준의 이용할 수 있습니다 또는 [ `[LinkWithAttribute]` ](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute), 계약 파일에 포함할 수 있는 (예: `AssemblyInfo.cs`).
사용 하는 경우는 [ `[LinkWithAttribute]` ](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute), 하면 바인딩, 대로 확인이 네이티브 라이브러리가 응용 프로그램과 함께 포함 됩니다 때 네이티브 라이브러리 해야 합니다. 예를 들어:

```csharp
// Specify only the library name as a constructor argument and specify everything else with properties:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget = LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]

// Or you can specify library name *and* link target as constructor arguments:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]
```

궁금할 것, 필요 하십니까 `-force_load` 명령과 이유는-ObjC 플래그의 코드를 컴파일하고 있지만, 범주 (링커/컴파일러 데드 코드가 elimination 잘라냅니다)를 지 원하는 데 필요한 메타 데이터를 유지 하지 않습니다는 Xamarin.iOS 런타임 시 필요 합니다.

<a name="Assisted_References" />

## <a name="assisted-references"></a>기술된 참조

작업 시트 및 경고 상자와 같은 일부 임시 개체는 추적 하기 위해 개발자를 위한 복잡 하 고 바인딩 생성기 여기 약간 합니다.

예를 들어 메시지를 표시 하 고 다음 생성 하는 클래스에는 `Done` 이벤트의 경우이 처리 하는 전통적인 방법 됩니다:

```csharp
class Demo {
    MessageBox box;

    void ShowError (string msg)
    {
        box = new MessageBox (msg);
        box.Done += { box = null; ... };
    }
}
```

위의 시나리오에는 개발자를 자신 및 하거나 누수 된 개체에 대 한 참조를 유지 하거나 적극적으로 자신의 상자에 대 한 참조를 해제 하려면 필요 합니다.  바인딩 코드를 생성자를 지원 하지만 사용자에 대 한 참조의 추적 하는 유지 하 고 선택을 취소 이므로 특수 메서드가 호출 되 면 위의 코드 기간이 다시:

```csharp
class Demo {
    void ShowError (string msg)
    {
        var box = new MessageBox (msg);
        box.Done += { ... };
    }
}
```

지역 변수와 함께 작동 하 고는 개체가 소멸 되는 경우 해당 참조를 지울 필요 하다 고 변수 인스턴스를 보관 하는 데 필요한 더 이상 어떤가요 확인 합니다.

를 이용 하려면이 클래스에 설정 이벤트 속성이 있어야 합니다.는 [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) 선언도는 `KeepUntilRef` 변수 개체에 해당 작업을 같은 완료 될 때 호출 되는 메서드의 이름으로 설정 이:

```csharp
[BaseType (typeof (NSObject), KeepUntilRef="Dismiss"), Delegates=new string [] { "WeakDelegate" }, Events=new Type [] { typeof (SomeDelegate) }) ]
class Demo {
    [Export ("show")]
    void Show (string message);
}
```

<a name="Inheriting_Protocols" />

## <a name="inheriting-protocols"></a>프로토콜을 상속

Xamarin.iOS v3.2 기준으로 표시 하는 프로토콜에서 상속 지원는 [ `[Model]` ](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute) 속성입니다. 와 같이 이러한 특정 API 패턴에 유용 `MapKit` 여기서는 `MKOverlay` 프로토콜에서 상속 되는 `MKAnnotation` , 프로토콜 및 다양 한 클래스에서 상속 하 여 적용 됩니다 `NSObject`합니다.

모든 구현 하는 프로토콜 복사 요구 지금까지 하지만 이러한 경우 이제 수 있으며 이러한 시각화는 `MKShape` 클래스에서 상속 된 `MKOverlay` 프로토콜과 것에서 필요한 메서드를 모두 자동으로 생성 합니다.

## <a name="related-links"></a>관련 링크

- [바인딩 예제](https://developer.xamarin.com/samples/BindingSample/)
