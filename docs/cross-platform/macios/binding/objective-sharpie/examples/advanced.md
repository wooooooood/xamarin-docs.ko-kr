---
title: 고급 (수동) 실제 예제
description: 이 문서에서는 xcodebuild 출력의 내부적인 목표 Sharpie 용도에 대 한 정보를 제공 하는 목표 Sharpie 대 한 입력으로 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 044FF669-0B81-4186-97A5-148C8B56EE9C
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: e820a0c208907a95dda4a50427bb4dac27b88964
ms.sourcegitcommit: bf18425f97b48661ab6b775195eac76b356eeba0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/01/2019
ms.locfileid: "64977900"
---
# <a name="advanced-manual-real-world-example"></a>고급 (수동) 실제 예제

**이 예제에서는 합니다 [Facebook에서 POP 라이브러리](https://github.com/facebook/pop)합니다.**

이 섹션에서는 바인딩, Apple의 사용 위치 하는 고급 방법을 `xcodebuild` 먼저 프로젝트를 빌드하려면 POP, 도구 및 목표 Sharpie에 대 한 입력 한 다음 수동으로 추론 합니다. 기본적으로 이전 섹션에서 내부에서 수행 하는 목표 Sharpie 다룹니다.

```
 $ git clone https://github.com/facebook/pop.git
Cloning into 'pop'...
   _(more git clone output)_

$ cd pop
```

POP 라이브러리 Xcode 프로젝트에 있으므로 (`pop.xcodeproj`), 바로 사용할 수 있습니다 `xcodebuild` POP을 만들려고 합니다. 이 프로세스는 목표 Sharpie 구문 분석 해야 하는 헤더 파일을 생성할 다시 수 있습니다. 이 때문에 중요 한 바인딩 되기 전에 작성 합니다. 통해 빌드할 때 `xcodebuild` 을 통과 하는 같은 SDK 식별자 및 아키텍처는 목표 Sharpie 전달할 (다시 말하지만 목표 Sharpie 3.0 일반적으로이를 수행할 수 있습니다!) 하려면:

```
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

일부로 많은 빌드 정보를 콘솔에 출력 됩니다 `xcodebuild`합니다. 위에 표시를 보면 "CpHeader" 대상이 실행 된 헤더 파일을 빌드 출력 디렉터리에 복사 된 가정해 보십시오. 이 경우 경우가 및 바인딩 쉽게: 네이티브 라이브러리의 빌드의 일부로 헤더 파일 종종 위치에 복사 됩니다는 "공개적 으로" 사용할 수 있는 쉽게 바인딩에 대 한 구문 분석 해야 할 수 있습니다. 이 경우 알게 POP의 헤더 파일에는 `build/Headers` 디렉터리입니다.

이제 POP 바인딩할 준비가 되었습니다. SDK에 대 한 빌드 하려고 한다는 것을 알게 `iphoneos8.1` 사용 하 여 합니다 `arm64` 아키텍처 및 신경 써야 하는 헤더 파일에 `build/Headers` POP git checkout 아래. 살펴보면는 `build/Headers` 디렉터리 헤더 파일의 수를 살펴보겠습니다.

```
$ ls build/Headers/POP/
POP.h                    POPAnimationTracer.h     POPDefines.h
POPAnimatableProperty.h  POPAnimator.h            POPGeometry.h
POPAnimation.h           POPAnimatorPrivate.h     POPLayerExtras.h
POPAnimationEvent.h      POPBasicAnimation.h      POPPropertyAnimation.h
POPAnimationExtras.h     POPCustomAnimation.h     POPSpringAnimation.h
POPAnimationPrivate.h    POPDecayAnimation.h
```

보면 `POP.h`, 것이 라이브러리의 기본 최상위 헤더 파일을 보면 `#import`s 기타 파일입니다. 이 인해 하기만 전달 `POP.h` 목표 Sharpie를 clang 백그라운드에서 나머지 부분에서 수행 될 작업:

```
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

전달 된 것을 알 수 있습니다는 `-scope build/Headers` 목표 Sharpie 인수입니다. C 및 Objective-c 라이브러리 해야 하기 때문에 `#import` 또는 `#include` 라이브러리와 바인딩 하려는 API 아니라 구현 세부 정보는 다른 헤더 파일은 `-scope` 목표 Sharpie에서 정의 되지 않은 모든 API를 무시 하도록 지정 하는 인수는 내 임의 위치에 파일이 `-scope` 디렉터리입니다.

하지만 찾을 수 있습니다는 `-scope` 인수는 명확 하 게 구현 된 라이브러리에 대 한 선택적 보통, 명시적으로 제공 하더라도 피해는 합니다.

지정한 또한 `-c -Ibuild/headers`합니다. 첫째, 합니다 `-c` 명령줄 인수를 해석할 중지 하 고 모든 후속 인수를 전달 하려면 Sharpie 목표를 지정 하는 인수 _clang 컴파일러에 직접_입니다. 따라서 `-Ibuild/Headers` 아래에서 검색할 clang 지시 하는 clang 컴파일러 인수를 포함 `build/Headers`, POP 헤더 거주 하는 것이입니다. 이 인수를 사용 하지 않고 clang 파일을 찾을 수 있는 위치 알지 못합니다입니다 `POP.h` 는 `#import`연산입니다. _목표 Sharpie를 사용 하 여 거의 모든 "문제" 하느냐, clang 전달할 대상을 파악_합니다.

### <a name="completing-the-binding"></a>바인딩이 완료

목표 Sharpie 지금 생성 `Binding/ApiDefinitions.cs` 고 `Binding/StructsAndEnums.cs` 파일입니다.

목표 Sharpie 기본 첫 번째 단계에서 바인딩, 이며 일부의 경우에는 것이 하기만 하면 됩니다. 하지만 목표 Sharpie에 완료 된 후 생성 된 파일을 수동으로 수정 하려면 개발자는 일반적으로 해야 위에서 설명한 대로 [문제를 해결](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) 는 처리할 수 없는 자동 도구입니다.

이 두 파일 이제 Mac 용 Visual Studio에서 바인딩 프로젝트에 추가 수 또는 직접 전달할 업데이트가 완료 되 면 합니다 `btouch` 또는 `bmac` 최종 바인딩을 생성 하는 도구입니다.

바인딩 프로세스를 세부적 설명은 참조 하십시오 우리의 [연습은 지침](~/ios/platform/binding-objective-c/walkthrough.md)합니다.
