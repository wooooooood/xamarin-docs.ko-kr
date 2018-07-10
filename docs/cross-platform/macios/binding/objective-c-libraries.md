---
title: Objective-c 라이브러리 바인딩
description: 이 문서에서는 C# 바인딩을 만들 Objective-c 코드를 이벤트, 메서드, 사용자 지정 컨트롤 등을 바인딩하는 방법을 설명 하는 방법의 대략적인 개요를 제공 합니다.
ms.prod: xamarin
ms.assetid: 8A832A76-A770-1A7C-24BA-B3E6F57617A0
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/06/2018
ms.openlocfilehash: 4c414e0e863f44045473a248576a3612b1719559
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854833"
---
# <a name="binding-objective-c-libraries"></a>Objective-c 라이브러리 바인딩

Xamarin.iOS 또는 Xamarin.Mac을 사용 하는 경우 타사 Objective-c 라이브러리를 사용 하려는 경우 발생할 수 있습니다. 이러한 상황에서는 네이티브 Objective-c 라이브러리에 대 한 C# 바인딩을 만들려면 Xamarin 바인딩 프로젝트를 사용할 수 있습니다. 프로젝트는 C#으로 iOS 및 Mac Api를 사용 하는 것과 동일한 도구를 사용 합니다.

이 문서에서는 Objective-c로 Api를 바인딩하는 방법을 설명 C Api만을 바인딩하는 경우 표준.NET 메커니즘은이 위해 사용할지 [P/Invoke framework](http://www.mono-project.com/docs/advanced/pinvoke/)합니다.
C 라이브러리를 정적으로 연결 하는 방법에 대 한 세부 정보에서 사용할 수는 [네이티브 라이브러리 연결](~/ios/platform/native-interop.md) 페이지입니다.

이 부록을 참조 하세요 [바인딩 유형 참조 가이드](~/cross-platform/macios/binding/binding-types-reference.md)합니다.
내부에서 어떤 일이 생기는 것에 대 한 자세한 정보를 계속 확인 하려는 경우 확인 하는 또한 우리의 [바인딩 개요](~/cross-platform/macios/binding/overview.md) 페이지입니다.

IOS 및 Mac 라이브러리 바인딩을 빌드할 수 있습니다.
하지만이 페이지에는 바인딩, 바인딩 Mac은 매우 유사 iOS에서 작동 하는 방법을 설명 합니다.

**IOS에 대 한 샘플 코드**

사용할 수는 [Binding 샘플 iOS](https://github.com/xamarin/monotouch-samples/tree/master/BindingSample) 바인딩을 사용 하 여 실험에 프로젝트입니다.

<a name="Getting_Started" />

## <a name="getting-started"></a>시작

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

바인딩을 만들 수는 가장 쉬운 방법은 Xamarin.iOS 바인딩 프로젝트를 만드는 경우
할 수 있는이 Visual Studio에서 Mac에 대 한 프로젝트 형식을 선택 하 여 **iOS > 라이브러리 > 바인딩 라이브러리**:

[![](objective-c-libraries-images/00-sml.png "이렇게 하려면 Visual Studio에서 Mac 용 iOS 바인딩 라이브러리 프로젝트 형식 선택")](objective-c-libraries-images/00.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

바인딩을 만들 수는 가장 쉬운 방법은 Xamarin.iOS 바인딩 프로젝트를 만드는 경우
프로젝트 형식을 선택 하 여 Windows의 Visual Studio에서 이렇게 하려면 **Visual C# > iOS > 바인딩 라이브러리 (iOS)**:

[![](objective-c-libraries-images/00vs-sml.png "iOS 바인딩 라이브러리 iOS")](objective-c-libraries-images/00vs.png#lightbox)

> [!IMPORTANT]
> 참고:에 대 한 바인딩 프로젝트 **Xamarin.Mac** mac 용 Visual Studio에만 지원 됩니다

-----

생성된 된 프로젝트에는 편집할 수 있는 작은 템플릿을 포함, 두 개의 파일이 포함 되어 있습니다: `ApiDefinition.cs` 및 `StructsAndEnums.cs`합니다.

`ApiDefinition.cs` 는 API 계약을 정의 하는 C#으로 기본 Objective C API는 프로젝션 하는 방법을 설명 하는 파일입니다. 구문 및이 파일의 내용을이 문서의 토론의 주제는 및의 내용을 C# 인터페이스 및 C# 대리자 선언으로 제한 됩니다. `StructsAndEnums.cs` 입력할 수 있는 데 필요한 모든 정의 인터페이스 및 대리자를 여는 파일입니다. 이 열거형 값 및 코드를 사용할 수 있는 구조를 포함 합니다.

<a name="Binding_an_API" />

## <a name="binding-an-api"></a>API를 바인딩

포괄적인 바인딩을 위해 Objective-c로 API 정의 이해 하 고.NET Framework 디자인 지침을 숙지 해야 합니다.

라이브러리를 바인딩하려면 일반적으로 API 정의 파일을 사용 하 여 시작 합니다. API 정의 파일을 단순히 C# 원본 파일을 C# 인터페이스는 데 도움이 되는 특성의 소수의 주석이 지정 된 바인딩을 포함 하는 경우  이 파일은 C#과 Objective-c 간의 계약에 새로운 기능을 정의 합니다.

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

라는 클래스를 정의 하는 위의 샘플 `Cocos2D.Camera` 에서 파생 되는 합니다 `NSObject` 기본 형식 (이 형식에서 가져온 `Foundation.NSObject`) 및 정적 속성을 정의 하는 (`ZEye`), 메서드와 인수 없이 사용 하는 두 메서드는 세 가지 인수입니다.

자세한 설명은 API 파일 및 사용할 수 있는 특성의 형식에 대해서는 합니다 [API 정의 파일](~/cross-platform/macios/binding/objective-c-libraries.md#The_API_definition_file) 아래의 섹션입니다.

전체 바인딩을 생성 하려면 일반적으로 네 가지 구성 요소를 사용 하 여 처리 됩니다.

-  API 정의 파일 (`ApiDefinition.cs` 템플릿에서).
-  선택 사항: 모든 열거형 형식, API 정의 파일을 여는 데 필요한 구조체 (`StructsAndEnums.cs` 템플릿에서).
-  생성 된 바인딩 확장 될 수 있습니다 또는 C# 친숙 한 API를 (C# 파일을 프로젝트에 추가한)을 제공 하는 선택 사항: 추가 원본입니다.
-  바인딩하는 네이티브 라이브러리입니다.

이 차트는 파일 간의 관계를 보여 줍니다.

 [![](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png "이 차트는 파일 간의 관계를 보여 줍니다.")](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png#lightbox)

API 정의 파일 (모든 멤버와 인터페이스를 포함할 수 있는) 네임 스페이스 및 인터페이스 정의가 포함 됩니다 및 클래스, 열거형, 대리자 또는 구조체 포함 하지 않아야 합니다. API 정의 파일은 API를 생성 하는 데 사용할 수 있는 계약 뿐입니다.

추가 코드는 필요한 열거형 또는 클래스를 지 원하는 "CameraMode" 위의 예제에서 별도 파일을 호스트 해야 CS 파일에 없는 경우 예를 들어 별도 파일에서 호스트 해야 하는 열거형 값 이면 `StructsAndEnums.cs` :

```csharp
public enum CameraMode {
    FlyOver, Back, Follow
}
```

합니다 `APIDefinition.cs` 파일와 결합 되는 `StructsAndEnum` 클래스 및 라이브러리의 코어 바인딩을 생성 하는 데 사용 됩니다. 결과 라이브러리를 사용할 수 있습니다-인 하지만 일반적으로 사용자를 위해 일부 C# 기능을 추가 하려면 결과 라이브러리를 조정 해야 합니다. 몇 가지 예로 구현 된 `ToString()` 메서드, C# 인덱서에 제공, 암시적 변환을 추가 하 고 몇 가지 기본 형식에서 또는 몇 가지 방법 중 강력한 형식의 버전을 제공 합니다. 이러한 향상 된이 기능 추가 C# 파일에 저장 됩니다. 단순히 프로젝트에 C# 파일을 추가 하 고 빌드 프로세스에 포함 됩니다.

코드를 구현 하는 방법을 보여 줍니다 프로그램 `Extra.cs` 파일입니다. 사용 하 게 partial 클래스의 조합에서 생성 된 partial 클래스를 보완 이러한으로 확인 합니다 `ApiDefinition.cs` 및 `StructsAndEnums.cs` 바인딩 핵심:

```csharp
public partial class Camera {
    // Provide a ToString method
    public override string ToString ()
    {
         return String.Format ("ZEye: {0}", ZEye);
    }
}
```

라이브러리 빌드 기본 바인딩이 생성 됩니다.

이 바인딩은 완료 하려면 프로젝트를 네이티브 라이브러리를 추가 해야 합니다.  프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 하 여 네이티브 라이브러리를 끌어서 놓아 네이티브 라이브러리 파인더에서 솔루션 탐색기에서 프로젝트 또는 프로젝트에 추가 하 여 이렇게 **추가**  >  **파일 추가** 네이티브 라이브러리를 선택 합니다.
규칙에 따라 네이티브 라이브러리 "lib" 단어로 시작 하 고 ".a" 확장명으로 끝나야 합니다. 이렇게 하면 Mac 용 Visual Studio 파일 두 개를 추가 합니다:.a 파일을 자동으로 채워진된 C# 파일 및 네이티브 라이브러리에 포함 하는 방법에 대 한 정보를 포함 합니다.

 [![](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png "Word lib 및 확장.a 인 끝 사용 하 여 규칙이 시작 하기 위해 네이티브 라이브러리")](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png#lightbox)

콘텐츠는 `libMagicChord.linkwith.cs` 파일에이 라이브러리를 사용할 수 있습니다 하는 방법에 대 한 정보 및 결과 DLL 파일로이 이진을 패키지 하려면 IDE에 지시 합니다.

```csharp
using System;
using ObjCRuntime;

[assembly: LinkWith ("libMagicChord.a", SmartLink = true, ForceLoad = true)]
```

전체 사용 하는 방법에 대 한 세부 정보를 [ `[LinkWith]` ](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute) 특성에 설명 되어 있습니다 합니다 [바인딩 형식 참조 가이드](~/cross-platform/macios/binding/binding-types-reference.md)합니다.

프로젝트를 빌드할 때 결국 사용 하 여 이제는 `MagicChords.dll` 바인딩과 네이티브 라이브러리를 포함 하는 파일입니다. 이 프로젝트를 배포할 수 있습니다 또는 자체에 대 한 다른 개발자에 게 결과 DLL을 사용 합니다.

경우에 따라 볼 수 있습니다 몇 열거형, 대리자 정의 또는 기타 유형의 해야 합니다. 이 단순히 계약으로 API 정의 파일에 배치 하지 마십시오

<a name="The_API_definition_file" />

## <a name="the-api-definition-file"></a>API 정의 파일

API 정의 파일 인터페이스의 숫자로 구성 됩니다. API 정의의 인터페이스를 클래스 선언으로 변환 됩니다 및으로 데코레이팅 해야 합니다 [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) 클래스에 대 한 기본 클래스를 지정 하는 특성입니다.

왜에서는 사용 하지 않은 클래스 인터페이스 대신 계약 정의 대 한 궁금할 합니다. API 정의 파일에서 메서드 본문을 제공 하지 않아도 하거나 예외를 throw 하거나 의미 있는 값을 반환 해야 하는 본문을 제공 하지 않고도 메서드에 대 한 계약을 작성할 수 있었습니다 때문에 인터페이스를 선택 했습니다.

하지만 클래스를 생성 하는 스 켈 레 톤으로 인터페이스를 사용 하 고 있으므로 드라이브 바인딩 특성을 사용 하 여 계약의 다양 한 부분 지정에 의존 해야 했습니다.

<a name="Binding_Methods" />

### <a name="binding-methods"></a>바인딩 메서드

할 수 있는 가장 간단한 바인딩 메서드를 바인딩하는 것입니다. 방금 C# 명명 규칙을 사용 하 여 인터페이스에서 메서드를 선언 및 사용 하 여 메서드를 데코 레이트 합니다 [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 특성입니다. 합니다 [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 특성은 C# 이름 앞에 Xamarin.iOS 런타임은 Objective-c로 이름 링크 합니다. 매개 변수를 [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 특성은 Objective-c 선택기의 이름입니다. 몇 가지 예:

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

위의 샘플 인스턴스 메서드를 바인딩할 수 있습니다 하는 방법을 보여 줍니다. 정적 메서드를 바인딩하려면 사용 해야 합니다 [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) 다음과 같은 특성:

```csharp
// A static method, that takes no arguments
[Static, Export ("refresh")]
void Beep ();
```

계약 인터페이스의 일부인 있고 인터페이스 개념이 없으므로 정적 및 인스턴스 선언 되므로 다시 한 번 특성에 의존 하는 데 필요한 때문에 이것이 필요 합니다. 이 메서드를 데코레이팅 할 수 있습니다 바인딩에서 특정 메서드를 숨길 하려는 경우는 [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) 특성입니다.

`btouch-native` 명령에서 null이 아니어야 하는 참조 매개 변수에 대 한 검사를 소개 합니다. 특정 매개 변수에 대해 null 값을 허용 하려는 경우 사용 합니다 [ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute) 다음과 같은 매개 변수에 특성:

```csharp
[Export ("setText:")]
string SetText ([NullAllowed] string text);
```

참조 형식으로 내보낼 때 합니다 [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 키워드 할당 의미 체계를 지정할 수도 있습니다. 이것은 데이터가 누출 됩니다 하는 데 필요 합니다.

<a name="Binding_Properties" />

### <a name="binding-properties"></a>바인딩 속성

메서드를 마찬가지로 Objective-c 속성 사용 하 여 바인딩됩니다 합니다 [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 특성 및 C# 속성에 직접 매핑됩니다. 메서드를 마찬가지로 속성으로 데코레이팅 될 수는 [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) 하며 [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) 특성입니다.

사용 하는 경우는 [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 특성 설명 u c h-네이티브 아래의 속성에 실제로 두 메서드를 바인딩합니다: getter 및 setter. 내보내기를 제공 하는 이름을 합니다 **basename** 및 setter의 첫 글자를 하면 "설정" 이라는 단어 앞에 추가 하 여 계산 됩니다는 **basename** 대문자나 소문자 및 수행 하는 선택기에는 인수입니다. 즉 `[Export ("label")]` 에 적용 된 속성은 실제로 "label" 바인딩합니다 및 "setLabel:" Objective-c 메서드.

Objective-c로 속성을 위에서 설명한 패턴을 따르지 않습니다 하 고 이름은 수동으로 덮어쓸 경우도 있습니다. 이러한 경우에이 바인딩을 사용 하 여 생성 되는 방식을 제어할 수 있습니다 합니다 [ `[Bind]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAttribute) getter 또는 setter를 예를 들어 특성:

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

여기서 getter 및 setter를 명시적으로 정의 된 에서처럼 합니다 `name` 및 `setName` 위의 바인딩.

사용 하 여 정적 속성에 대 한 지원 외에도 [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute)를 사용 하 여 스레드 정적 속성을 데코레이팅 할 수 있습니다 [ `[IsThreadStatic]` ](~/cross-platform/macios/binding/binding-types-reference.md#IsThreadStaticAttribute)예를 들어:

```csharp
[Export ("currentRunLoop")][Static][IsThreadStatic]
NSRunLoop Current { get; }
```

메서드를 사용 하면 일부 매개 변수를 사용 하 여 플래그를 지정 하는 것 처럼 [ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute)를 적용할 수 있습니다 [ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute) 는 null을 예를 들어 속성에 대 한 유효한 값으로 나타내기 위해 속성:

```csharp
[Export ("text"), NullAllowed]
string Text { get; set; }
```

합니다 [ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute) setter에서 직접 매개 변수를 지정할 수도 있습니다.

```csharp
[Export ("text")]
string Text { get; [NullAllowed] set; }
```

#### <a name="caveats-of-binding-custom-controls"></a>사용자 지정 컨트롤을 바인딩하는 주의

사용자 지정 컨트롤에 대 한 바인딩을 설정 하는 경우에 일정을 고려해 야 합니다.

1. **바인딩 속성 정적 이어야** -이 속성의 바인딩을 정의 하는 경우는 [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) 특성을 사용 해야 합니다.
 2. **속성 이름이 정확히 일치 해야** -이름 속성을 바인딩하는 데 사용자 지정 컨트롤의 속성 이름을 정확히 일치 해야 합니다.
3. **속성 형식이 정확히 일치 해야** -변수 형식 속성을 바인딩하는 데 사용자 지정 컨트롤에서 속성의 형식에 정확히 일치 해야 합니다.
4. **중단점 및 getter/setter** -중단점 배치 getter에서 또는 속성의 setter 메서드를 적중 되지 것입니다.
5. **콜백을 관찰** -사용자 지정 컨트롤의 속성 값의 변경 내용을 알릴 관찰 콜백을 사용 해야 합니다.

위에 나열 된 제한 사항 중 하나를 확인 하지 못했습니다 런타임에 자동으로 실패 한 바인딩이 발생할 수 있습니다.

<a name="MutablePattern" />

#### <a name="objective-c-mutable-pattern-and-properties"></a>Objective-c로 변경할 수 있는 패턴 및 속성

Objective-c 프레임 워크 일부 클래스를 변경할 수 있는 하위 클래스를 사용 하 여 변경할 수 없는 위치는 관용구를 사용 합니다. 예를 들어 `NSString` 변경할 수 없는 버전이 동안 `NSMutableString` 변이 허용 하는 서브 클래스는 합니다.

이 해당 클래스에 getter, setter 같지만 속성을 포함 하는 변경할 수 없는 기본 클래스를 참조 하세요. 일반적입니다. 한 setter를 소개 하기 위해 버전을 변경할 수 있습니다. C#을 사용 하 여 실제로 가능한이 아니므로이 관용구 C#을 사용 하 여 작동 하는 방법이에 매핑 했습니다.

C#에 매핑되는이 방법은 기본 클래스에 대해 getter 및 setter를 추가 하지만와 setter에 플래그를 지정 하는 것을 [ `[NotImplemented]` ](~/cross-platform/macios/binding/binding-types-reference.md#NotImplementedAttribute) 특성입니다.

그런 다음 변경할 수 있는 서브 클래스를 사용 하 여 있습니다 합니다 [ `[Override]` ](~/cross-platform/macios/binding/binding-types-reference.md#OverrideAttribute) 속성 속성이 부모 동작을 재정의 하 고 실제로 확인 하기 위해 특성입니다.

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

합니다 `btouch-native` 클래스에 지정된 된 클래스에 대 한 도구에서는 생성자를 자동으로 생성 됩니다 `Foo`를 생성 합니다.

-  `Foo ()`: 기본 생성자 (Objective-c의 "초기화" 생성자에는 maps)
-  `Foo (NSCoder)`: NIB 파일의 역직렬화 하는 동안 사용 된 생성자 (Objective-c의 매핑됩니다 "initWithCoder:" 생성자).
-  `Foo (IntPtr handle)`: 생성자는 핸들 기반 만들기에 대 한 호출 됩니다. 런타임에서 런타임에서 관리 되지 않는 개체에서 관리 되는 개체를 노출 해야 하는 경우.
-  `Foo (NSEmptyFlag)`:이 두 번 초기화를 방지 하기 위해 파생된 클래스에서 사용 됩니다.

인터페이스 정의 내부에 다음 서명을 사용 하 여 선언 해야 정의 하는 생성자의 경우: 반환 해야는 `IntPtr` 메서드의 이름과 값 생성자를 기준으로 해야 합니다. 예를 들어 바인딩할는 `initWithFrame:` 생성자를 사용할 것:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);
```

<a name="Binding_Protocols" />

### <a name="binding-protocols"></a>바인딩 프로토콜

API 디자인 문서 섹션에서에 설명 된 대로 [모델과 프로토콜을 설명](~/ios/internals/api-design/index.md#Models), Xamarin.iOS를 사용 하 여 플래그가 지정 된 클래스로 Objective-c 프로토콜을 매핑하는 [ `[Model]` ](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute) 특성입니다. Objective-c 대리자 클래스를 구현할 때 일반적으로 사용 됩니다.

일반 바인딩된 클래스와 대리자 클래스 간의 가장 큰 차이점은 대리자 클래스가 하나 이상의 선택적 메서드를 사용할 수 있습니다.

예를 들어를 `UIKit` 클래스 `UIAccelerometerDelegate`, Xamarin.iOS에서 바인딩하는 방법입니다.

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface UIAccelerometerDelegate {
        [Export ("accelerometer:didAccelerate:")]
        void DidAccelerate (UIAccelerometer accelerometer, UIAcceleration acceleration);
}
```

에 대 한 정의에 하나의 선택적 방법 이므로 `UIAccelerometerDelegate` 아무 작업을 수행 하는 합니다. 프로토콜에 필요한 메서드를 추가 해야 하지만 합니다 [ `[Abstract]` ](~/cross-platform/macios/binding/binding-types-reference.md#AbstractAttribute) 특성을 메서드에 있습니다. 이렇게 하면 실제로 메서드의 본문을 제공 하기 구현의 사용자.

일반적으로 프로토콜은 메시지에 응답 하는 클래스에 사용 됩니다. 일반적으로 이렇게 objective-c에서 프로토콜의 메서드에 응답 하는 개체의 인스턴스를 "delegate" 속성에 할당 합니다.

모든 두 Objective-c로 느슨하게 결합 된 스타일을 지원 하기 위해 Xamarin.iOS에서 규칙은의 인스턴스는 `NSObject` 할당할 수 있습니다도 노출 하는 대리자에는 강력한 형식의 버전을 합니다. 이 따라서 일반적으로 제공 모두를 `Delegate` 강력한 형식인 속성 및 `WeakDelegate` 느슨한 형식입니다. 일반적으로 사용 하 여 느슨한 형 버전을 바인딩하기만 [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute)를 사용 하 여 합니다 [ `[Wrap]` ](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) 강력한 형식의 버전을 제공 하는 특성입니다.

바인딩하는 방법 표시는 `UIAccelerometer` 클래스:

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

MonoTouch 7.0 새롭고 향상 된 프로토콜 바인딩 기능부터 통합 되어 있습니다.  이 새로운 지원을 사용 하면 간단 하 게 지정된 된 클래스에서 하나 이상의 프로토콜 채택을 위한 Objective-c 관용구를 사용 합니다.

모든 프로토콜 정의 대 한 `MyProtocol` Objective-c 한지 이제는 `IMyProtocol` 모든 선택적 메서드를 제공 하는 확장 클래스 뿐만 아니라 프로토콜에서 필요한 모든 메서드를 나열 하는 인터페이스입니다.  위의 편집기를 사용 하면 이전 추상 모델 클래스의 별도 서브 클래스를 사용 하지 않고도 프로토콜 메서드를 구현 하는 개발자는 Xamarin Studio에서 새로운 지원을 통해 결합 합니다.

포함 된 모든 정의 된 [ `[Protocol]` ](~/cross-platform/macios/binding/binding-types-reference.md#ProtocolAttribute) 특성 프로토콜을 사용 한다고 가정 하는 방법은 크게 개선 하는 세 가지 지원 클래스를 실제로 생성 됩니다.

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

합니다 **클래스 구현은** 의 개별 메서드를 재정의 하 고 전체 형식 안전성을 가져올 수 있는 완전 한 추상 클래스를 제공 합니다.  이 밖에도 C# 다중 상속을 지원 하지 않는 인해 수 있는 시나리오를 다른 기본 클래스를 포함 해야 하지만 여전히 위치 하는 인터페이스를 구현 하려는 합니다

생성 된 **인터페이스 정의** 제공 됩니다.  이 프로토콜에서 필요한 모든 메서드를 사용 하는 인터페이스입니다.  개발자를 단순히 인터페이스를 구현 하 여 프로토콜을 구현 하려는 있습니다.  런타임은 프로토콜 도입으로 형식을 자동으로 등록 됩니다.

알림 인터페이스만 필요한 메서드를 나열 하는 선택적 메서드를 노출 합니다.  즉, 프로토콜을 채택 하는 클래스 필요한 메서드를 확인 하는 전체 형식 받습니다 있지만 약한 형식 지정에 의존 해야 합니다 (사용 하 여 직접 [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 특성 및 서명 일치) 선택적 프로토콜 메서드입니다.

바인딩 도구는 편리 하 게 프로토콜을 사용 하는 API를 사용 하려면, 모든 선택적 메서드를 노출 하는 확장 메서드 클래스도 생성 됩니다.  이 API를 사용 중인으로 수 프로토콜 메서드를 모두 있는 것으로 처리할 것을 의미 합니다.

API에서 프로토콜 정의 사용 하려는 경우에 API 정의에서 기본 빈 인터페이스를 작성 해야 합니다.  MyProtocol API에서 사용 하려는 경우이 작업을 수행 해야 합니다.

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

위의 필요 하기 때문에 바인딩 시간에는 `IMyProtocol` 존재 하지 않습니다, 즉 빈 인터페이스를 제공 해야 하는 이유입니다.

#### <a name="adopting-protocol-generated-interfaces"></a>프로토콜에서 생성 된 인터페이스를 채택합니다.

구현할 때마다 다음과 같은 프로토콜에 대해 생성 된 인터페이스 중 하나:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

인터페이스 메서드의 구현은 자동으로 내보내지는 적절 한 이름을 사용 하 여이에 해당 되므로:

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

Objective C에서 비슷하며, C#의 확장 메서드를 새 메서드로 클래스를 확장 하는 것이 같습니다. 사용할 수 있는 경우 다음이 방법 중 하나를 [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) 특성 Objective-c 메시지의 받는 사람으로 메서드를 플래그 합니다.

예를 들어 Xamarin.iOS에서에서는 바인딩된에 정의 된 확장 메서드 `NSString` 때 `UIKit` 의 방법으로 가져온는 `NSStringDrawingExtensions`, 다음과 같은:

```csharp
[Category, BaseType (typeof (NSString))]
interface NSStringDrawingExtensions {
    [Export ("drawAtPoint:withFont:")]
    CGSize DrawString (CGPoint point, UIFont font);
}
```

<a name="Binding_Objective-C_Argument_Lists" />

### <a name="binding-objective-c-argument-lists"></a>인수 목록 Objective-c 바인딩

Objective-c는 variadic 인수를 지원합니다. 예를 들어:

```objc
- (void) appendWorkers:(XWorker *) firstWorker, ...
  NS_REQUIRES_NIL_TERMINATION ;
```

C#에서이 메서드를 호출 하려는 다음과 같은 서명을 만듭니다.

```csharp
[Export ("appendWorkers"), Internal]
void AppendWorkers (Worker firstWorker, IntPtr workersPtr)
```

이 라이브러리에 노출 하지만, 사용자에 게 위의 API 숨기기 내부로 메서드를 선언 합니다. 다음과 같은 메서드를 작성할 수 있습니다.

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

### <a name="binding-fields"></a>필드 바인딩

경우에 따라 라이브러리에 선언 된 공용 필드에 액세스 하려고 합니다.

이러한 필드는 일반적으로 참조 해야 하는 문자열 또는 정수 값을 포함 합니다. 일반적으로 특정 알림 메시지를 나타내는 문자열로 및 사전에 키로 사용 됩니다.

필드에 바인딩하려면 인터페이스 정의 파일에 속성을 추가 하 고 사용 하 여 속성을 데코 레이트 합니다 [ `[Field]` ](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute) 특성입니다. 이 특성은 하나의 매개 변수를 사용: C 이름의 기호를 조회 합니다. 예를 들어:

```csharp
[Field ("NSSomeEventNotification")]
NSString NSSomeEventNotification { get; }
```

파생 되지 않은 정적 클래스에서 다양 한 필드를 배치 하려는 경우 `NSObject`를 사용할 수는 [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute_Class) 다음과 같은 클래스의 특성:

```csharp
[Static]
interface LonelyClass {
    [Field ("NSSomeEventNotification")]
    NSString NSSomeEventNotification { get; }
}
```

위의 생성을 `LonelyClass` 에서 파생 되지 않습니다 `NSObject` 에 대 한 바인딩을 포함 됩니다는 `NSSomeEventNotification` 
 `NSString` 으로 노출는 `NSString`합니다.

합니다 [ `[Field]` ](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute) 특성 데이터 형식에 적용할 수 있습니다.

-  `NSString` 참조 (읽기 전용 속성만)
-  `NSArray` 참조 (읽기 전용 속성만)
-  32 비트 정수 (`System.Int32`)
-  64 비트 정수 (`System.Int64`)
-  32 비트 부동 소수점 수 (`System.Single`)
-  64 비트 부동 소수점 수 (`System.Double`)
-  `System.Drawing.SizeF`
-  `CGSize`

기본 필드 이름과 필드 위치, 라이브러리 이름을 전달 하 여 라이브러리 이름을 지정할 수 있습니다.

```csharp
[Static]
interface LonelyClass {
    [Field ("SomeSharedLibrarySymbol", "SomeSharedLibrary")]
    NSString SomeSharedLibrarySymbol { get; }
}
```

정적으로 연결 하는 경우 라이브러리가 없을 바인딩을 사용 해야 하므로 `__Internal` 이름:

```csharp
[Static]
interface LonelyClass {
    [Field ("MyFieldFromALibrary", "__Internal")]
    NSString MyFieldFromALibrary { get; }
}
```

<a name="Binding_Enums" />

### <a name="binding-enums"></a>열거형 바인딩

추가할 수 있습니다 `enum` 바인딩을에서 직접 파일을 쉽게 (해야 하는 바인딩 및 최종 프로젝트 모두에서 컴파일할 수)는 다른 소스 파일을 사용 하지 않고 API 정의 내에서 사용 합니다.

예제:

```csharp
[Native] // needed for enums defined as NSInteger in ObjC
enum MyEnum {}

interface MyType {
    [Export ("initWithEnum:")]
    IntPtr Constructor (MyEnum value);
}
```

바꾸려면 고유한 열거형을 만들 수 이기도 `NSString` 상수입니다. 생성기는이 예제의 **자동으로** 열거형 값 및 NSString 상수를 변환 하는 메서드를 만듭니다.

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

위의 예제에서 데코 레이트 하도록 결정할 수 있습니다 `void Perform (NSString mode);` 사용 하 여는 [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) 특성입니다. 이 그러면 **숨기기** 바인딩 소비자에서 상수 기반 API입니다.

편리한 API 대체 사용으로 형식 서브클래싱 제한 됩니다 있지만 [ `[Wrap]` ](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) 특성입니다. 생성 된 해당 메서드는 아닙니다. `virtual`, 있습니다 즉, 여부 될 수 있는 있으며 재정의할 수, 적합 하지 않습니다.

대안은 원래 표시할 `NSString`-을 기반으로 정의 `[Protected]`합니다. 이렇게 하면 작업에 필요한 경우 하위 클래스 및 wrap'ed 버전은 계속 작동 및 재정의 된 메서드를 호출 합니다.

### <a name="binding-nsvalue-nsnumber-and-nsstring-to-a-better-type"></a>바인딩 `NSValue`하십시오 `NSNumber`, 및 `NSString` 더 나은 형식

[ `[BindAs]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) 특성을 사용 하면 바인딩 `NSNumber`, `NSValue` 및 `NSString`(열거형)를 보다 정확 하 게 C# 형식으로. 특성을 더 유용 하 고 정확 하 게 만드는 데 사용할 수는 네이티브 API 통한.NET API.

(반환 값)에 대 한 메서드, 매개 변수 및 속성을 데코 레이트 [ `[BindAs]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute)합니다. 단, 멤버는 **안** 내를 [ `[Protocol]` ](~/cross-platform/macios/binding/binding-types-reference.md#ProtocolAttribute) 또는 [ `[Model]` ](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute) 인터페이스입니다.

예를 들어:

```csharp
[return: BindAs (typeof (bool?))]
[Export ("shouldDrawAt:")]
NSNumber ShouldDraw ([BindAs (typeof (CGRect))] NSValue rect);
```

출력 됩니다.

```csharp
[Export ("shouldDrawAt:")]
bool? ShouldDraw (CGRect rect) { ... }
```

하면 내부적으로 `bool?`  <->  `NSNumber` 하 고 `CGRect`  <->  `NSValue` 변환 합니다.

[`[BindAs]`](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) 에서는 다음 배열을 `NSNumber` `NSValue` 고 `NSString`(열거형)입니다.

예를 들어:

```csharp
[BindAs (typeof (CAScroll []))]
[Export ("supportedScrollModes")]
NSString [] SupportedScrollModes { get; set; }
```

출력 됩니다.

```csharp
[Export ("supportedScrollModes")]
CAScroll [] SupportedScrollModes { get; set; }
```

`CAScroll` `NSString` 열거형을 지 원하는 오른쪽 가져온 됩니다 `NSString` 값 및 형식 변환을 처리 합니다.

참조 하십시오 합니다 [ `[BindAs]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) 설명서를 지원 되는 변환 형식을 참조 하십시오.

<a name="Binding_Notifications" />

### <a name="binding-notifications"></a>바인딩 알림

알림은에 게시 되는 메시지는 `NSNotificationCenter.DefaultCenter` 다른 응용 프로그램의 특정 부분에서 메시지를 브로드캐스트 메커니즘으로 사용 됩니다. 개발자는 일반적으로 사용 하 여 알림을 구독 합니다 [NSNotificationCenter](https://developer.xamarin.com/api/type/Foundation.NSNotificationCenter/)의 [AddObserver](https://developer.xamarin.com/api/type/Foundation.NSNotificationCenter/M/AddObserver/) 메서드. 에 저장 된 페이로드를 응용 프로그램에 알림 센터에 메시지를 게시 하는 경우 일반적으로 포함 된 [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/) 사전입니다. 이 사전은 약하게 형식화 하 고 사용자는 일반적으로 키가 사전에 사전에 저장할 수 있는 값의 형식을 사용할 수 있는 설명서에서를 읽이 필요가 발생 하기 쉬우므로 오류는 정보를 가져오기. 경우에 따라 키의 존재도 부울 값으로 사용 됩니다.

Xamarin.iOS 바인딩 생성기 알림을 바인딩하는 개발자를 위한 지원을 제공 합니다. 설정 하면이 작업을 수행 하는 [ `[Notification]` ](~/cross-platform/macios/binding/binding-types-reference.md#NotificationAttribute) 도 되었습니다 속성에 특성으로 태그가 지정을 [ `[Field]` ](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute) 속성 (수 공용 또는 개인).

이 특성을 하지 않는 페이로드를 전달 하는 알림에 대 한 인수 없이 사용할 수 있습니다 하거나 지정할 수 있습니다는 `System.Type` "EventArgs"로 끝나는 이름의 일반적으로 API 정의에서 다른 인터페이스를 참조 하는 합니다. 생성기 바뀝니다 인터페이스 클래스를 서브클래싱하는 `EventArgs` 여기에 나열 된 속성을 모두 포함 됩니다. 합니다 [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 특성은 값을 인출 Objective-c로 사전 조회 하는 데 키의 이름을 나열 하는 EventArgs 클래스에 사용 해야 합니다.

예를 들어:

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassDidStartNotification")]
    NSString DidStartNotification { get; }
}
```

위의 코드는 중첩 된 클래스를 생성 하는 `MyClass.Notifications` 다음 메서드를 사용 하 여:

```csharp
public class MyClass {
   [..]
   public Notifications {
      public static NSObject ObserveDidStart (EventHandler<NSNotificationEventArgs> handler)
   }
}
```

사용자 코드의 다음 쉽게 알림을 구독할 수에 게시 합니다 [NSDefaultCenter](https://developer.xamarin.com/api/property/Foundation.NSNotificationCenter.DefaultCenter/) 다음과 같은 코드를 사용 하 여:

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
});
```

반환 된 값 `ObserveDidStart` 쉽게 다음과 같은 알림 수신을 중지할 데 사용할 수 있습니다.

```csharp
token.Dispose ();
```

하거나 호출할 수 있습니다 [NSNotification.DefaultCenter.RemoveObserver](https://developer.xamarin.com/api/member/Foundation.NSNotificationCenter.RemoveObserver/p/Foundation.NSObject/) 토큰을 전달 합니다. 도우미를 지정 해야 알림이 매개 변수를 포함할 경우 `EventArgs` 다음과 같은 인터페이스:

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

위의 생성 됩니다는 `MyScreenChangedEventArgs` 클래스를 `ScreenX` 및 `ScreenY` 에서 데이터를 인출 하는 속성을 [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/) "ScreenXKey" 및 "ScreenYKey" 키를 사용 하 여 사전 각각 적절 한 변환을 적용 합니다. 합니다 `[ProbePresence]` 특성을 사용 하는 생성기에 대 한 키에 설정 된 경우 프로브를 `UserInfo`, 값을 추출 하려고 하는 대신 합니다. 이 경우 키의 현재 상태 (일반적으로 부울 값)의 값에 사용 됩니다.

이 옵션을 사용 하면 다음과 같은 코드를 작성할 수 있습니다.

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

<a name="Binding_Categories" />

### <a name="binding-categories"></a>바인딩 범주

범주는 클래스에서 사용 가능한 메서드와 속성 집합을 확장 하는 데 사용 하는 Objective-c 메커니즘입니다.   실제로 사용할 하거나 기본 클래스의 기능을 확장 하 (예를 들어 `NSObject`) 특정 프레임 워크에 연결 되는 경우 (예를 들어 `UIKit`), 해당 메서드를 사용할 수 있지만 새 프레임 워크에 연결 된 경우에 수행 합니다.   경우에 따라 다른 기능으로는 클래스의 기능을 구성 하려면 사용 됩니다.   비슷하며, C# 확장 메서드는 합니다. 이 범주는 모양을 주기-c:는

```csharp
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

위의 예제에서는 경우에 인스턴스를 확장 하는 라이브러리는 것 `UIView` 메서드를 사용 하 여 `makeBackgroundRed`합니다.

바인딩하려는 이러한 경우 사용할 수 있습니다 합니다 [ `[Category]` ](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute) 인터페이스 정의에 특성입니다.  사용 하는 경우는 [ `[Category]` ](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute) 특성의 의미를 [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) 특성이 기본 클래스 확장을 확장 하는 형식이 되도록 지정 하는 데 사용 되 고에서 변경 합니다.

에서는 다음 방법을 `UIView` 확장은 바인딩된 있으며 C# 확장 메서드로 설정 합니다.

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

위의 만듭니다는 `MyUIViewExtension` 포함 하는 클래스는 `MakeBackgroundRed` 확장 메서드.  즉, 이제 "MakeBackgroundRed"를 호출할 수 있도록 모든 `UIView` 목표 c 얻게 동일한 기능을 제공 하는 하위 클래스 경우에 따라 다른 시스템 클래스를 확장할 필요가 있지만 장식 용도로 기능을 구성 하려면 범주가 사용 됩니다.  다음과 같습니다.

```csharp
@interface SocialNetworking (Twitter)
- (void) postToTwitter:(Message *) message;
@end

@interface SocialNetworking (Facebook)
- (void) postToFacebook:(Message *) message andPicture: (UIImage*)
picture;
@end
```

사용할 수 있지만 합니다 [ `[Category]` ](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute) 특성도 선언의이 장식 스타일을 추가할 수 있습니다만 해당 클래스 정의에 모두 있습니다.  둘 다 동일한 달성 합니다.

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

범주를 병합 합니다. 이러한 경우에만 짧습니다.

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

### <a name="binding-blocks"></a>바인딩 요소

블록은 C# 무명 메서드는 이와 동일한 기능을 보여주기 위한 것입니다. 상태로 Apple에서 도입 된 새 구문을 예를 들어를 `NSSet` 클래스에는 이제이 메서드는 노출 합니다.

```csharp
- (void) enumerateObjectsUsingBlock:(void (^)(id obj, BOOL *stop) block
```

위의 설명 라는 메서드를 선언 `enumerateObjectsUsingBlock:` 명명 된 인수 하나를 사용 하는 `block`합니다. 이 블록 있는 현재 환경 ("this"이 포인터에 대 한 지역 변수 및 매개 변수)를 캡처를 지 원하는 C# 무명 메서드와 비슷합니다. 위의 `NSSet` 메서드는 `NSObject` (`id obj` 부분) 및 부울 (`BOOL *stop`) 부분에 대한 포인터의 두 매개 변수가있는 블록을 호출합니다.

그런 다음 인터페이스의 각 속성에는 강력한 형식의 접근자를 될 것입니다.

```csharp
// This declares the callback signature for the block:
delegate void NSSetEnumerator (NSObject obj, ref bool stop)

// Later, inside your definition, do this:
[Export ("enumerateObjectUsingBlock:")]
void Enumerate (NSSetEnumerator enum)
```

기본적으로 코드는 접근자를 만드는 정적 클래스의 "키" 접미사를 사용 하 여 속성의 이름을 사용 됩니다.

```csharp
var myset = new NSMutableSet ();
myset.Add (new NSString ("Foo"));

s.Enumerate (delegate (NSObject obj, ref bool stop){
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

즉, 외부 파일 나 getter 및 setter 속성 마다 수동으로 만드는 것도 수동으로 키를 조회 하지 않아도 더 이상 강력한 형식의 접근자를 사용 하 여 만드는 직접.

```csharp
var myset = new NSMutableSet ();
mySet.Add (new NSString ("Foo"));

s.Enumerate ((obj, stop) => {
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

<a name="GeneratingAsync" />

### <a name="asynchronous-methods"></a>비동기 메서드

다음은 전체 바인딩을 모양을입니다.

참조 해야 하는 경우에 [ 멤버 다른 필드를 (즉 이름이 아니라 접미사를 사용 하 여 속성의 `[Async]`)를 사용 하 여 속성을 데코 레이트 수는 ](~/cross-platform/macios/binding/binding-types-reference.md#AsyncAttribute)   는 이름 특성 사용 하려고 합니다.  When you apply this to a method, the binding generator will generate a version of that method with the suffix `Async`.  If the callback takes no parameters, the return value will be a `Task`, if the callback takes a parameter, the result will be a `Task<T>`.  If the callback takes multiple parameters, you should set the `ResultType` or `ResultTypeName` to specify the desired name of the generated type which will hold all the properties.

예제:

```csharp
[Export ("loadfile:completed:")]
[Async]
void LoadFile (string file, Action<string> completed);
```

The above code will generate both the LoadFile method, as well as:

```csharp
[Export ("loadfile:completed:")]
Task<string> LoadFileAsync (string file);
```

<a name="Surfacing_Strong_Types" />

### <a name="surfacing-strong-types-for-weak-nsdictionary-parameters"></a>Surfacing strong types for weak NSDictionary parameters

In many places in the Objective-C API, parameters are passed as weakly-typed `NSDictionary` APIs with specific keys and values, but these are error prone (you can pass invalid keys, and get no warnings; you can pass invalid values, and get no warnings) and frustrating to use as they require multiple trips to the documentation to lookup the possible key names and values.

The solution is to provide a strongly-typed version that provides the strongly-typed version of the API and behind the scenes maps the various underlying keys and values.

So for example, if the Objective-C API accepted an `NSDictionary` and it is documented as taking the key `XyzVolumeKey` which takes an `NSNumber` with a volume value from 0.0 to 1.0 and a `XyzCaptionKey` that takes a string, you would want your users to have a nice API that looks like this:

```csharp
public class  XyzOptions {
    public nfloat? Volume { get; set; }
    public string Caption { get; set; }
}
```

The `Volume` property is defined as nullable float, as the convention in Objective-C does not require these dictionaries to have the value, so there are scenarios where the value might not be set.

To do this, you need to do a few things:

* Xamarin.iOS 런타임은 자동으로 변환에 C# 배열 [ 변환을 수행 하는 다시 예를 들어 허수 Objective-c 메서드는 반환 하 고는 ](https://developer.xamarin.com/api/type/Foundation.DictionaryContainer/) 의 :
* Declare overloads for the methods taking `NSDictionary` to take the new strongly-typed version.

사용자 추측, 또는 배열에 포함 된 개체의 실제 형식을 확인 하려면 설명서를 조회 하도록 하지 않고도 실제 형식 사용 하 여 적절 한 코드 완성 기능을 제공 하기 위해 IDE를 허용 하는이 강력한 형식의 C# 배열을 사용 하는 개념이입니다.  We first explore how to do this manually so you understand what is going on, and then the automatic approach.

You need to create a supporting file for this, it does not go into your contract API.  This is what you would have to write to create your XyzOptions class:

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

You then should provide a wrapper method that surfaces the high-level API, on top of the low-level API.

```csharp
[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

If your API does not need to be overwritten, you can safely hide the NSDictionary-based API by using the [`[Internal]`](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) attribute.

As you can see, we use the [`[Wrap]`](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) attribute to surface a new API entry point, and we surface it using our strongly-typed `XyzOptions` class.  이며이 응용 프로그램에서 메서드는 일반적으로 사용 됩니다.

C# 개발자에 게 편리한 바인딩을 확인, 일반적으로 제공한 사용 하는 메서드를 `XyzOptionsKeys` C# 대리자 및 람다 식 대신 사용할 수 있는 매개 변수는 합니다.  You would typically group the keys that an API surfaces in a static class like `XyzOptionsKeys`, like this:

```csharp
[Static]
class XyzOptionKeys {
    [Field ("kXyzVolumeKey")]
    NSString VolumeKey { get; }

    [Field ("kXyzCaptionKey")]
    NSString CaptionKey { get; }
}
```

이제 다음과 같은 사용자 코드를 작성할 수 있습니다.  This avoids plenty of the boilerplate, and you can define the dictionary directly in your API contract, instead of using an external file.

To create a strongly-typed dictionary, introduce an interface in your API and decorate it with the [StrongDictionary](~/cross-platform/macios/binding/binding-types-reference.md#StrongDictionary) attribute.  This tells the generator that it should create a class with the same name as your interface that will derive from `DictionaryContainer` and will provide strong typed accessors for it.

The [`[StrongDictionary]`](~/cross-platform/macios/binding/binding-types-reference.md#StrongDictionary) attribute takes one parameter, which is the name of the static class that contains your dictionary keys.  Then each property of the interface will become a strongly-typed accessor.  By default, the code will use the name of the property with the suffix "Key" in the static class to create the accessor.

This means that creating your strongly-typed accessor no longer requires an external file, nor having to manually create getters and setters for every property, nor having to lookup the keys manually yourself.

This is what your entire binding would look like:

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

In case you need to reference in your `XyzOption` members a different field (that is not the name of the property with the suffix `Key`), you can decorate the property with an [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) attribute with the name that you want to use.

<a name="Type_mappings" />

## <a name="type-mappings"></a>두 번째 방법은 방법을 보여 줍니다는 Objective-c 메서드로 수 개체를 사용 내용이 수정.

이 순수 출력 값이 아닌 참조로 전달 되는 것입니다.

<a name="Simple_Types" />

### <a name="simple-types"></a>바인딩을이 같습니다.

메모리 관리 특성

|Objective-c 형식 이름|Xamarin.iOS 통합 API 형식|
|---|---|
|`BOOL`, `GLboolean`|`bool`|
|`NSInteger`|`nint`|
|`NSUInteger`|`nuint`|
|`CFTimeInterval` / `NSTimeInterval`|`double`|
|`NSString` ([NSString 바인딩을 자세한](~/ios/internals/api-design/nsstring.md))|`string`|
|`char *`|`string` (참고: [ `[PlainString]` ](~/cross-platform/macios/binding/binding-types-reference.md#plainstring))|
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

Xamarin.iOS 런타임은 자동으로 변환에 C# 배열 `NSArrays` 변환을 수행 하는 다시 예를 들어 허수 Objective-c 메서드는 반환 하 고는 `NSArray` 의 `UIViews`:

```csharp
// Get the peer views - untyped
- (NSArray *)getPeerViews ();

// Set the views for this container
- (void) setViews:(NSArray *) views
```

다음과 같이 바인딩됩니다.

```csharp
[Export ("getPeerViews")]
UIView [] GetPeerViews ();

[Export ("setViews:")]
void SetViews (UIView [] views);
```

사용자 추측, 또는 배열에 포함 된 개체의 실제 형식을 확인 하려면 설명서를 조회 하도록 하지 않고도 실제 형식 사용 하 여 적절 한 코드 완성 기능을 제공 하기 위해 IDE를 허용 하는이 강력한 형식의 C# 배열을 사용 하는 개념이입니다.

배열에 포함 된 실제 가장 많이 파생 된 형식은 다운 되지 추적할 수 없는 경우에 사용할 수 있습니다 `NSObject []` 반환 값으로.

<a name="Selectors" />

### <a name="selectors"></a>선택기

특수 형식으로 서의 Objective C API에 선택 기가 표시 `SEL`합니다. 형식에 매핑하는 선택기를 바인딩하는 경우 `ObjCRuntime.Selector`합니다.  일반적으로 선택기 개체, 대상 개체와 선택기를 사용 하 여 대상 개체를 호출 하는 API에서 노출 됩니다. C# 대리자에 해당 둘 다 기본적으로 제공 합니다: 메서드를 호출할 개체와 호출할 메서드를 모두 캡슐화 하는 것입니다.

다음은 바인딩을 모습입니다.

```csharp
interface Button {
   [Export ("setTarget:selector:")]
   void SetTarget (NSObject target, Selector sel);
}
```

이며이 응용 프로그램에서 메서드는 일반적으로 사용 됩니다.

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

C# 개발자에 게 편리한 바인딩을 확인, 일반적으로 제공한 사용 하는 메서드를 `NSAction` C# 대리자 및 람다 식 대신 사용할 수 있는 매개 변수는 `Target+Selector`합니다. 숨기기 일반적으로이 작업을 수행 하는 `SetTarget` 메서드를 사용 하 여 플래그를 [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) 특성 및 다음 다음과 같은 새로운 도우미 메서드를 노출:

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

사용 하는 메서드를 바인딩하는 경우는 `NSString`에 C# 문자열 형식으로 모두 반환 형식 및 매개 변수를 대체할 수 있습니다.

경우에만 사용 하는 것이 좋습니다는 `NSString` 직접 경우 문자열을 토큰으로 사용 됩니다. 문자열에 대 한 자세한 내용은 및 `NSString`읽어보세요 합니다 [NSString에서 API 디자인](~/ios/internals/api-design/nsstring.md) 문서.

일부 드문 경우에서 API는 C와 같은 문자열 노출 될 수 있습니다 (`char *`)는 Objective C 문자열 대신 (`NSString *`). 이러한 경우에 주석을 달 수 있습니다 사용 하 여 매개 변수를 [ `[PlainString]` ](~/cross-platform/macios/binding/binding-types-reference.md#plainstring) 특성입니다.

<a name="outref_parameters" />

### <a name="outref-parameters"></a>out / ref 매개 변수

일부 Api 매개 변수, 반환 값 또는 참조로 매개 변수를 전달 합니다.

일반적으로 시그니처는 다음과 같습니다.

```csharp
- (void) someting:(int) foo withError:(NSError **) retError
- (void) someString:(NSObject **)byref
```

첫 번째 예제에서는 오류 코드에 대 한 포인터를 반환 하는 일반적인 Objective-c 관용구를 보여 줍니다.는 `NSError` 포인터를 전달 하 고 값이 반환 될 때 설정 됩니다.   두 번째 방법은 방법을 보여 줍니다는 Objective-c 메서드로 수 개체를 사용 내용이 수정.   이 순수 출력 값이 아닌 참조로 전달 되는 것입니다.

바인딩을이 같습니다.

```csharp
[Export ("something:withError:")]
void Something (nint foo, out NSError error);
[Export ("someString:")]
void SomeString (ref NSObject byref);
```

<a name="Memory_management_attributes" />

### <a name="memory-management-attributes"></a>메모리 관리 특성

사용 하는 경우는 [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 특성을 전달 하는 호출된 된 메서드에 의해 보존 되는 데이터, 예를 들어 두 번째 매개 변수로 전달 하 여 인수 의미 체계를 지정할 수 있습니다.

```csharp
[Export ("method", ArgumentSemantic.Retain)]
```

위에서 "보관" 의미 체계를 가진 것으로 값을 플래그입니다. 사용할 수 있는 의미 체계는:

-  할당
-  복사
-  보유

<a name="Style_Guidelines" />

### <a name="style-guidelines"></a>스타일 지침

<a name="Using_[Internal]" />

#### <a name="using-internal"></a>[내부]를 사용 하 여

사용할 수는 [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) 공용 API에서 메서드를 숨기려면 특성입니다. 노출 된 API는 너무 낮은 수준 구현에서이 메서드를 기준으로 별도 파일을 제공 하려는 경우에서이 작업을 수행 하는 것이 좋습니다.

바인딩 생성기에서 한계에 부 딪 실행에서는 일부 고급 시나리오에서 바인딩되지 않은 형식을 노출할 수는 예를 들어 고유한 방식으로 바인딩할 및 래핑할 형식과 직접 고유한 방식으로 원하는 때에 다음이 사용할 수도 있습니다.

<a name="Event_Handlers_and_Callbacks" />

## <a name="event-handlers-and-callbacks"></a>이벤트 처리기 및 콜백에서

Objective-c 클래스는 일반적으로 알림 브로드캐스트 또는 대리자 클래스 (Objective-c 대리자)에 메시지를 전송 하 여 정보를 요청 합니다.

이 모델에서 완전히 지원 하 고 Xamarin.iOS에 표시 하는 동안 발생할 수 번거롭습니다. Xamarin.iOS에는 C# 이벤트 패턴 및 이러한 상황에서 사용할 수 있는 클래스에서 메서드 콜백 시스템을 노출 합니다. 이렇게 하면 실행 하기 위해 다음과 같은 코드.

```csharp
button.Clicked += delegate {
    Console.WriteLine ("I was clicked");
};
```

바인딩 생성기는 C# 패턴으로 Objective-c 패턴을 매핑할 필요한 입력을 줄일 수 있습니다.

Xamarin.iOS 1.4를 사용 하 여 시작을 특정 Objective-c 대리자에 대 한 바인딩을 만들어 C# 이벤트와 호스트 유형에 대 한 속성으로 대리자를 노출 하는 생성기를 지시할 수 됩니다.

이 프로세스에 관련 된 두 클래스는, 현재는 이벤트를 내보냅니다 하 고 전송에 해당 하는 것은 호스트 클래스를 `Delegate` 또는 `WeakDelegate` 및 실제 대리자 클래스입니다.

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

클래스를 래핑할 다음을 수행 해야 합니다.

-  호스트 클래스에 추가 하 여 [`[BaseType]`](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute)  
   해당 대리자 및 C# 이름을 노출 하는 역할을 하는 형식 선언 합니다. 위의 예에서 `typeof (MyClassDelegate)` 고 `WeakDelegate` 각각.
-  두 개 이상의 매개 변수가 있는 각 메서드에 대리자 클래스에 자동으로 생성 된 EventArgs 클래스에 대 한 사용 하려는 형식을 지정 해야 합니다.

바인딩 생성기를 래핑하는 단일 이벤트 대상만 제한 되지 않습니다., 이므로 일부 Objective-c 클래스를 둘 이상의 메시지를 내보내는 대리자 가능한 배열을이 설치를 지원 하기 위해 제공 해야 합니다. 대부분의 설치 프로그램을 필요 하지 않습니다 하지만 생성기는 이러한 사례를 지원 하도록 준비 합니다.

결과 코드가 표시 됩니다.

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

`EventArgs` 의 이름을 지정 하는 데 사용 되는 `EventArgs` 클래스를 생성 합니다. 서명 당 하나를 사용 해야 (이 예제에서는 합니다 `EventArgs` 포함 됩니다는 `With` 형식 nint 속성).

위의 정의 사용 하 여 생성기는 생성 된 MyClass에 다음 이벤트를 생성 합니다.

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

콜백 이벤트 호출에서와 마찬가지로, 차이점은 여러 잠재적인 구독자는 대신 (여러 메서드를 후크 할 수는 예를 들어를 `Clicked` 이벤트 또는 `DownloadFinished` 이벤트) 콜백을 단일 구독자를 하나만 사용할 수 있습니다.

유일한 차이점은 이름을 노출 하는 대신, 프로세스는 동일 합니다 `EventArgs` 실제로 EventArgs 생성 되는 클래스 이름 결과 C# 대리자 이름 하는 데 사용 됩니다.

대리자 클래스의 메서드는 값을 반환 하는 경우 바인딩 생성기는 부모 클래스 대신 이벤트의에서 대리자 메서드를이 매핑됩니다. 이러한 경우에 반환 되어야 하는 메서드에서 사용자 연결 하지 않는 경우 대리자에 기본값을 제공 해야 합니다. 이 작업에 [ `[DefaultValue]` ](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueAttribute) 하거나 [ `[DefaultValueFromArgument]` ](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueFromArgumentAttribute) 특성입니다.

[`[DefaultValue]`](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueAttribute) 반환 값을 하드 코딩 됩니다 하는 동안 [ `[DefaultValueFromArgument]` ](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueFromArgumentAttribute) 입력된 인수를 지정 하는 데 사용 됩니다.

<a name="Enumerations_and_Base_Types" />

## <a name="enumerations-and-base-types"></a>기본 형식과 열거형

열거형 또는 u c h 인터페이스 정의 시스템에서 직접 지원 되지 않는 기본 형식을 참조할 수 있습니다. 이렇게 하려면 별도 파일로 core 형식과 열거형 놓고 u c h를 제공 하는 추가 파일 중 일부로 포함 합니다.

<a name="Linking_the_Dependencies" />

## <a name="linking-the-dependencies"></a>종속성 링크

응용 프로그램의 일부분이 아닌 Api에 바인딩하는 경우에 이러한 라이브러리에 대 한 실행 파일은 연결 되었는지 확인 해야 합니다.

Xamarin.iOS 라이브러리를 연결 하는 방법을 알려 주어 야,이 수행할 수 있습니다를 호출 하도록 빌드 구성을 변경 하 여는 `mtouch` 일부 추가 사용 하 여 명령을 빌드를 사용 하 여 새 라이브러리를 사용 하 여 연결 하는 방법을 지정 하는 인수는 "-gcc_flags" 옵션 다음과 같이 프로그램에 필요한 모든 추가 라이브러리를 포함 하는 따옴표 붙은 문자열 뒤에:

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load -lSystemLibrary -framework CFNetwork -ObjC"
```

위의 예제에서는 연결 `libMyLibrary.a`, `libSystemLibrary.dylib` 및 `CFNetwork` 프레임 워크 라이브러리를 초기화 합니다.

하거나 어셈블리 수준 이용할 수 있습니다 [ `[LinkWithAttribute]` ](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute), 계약 파일에 포함할 수 있는 (같은 `AssemblyInfo.cs`).
사용 하는 경우는 [ `[LinkWithAttribute]` ](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute), 네이티브 라이브러리 내리는 바인딩, 응용 프로그램을 사용 하 여 네이티브 라이브러리를 포함은이 시점에 제공 해야 합니다. 예를 들어:

```csharp
// Specify only the library name as a constructor argument and specify everything else with properties:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget = LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]

// Or you can specify library name *and* link target as constructor arguments:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]
```

궁금할 수 있습니다, 수행 해야 하는 이유 `-force_load` 명령인 이유 이며-ObjC 플래그의 코드를 컴파일할 수 있지만 범주 (링커/컴파일러 데드 코드가 제거 하면 제거 것)를 지 원하는 데 필요한 메타 데이터를 유지 하지 않습니다 있으며 Xamarin.iOS에 대 한 런타임 시 필요 합니다.

<a name="Assisted_References" />

## <a name="assisted-references"></a>기술된 참조

작업 시트 및 경고 상자와 같은 일부 임시 개체는 추적 하기 위해 개발자에 번거로운 및 바인딩 생성기 약간를 여기서 도움이 합니다.

예를 들어 메시지를 표시 하 고 그런 다음 생성 되는 클래스를 있다면를 `Done` 이벤트 들이 처리 하는 기존의 방법은 것:

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

위 시나리오에서 개발자는 자신 및 하거나 누수 된 개체에 대 한 참조를 유지 하거나 적극적으로 자신의 상자에 대 한 참조를 해제 하려면 필요 합니다.  바인딩 코드 생성기를 지원 하지만 사용자에 대 한 참조의 추적 및 지우기 특수 메서드를 호출 하는 경우, 위 코드 다음 할:

```csharp
class Demo {
    void ShowError (string msg)
    {
        var box = new MessageBox (msg);
        box.Done += { ... };
    }
}
```

지역 변수를 사용 하 여 작동 하는지는 개체가 소멸 되는 경우 참조를 제거할 필요 하다 고 및 변수 인스턴스를 보관 하는 데 필요한 더 이상 성은 알 수 없습니다.

를 이용 하려면이 클래스 설정 된 이벤트 속성 있어야 합니다 [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) 선언 및는 `KeepUntilRef` 변수가 개체 처럼 해당 작업을 완료 될 때 호출 되는 메서드의 이름으로 설정 이:

```csharp
[BaseType (typeof (NSObject), KeepUntilRef="Dismiss"), Delegates=new string [] { "WeakDelegate" }, Events=new Type [] { typeof (SomeDelegate) }) ]
class Demo {
    [Export ("show")]
    void Show (string message);
}
```

<a name="Inheriting_Protocols" />

## <a name="inheriting-protocols"></a>프로토콜을 상속

Xamarin.iOS v3.2 기준으로 사용 하 여 표시 된 프로토콜에서 상속 지원 합니다 [ `[Model]` ](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute) 속성입니다. 같은 특정 API 패턴에서 유용 `MapKit` 여기서는 `MKOverlay` 프로토콜에서 상속 되는 `MKAnnotation` , 프로토콜 및 다양 한 클래스에서 상속 하는 채택 된 `NSObject`합니다.

지금까지 모든 구현 프로토콜 복사할 필요 했습니다 하지만 이러한 경우 수 이제는 `MKShape` 클래스에서 상속 된 `MKOverlay` 프로토콜 하며 필요한 모든 메서드를 자동으로 생성 합니다.

## <a name="related-links"></a>관련 링크

- [바인딩 샘플](https://developer.xamarin.com/samples/BindingSample/)
- [Objective-c 바인딩 라이브러리를 빌드할 Xamarin University 과정:](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University 과정: 목표 Sharpie 사용 하 여 Objective-c 바인딩 라이브러리를 빌드](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
