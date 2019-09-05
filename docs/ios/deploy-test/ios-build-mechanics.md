---
title: iOS 빌드 메커니즘
description: 이 가이드에서는 앱의 시간을 맞추는 방법 및 빠른 빌드를 위해 모든 빌드 구성에 사용 가능한 메서드를 사용하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 06FD3940-D666-4C9E-BC3E-BBE481EF8012
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/18/2017
ms.openlocfilehash: ec510d46bf1b46cf8fd70c8f4d43b3108f46a010
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70284783"
---
# <a name="ios-build-mechanics"></a>iOS 빌드 메커니즘

_이 가이드에서는 앱의 시간을 맞추는 방법 및 빠른 빌드를 위해 모든 빌드 구성에 사용 가능한 메서드를 사용하는 방법을 설명합니다._

뛰어난 애플리케이션을 개발하려면 단순히 작동하는 코드를 작성하는 것보다 더 많은 것이 필요합니다. 잘 작성된 앱이란 더 작고 더 빠르게 실행되는 앱을 사용하여 더 빠르게 빌드를 수행할 수 있도록 최적화 기능을 포함해야 합니다. 이러한 최적화는 사용자는 물론이고 프로젝트를 작업하는 모든 개발자에게 보다 나은 환경을 제공합니다. 앱을 처리할 때 모든 항목이 제 시간에 수행되도록 보장하려면 최적화가 필수입니다. 

기본 옵션은 안전하고 빠르지만 모든 상황에 최적인 것은 아닙니다. 또한 개별 프로젝트에 따라 다양한 옵션이 개발 주기를 느리게 하거나 단축할 수 있습니다. 예를 들어 기본 제거에는 시간이 걸리지만, 매우 작은 크기를 얻는다면 제거에 걸린 시간은 신속한 배포를 통해 복구되지 않습니다. 반면, 기본 제거로 앱을 현저하게 축소할 수 있으며, 이 경우 더 빠르게 배포됩니다. 이는 프로젝트마다 다르며, 이것을 알 수 있는 유일한 방법은 테스트입니다.

Xamarin 빌드 속도는 프로세서 기능, 버스 속도, 실제 메모리의 양, 디스크 속도, 네트워크 속도 등 성능에 영향을 미칠 수 있는 컴퓨터의 다양한 용량과 기능의 영향을 받을 수 있습니다. 이러한 성능 제한은 이 문서의 범위를 벗어나며 개발자의 책임입니다.


## <a name="timing-apps"></a>타이밍 앱

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Mac용 Visual Studio 내에서 진단 MSBuild 출력을 사용하려면:

