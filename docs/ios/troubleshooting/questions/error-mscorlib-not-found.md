---
title: '런타임 오류: 어셈블리 mscorlib.dll을 찾을 수 없거나 로드할 수 없습니다.'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 1027E16C-2C14-4BB5-AAAB-342F3E28E22E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: 1ca6aa8c08d51b91f6a31407328f8949062bbede
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031157"
---
# <a name="runtime-error-the-assembly-mscorlibdll-was-not-found-or-could-not-be-loaded"></a>런타임 오류: 어셈블리 mscorlib.dll을 찾을 수 없거나 로드할 수 없습니다.

```
<Warning>: The assembly mscorlib.dll was not found or could not be loaded.
<Warning>: It should have been installed in the `/Developer/MonoTouch/Source/monotouch/builds/install/target64/lib/mono/2.0/mscorlib.dll' directory.
<Warning>: Service exited with abnormal code: 1
```

이 문제는 서명/IPA 만들기 `.xcarchive`에 *숨겨진* `.monotouch-32` 및 `.monotouch-64` 폴더가 없는 경우에 발생 합니다 .이는 런타임 오류를 트리거합니다.
