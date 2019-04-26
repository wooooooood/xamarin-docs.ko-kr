---
title: 다음 메시지가 표시되며 내 앱 제출이 실패한 이유는 무엇인가요? 허용 되지 않는 경로 ("iTunesMetadata.plist")에서 찾음...?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: AE1BBDC6-4D3A-4471-876B-FE28B6E59A24
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/03/2018
ms.openlocfilehash: a4d2769bb0cb4e119f1c90353657471b44eb6c7c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61420907"
---
# <a name="why-does-my-app-submission-fail-with-disallowed-paths--itunesmetadataplist--found-at--"></a>다음 메시지가 표시되며 내 앱 제출이 실패한 이유는 무엇인가요? 허용 되지 않는 경로 ("iTunesMetadata.plist")에서 찾음...?

> 오류: ERROR ITMS-90047: "경로 ("iTunesMetadata.plist")에서 찾을 수 없습니다. Payload/iPhoneApp1.app"

이 오류와 같은 문제에 도달 하지 못하게 하려면 Apple 앱 스토어 확인 프로세스에 변경의 결과인 [버그 29180](https://bugzilla.xamarin.com/show_bug.cgi?id=29180)합니다. 이 특정 오류는 _되지_ 에 관련 된 특정 버전의 Xamarin이 설치 되어 있는 것 이므로 다운 그레이드 됩니다 _하지_ 도움말입니다.

포럼 토론을 참조 하세요 [여기](https://forums.xamarin.com/discussion/40388/disallowed-paths-itunesmetadata-plist-found-at-when-submitting-to-app-store/p1) 대안 정보 및이 문제에 대 한 최신 업데이트에 대 한 합니다.