1. **Mac용 Visual Studio > 기본 설정...** 클릭
2. 왼쪽 트리 뷰에서 **프로젝트 > 빌드** 선택
3. 오른쪽 패널에서 로그 세부 정보 표시 드롭다운을 **진단**으로 설정합니다.  [![](ios-build-mechanics-images/image2.png "로그 세부 정보 표시 설정")](ios-build-mechanics-images/image2.png#lightbox)
4. **확인** 을 클릭합니다.
5. Mac용 Visual Studio 다시 시작
6. 패키지를 지우고 다시 빌드
7. 빌드 출력 단추를 클릭하여 오류 패드 내에서 진단 출력 보기(보기 > 패드 > 오류)


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio 내에서 진단 MSBuild 출력을 사용하려면:

1. **도구 > 옵션...** 클릭
2. 왼쪽 트리 뷰에서 **프로젝트 및 솔루션 > 빌드 및 실행** 선택
3. 오른쪽 패널에서 *MSBuild 빌드 출력 세부 정보 표시 드롭다운*을 **진단**으로 설정합니다.  [![](ios-build-mechanics-images/image2-vs.png "MSBuild 빌드 출력 세부 정보 표시 설정")](ios-build-mechanics-images/image2-vs.png#lightbox)
4. **확인** 을 클릭합니다.
5. 패키지를 지우고 다시 빌드합니다.
6. 출력 패널에 진단 출력이 표시됩니다.

-----

## <a name="timing-mtouch"></a>타이밍 mtouch

Mtouch 빌드 프로세스과 관련된 정보를 표시하려면 **프로젝트 옵션**에서 mtouch 인수에 `--time --time`을 전달합니다. 출력은 `MTouch` 작업을 검색하여 빌드 출력에서 찾을 수 있습니다.

```csharp
Setup: 36 ms
Resolve References: 982 ms
Extracted native link info: 987 ms
...
Total time: 1554 ms
```

## <a name="connecting-from-visual-studio-with-build-host"></a>빌드 호스트를 사용하여 Visual Studio에서 연결

기술적으로 Xamarin 도구는 OS X 10.10 Yosemite 이상을 실행할 수 있는 모든 Mac에서 작동합니다. 그러나 Mac의 성능에 따라 개발자 환경 및 빌드 시간이 영향을 받을 수 있습니다.

연결이 끊어진 상태에서는 Windows의 Visual Studio가 C# 컴파일 단계만 수행하고 연결 또는 AOT 컴파일을 수행하거나 앱을  _.app_  번들로 패키지하거나 앱 번들을 서명하려는 시도를 하지 않습니다. (C# 컴파일 단계에서는 거의 성능 병목 현상이 거의 발생하지 않습니다.) Mac용 Visual Studio의 Mac 빌드 호스트에서 바로 빌드하여 파이프라인에서 빌드 속도가 느려지는 지점을 찾아보세요.


뿐만 아니라, 응답 속도가 느려지는 보다 일반적인 위치 중 하나는 Windows 컴퓨터와 Mac 빌드 호스트 간의 네트워크 연결입니다. 그 원인은 네트워크상의 물리적 제한일 수도 있고(무선 연결을 사용하는 경우) 또는 포화된 컴퓨터(예: 클라우드 서비스의 Mac)를 통과해야 하기 때문일 수도 있습니다.

## <a name="simulator-tricks"></a>시뮬레이터 트릭

 
모바일 애플리케이션을 개발할 때에는 코드를 신속하게 배포하는 것이 중요합니다. 속도 및 디바이스 프로비전 요구 사항 미충족을 비롯한 다양한 이유로, 개발자가 사전 설치된 시뮬레이터 또는 에뮬레이터에 배포하기로 선택하는 경우가 종종 있습니다. 개발자 도구를 생산하는 제조업체의 입장에서 시뮬레이터 또는 에뮬레이터를 제공하기 위한 의사 결정을 한 마디로 요약하자면, 속도와 호환성 사이에서 적당한 타협이 필요합니다. 

Apple은 제한이 적은 코드 실행 환경을 구축하여 호환성보다 속도를 우선하는 iOS 개발용 시뮬레이터를 제공합니다. 제한이 적은 이 환경에서는 Xamarin이 시뮬레이터에 JIT(Just Time) 컴파일러를 사용할 수 있으며(디바이스의 [AOT](~/ios/internals/architecture.md)와는 반대로), 따라서 런타임에 빌드가 네이티브 코드로 컴파일됩니다. Mac이 디바이스보다 훨씬 빠르기 때문에 이렇게 하면 성능이 향상됩니다.

시뮬레이터는 공유 애플리케이션 시작 관리자를 사용하며, 매번 시작 관리자를 빌드하는 대신 디바이스에서 필요할 때 다시 사용할 수 있습니다.

위의 정보를 염두에 두고, 아래 목록에서 최적의 성능을 제공하기 위해 시뮬레이터에서 앱을 빌드하고 배포할 때 수행해야 하는 단계에 대한 정보를 확인하세요.
 
### <a name="tips"></a>팁

- 빌드 시: 
  - 프로젝트 옵션에서 **PNG 이미지 최적화** 옵션의 선택을 취소합니다. 시뮬레이터에서 빌드할 때에는 이 최적화가 필요 없습니다.
  - 링커를 **연결하지 않음**로 설정합니다. 링커를 실행하면 상당한 시간이 걸리므로 사용하지 않는 것이 훨씬 빠릅니다.
  - `--nofastsim` 플래그를 사용하여 공유 애플리케이션 시작 관리자를 사용하지 않도록 설정하면 시뮬레이터 빌드가 훨씬 느려집니다. 이 플래그가 더 이상 필요 없으면 제거하세요.
  - 이 경우 공유 simlauncher 주 실행 파일을 다시 사용할 수 없고 애플리케이션 관련 실행 파일을 모든 빌드에 대해 컴파일해야 하므로 네이티브 라이브러리를 사용하면 속도가 느립니다.
- 배포 시
  - 되도록이면 시뮬레이터를 항상 실행합니다. 시뮬레이터 콜드 부팅에 최대 12초가 걸릴 수 있습니다.
- 추가 팁
  - 다시 빌드는 빌드 전에 정리되므로 다시 빌드보다는 빌드를 사용합니다. 정리 시에는 사용 가능한 참조가 제거되므로 정리에 시간이 오래 걸릴 수 있습니다.
  - 시뮬레이터는 샌드박스를 적용하지 않는다는 사실을 활용합니다. 비디오 또는 프로젝트에 포함된 기타 자산처럼 크기가 큰 리소스는 시뮬레이터에서 앱이 시작될 때마다 비용이 많이 드는 파일 복사 작업을 만들 수 있습니다. 이러한 파일을 홈 디렉터리에 배치하고 애플리케이션에서 전체 파일 경로로 참조하면 이처럼 비용이 많이 드는 작업을 방지할 수 있습니다.  
  - 확실하지 않은 경우 `--time --time` 플래그를 사용하여 변경 내용을 측정합니다.

아래 스크린샷은 iOS 옵션에서 시뮬레이터에 대해 이러한 옵션을 설정하는 방법을 보여줍니다.

[![](ios-build-mechanics-images/image3.png "옵션 설정")](ios-build-mechanics-images/image3.png#lightbox)

## <a name="device-tricks"></a>디바이스 트릭

시뮬레이터는 iOS 디바이스에 사용되는 빌드의 작은 하위 집합과 비슷하므로 디바이스에 배포하는 것은 시뮬레이터에 배포하는 것과 비슷합니다. 디바이스에 대해 빌드하려면 여러 추가 단계가 필요하지만, 앱을 최적화하는 추가 기회를 얻을 수 있다는 장점이 있습니다.

### <a name="build-configurations"></a>빌드 구성

iOS 앱을 배포할 때 제공되는 여러 가지 빌드 구성이 있습니다. 각 구성을 정확하게 이해하여 최적화가 필요한 시기와 이유를 알아야 합니다.

- 디버그
  - 앱이 개발 중일 때 사용해야 하는 기본 구성이며, 따라서 최대한 빨라야 합니다.
- Release
  - 릴리스 빌드는 사용자에게 제공되는 빌드이며 무엇보다도 성능에 초점을 맞춰야 합니다. 릴리스 구성을 사용할 때 LLVM 최적화 컴파일러를 사용하여 PNG 파일을 최적화할 수 있습니다.

 
빌드와 배포 간의 관계를 이해하는 것이 중요합니다. 배포 시간은 애플리케이션 크기의 함수입니다. 애플리케이션이 클수록 배포 시간이 길어집니다. 앱 크기를 최소화하면 배포 시간을 줄일 수 있습니다.

앱 크기를 줄이면 빌드 시간도 줄일 수 있습니다. 애플리케이션에서 코드를 제거하는 것이 사용되지 않는 코드를 컴파일하는 것보다 시간이 적게 걸리기 때문입니다. 개체 파일 크기가 작으면 연결 속도가 빠르고, 따라서 생성할 기호 수가 적은 작은 실행 파일이 만들어집니다. 따라서 공간이 절약되는 추가적인 장점이 있으며, 이러한 이유로 모든 디바이스 빌드에 기본적으로 **SDK 연결**이 사용됩니다. 

> [!NOTE]
> 사용되는 IDE에 따라 **SDK 연결** 옵션이 링크 프레임워크 SDK 전용 또는 SDK 연결 어셈블리 전용으로 표시될 수 있습니다.
 

### <a name="tips"></a>팁

- 빌드: 
  - 단일 아키텍처(예: ARM64)를 빌드하는 것이 FAT 이진 파일(예: ARMv7 + ARM64)보다 빠릅니다.
  - 디버깅할 때 PNG 파일을 최적화하지 마세요.
  - 모든 어셈블리를 연결하는 방안을 고려해 봅니다. 모든 어셈블리를 최적화 
  -  `--dsym=false`를 사용하여 디버그 기호 생성을 해제합니다. 그러나 디버그 기호 생성을 해제하면 앱이 제거되지 않은 경우에만, 앱을 빌드한 컴퓨터에서만 크래시 보고서가 기호화됩니다.

 
다음과 같은 항목을 피해야 합니다.

- Fat 이진 파일(디버그) 
- 링커 `--nolink` 사용 안 함 
- 제거 사용 안 함 
  - 기호 `--nosymbolstrip` 
  - IL(릴리스) `--nostrip`.  
 
추가 팁 

- 시뮬레이터와 마찬가지로, 다시 빌드보다는 빌드를 사용합니다. 
  - AOT 어셈블리(개체 파일)는 캐시됩니다. 
- dsymutil을 실행하는 기호 때문에 디버그 빌드는 시간이 오래 걸리며, 결국 크기가 커지기 때문에 디바이스를 업로드하는 데 더 오랜 시간이 걸립니다. 
- 릴리스 빌드는 기본적으로 어셈블리를 IL 제거합니다. 이 작업에는 시간이 얼마 걸리지 않으며 디바이스에 더 작은 앱을 배포하는 것으로 만회할 수 있습니다.
- 모든 빌드(디버그)에 큰 정적 파일을 배포하지 마세요. 
  - UIFileSharingEnabled(info.plist)를 사용합니다. 
    - 자산을 한 번만 업로드하면 됩니다. 
- 확실하지 않은 경우 `--time --time` 플래그를 사용하여 변경 내용을 측정합니다.

아래 스크린샷은 iOS 옵션에서 시뮬레이터에 대해 이러한 옵션을 설정하는 방법을 보여줍니다.

[![](ios-build-mechanics-images/image4.png "옵션 설정")](ios-build-mechanics-images/image4.png#lightbox)

## <a name="using-the-linker"></a>링커 사용

애플리케이션을 빌드할 때 mtouch는 관리 코드에 대한 링커를 사용하며, 애플리케이션에서 사용하지 않는 코드를 제거합니다. 이렇게 하면 이론적으로 빌드 크기가 작아지고, 따라서 속도가 빨라집니다. 링커에 대한 자세한 내용은 [iOS에서 연결](~/ios/deploy-test/linker.md) 가이드를 참조하세요.

링커를 사용할 때 다음 옵션을 고려하세요.

- 디바이스 빌드에 대해 **연결하지 않음**을 선택하면 매우 긴 시간이 걸리고, 생성되는 앱의 크기가 커집니다. 
  - Apple에서는 크기 제한을 초과하는 앱을 거부합니다. `MinimumOSVersion`에 따라 크기 제한이 60MB일 수도 있습니다. 
  - 기본 실행 파일이 포함됩니다. 
  - 시뮬레이터 빌드에서는 JIT 컴파일이 사용되므로(디바이스의 AOT와는 달리) 연결하지 않음을 사용하는 것이 더 빠릅니다.
- SDK 연결은 기본 옵션입니다.
- 모두 연결은 NuGets 또는 Components처럼 자신의 고유 코드가 아닌 코드를 사용하는 경우에 특히 안전하지 않을 수 있습니다. 어셈블리를 연결하지 않기로 선택하면 이러한 서비스의 모든 코드가 애플리케이션에 포함되므로 앱 크기가 커질 수 있습니다. 
  - 그러나 **모두 연결**을 선택하면 애플리케이션 크래시 가능성이 있으며, 특히 외부 구성 요소가 사용되는 경우에 더욱 가능성이 높습니다. 이는 특정 형식에 리플렉션을 사용하는 일부 구성 요소 때문입니다.
  - 정적 분석과 리플렉션은 함께 작동하지 않습니다. 

[`[Preserve]` 특성](~/ios/deploy-test/linker.md)을 사용하여 항목을 애플리케이션 내부에 유지하라고 도구에 지시할 수 있습니다. 

소스 코드에 대한 액세스 권한이 없는 경우 또는 소스 코드가 도구에서 생성되며 소스 코드를 변경할 생각이 없는 경우 보존해야 하는 모든 형식 및 구성원에 대해 설명하는 XML 파일을 만들어서 소스 코드를 연결할 수 있습니다. 그런 후 마치 Attributes를 사용하는 것처럼 코드를 정확하게 처리하는 `--xml={file.name}.xml` 플래그를 프로젝트 옵션에 추가할 수 있습니다.


### <a name="partially-linking-applications"></a>애플리케이션을 부분적으로 연결 

애플리케이션을 부분적으로 연결하는 것도 가능하며, 이렇게 하면 애플리케이션의 빌드 시간을 최적화하는 데 도움이 됩니다.

- `Link All`을 사용하고 일부 어셈블리를 건너뛰기 
  - 애플리케이션 크기 최적화 중 일부는 손실됩니다.
  - 소스 코드에 액세스할 필요가 없습니다.
  - `--linkall --linkskip=fieldserviceiOS`를 예로 들 수 있습니다.
 
- 필요한 어셈블리에 대한 `Link SDK` 옵션 및 `[LinkerSafe]` 특성 사용 
  - 소스 코드에 액세스해야 합니다.
  - 어셈블리를 연결해도 안전하며 어셈블리가 마치 Xamarin SDK인 것처럼 처리된다는 사실을 시스템에 알려줍니다.
 
### <a name="objective-c-bindings"></a>Objective-C 바인딩 

- 바인딩에 대한 `[Assembly: LinkerSafe]` 특성을 사용하면 시간과 크기를 줄일 수 있습니다.

- SmartLink 
  - 네이티브 쪽에서 수행 
  - `[LinkWith (SmartLink=true)]` 특성 사용
  - 이렇게 하면 연결 중인 라이브러리에서 네이티브 링커가 네이티브 코드를 쉽게 제거할 수 있습니다. 
  - 이때는 기호의 동적 조회가 함께 작동하지 않습니다. 

## <a name="summary"></a>요약

이 가이드에서는 iOS 애플리케이션의 시간을 맞추는 방법과 프로젝트의 빌드 구성 및 옵션에 따라 고려해야 할 옵션에 대해 설명했습니다. 

<!-----
# Benchmarks

## Layer 1: building again after making modifications, but _without_ cleaning should be faster 
 
The app should build a bit more quickly if you have only made changes to a subset of the libraries and you do not clean the build before re-deploying. 
 
 
 
### Clean build time 
178 seconds 
 
 
### Build again (without cleaning) after making _no changes_ 
12.5 seconds 
 
 
### Build again (without cleaning) after changing 1 line in "ViewIOS/ImageResourcesHelper.cs" 
3 trials: 45 seconds, 43 seconds, 43 seconds 
 
 
### Build again (without cleaning) after changing 1 line in each of the following files 
 
- ViewIOS/ImageResourcesHelper.cs 
- Sales.Native.Core.Tools/UIComponents/ListView/IListView.cs 
- View.Models/Mailing/MailingModel.cs 
 
3 trials: 45 seconds, 45 seconds, 45 seconds 
 
 
 
### Build again (without cleaning) after changing 1 line in each of the following files 
 
- ViewIOS/ImageResourcesHelper.cs 
- Sales.Native.Core.Tools/UIComponents/ListView/IListView.cs 
- View.Models/Mailing/MailingModel.cs 
- Sales.Native.Core.IOS.Ext/ServiceInterfaces/AlertDialog/Dialog.cs 
- Sales.Native.Core.Tools.IOS.Ext/BaseViews/BaseNavigationViewController.cs 
- View.Common/Services/DataTransferResult.cs 
 
45 seconds 
 
 
 
 
 
 
## Layer 2: "app thinning" aka "device specific builds" 
 
The idea of "app thinning" is that the IDE will only build the 1 architecture needed for the specific device that you're deploying to (rather than _both_ 32-bit and 64-bit architectures). 
 
As of the latest "Xamarin 4" builds, you can now enable "app thinning" in Visual Studio via the "Project Options -> iOS Build -> Enable device-specific builds" setting. 
 
Or if you prefer you can achieve a similar result by changing the "Project Options -> iOS Build -> Advanced [tab] -> Supported architectures" to select just _one_ architecture (for example ARM64 if you are developing on a 64-bit device). 
 
 
 
(Caveat: I ran the following builds in Visual Studio for Mac on the Mac rather than on the command line.) 
 
### Clean build time without "device specific builds" 
177 seconds 
 
 
 
### Clean build time _with_ "device specific builds"  
2 trials: 106 seconds, 98 seconds 
 
 
 
### Build again (without cleaning) after changing 1 line in "ViewIOS/ImageResourcesHelper.cs" 
2 trials: 31 seconds, 31 seconds 
 
 
* * * 
 
 
## Using the same strategy, but explicitly setting "Supported architectures" to select ARM64 _only_ (rather than using "device specific builds") 
 
(These builds were again run on the command line using `xbuild`.) 
 
 
 
### Clean build time with "Supported architectures" set to ARM64 _only_ 
2 trials: 80 seconds, 91 seconds 
 
 
 
### Build again (without cleaning) after changing 1 line in "ViewIOS/ImageResourcesHelper.cs" 
2 trials: 26 seconds, 26 seconds 
 
 
 
 
 
[1] Mac system used for testing: MacBookAir5,2 
 
- 2.0 GHz Core i7 (I7-3667U) 
 
2 Cores with hyper-threading 
 
L2 Cache (per Core): 256 KB 
L3 Cache: 4 MB 
 
- Standard MacBook soldered-in solid-state storage 
 
- 8 GB RAM 
---->


## <a name="related-links"></a>관련 링크

- [블로그 게시물](https://blog.xamarin.com/xamarin-ios-build-improvements/)
- [iOS에서 연결](~/ios/deploy-test/linker.md)
- [사용자 지정 링커 구성](~/cross-platform/deploy-test/linker.md)
