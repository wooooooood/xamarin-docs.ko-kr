---
title: 고급 (수동) 실제 예제
ms.prod: xamarin
ms.assetid: 044FF669-0B81-4186-97A5-148C8B56EE9C
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 82bca525433e5c8fea3a29250afb83962f2e64fc
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
---
# <a name="advanced-manual-real-world-example"></a>고급 (수동) 실제 예제


**사용 하 여이 예제는 [Facebook에서 POP 라이브러리](https://github.com/facebook/pop)합니다.**


이 섹션에서는 Apple의를 사용 하는 바인딩 대 한 고급 액세스 `xcodebuild` POP 프로젝트를 빌드하 도구 클릭 한 다음 수동으로 Sharpie 목표에 대 한 입력을 추론 합니다. 기본적으로 이전 섹션의 내부에서 수행 하는 목표 Sharpie 다룹니다.

```csharp
 $ git clone https://github.com/facebook/pop.git
Cloning into 'pop'...
   _(more git clone output)_

$ cd pop
```

POP 라이브러리 Xcode 프로젝트에 있기 때문에 (`pop.xcodeproj`)만 사용할 수 `xcodebuild` POP 빌드할 수 있습니다. 이 프로세스는 목표 Sharpie 구문 분석 해야 하는 헤더 파일을 생성할 차례로 수 있습니다. 이 때문에 바인딩을 중요 하기 전에 작성 합니다. 통해 빌드할 때 `xcodebuild` 전달 SDK 식별자와 동일한 아키텍처를 확인 하려면 목표 Sharpie에 전달할 (및 기억 목표 Sharpie 3.0에서 일반적으로 이렇게 하려면!):

```csharp
$ xcodebuild -sdk iphoneos9.0 -arch arm64

Build settings from command line:
    ARCHS = arm64
    SDKROOT = iphoneos8.1
 
=== BUILD TARGET pop OF PROJECT pop WITH THE DEFAULT CONFIGURATION (Release) ===
 
...
 
CpHeader pop/POPAnimationTracer.h build/Headers/POP/POPAnimationTracer.h
    cd /Users/aaron/src/sharpie/pop
    export PATH="/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/usr/bin:/Applications/Xcode.app/Contents/Developer/usr/bin:/Users/aaron/bin::/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/opt/X11/bin:/usr/local/git/bin:/Users/aaron/.rvm/bin"
    builtin-copy -exclude .DS_Store -exclude CVS -exclude .svn -exclude .git -exclude .hg -strip-debug-symbols -strip-tool /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/strip -resolve-src-symlinks /Users/aaron/src/sharpie/pop/pop/POPAnimationTracer.h /Users/aaron/src/sharpie/pop/build/Headers/POP
 
...
 
** BUILD SUCCEEDED **
```

일부로 많은 빌드 정보를 콘솔에 출력 됩니다 `xcodebuild`합니다. 위에 표시 된 대로 "CpHeader" 대상 실행 된 확인할 수 있습니다 헤더 파일이 빌드 출력 디렉터리에 복사 되었으므로 가정해 보십시오. 이 경우는 수 있으며 바인딩 보다 쉽게: 네이티브 라이브러리 빌드의 일환으로, 헤더 파일 종종 위치에 복사 됩니다는 "공개적 으로" 사용 가능 바인딩에 대 한 구문 분석을 쉽게 만들 수 있는 합니다. 이 경우 회원님의 있는 POP의 헤더 파일은 `build/Headers` 디렉터리입니다.

이제 POP 바인딩할 준비가 되었습니다. 회원님의 SDK에 대 한 빌드 한다고 `iphoneos8.1` 와 `arm64` 아키텍처 및 헤더 파일에 관심 있는 `build/Headers` 아래 POP git 체크 아웃 합니다. 대해는 `build/Headers` 디렉터리 헤더 파일의 수를 살펴보겠습니다.

```csharp
$ ls build/Headers/POP/
POP.h                    POPAnimationTracer.h     POPDefines.h
POPAnimatableProperty.h  POPAnimator.h            POPGeometry.h
POPAnimation.h           POPAnimatorPrivate.h     POPLayerExtras.h
POPAnimationEvent.h      POPBasicAnimation.h      POPPropertyAnimation.h
POPAnimationExtras.h     POPCustomAnimation.h     POPSpringAnimation.h
POPAnimationPrivate.h    POPDecayAnimation.h
```

보면 `POP.h`, 것이 라이브러리의 기본 최상위 헤더 파일을 볼 수 있습니다 `#import`s 다른 파일입니다. 이 인해만 하면 전달 `POP.h` 목표 Sharpie를 clang 나머지 백그라운드에서 수행 될 작업:

```csharp
$ sharpie bind -output Binding -sdk iphoneos8.1 \
    -scope build/Headers build/Headers/POP/POP.h \
    -c -Ibuild/Headers -arch arm64

Parsing Native Code...

Binding...
  [write] ApiDefinitions.cs
  [write] StructsAndEnums.cs

Binding Analysis:
  Automated binding is complete, but there are a few APIs which have
  been flagged with [Verify] attributes. While the entire binding
  should be audited for best API design practices, look more closely
  at APIs with the following Verify attribute hints:

  ConstantsInterfaceAssociation (1 instance):
    There's no fool-proof way to determine with which Objective-C
    interface an extern variable declaration may be associated.
    Instances of these are bound as [Field] properties in a partial
    interface into a near-by concrete interface to produce a more
    intuitive API, possibly eliminating the 'Constants' interface
    altogether.

  StronglyTypedNSArray (4 instances):
    A native NSArray* was bound as NSObject[]. It might be possible
    to more strongly type the array in the binding based on
    expectations set through API documentation (e.g. comments in the
    header file) or by examining the array contents through testing.
    For example, an NSArray* containing only NSNumber* instances can
    be bound as NSNumber[] instead of NSObject[].

  MethodToProperty (2 instances):
    An Objective-C method was bound as a C# property due to
    convention such as taking no parameters and returning a value (
    non-void return). Often methods like these should be bound as
    properties to surface a nicer API, but sometimes false-positives
    can occur and the binding should actually be a method.

  Once you have verified a Verify attribute, you should remove it
  from the binding source code. The presence of Verify attributes
  intentionally cause build failures.

  For more information about the Verify attribute hints above,
  consult the Objective Sharpie documentation by running 'sharpie
  docs' or visiting the following URL:

    http://xmn.io/sharpie-docs

Submitting usage data to Xamarin...
  Submitted - thank you for helping to improve Objective Sharpie!

Done.
```

전달 된 것을 알 수 있습니다는 `-scope build/Headers` 인수 목표 Sharpie입니다. Objective C 및 C 라이브러리 해야 하기 때문에 `#import` 또는 `#include` 기타 헤더 파일을 라이브러리 및을 바인딩하려면, API 하지의 구현 세부 정보는 `-scope` 목표 Sharpie에 정의 되어 있지 않은 모든 API를 무시 하도록 지정 하는 인수는 내 임의 위치에 파일이 `-scope` 디렉터리입니다.

그러나 알 수는 `-scope` 인수는 명확 하 게 구현 된 라이브러리에 대 한 선택적 보통, 하더라도 피해 명시적으로 제공 하는 합니다.

지정한 기는 또한 `-c -Ibuild/headers`합니다. 첫째, 고 `-c` 명령줄 인수를 해석할 중지 하 고 모든 후속 인수를 전달 하려면 Sharpie 목표를 지정 하는 인수 _clang 컴파일러에 직접_합니다. 따라서 `-Ibuild/Headers` 에서 검색할 clang 지시 하는 clang 컴파일러 인수를 포함 하는 `build/Headers`, POP 헤더 거주 변수인 합니다. Clang이이 인수를 사용 하지 않고 파일을 배치할 위치를 인식 하지 못하기는 `POP.h` 은 `#import`연산입니다. _프록시 서버 설정을 clang에 전달할 확인을 하느냐, 목표 Sharpie를 사용 하 여 거의 모든 "문제"_ 합니다.

### <a name="completing-the-binding"></a>바인딩이 완료

목표 Sharpie 이제 벌어 들인 `Binding/ApiDefinitions.cs` 및 `Binding/StructsAndEnums.cs` 파일입니다.

목표 Sharpie 기본 첫 번째 단계에서 바인딩, 이며 일부의 경우에는 것이 하기만 하면 됩니다. 하지만 개발자 일반적으로 목표 Sharpie를 완료 한 후 생성 된 파일을 수동으로 수정 해야 할 위에서 설명한 것 처럼 [문제를 수정](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) 를 처리할 수 없는 자동으로 도구를 통해 합니다.

이 두 파일 수 이제 Mac 용 Visual Studio에서 바인딩 프로젝트에 추가할 수 또는 직접 전달 될 업데이트 완료 되 면는 `btouch` 또는 `bmac` 최종 바인딩을 생성 하는 도구입니다.

바인딩 프로세스의 철저 한 설명을 참조 하십시오 우리의 [전체 연습 지침](~/ios/platform/binding-objective-c/walkthrough.md)합니다.

