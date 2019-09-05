---
title: '다음 메시지가 표시되며 내 iOS 9 앱이 실패한 이유는 무엇인가요? System.Exception: Objective-C 개체를 마샬링하지 못했습니다.'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8805ABEC-48D4-4CCB-A226-3A5B2ECE4BF0
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 04/03/2018
ms.openlocfilehash: 62a63dc5156d1acf9ad6ca15029978131c151726
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290465"
---
# <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-object"></a>다음 메시지가 표시되며 내 iOS 9 앱이 실패한 이유는 무엇인가요? System.Exception: Objective-C 개체를 마샬링하지 못했습니다.

다음 형식의 오류가 표시 될 수 있습니다.

> System.Exception: 목표-C 개체를 마샬링하는 데 실패 했습니다. 이 개체에 대 한 기존 관리 되는 인스턴스를 찾을 수 없습니다 ...

IOS 9의 API 변경에는 이제 기본 API에서 예상 하는 것 처럼 비관리 코드를 호출할 때 콜백 생성자를 사용 해야 합니다. 다음 줄을 사용 하 여 콜백 생성자를 클래스에 추가 합니다. 

`public foo (IntPtr handle) : base (handle)` 

### <a name="next-steps"></a>다음 단계

추가 지원이 필요 하면 microsoft에 문의 하거나, 위의 정보를 사용한 후에도이 문제가 계속 발생 하는 경우 [Xamarin에 사용할 수 있는 지원 옵션](~/cross-platform/troubleshooting/support-options.md) 을 참조 하세요. 연락처 옵션, 제안 사항 및 필요한 경우 새 버그를 제출 하는 방법에 대 한 자세한 내용은을 참조 하세요. . 
