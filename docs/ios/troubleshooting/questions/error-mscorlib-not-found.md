---
title: '런타임 오류: 어셈블리 mscorlib.dll 찾을 수 없거나 로드할 수 없습니다'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 1027E16C-2C14-4BB5-AAAB-342F3E28E22E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 3510ccafed3ed1b57d8ecd1b99ee8d10f952c57c
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350890"
---
# <a name="runtime-error-the-assembly-mscorlibdll-was-not-found-or-could-not-be-loaded"></a>런타임 오류: 어셈블리 mscorlib.dll 찾을 수 없거나 로드할 수 없습니다

```
<Warning>: The assembly mscorlib.dll was not found or could not be loaded.
<Warning>: It should have been installed in the `/Developer/MonoTouch/Source/monotouch/builds/install/target64/lib/mono/2.0/mscorlib.dll' directory.
<Warning>: Service exited with abnormal code: 1
```

이런 경우는 *숨겨진* `.monotouch-32` 및 `.monotouch-64` 폴더에서 누락 된를 `.xcarchive` 서명에 / IPA 만들기, 런타임 오류를 발생 시키는.

