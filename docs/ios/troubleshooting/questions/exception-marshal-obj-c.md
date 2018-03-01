---
title: "이유는 응용 프로그램 iOS 9와 함께 실패: System.Exception: Objective-c 개체를 마샬링하는 실패 했습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 8805ABEC-48D4-4CCB-A226-3A5B2ECE4BF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: baba2526eefa1b69d47da47b73ea0bd417ecdc57
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-object"></a>이유는 응용 프로그램 iOS 9와 함께 실패: System.Exception: Objective-c 개체를 마샬링하는 실패 했습니다.

이 폼의 오류가 표시 될 수 있습니다.

> Objective C 개체를 마샬링하 System.Exception: 하지 못했습니다... 이 개체에 대 한 기존 관리 되는 인스턴스를 찾을 수 없습니다...

IOS 9에서에서 API 변경 예상 이제 기본 API로 비관리 코드를 호출할 때 콜백 생성자를 사용 하도록 해야 합니다. 콜백 생성자 클래스를 추가 하려면 다음 줄을 사용 합니다. 

`public foo (IntPtr handle) : base (handle) ` 

### <a name="next-steps"></a>다음 단계

추가 지원을 문의 또는 위의 정보를 활용 한 후에이 문제가 여전히 발생 하는 경우를 참조 하십시오 [지원 옵션에 대해 Xamarin에 대 한 사용할 수 있는?](~/cross-platform/troubleshooting/support-options.md) 연락처 옵션, 제안 사항에 대 한 내용은 하는 방법 필요한 경우 새 버그를 파일입니다. 
