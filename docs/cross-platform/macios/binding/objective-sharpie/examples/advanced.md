---
title: 고급 (수동) 실제 예
description: 이 문서에서는 xcodebuild의 출력을 객관적인 Sharpie에 대 한 입력으로 사용 하는 방법에 대해 설명 합니다 .이를 통해 Sharpie가 내부적으로 수행 하는 작업을 파악할 수 있습니다.
ms.prod: xamarin
ms.assetid: 044FF669-0B81-4186-97A5-148C8B56EE9C
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: a4a6df4916ae5dcc2a0f826d2f0ab9d09167ba5f
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290043"
---
# <a name="advanced-manual-real-world-example"></a>고급 (수동) 실제 예

**이 예제에서는 [Facebook의 POP 라이브러리](https://github.com/facebook/pop)를 사용 합니다.**

이 섹션에서는 Apple `xcodebuild` 도구를 사용 하 여 먼저 POP 프로젝트를 빌드한 다음, 목표 Sharpie에 대 한 입력을 수동으로 추론 하는 바인딩에 대 한 고급 접근 방법을 설명 합니다. 이는 Sharpie가 이전 섹션의 내부적으로 수행 하는 작업을 설명 합니다.

```
 $ git clone https://github.com/facebook/pop.git
Cloning into 'pop'...
   _(more git clone output)_

$ cd pop
```

Pop 라이브러리에 Xcode 프로젝트 (`pop.xcodeproj`)가 있으므로를 사용 `xcodebuild` 하 여 pop를 빌드할 수 있습니다. 이 프로세스는 목표 Sharpie이 구문 분석 해야 할 수 있는 헤더 파일을 생성할 수 있습니다. 이 때문에 바인딩이 중요 합니다. 을 (를 `xcodebuild` ) 통해 빌드할 때 목적 Sharpie으로 전달 하려는 것과 동일한 SDK 식별자와 아키텍처를 전달 해야 합니다. 3.0 Sharpie 목표는 일반적으로이 작업을 수행할 수 있습니다.

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

콘솔에는의 `xcodebuild`일부로 빌드 정보가 많이 출력 됩니다. 위에 표시 된 것 처럼 헤더 파일이 빌드 출력 디렉터리에 복사 된 "CpHeader" 대상이 실행 된 것을 볼 수 있습니다. 이 경우에는 이러한 경우가 종종 있으며 바인딩을 용이 하 게 합니다. 기본 라이브러리 빌드의 일부로 헤더 파일은 일반적으로 "공용" 사용 가능 위치에 복사 되므로 바인딩을 위한 구문 분석을 용이 하 게 만들 수 있습니다. 이 경우 POP의 헤더 파일이 `build/Headers` 디렉터리에 있다는 것을 알 수 있습니다.

이제 POP를 바인딩할 준비가 되었습니다. 아키텍처를 `build/Headers` `iphoneos8.1` `arm64` 사용 하 여 SDK에 대해 빌드 하려고 하 고, 관심 있는 헤더 파일이 POP git 체크 인 아래에 있음을 알고 있습니다. `build/Headers` 디렉터리를 살펴보면 많은 헤더 파일이 표시 됩니다.

```
$ ls build/Headers/POP/
POP.h                    POPAnimationTracer.h     POPDefines.h
POPAnimatableProperty.h  POPAnimator.h            POPGeometry.h
POPAnimation.h           POPAnimatorPrivate.h     POPLayerExtras.h
POPAnimationEvent.h      POPBasicAnimation.h      POPPropertyAnimation.h
POPAnimationExtras.h     POPCustomAnimation.h     POPSpringAnimation.h
POPAnimationPrivate.h    POPDecayAnimation.h
```

`POP.h`를 살펴보면 라이브러리의 주 최상위 헤더 파일이 다른 파일 임을 `#import`확인할 수 있습니다. 이로 인해 Sharpie는 목표를 달성 하 `POP.h` 는 데만 필요 하며, clang는 백그라운드에서 나머지 작업을 수행 합니다.

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

목표 Sharpie에 `-scope build/Headers` 인수를 전달 했습니다. C 및 객관적인 c 라이브러리는 바인딩하려는 `#import` api `#include` 가 아니라 라이브러리의 구현 세부 정보를 가진 기타 헤더 파일을 가져야 하기 때문에 인수는 `-scope` Sharpie에 정의 되지 않은 모든 api를 무시 하는 것을 목표로 합니다. `-scope` 디렉터리 내의 파일입니다.

인수는 `-scope` 명확 하 게 구현 된 라이브러리의 경우 선택적 이지만 명시적으로 제공 하는 데에는 나쁜 영향을 미치지 않습니다.

또한를 지정 `-c -Ibuild/headers`했습니다. 첫째, 인수 `-c` 는 Sharpie가 명령줄 인수 해석을 중지 하 고 모든 후속 인수를 _clang 컴파일러에 직접_전달 한다는 것을 목표로 합니다. 따라서는 POP 헤더가 라이브 인에서 `build/Headers`포함 된을 검색 하도록 clang에 게 지시 하는 clang 컴파일러 인수입니다. `-Ibuild/Headers` 이 인수를 `POP.h` 사용 `#import`하지 않으면 clang에서 파일을 찾을 수 있는 위치를 알 수 없습니다. _객관적인 Sharpie 있지만를 사용 하는 거의 모든 "문제"는 clang에 전달할 사항을 파악 하는 데 사용_됩니다.

### <a name="completing-the-binding"></a>바인딩 완료

이제 목표 Sharpie에서 및 `Binding/ApiDefinitions.cs` 파일 `Binding/StructsAndEnums.cs` 을 생성 했습니다.

이는 바인딩에서 기본적인 첫 번째 단계를 수행 하는 것 이며, 경우에 따라 필요한 경우가 Sharpie 있습니다. 그러나 위에서 설명한 대로 개발자는 목표 Sharpie 완료 된 후에 생성 된 파일을 수동으로 수정 하 여 도구에서 자동으로 처리할 수 없는 [문제를 해결](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) 해야 합니다.

업데이트가 완료 되 면 이제 이러한 두 파일을 Mac용 Visual Studio의 바인딩 프로젝트에 추가 하거나 `btouch` 또는 `bmac` 도구에 직접 전달 하 여 최종 바인딩을 생성할 수 있습니다.

바인딩 프로세스에 대 한 자세한 설명은 [전체 연습 지침](~/ios/platform/binding-objective-c/walkthrough.md)을 참조 하세요.
