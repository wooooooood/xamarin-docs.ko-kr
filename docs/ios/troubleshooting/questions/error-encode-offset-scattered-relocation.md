---
title: '컴파일 오류: 분산 재배치에서 오프셋 X를 인코딩할 수 없습니다.'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 84158C42-FE79-415A-A44A-D48C9E5E6760
ms.technology: xamarin-ios
author: chamons
ms.author: chhamo
ms.date: 08/07/2019
ms.openlocfilehash: df974649c8c9f1d1fb4c13d6d801eb0f6a3d1e77
ms.sourcegitcommit: 2e5a6b8bcd1a073b54604f51538fd108e1c2a8e5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68876232"
---
# <a name="compile-error-can-not-encode-offset-x-in-resulting-scattered-relocation"></a>컴파일 오류: 분산 재배치에서 오프셋 X를 인코딩할 수 없습니다.

```
1>C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\MSBuild\Xamarin\iOS\Xamarin.iOS.Common.targets(804,3): error GDC116A36: can not encode offset ‘0x1155504’ in resulting scattered relocation.
1> .long mono_aot_Xamarin_iOS_got - . + 12 (TaskId:141)
1> ^ (TaskId:141)
1>C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\MSBuild\Xamarin\iOS\Xamarin.iOS.Common.targets(804,3): error GDC116A36: can not encode offset ‘0x11555B8’ in resulting scattered relocation.
1> .long mono_aot_Xamarin_iOS_got - . + 16 (TaskId:141)
```

이 문제는 ARMv7와 같은 32 비트 아키텍처에 대해 빌드할 때 최종 바이너리가 네이티브 도구 체인에 비해 너무 클 경우 발생 합니다.

이 문제를 해결 하려면 다음을 수행 하십시오.

- 프로젝트 옵션의 iOS 빌드 창에서 해당 아키텍처에 대 한 빌드를 삭제 합니다.
- 링크를 사용 하거나 수동으로 코드를 제거 하 여 최종 실행 파일 크기 줄이기
