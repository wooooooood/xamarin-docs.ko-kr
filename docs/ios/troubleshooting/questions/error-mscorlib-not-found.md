---
title: '런타임 오류: 어셈블리 mscorlib.dll을 찾을 수 없거나 로드할 수 없습니다.'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 1027E16C-2C14-4BB5-AAAB-342F3E28E22E
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 04/03/2018
ms.openlocfilehash: 4a8e3827deadd5c5d183c61c53cbe8346949759b
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290497"
---
# <a name="runtime-error-the-assembly-mscorlibdll-was-not-found-or-could-not-be-loaded"></a>런타임 오류: 어셈블리 mscorlib.dll을 찾을 수 없거나 로드할 수 없습니다.

```
<Warning>: The assembly mscorlib.dll was not found or could not be loaded.
<Warning>: It should have been installed in the `/Developer/MonoTouch/Source/monotouch/builds/install/target64/lib/mono/2.0/mscorlib.dll' directory.
<Warning>: Service exited with abnormal code: 1
```

이 문제는 서명/IPA 만들기 `.monotouch-64` `.xcarchive` 에 대해 *숨겨진* `.monotouch-32` 및 폴더를 누락 하 여 런타임 오류를 트리거할 때 발생 합니다.

