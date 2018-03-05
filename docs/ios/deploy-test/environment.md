---
title: "환경"
ms.topic: article
ms.prod: xamarin
ms.assetid: 67BFD4E1-276C-4B9F-9BD8-A5218D2BD529
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ef5cfa2a9eee0d5d01922e5b7b3f89a209396c56
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="environment"></a>환경

*실행 환경*은 프로그램 실행에 영향을 주는 환경 변수의 집합입니다. 환경 변수는 프로젝트의 속성에서 임시로 설정할 수도 있고, mtouch 패키징 도구에 추가 인수를 지정하여 영구적으로 설정할 수도 있습니다.

## <a name="temporary-environment-variables"></a>임시 환경 변수

임시 환경 변수는 프로젝트의 **속성**/**옵션** 창에 있는 **실행 > 일반** 섹션에서 설정합니다. 이러한 환경 변수는 Mac용 Visual Studio를 사용하여 응용 프로그램이 시작되는 경우에만 적용되며, 앱을 탭하여 수동으로 앱을 시작하는 경우에는 이러한 환경 변수가 설정되지 않습니다.

## <a name="permanent-environment-variables"></a>영구 환경 변수

영구 환경 변수는 mtouch 패키징 도구에 추가 인수를 지정하여 설정됩니다. 이러한 환경 변수는 실행 파일로 컴파일되며, 앱이 Xamarin Studio에서 시작되지 않더라도 설정됩니다.

## <a name="example"></a>예

```csharp
# log all exceptions to the device log
--setenv:MONO_TRACE=E:all

# to pass multipe environment variables, use --setenv multiple times
--setenv:MONO_TRACE=E:all --setenv:GC_DONT_GC=1
```

